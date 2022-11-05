---
title:  "JPA 07. Querydsl"
categories:
  - JPA
---

## 목차

- [목차](#목차)
- [Querydsl](#querydsl)
- [fetch()](#fetch)
- [페이징](#페이징)

<br/><br/><br/><br/><br/>




---
<br/>

## Querydsl
- 동적쿼리 작성을 용이하게 함
- 쿼리문을 작성하기 위해 Q 타입 클래스를 사용
- JPQL을 만들어주는 빌더 역할
- 구분
  1. JPAQuery
     - EntityManager를 통해서 질의 처리
     - JPQL 쿼리문 사용
     - JPAQueryFactory를 통해 JPAQuery를 만들어 낼 수 있음
  2. JPAQueryFactory
     - EntityManager를 통해서 질의 처리
     - JPQL 쿼리문 사용
  3. JPASQLQuery, SQLQuery
     - EntityManager를 통해서 질의 처리
     - SQL 쿼리문 사용
     - 엔티티 클래스를 만들지 않고 사용 가능
     - 이미 테이블이 존재하는 경우 이를 바탕으로 쿼리타입을 생성
  4. SQLQueryFactory
     - JDBC를 이용하여 질의 처리
     - SQL 쿼리문 사용
     - 엔티티 클래스를 만들지 않고 사용 가능
     - 이미 테이블이 존재하는 경우 이를 바탕으로 쿼리타입을 생성



<br/>

```java
JPAQueryFactory query = new JPAQueryFactory(em);
QPerson person = new QPerson("p"); // 별칭
List<Person> persons = query.select(person)
        .from(person)
        .fetch();

List<Person> persons2 = query.selectFrom(person)
                .where(person.name.contains("k"), person.age.gt(22))
                .fetch();
```

```sql
-- persons2 sql
select
    person0_.id as id1_14_,
    person0_.age as age2_14_,
    person0_.name as name3_14_ 
from
    Person person0_ 
where
    (
        person0_.name like ? escape '!'
    ) 
    and person0_.age>?
```


<br/><br/><br/>

## fetch()
1. fetch : 여러 개의 조회 결과를 컬렉션으로 반환
2. fetchOne : 한 건의 조회 결과를 지정한 타입으로 반환
3. fetchFirst : 여러 개의 조회 결과 중 가장 처음 결과만 반환
4. fetchCount : 조회 결과가 아닌 조회에 포함된 레코드 개수를 long 타입 반환
5. fetchResults : 조회한 리스트와 함께 전체 개수를 QueryResults로 반환




## 페이징
방법
1. offset, limit 사용
  - offset(long offset) : 시작 지점
  - limit(@Nonnegative long limit) : 개수 (시작점 부터 n개의 행 가져옴)
2.  QueryModifiers 사용
  - QueryModifiers(Long limit, Long offset) 


```java
JPAQueryFactory query = new JPAQueryFactory(em);
QPerson person = new QPerson("p");
QAddress address = new QAddress("a");
// 시작점 0 부터 2개의 행 가져옴
List<Person> persons = query.selectFrom(person)
        .orderBy(person.age.asc())
        .offset(0)
        .limit(2)
        .fetch();

// persons와 동일 결과
QueryModifiers queryModifiers = new QueryModifiers(2L, 0L);
List<Person> persons2 = query.selectFrom(person)
        .orderBy(person.age.asc())
        .restrict(queryModifiers)
        .fetch();
```

<br/><br/><br/><br/><br/>
<h5>

[참고 강의 : 스프링과 JPA를 이용한 웹개발](http://www.kocw.net/home/cview.do?cid=5e6aec4a9ae2dd45)   
[참고 서적 : 김영한 - 자바 ORM 표준 JPA 프로그래밍](https://product.kyobobook.co.kr/detail/S000000935744)

</h5>