---
title:  "Autowired VS private final"
categories:
  - springBoot
---

## 의존
- 변경에 의해 영향을 받는 관계
- 의존대상 B가 변하면 A에 영향 있음


## 의존 객체 구하는 방법
1. 의존성 주입
   - 의존하는 객체를 직접 생성하는 대신 의존 객체를 전달받는 방식을 사용
   - 의존 객체의 변경이 유연함
2. 객체 직접 생성
   - 클래스 내부에서 의존 객체를 직접 생성
   - 필드 또는 생성자에서 new 연산자를 이용해서 객체를 생성
   - 쉽지만 유지보수 관점에서 문제점을 유발할 수 있음

```java
// 객체 직접 생성
public class MemberService {
  private MemberDao = new MemberDao();

  public void regist(RegisterRequest req) {
    Member member = memberDao.selectByEmail.selectByEmail(req.getEmail());
    if(member != null) {
      throw new DuplicateMemeberException(req.getEmail());
    }
    memberDao.insert(new Member(req));
  }
}
```
<h5>출처 : 스프링5 프로그래밍 입문</h5>








## 2. 의존성 주입 (DI : Dependeny Injection)
- 
- 클래스 모델이나 코드에는 런타임 시점의 의존관계가 드러나지 않는다. 그러기 위해서는 인터페이스만 의존하고 있어야 한다.
- 런타임 시점의 의존관계는 컨테이너나 팩토리 같은 제3의 존재가 결정한다.
- 의존관계는 사용할 오브젝트에 대한 레퍼런스를 외부에서 제공(주입)해줌으로써 만들어진다.

- 이일민, 토비의 스프링 3.1, 에이콘(2012), p114






### 1. 필드 주입 @Autowired



### 2. 생성자 주입 private
- 의존 객체를 직접 생성하지 않고 생성자를 통헤 전달 받음
- 생성자를 통해 의존하고 있는 객체를 주입 받은 것

```java
// 객체 직접 생성
public class MemberService {
  private MemberDao;

  public MemberController(MemberService memberService) {
    this.memeberService = memberService;
  }

  public void regist(RegisterRequest req) {
    Member member = memberDao.selectByEmail.selectByEmail(req.getEmail());
    if(member != null) {
      throw new DuplicateMemeberException(req.getEmail());
    }
    memberDao.insert(new Member(req));
  }
}
```

