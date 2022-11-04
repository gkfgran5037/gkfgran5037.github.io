---
title:  "JPA Annotation"
categories:
  - JPA
---

## 목차

- [목차](#목차)
- [@Entity](#entity)
- [@Table](#table)
- [@OrderBy](#orderby)
- [@Id](#id)
- [@GeneratedValue](#generatedvalue)
  - [1. GenerationType.IDENTITY](#1-generationtypeidentity)
  - [2. GenerationType.SEQUENCE](#2-generationtypesequence)
  - [@SequenceGenerator](#sequencegenerator)
  - [3. Table](#3-table)
  - [@TableGenerator](#tablegenerator)
  - [4. AUTO](#4-auto)
- [@Column](#column)
- [@Enumerated](#enumerated)
- [@Temporal](#temporal)
- [@Lob](#lob)
- [@Transient](#transient)
- [@Access](#access)
- [@JoinColumn](#joincolumn)
- [@OneToMany](#onetomany)
- [@ManyToOne](#manytoone)
- [@OneToOne](#onetoone)
- [@ManyToMany](#manytomany)
  - [@JoinTable](#jointable)
- [식별 관계](#식별-관계)
  - [복합 기본 키](#복합-기본-키)
  - [@IdClass](#idclass)
  - [@EmbeddedId](#embeddedid)
- [비식별 관계](#비식별-관계)
- [@MappedSuperclass](#mappedsuperclass)
  - [@Inheritance](#inheritance)
  - [@DiscriminatorColumn](#discriminatorcolumn)
  - [@DiscriminatorValue](#discriminatorvalue)





<br/><br/><br/><br/><br/>





---

## @Entity
객체와 테이블 매핑
테이블과 링크될 클래스임을 나타냄
  - 기본값으로 클래스의 카멜케이스 이름을 언더스코어 네이밍(_)으로 테이블 이름을 매칭함
    - SalesManager.java -> sales_mansger table
  - 기본 생성자 필수
    - JPA가 엔티티 객체를 생성할 때, 기본 생성자를 사용
  - final 클레스, enum, interface, inner 클래스에는 사용할 수 없음
  - 저장할 필드에 final 사용하면 안 됨

<br/> 

| 어노테이션  | 속성   | 기능               | 기본값    |
|--------|------|------------------|--------|
| @Entity | name | JPA에서 사용할 엔티티 이름 | 클래스 이름 |  
<br/><br/>



## @Table
- 엔티티와 매핑되는 테이블을 지정
- 엔티티명과 테이블명이 다를 경우 이름을 지정 필요
  - 생략 시 매핑한 엔티티 이름을 테이블 이름으로 사용

<br/>

| 어노테이션  | 속성                          | 기능                                                      | 기본값    |
|--------|-----------------------------|---------------------------------------------------------|--------|
| @Table | name                        | 매핑할 테이블 이름                                              | 엔티티 이름 |
|        | catalog                     | catalog 기능이 있는 데이터베이스에서 catalog 매핑                      |        |
|        | schema                      | schema 기능이 있는 데이터베이스에서 schema 매핑                        |        |
|        | uniqueConstraints<br/>(DDL) | DDL 생성 시에 유니크 제약조건을 만듦.<br/>2개 이상의 복합 유니크 제약조건도 만들 수 있음 |
<br/>


```java
@Entity
@Table(name = "MEMBER", uniqueConstraints = {@UniqueConstraint(
        name = "NAME_AGE_UNIQUE",
        columnNames = {"NAME", "AGE"}
)})
public class Member {
  @Id
  @Column(name = "USER_ID")
  private String id;

  @Column(name = "NAME", nullable = false, length = 10)
  private String userName;

  private int age;

  // ~
}
```
<br/><br/>


## @OrderBy
- 정렬 수행

<br/><br/>














---

## @Id
해당 테이블의 PK 필드
- 기본 키 매핑 방법
  1. 직접 할당 - em.persist() 호출 전 직접 식별자 값 할당
  2. 자동 생성

<br/>

| 어노테이션           | 속성       | 전략       | 기능                                                         | 방언        | SQL 기능         |
|-----------------|----------|----------|------------------------------------------------------------|-----------|----------------|
| @GeneratedValue | strategy | IDENTITY | 데이터베이스에 엔티티를 저장해서 식별자 값을 획득한 후 영속성 컨텍스트에 저장                                        | MySQL 등   | AUTO_INCREMENT |
|                 |          | SEQUENCE | 데이터베이스 시퀀스에서 식별자 값 획득 후 영속성 컨텍스트에 저장                                  | Oracle 등  | SEQUENCE       |
|                 |          | TABLE    | 데이터베이스 시퀀스 생성용 테이블에서 식별자 값을 획득 후 영속성 컨텍스트에 저장                                                | 모든 데이터베이스 |                |
|                 |          | AUTO     | 선택한 데이터베이스 방언에 따라 IDENTITY, SEQUENCE, TABLE 전략 중 하나를 자동 선택 |                                                                                             |

<br/><br/>



## @GeneratedValue
PK의 생성 규칙
- 식별자를 직접 할당하지 않고 자동으로 생성
- 생략 시 Auto
- @GeneratedValue(strategy = GenerationType.방식)
- 값을 생성하는 방식 
  - Identity
  - Sequence
  - Table
  - Auto
  
<br/><br/>


### 1. GenerationType.IDENTITY
- 기본 키 생성을 데이터베이스에 위임
- 데이터베이스에 엔티티를 저장해서 식별자 값을 획득한 후 영속성 컨텍스트에 저장
- Long, int 등 O, String X
- 엔티티를 데이터베이스에 저장해야 식별자를 구할 수 있음
- e.persist()를 호출하는 즉시 INSERT SQL이 데이터베이스에 전달됨
- 쓰기 지연이 동작하지 않음
  
<br/>

```java
@Id
@GeneratedValue(strategy = GenerationType.IDENTITY)
@Column(name = "USER_ID")
private Long id;
```
<br/><br/>




### 2. GenerationType.SEQUENCE
### @SequenceGenerator
- 데이터베이스 시퀀스에서 식별자 값 획득 후 영속성 컨텍스트에 저장
- em.persist()를 호출할 때 먼저 데이터베이스 시퀀스를 사용해서 식별자 조회
- 조회한 식별자를 엔티티에 할당한 후 commit 시 데이터베이스에 전달됨

<br/>

```java
@Entity
// 시퀀스 생성기 등록
@SequenceGenerator(
        name = "BOARD_SEQ_GENERATOR", // 시퀀스 생성기명
        sequenceName = "BOARD_SEQ",   // 매핑할 데이터베이스 시퀀스 이름
        initialValue = 1,  // 시작값
        allocationSize = 1 // 증가값
)
public class Member {
    @Id
    @GeneratedValue(strategy = GenerationType.SEQUENCE, generator = "BOARD_SEQ_GENERATOR")
    @Column(name = "USER_ID")
    private Long id;

    // ~
}
```
<br/><br/>




### 3. Table
### @TableGenerator
- 키 생성 테이블 사용
- 데이터베이스 시퀀스 생성용 테이블에서 식별자 값을 획득 후 영속성 컨텍스트에 저장
- 키 생성 전용 테이블을 하나 만들고 여기에 이름과 값으로 사용할 컬럼을 만들어 데이터베이스 시퀀스를 흉내내는 전략

```java
@Entity
// 식별자 생성기 등록
@TableGenerator(
    name = "BOARD_SEQ_GENERATOR", // 식별자 생성기 이름
    table = "MY_SEQUENCES",       // 키생성 테이블명
    pkColumnValue = "BOARD_SEQ",  // 키로 사용할 값 이름
    allocationSize = 1            // 키원스 한 번 호출에 증가하는 수
)
public class Board {
    @Id
    @GeneratedValue(strategy = GenerationType.TABLE, generator = "BOARD_SEQ_GENERATOR")
    private Long id;

    // ~ 
}
```
<br/><br/>



### 4. AUTO
- 선택한 데이터베이스 방언에 따라 IDENTITY, SEQUENCE, TABLE 전략 중 하나를 자동 선택
- 데이터베이스를 변경해도 코드를 수정할 필요가 없음
- SEQUENCE, TABLEdl 전략이 선택될 시 시퀀스나 키 생성용 테이블을 미리 만들어 두어야 함

<br/>

```java
public class Board {
    @Id
    @GeneratedValue(strategy = GenerationType.AUTO)
    private Long id;

    // ~ 
}
```
<br/><br/>















---

## @Column
테이블의 칼럼을 나타내며 굳이 선언하지 않더라도 해당 클래스의 필드는 모두 컬럼이 됨
  - 기본값 외에 추가로 변경이 필요한 옵션이 있으면 사용
    - name : 컬럼명
    - length : 길이 변경 (문자열의 경우 varchar(255)가 기본값)
    - nullable : null 허용 여부
    - columnDefinition : 컬럼 타입 변경

<br/>

```java
@Getter
@NoArgsConstructor // 기본 생성자 자동 추가
@Entity // 테이블과 링크될 클래스
public class Posts {
    @Id // PK
    @GeneratedValue(strategy =  GenerationType.IDENTITY)
    private Long id;

    @Column(length = 500, nullable = false)
    private String title;

    @Column(columnDefinition = "TEXT", nullable = false)
    private String content;

    private String author;

    @Builder // 해당 클래스의 빌더 패턴 클래스를 생성
    public Posts(String title, String content, String author) {
        this.title = title;
        this.content = content;
        this.author = author;
    }
}
```  
<br/><br/>


## @Enumerated
- 자바의 enum 타입을 매핑할 때 사용
- value 
  - EnumType.ORDNIAL : enum 순서를 데이터베이스에 저장 (기본값)
  - EnumType.STRING : enum 이름을 데이터베이스에 저장

<br/>

```java
@Enumerated(EnumType.STRING)
private RoleType roleType;
```

```java
public enum RoleType {
    ADMIN, USER
}
```

```java
member.setRoleType(RoleType.ADMIN);
```
<br/><br/>




## @Temporal
- 날짜 타입을 매핑할 때 사용

<br/>

```java
@Temporal(TemporalType.TIMESTAMP)
private Date createdDate;
```
<br/><br/>




## @Lob
- 데이터베이스 BLOB, CLOB 타입과 매핑
  - CLOB : 문자 (String, char[], java.sql.CLOB)
  - BLOB : 문자 외 (byte[], java.sql.BLOB)

<br/>

```java
@Lob
private String description;
```
<br/><br/>




## @Transient
- 테이블의 컬럼에 매핑되지는 않지만 비즈니스 로직 수행 등의 이유로 유지해야 하는 상태(값)가 있을 경우

<br/>

```java
@Transient
private int ranking;
```

<br/><br/>




## @Access
- JPA가 엔티티 데이터에 접근하는 방식 지정
  - 필드 접근 AccessType.FIELD : 필드에 직접 접근. private 이어도 접근 가능
  - 프로퍼티 접근 AccessType.PROPERTY : 접근자 사용.
<br/><br/>






## @JoinColumn
- FK로 사용되는 컬럼
- @JoinColumn(name = "조인할 컬럼")
  
<br/><br/><br/><br/><br/>

              













---
## @OneToMany 
- 나의 Entity(일) <- 반대편 Entity(다)
- @OneToMany(mappedBy = "외래키 필드명")
- 매핑된 필드 명시
- 일대다 단반향으로만 연관관계를 매핑하면 안됨
- 속성
  1. mappedBy = "필드명"
  2. cascade = CascadeType.ALL
     - 영속성 전이 기능
     - 특정 엔티티를 영속 상태로 만들 때 연관된 엔티티도 함께 영속 상태로 만듦
  3. orphanRemoval = true
     - Many-To-One 관계에서 child는 부모가 존재하지 않을 경우 부모 엔티티를 삭제하면 자식 엔티팉도 자동 삭제
     - ex) null로 변경 시 변경 감지에서 객체 관점에서 연관된 엔티티가 더 이상 없다고 생각하고 기존의 연관 Entity 삭제


<br/><br/>


## @ManyToOne
- 나의 Entity(다) -> 반대편 Entity(일)
- @ManyToOne(fetch = FetchType.타입)
  1. FetchType.LAZY : 연관된 엔티티는 가져오지 않음
  2. FetchType.EAGER
     - 명시하지 않아도 연관된 엔티티도 함께 조회
     - 반대편 Entity에 현재 Entity 조인하여 조회
     - JPQL을 사용할 때 N+1문제가 발생 (쿼리 1개를 날리면서 추가적으로 N개의 쿼리가 나감)

<br/><br/>
<img width="575" alt="02_다대일" src="https://user-images.githubusercontent.com/42172353/197381091-a3ecedf5-4079-417b-a560-eed5c0cb3b9f.png">
<h5>[출처 - 스프링과 JPA를 이용한 웹개발] http://www.kocw.net/home/cview.do?cid=5e6aec4a9ae2dd45 </h5>
<br/><br/>


```java
@Entity
public class Item {
    @Id
    @GeneratedValue(strategy = GenerationType.IDENTITY)
    @Column(name = "ITEM_ID")
    private Long id;

    private String name;
    private int price;

    // FK (다) - 다대일
    @ManyToOne(fetch = FetchType.LAZY)
    @JoinColumn(name = "PURCHASE_ORDER_ID")
    PurchaseOrder order;

    // getter, setter
}
```

<br/>

```java
@Entity
public class PurchaseOrder {
    @Id
    @GeneratedValue(strategy = GenerationType.IDENTITY)
    @Column(name = "PURCHASE_ORDER_ID")
    private long id;
    private String userName;

    // (일) - 일대다
    @OneToMany(mappedBy = "order")
    private List<Item> items = new ArrayList<Item>();

    // getter, setter
}
```

<br/>

```java
Item item1 = new Item();
item1.setName("치킨");
Item item2 = new Item();
item2.setName("치즈볼");

PurchaseOrder order = new PurchaseOrder();
order.setUserName("kim");
order.getItems().add(item1);
order.getItems().add(item2);

item1.setOrder(order);
item2.setOrder(order);

em.persist(order); // order(일) 먼저 할 경우 insert
em.persist(item1); // item(다) 먼저 할 경우 insert 후 update 처리됨
em.persist(item2);
```

<h5>[출처 - 스프링과 JPA를 이용한 웹개발] http://www.kocw.net/home/cview.do?cid=5e6aec4a9ae2dd45 </h5>
<br/><br/><br/><br/><br/>













---
<br/>

## @OneToOne
- Parent-Child 관계로 표현
- FK 위치 : Parent, Child 중 선택 지정 가능
     
<img width="80%" src="https://user-images.githubusercontent.com/42172353/197381092-3dc8deb3-f005-40ea-a083-ef323085dfeb.png">
<h5>[출처 - 스프링과 JPA를 이용한 웹개발] http://www.kocw.net/home/cview.do?cid=5e6aec4a9ae2dd45 </h5>
<br/><br/><br/>


```java
@Entity
public class Manuscript {

    @Id
    @GeneratedValue(strategy = GenerationType.IDENTITY)
    @Column(name = "MANUSCRIPT_ID")
    private Long id;


    // FK (일 parent)
    @OneToOne(mappedBy = "manuscript") // oneToOne 필드명
    private Book book;

    // getter setter
}
```

<br/>

```java
@Entity
public class Book {

    @Id
    @GeneratedValue(strategy = GenerationType.IDENTITY)
    @Column(name = "BOOK_ID")
    private Long id;

    // FK 관리 주체 필드 = 연관 관계 주인 (다 child)
    @OneToOne
    @JoinColumn(name = "MANUSCRIPT_ID") // JoinColumn 컬럼명
    private Manuscript manuscript;

    // getter setter
}
```

<h5>[출처 - 스프링과 JPA를 이용한 웹개발] http://www.kocw.net/home/cview.do?cid=5e6aec4a9ae2dd45 </h5>
<br/><br/><br/><br/><br/>














---

<br/>

## @ManyToMany
- 다대다 연관 관계
- 관계형 데이터베이스는 정규화된 테이블 2개로 다대다 관계를 표현할 수 없음
- 보통 다대다(@ManyToMany)가 일대다, 다대일 관계로 풀어내는 연결 테이블 사용

<br/>
<img width="80%" src="https://user-images.githubusercontent.com/42172353/197381090-fdedd1e4-6d8e-49d7-aa0d-8ce36521c271.png">
<h5>[출처 - 스프링과 JPA를 이용한 웹개발] http://www.kocw.net/home/cview.do?cid=5e6aec4a9ae2dd45 </h5>
<br/><br/><br/>

### @JoinTable
- @ManyToMany 연결테이블 지정
- joinColumns : 현재 방향과 매핑
- inverseJoinColumns : 반대방향과 매핑

<br/><br/>

```java
@Entity
@Table(name = "MEMBER")
public class Member {

    @Id
    @Column(name = "MEMBER_ID")
    private String id;

    // fk 관리 주체
    /* @JoinTable(name = 연결테이블
            joinColumns = @JoinColumn(name = 현재 방향과 매핑),
            inverseJoinColumns = @JoinColumn(name = 반대방향과 매핑)) */
    @ManyToMany
    @JoinTable(name = "MEMBER_PRODUCT",
            joinColumns = @JoinColumn(name = "MEMBER_ID"),
            inverseJoinColumns = @JoinColumn(name = "PRODUCT_ID"))
    private List<Product> products = new ArrayList<Product>();

    // getter setter 

    // 양방향 관리
    public void addProduct(Product product) {
        this.products.add(product);
        product.getMembers().add(this);
    }
}
```

<br/>

```java
@Entity
public class Product {

    @Id
    @Column(name = "PRODUCT_ID")
    private String id;

    private String name;

    @ManyToMany(mappedBy = "products") // 역방향 추가
    private List<Member> members;
    
    // getter setter 
}
```

<h5>[출처 - 자바 ORM 표준 JPA 프로그래밍 (김영한 저)]</h5>
<br/><br/><br/><br/><br/>









## 식별 관계
- 부모 테이블의 기본 키를 받아서 자신의 기본 키 + 외래 키로 사용하는 것
- 식별자 클래스로 기본 키를 묶어서 복합 기본 키로 사용

<br/><br/>

### 복합 기본 키
- 두개 이상의 키를 기본 키로 지정한 것
- 별도의 식별자 클래스 필요 
- 일대다, 다대일 관계로 풀어내는 연결 테이블 사용

<br/><br/>

### @IdClass
- 식별자 클래스
- 복합 키는 별도의 식별자 클래스 필요
- Serializable 구현 필요
- 기본 생성자 있어야 함
- 식별자 클래스는 public 이어야 함

<br/><br/>

### @EmbeddedId


<br/>

```java
@Entity
@IdClass(MemberProductId.class)  
public class MemberProduct { // 식별 관계

    @Id // 복합키
    @ManyToOne
    @JoinColumn(name = "MEMBER_ID")
    private MemberT member;

    @Id // 복합키
    @ManyToOne
    @JoinColumn(name = "PRODUCT_ID")
    private Product product;

    private int orderAmount;

    // getter setter
}
```

<br/>

```java
// 식별자 클래스
public class MemberProductId implements Serializable {

    private String member;
    private String product;

    // equals hashCode
    // getter setter
}
```

<h5>[출처 - 자바 ORM 표준 JPA 프로그래밍 (김영한 저)]</h5>
<br/><br/><br/><br/><br/>









## 비식별 관계
- 받아온 식별자는 외래 키로만 사용, 새로운 식별자 추가
- 데이터베이스에서 자동으로 생성해주는 대리 키를 Long 값으로 사용하는 것
- 장점
  - 간편
  - 영구히 쓸 수 있음
  - 비즈니스에 의존하지 않음
  - ORM 매핑 시에 복합 키를 만들지 않아도 되므로 간단히 매핑 가능
- 분류
  1. 선택적 비식별 관계
      - 외래 키에 null 허용
      - 외부 조인(Outer Join) 사용
  2. 필수적 비식별 관계
      - 외래 키에 null 허용하지 않음
      - 내부 조인(Inner Join) 사용


```java
@Entity
@Table(name = "MEMBER")
public class Member {

    @Id
    @Column(name = "MEMBER_ID")
    private String id;

    @Column(name = "USER_NAME")
    private String userName;

    @OneToMany(mappedBy = "member") // FK (일 parent)
    private List<Order> orders = new ArrayList<Order>();

    // getter setter
}
```

<br/>

```java
@Entity
public class Product {

    @Id
    @Column(name = "PRODUCT_ID")
    private String id;

    private String name;

    // getter setter
}
```

<br/>

```java
@Entity
public class Order { // 비식별 관계

    @Id @GeneratedValue
    @Column(name = "ORDER_ID")
    private Long id;

    @ManyToOne
    @JoinColumn(name = "MEMBER_ID")
    private Member member;

    @ManyToOne
    @JoinColumn(name = "PRODUCT_ID")
    private Product product;

    private int orderAmount;

    // getter setter
}
```

<h5>[출처 - 자바 ORM 표준 JPA 프로그래밍 (김영한 저)]</h5>
<br/><br/><br/><br/><br/>














---
<br/>

## @MappedSuperclass
- 각각의 구현클래스를 개별 테이블에 매핑하는 전략
- 여러 엔티티에서 공통으로 사용하는 매핑 정보만 상속받고 싶을 떄 사용
- 다수의 엔티티가 공통 속성을 공유할 수 있음
- mapped superclass가 지정된 클래스는 엔티티가 아니므로 테이블에 매핑되는 것은 아님
- 주로 createdBy, createdOn과 같이 테이블에서 공통적으로 사용되는 반복되는 필드를 각
테이블에 끼워 넣을 때 사용

<br/>

```java
@MappedSuperclass
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
public class Book2 extends Publication {

    @Column
    private int pages;
}
```
<br/><br/><br/><br/><br/>






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



<h5>

[참고 강의 : 스프링과 JPA를 이용한 웹개발](http://www.kocw.net/home/cview.do?cid=5e6aec4a9ae2dd45)   
[참고 서적 : 김영한 - 자바 ORM 표준 JPA 프로그래밍](https://product.kyobobook.co.kr/detail/S000000935744)

</h5>
<br/><br/><br/><br/><br/>
<br/><br/><br/><br/><br/>
<br/><br/><br/><br/><br/>



