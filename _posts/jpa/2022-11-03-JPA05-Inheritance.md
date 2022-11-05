---
title:  "JPA 05. 상속관계, Cascade"
categories:
  - Spring Boot
---

## 목차

- [상속전략](#상속전략)
  - [1. @MappedSuperclass](#1-mappedsuperclass)
  - [2. Table per Class](#2-table-per-class)
  - [3. Single Table](#3-single-table)
    - [@Inheritance](#inheritance)
    - [@DiscriminatorColumn](#discriminatorcolumn)
    - [@DiscriminatorValue](#discriminatorvalue)
  - [4. 조인 전략](#4-조인-전략)
  - [Cascade](#cascade)

<br/><br/><br/><br/><br/>




---
<br/>

# 상속전략

## 1. @MappedSuperclass
- 각각의 구현클래스를 개별 테이블에 매핑하는 전략
- 다수의 엔티티가 공통 속성을 공유할 수 있음
- mapped superclass가 지정된 클래스는 엔티티가 아니므로 테이블에 매핑되는 것은 아님
- 주로 createdBy, createdOn과 같이 테이블에서 공통적으로 사용되는 반복되는 필드를 각
테이블에 끼워 넣을 때 사용

<br/>

```java
// Entity X 테이블 생성 안 함
@MappedSuperclass // 슈퍼 클래스
public abstract class Publication {

    @Id @GeneratedValue(strategy = GenerationType.IDENTITY)
    @Column(name = "PUBLICATION_ID", updatable = false, nullable = false)
    protected Long id;

    @Column
    protected String title;

    @Column(name = "version")
    private int version;

    // getter setter
}
```

<br/>

```java
@Entity
public class Book2 extends Publication { // 상속 받음

    @Column
    private int pages;
}
```
결과  
<img width = "80%" src = "https://user-images.githubusercontent.com/42172353/199655535-1d4668fa-4f3e-43c3-aaae-d9163c084e9c.png" />

<h5>[출처 - 자바 ORM 표준 JPA 프로그래밍 (김영한 저)]</h5>
<br/><br/><br/>





## 2. Table per Class
- 구현 클래스마다 테이블 전략
- 서브 타입마다 하나의 테이블은 만듦
- 자식 테이블 각각에 필요한 컬럼 모두 있음
- superclass 또한 엔티티가 됨
- 권장되는 방법은 아님
- 공통사항(Publication)이 여기에도 있고 저기에도 있는 중복적인 구조
<br/><br/><br/>





## 3. Single Table
- 하나의 테이블에 저장하는 방식 (통합 테이블로 변환)
- 하나의 테이블만 조회하면 되므로 성능 측면에서 가장 유리
- Null허용 필드가 많아지는 단점이 존재
- 데이터 무결성이 꺠질 위험성이 높음

<br/><br/>

### @Inheritance
- 상속 매핑
- 부모 클래스에 사용
- 전략
  1. InheritanceType.JOINED
     - 조인 전략
     - 테이블 정규화됨
     - 외래 키 참조 무결성 제약조건 활용 가능
     - 조회할 떄 조인이 많이 사용되므로 성능 저하될 수 있음
  2. InheritanceType.SINGLE_TABLE
     - 단일 테이블 전략
     - 테이블 하나에 모든 것을 통합하므로 구분 컬럼 필수
     - 조인이 필요 없으므로 일반적으로 조회 성능이 빠름
     - 자식 엔티티가 매핑한 컬럼 모두 null 허용해야 함
 <br/><br/>



### @DiscriminatorColumn
- 부모 클래스에 구분 컬럼을 지정 (자식 테이블 구분)
<br/><br/>

### @DiscriminatorValue
- 엔티티를 저장할 때 구분 컬럼에 입력할 값 지정
- DiscriminatorColumn에 표시될 값

<br/>

```java
@Entity
@Inheritance(strategy = InheritanceType.SINGLE_TABLE) // 싱글 테이블로 관리
@DiscriminatorColumn(name = "PUB_TYPE") // 엔티티
public abstract class Publication {

    @Id @GeneratedValue(strategy = GenerationType.IDENTITY)
    @Column(name = "PUBLICATION_ID", updatable = false, nullable = false)
    protected Long id;

    @Column
    protected String title;

    @Column(name = "version")
    private int version;

    //getter setter
}
```

<br/>

```java
@Entity
@DiscriminatorValue("BOOK") // PUB_TYPE 컬럼에 저장될 타입명
public class Book extends Publication {

    @Column
    private int pages; // nullable
    
    //getter setter
}
```

결과  
<img width = "80%" src = "https://user-images.githubusercontent.com/42172353/199654917-9919c8df-acb6-42b7-b5b6-71f0f14b0c7d.png"/>
<h5>[출처 - 자바 ORM 표준 JPA 프로그래밍 (김영한 저)]</h5>
<br/><br/><br/>


 



## 4. 조인 전략
- 각각을 모두 테이블로 만들고 조회할 때 조인 사용
- abstract superclass도 DB 테이블로 매핑(모든 공유 속성을 포함하는 테이블)

<br/>

```java
@Entity
@Inheritance(strategy = InheritanceType.JOINED) // 조인
@DiscriminatorColumn(name = "PUB_TYPE") // 엔티티
public abstract class Publication {

    @Id @GeneratedValue(strategy = GenerationType.IDENTITY)
    @Column(name = "PUBLICATION_ID", updatable = false, nullable = false)
    protected Long id;

    @Column
    protected String title;

    @Column(name = "version")
    private int version;

    //getter setter
}
```

<br/>

```java
@Entity
@DiscriminatorValue("BOOK") // PUB_TYPE 컬럼에 저장될 타입명
public class Book extends Publication {

    @Column
    private int pages; // nullable
    
    //getter setter
}
```

결과  
<img width = "80%" src = "https://user-images.githubusercontent.com/42172353/199661217-622a8dfa-c371-4fbc-9a7c-a7de33c4faa5.png"/>
<h5>[출처 - 자바 ORM 표준 JPA 프로그래밍 (김영한 저)]</h5>
<br/><br/><br/><br/><br/>
<br/><br/><br/><br/><br/>



---
<br/>

## Cascade
- 영속성 전이 기능
- 특정 엔티티를 영속 상태로 만들 때 연관된 엔티티도 함께 영속 상태로 만듦
- To-Many 연관관계에서 CascadeType.REMOVE 문제
  - 의도한 것 이상으로 데이터 삭제 됨

<br/>

```java
@Entity
public class Person {

    @Id @GeneratedValue(strategy = GenerationType.IDENTITY)
    private int id;
    private String name;

    @OneToMany(mappedBy = "person", cascade = CascadeType.ALL)
    private List<Address> addresses = new ArrayList<Address>();

    // getter setter
}
```

<br/>

```java
private static void logic(EntityManager em) {
    // ~
    em.persist(person); // 저장
    // ~  
    em.remove(em.find(Person.class, 1)); // 삭제
}
```

<br/><br/><br/><br/><br/>
<h5>

[참고 강의 : 스프링과 JPA를 이용한 웹개발](http://www.kocw.net/home/cview.do?cid=5e6aec4a9ae2dd45)   
[참고 서적 : 김영한 - 자바 ORM 표준 JPA 프로그래밍](https://product.kyobobook.co.kr/detail/S000000935744)

</h5>