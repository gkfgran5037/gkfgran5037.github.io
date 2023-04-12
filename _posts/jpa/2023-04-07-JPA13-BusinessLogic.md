---
title:  "JPA 13. [Refecoring 2] 엔티티와 비즈니스 로직 분리"
categories:
  - jpa
---

## 리팩토링 계기
엔티티들은 서로의 객체를 직접 조작하고 있었으며, 비즈니스 로직이 엔티티안에 있었다. 이는 결합도가 높으며 유지보수에 어려움이 있어 보여 리팩토링을 하게 됐다. 
다음은 단계별로 리팩토링 하는 과정이다.
- 엔티티들은 서로의 객체를 직접 조작 -> 각 엔티티의 로직만 관리, 서비스단 분리
- 중복 코드 발생 -> 추상화 클래스, 템플릿 메서드 패턴으로 관리

<br/><br/><br/>




---

## 변경 전 코드

### FeedMeet.java

```java
@Entity
@Table(indexes = @Index(name="unique_idx_feedmeet_feed_user", columnList = "FEED_ID, USER_ID", unique = true))
public class FeedMeet extends BaseEntity {

    @Id
    @GeneratedValue(strategy = GenerationType.IDENTITY)
    private Long id;

    @ManyToOne(fetch = FetchType.LAZY)
    @JoinColumn(name = "FEED_ID", referencedColumnName = "ID")
    private Feed feed;

    @ManyToOne(fetch = FetchType.LAZY)
    @JoinColumn(name = "USER_ID", referencedColumnName = "ID")
    private User user;

    @Enumerated(EnumType.STRING)
    private ParticipantStatus status;

    public FeedMeet approve(){
        setStatus(ParticipantStatus.PARTICIPATING);
        this.feed.addParticipant(this.user);
        return this;
    }

    public void setStatus(ParticipantStatus status) {
        this.status = status;
    }

    // ~
}

```

### Feed.java

```java
@Entity
public class Feed extends BaseEntity {

    @Id
    @GeneratedValue(strategy = GenerationType.IDENTITY)
    private Long id;

    @ManyToOne(fetch = FetchType.LAZY)
    @JoinColumn(name = "writer", referencedColumnName = "id")
    private User writer;

    @OneToMany(mappedBy = "feed", cascade = CascadeType.ALL)
    private List<FeedMeet> feedMeets = new ArrayList<>();

    @Enumerated(EnumType.STRING)
    private FeedStatus status;

    private int maxUser;

    private int maxMale;

    private int maxFemale;

    private int curMale;

    private int curFemale;

    public void addParticipant(User user) {
        availableToAdd();

        if (user.getGender().equals(Gender.MALE)) {
            addCurMale();
            return;
        }
        addCurFemale();
    }

    public void addCurMale() {
        this.curMale++;
    }

    public void addCurFemale() {
        this.curFemale++;
    }

    public void availableToAdd() {
        // 정원 체크
        if (this.maxUser == curMale + curFemale) {
            throw new IllegalArgumentException("정원 다 참");
        }
    }

    // ~
}

```

<br/><br/><br/>




## 문제 1 : Feed 객체를 직접적으로 조작하고 있음 -> 각 엔티티의 로직만 관리, 서비스단 분리
1. 문제
   - FeedMeet 엔티티에서 Feed를 직접 조작하고 있음
   - 객체간의 의존성 높음
   - this.feed.addParticipant(this.user); 
2. 해결 방법
   - FeedMeet의 서비스단에서 각각 호출


### 변경 전
<br/>

```java
// FeedMeetService.java
@Transactional
public FeedMeet approve(final Long feedMeetId) {
    FeedMeet feedMeet = read(feedMeetId);
    User currentUser = userService.findCurrentUser();

    checkAvailableToApproveByFeedMeetAndActor(feedMeet, currentUser);

    return feedMeet.approve();
}



// FeedMeet.java
public FeedMeet approve(){
    setStatus(ParticipantStatus.PARTICIPATING);
    // this.user를 매개변수로 던짐 = FeedMeet 객체의 user 필드가 참가자로 추가되는 것
    this.feed.addParticipant(this.user);
    return this;
}

public void setStatus(ParticipantStatus status) {
    this.status = status;
}



// Feed.java
public void addParticipant(User user) {
    availableToAdd();

    if (user.getGender().equals(Gender.MALE)) {
        addCurMale();
        return;
    }
    addCurFemale();
}

```
<br><br><br>



### 변경 후
<br/>

```java
// FeedMeetService.java
@Transactional
public FeedMeet approve(final Long feedMeetId) {
    FeedMeet feedMeet = read(feedMeetId);
    User currentUser = userService.findCurrentUser();

    checkAvailableToApproveByFeedMeetAndActor(feedMeet, currentUser);

    // FeedMeet 로직
    feedMeet.setStatus(ParticipantStatus.PARTICIPATING);
    // Feed 로직
    Feed feed = feedMeet.getFeed();
    feed.addParticipant(feedMeet.getUser());
    return feedMeet;
}



// FeedMeet.java
// - approve() 등 서비스 로직 제거
public void setStatus(ParticipantStatus status) {
    this.status = status;
}



// Feed.java
public void addParticipant(User user) {
    availableToAdd();

    if (user.getGender().equals(Gender.MALE)) {
        addCurMale();
        return;
    }
    addCurFemale();
}
```

<br><br><br><br><br>
<br><br><br><br><br>


















---

## 문제 2 : 중복 코드 발생 -> 추상화 클래스로 관리

1. 문제
   - FeedMeet의 상태를 바꾸는 로직이 거의 중복됨
   - 각 상태별 체킹하는 항목 중복됨
   - FeedMeetStatus와 체킹 항목, 인원수 증감 부분 외 큰 틀은 동일함
   - 프로세스 ) FeedMeet 객체 조회 -> 체킹 로직 -> FeedMeet 상태 변경 -> Feed 참여인원 변경
2. 해결 방법
   - FeedMeet 상태로 값을 변경하는 추상화 클래스 생성 (ParticipationChanger.java)
     1. 템플릿 메서드 패턴 changeStatus(long feedMeetId)
        - 전체적인 프로세스를 정의
        - 하위 프로세스(객체 조회, 체킹 로직 등)들을 호출하는 메서드 생성
     2. 각 상태별로 다르게 구현되는 체킹 로직, 상태 변경 등은 protected 메서드로 지정
        - 일부 로직은 추상 메서드를 호출하여 자식 클래스에서 구현하도록 위임
        - 이를 사용하는 코드는 상세한 구현 내용에 대해서는 신경쓰지 않고도 이 메서드를 호출하여 사용할 수 있음
   - ApproveChanger.java, CancelChanger.java 등으로 자식 클래스 생성
     1. 각 상태별로 다르게 구현되는 체킹 로직, 상태 변경 등은 Override하여 구현
     2. participation.changeStatus(feedMeetId) 로 상태 변경
<br/><br/><br/>



### 변경 전
<br>

```java
// FeedMeetService.java

@Transactional
public FeedMeet approve(final Long feedMeetId) {
    FeedMeet feedMeet = read(feedMeetId);
    User currentUser = userService.findCurrentUser();

    checkAvailableToApproveByFeedMeetAndActor(feedMeet, currentUser);

    feedMeet.setStatus(ParticipantStatus.PARTICIPATING);
    Feed feed = feedMeet.getFeed();
    feed.addParticipant(feedMeet.getUser());
    return feedMeet;
}

@Transactional
public FeedMeet cancel(final Long feedMeetId) {
    FeedMeet feedMeet = read(feedMeetId);
    User currentUser = userService.findCurrentUser();
    checkAvailableToCancelByFeedMeetAndActor(feedMeet, currentUser);

    feedMeet.setStatus(ParticipantStatus.CANCEL);
    Feed feed = feedMeet.getFeed();
    feed.deductParticipant(feedMeet.getUser());
    return feedMeet;
}

@Transactional
public FeedMeet refusal(final Long feedMeetId) {
    FeedMeet feedMeet = read(feedMeetId);
    User currentUser = userService.findCurrentUser();
    checkAvailableToRefusalByFeedMeetAndActor(feedMeet,currentUser);

    feedMeet.setStatus(ParticipantStatus.REFUSAL);
    Feed feed = feedMeet.getFeed();
    feed.deductParticipant(feedMeet.getUser());
    return feedMeet;
}

private void checkAvailableToApproveByFeedMeetAndActor(FeedMeet feedMeet, User actor){
    if(!actor.isWritable()){
        throw new IllegalArgumentException("쓰기 권한 없음");
    }
    if (!feedMeet.isFeedWriter(actor)) {
        throw new IllegalArgumentException("해당 게시글의 작성자가 아닙니다.");
    }
    if (!feedMeet.isAvailableToApproveStatus()) {
        throw new IllegalArgumentException("승인할 수 없는 상태입니다. 상태 = " + feedMeet.getStatus().getName());
    }
}
private void checkAvailableToCancelByFeedMeetAndActor(FeedMeet feedMeet, User actor){
    if(!feedMeet.equalsParticipant(actor)) {
        throw new IllegalArgumentException("본인의 신청내역만 취소가 가능합니다.");
    }
    if (!feedMeet.isAvailableToCancelStatus()) {
        throw new IllegalArgumentException("승인할 수 없는 상태입니다. 상태 = " + feedMeet.getStatus().getName());
    }
    if(feedMeet.isWriterSelf()) {
        throw new IllegalArgumentException("게시글의 작성자는 항상 참여상태여야 합니다.");
    }
}
private void checkAvailableToRefusalByFeedMeetAndActor(FeedMeet feedMeet, User actor){
    if(!actor.isWritable()){
        throw new IllegalArgumentException("쓰기 권한 없음");
    }
    if (!feedMeet.isFeedWriter(actor)) {
        throw new IllegalArgumentException("해당 게시글의 작성자가 아닙니다.");
    }
    if (!feedMeet.isAvailableToRefusalStatus()) {
        throw new IllegalArgumentException("승인할 수 없는 상태입니다. 상태 = " + feedMeet.getStatus().getName());
    }
    if(feedMeet.isWriterSelf()) {
        throw new IllegalArgumentException("게시글의 작성자는 항상 참여상태여야 합니다.");
    }
}
```
<br/><br/><br/><br/><br/>










### 변경 후
<br>

#### FeedMeetService.java

```java
@Service
@RequiredArgsConstructor
public class FeedMeetService {
    private final FeedMeetRepository feedMeetRepository;

    private final FeedRepository feedRepository;

    private final UserService userService;

    @Transactional
    public FeedMeet approve(final Long feedMeetId) {
        ApproveChanger participation = new ApproveChanger(this, userService);
        return participation.changeStatus(feedMeetId);
    }

    @Transactional
    public FeedMeet cancel(final Long feedMeetId) {
        CancelChanger participation = new CancelChanger(this, userService);
        return participation.changeStatus(feedMeetId);
    }

    @Transactional
    public FeedMeet refusal(final Long feedMeetId) {
        RefusalChanger participation = new RefusalChanger(this, userService);
        return participation.changeStatus(feedMeetId);
    }

    // ~
}

```
<br/><br/><br/>




#### FeedMeet.java

```java
@Getter
@Entity
@NoArgsConstructor
@Table(indexes = @Index(name="unique_idx_feedmeet_feed_user", columnList = "FEED_ID, USER_ID", unique = true))
public class FeedMeet extends BaseEntity {
    // 식별값
    @Id
    @GeneratedValue(strategy = GenerationType.IDENTITY)
    private Long id;

    // 피드
    @ManyToOne(fetch = FetchType.LAZY)
    @JoinColumn(name = "FEED_ID", referencedColumnName = "ID")
    private Feed feed;

    // 참여자
    @ManyToOne(fetch = FetchType.LAZY)
    @JoinColumn(name = "USER_ID", referencedColumnName = "ID")
    private User user;

    @Enumerated(EnumType.STRING)
    private ParticipantStatus status;

    public void setStatus(ParticipantStatus status) {
        this.status = status;
    }

    // approve() 등 서비스 관련 로직은 엔티티에서 제거
    // ~
}

```
<br/><br/><br/>




#### ParticipationChanger.java

```java
public abstract class ParticipationChanger {

    private final FeedMeetService feedMeetService;
    private final UserService userService;

    protected ParticipationChanger(FeedMeetService feedMeetService, UserService userService) {
        this.feedMeetService = feedMeetService;
        this.userService = userService;
    }

    public FeedMeet changeStatus(long feedMeetId) {
        FeedMeet feedMeet = feedMeetService.read(feedMeetId);
        checkChangeAvailable(feedMeet, userService.findCurrentUser());
        changeFeedMeetStatus(feedMeet);
        changeParticipantNumber(feedMeet.getFeed(), feedMeet.getUser());
        return feedMeet;
    }

    protected void checkChangeAvailable(FeedMeet feedMeet, User currentUser) {
    }

    protected void changeFeedMeetStatus(FeedMeet feedMeet) {

    }

    protected void changeParticipantNumber(Feed feed, User actor) {
    }

    protected void checkWriterPermission(User currentUser) {
        if(!currentUser.isWritable()){
            throw new IllegalArgumentException("쓰기 권한 없음");
        }
    }

    protected void checkFeedWriter(FeedMeet feedMeet, User currentUser) {
        if (!feedMeet.isFeedWriter(currentUser)) {
            throw new IllegalArgumentException("해당 게시글의 작성자가 아닙니다.");
        }
    }

    protected void checkIsWriterSelf(FeedMeet feedMeet) {
        if(feedMeet.isWriterSelf()) {
            throw new IllegalArgumentException("게시글의 작성자는 항상 참여상태여야 합니다.");
        }
    }
}

```
<br/><br/><br/>





#### CancelChanger.java

```java
public class CancelChanger extends ParticipationChanger {

    private final ParticipantStatus status = ParticipantStatus.CANCEL;


    public CancelChanger(FeedMeetService feedMeetService, UserService userService) {
        super(feedMeetService, userService);
    }

    @Override
    protected void checkChangeAvailable(FeedMeet feedMeet, User currentUser) {
        checkParticipant(feedMeet, currentUser);
        checkIsWriterSelf(feedMeet);
        checkCancelStatus(feedMeet);
    }

    @Override
    protected void changeFeedMeetStatus(FeedMeet feedMeet) {
        feedMeet.setStatus(status);
    }

    @Override
    protected void changeParticipantNumber(Feed feed, User actor) {
        feed.deductParticipant(actor);
    }

    private void checkParticipant(FeedMeet feedMeet, User currentUser) {
        if(!feedMeet.equalsParticipant(currentUser)) {
            throw new IllegalArgumentException("본인의 신청내역만 취소가 가능합니다.");
        }
    }

    private void checkCancelStatus(FeedMeet feedMeet) {
        if (!feedMeet.isAvailableToCancelStatus()) {
            throw new IllegalArgumentException("취소할 수 없는 상태입니다. 상태 = " + feedMeet.getStatus().getName());
        }
    }
}

``` 











---

```java
```