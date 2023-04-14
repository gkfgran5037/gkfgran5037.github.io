---
title:  "JPA 14. [Refecoring 3] Fetch Type"
categories:
  - jpa
---

## Fetch Join
엔티티를 조회할 때 연관된 엔티티까지 전부 다 한꺼번에 가져오는 기능.
- 1차캐시에 데이터가 있어서 DB를 거치지 않고 데이터를 꺼내서 바로 반환함.
- JPQL로 작성
  - JpqRepository에서 제공해주는 것 아님
- INNER JOIN으로 호출됨
- 연관관계의 연관관계가 있을 경우에도 하나의 쿼리문으로 표현할 수 있으므로 매우 유리

<br/>

```java 
@Query("SELECT f FROM Feed f JOIN fetch f.목록")
```
<br/><br/><br/>



### 단점
  1. 연관관계 설정해놓은 FetchType을 사용할 수 없음
       - Fetch Join 사용 시 데이터 호출 시점에 모든 연관관계의 데이터를 가져오기 때문에 FetchType을 Lazy로 해놓는 것이 무의미함
  2. 페이징 쿼리를 사용할 수 없음.
       - 하나의 쿼리문으로 가져오다 보니 페이징 단위로 데이터를 가져오는 것이 불가능
  3. MultipleBagFetchException
        - 2row 이상의 OneToMany 자식 테이블에 Fetch Join을 선언했을 떄 발생

<br/><br/><br/>





---
<br/>

### 1. Fetch join의 페이징 문제
- 페이징과 함께 사용될 때 성능 문제가 발생할 수 있음
- 페이지 크기에 상관없이 모든 연관된 엔티티를 가져와서 인메모리(Ram, Heap Area)에 넣어놓고 가공하는 작업을 거치게 됨
- 연관된 엔티티가 많은 경우 메모리 사용량이 증가, 데이터베이스 쿼리의 성능 저하
- 쿼리문으로 LIMIT 안나감 
  - DB에서 limit 사용 시 5명의 정보가 아닌 인덱스 5개를 가져오며 데이터 누락 발생
- 해결 방법 : 
  1. ManyToOne 일 때 페이징 처리
  2. @BatchSize 사용

<br><br><br>



#### FeedService.java
<br>

```java
@ApiOperation(value = "피드 목록 조회", notes = "피드 목록 조회")
@GetMapping("/feeds")
public ApiResponse<List<FeedsRespDto>> feeds() {
    return ApiResponse.ok(feedService.findAllDesc());
}
```

<br><br><br>



#### FeedService.java
<br>

```java
@Transactional(readOnly = true)
public List<FeedsRespDto> findAllDesc() {
    return feedRepository.findAllByOrderByIdDesc()
            .stream()
            .map(FeedsRespDto::new)
            .collect(Collectors.toList());
}

```
<br><br><br>



#### FeedRepository.java
<br>

```java
// Feed - N : 1 -> User(writer)
@Query("SELECT f FROM Feed f" +
        " INNER JOIN FETCH f.writer")
List<Feed> findAllByOrderByIdDesc();

// Feed <- 1 : N -> FeedMeet
// FeedMeet - N : 1 -> User
@Query("SELECT f FROM Feed f" +
        " INNER JOIN FETCH f.feedMeets fm" +
        " INNER JOIN FETCH fm.user" +
        " WHERE f.id = :feedId")
Feed findByIdWithFeedMeets(@Param("feedId") long feedId);
```









---

### 변경 후 

```java
@ApiOperation(value = "피드 목록 조회", notes = "피드 목록 조회")
@GetMapping("/feeds")
public ApiResponse<List<FeedsRespDto>> feeds(@PageableDefault(size = 5) Pageable pageable) {
    return ApiResponse.ok(feedService.findAllDesc(pageable));
}
```

```java
@Transactional(readOnly = true)
public List<FeedsRespDto> findAllDesc(Pageable pageable) {
    return feedRepository.findAllByOrderByIdDesc(pageable)
            .stream()
            .map(FeedsRespDto::new)
            .collect(Collectors.toList());
}
```

```java
@Query("SELECT f FROM Feed f" +
        " INNER JOIN FETCH f.writer")
List<Feed> findAllByOrderByIdDesc(Pageable pageable);
```