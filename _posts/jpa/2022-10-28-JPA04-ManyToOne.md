---
title:  "JPA 연관관계"
categories:
  - JPA
---

# 연관관계
- 테이블은 외래키 하나로 테이블 간 연관관계를 설정
- 연관관계의 ownership 결정 필요
  - ownership : 외래키를 관리하는 주체
  - RDB에서 외래키는 '다'쪽에 있음

<br/><br/>



## @JoinColumn
- 외래 키를 매핑할 때 사용

<br/><br/>



### 연관관계의 주인
-두 객체 연관관계 중 하나를 정해서 테이블의 외래키를 관리
- 데이터베이스 연관관계와 매핑되고 외래 키를 관리 가능(등록, 수정, 삭제)
  - 주인 아닌 쪽은 읽기만 가능
- 주인은 mappedBy 속성을 사용하지 않음
- 주인은 테이블에 외래키가 있는 곳으로 정함 many

<br/><br/><br/><br/><br/>

---
<br/>


## Many-TO-One (One-To-Many)

### @OneToMany 
- Parent Entity(일) <- Child Entity(다)
- @OneToMany(mappedBy = "자식 외래키 필드명")
- 매핑된 필드 명시
- 일대다 단반향으로만 연관관계를 매핑하면 안됨 = 양방향으로 매핑 필수

<br/>

### @ManyToOne
  - Child Entity(다) -> Parent Entity(일)
  - Child Entity가 Parent Entity의 PK를 가지는 구조 구현
  - @ManyToOne(fetch = FetchType.타입)
    - FetchType.LAZY : 연관된 엔티티는 가져오지 않음
    - FetchType.EAGER
      - 명시하지 않아도 연관된 엔티티도 함께 조회
      - Parent Entity에 Child Entity를 아우터 조인하여 조회
      - JPQL을 사용할 때 N+1문제가 발생 (쿼리 1개를 날리면서 추가적으로 N개의 쿼리가 나감)
  - @JoinColumn : FK로 사용되는 컬럼

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

    // FK 관리 주체 필드 = 연관 관계 주인 (다 child)
    @ManyToOne(fetch = FetchType.LAZY)
    @JoinColumn(name = "PURCHASE_ORDER_ID") // JoinColumn 컬럼명
    PurchaseOrder order;

    // ~ getter setter

    // 양방향 관계 설정
    public void setOrder(Team team) {
        // 기존 팀과 관계를 제거
        if(this.order != null) {
          this.order.getItems().remove(this);
        }
        this.order = order; // 연관관계 설정(연관관계의 주인)
        team.getItems().add(this); // 주인이 아닌 곳만 연관관계 설정 - null값으로 저장됨
    }
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

    // FK (일 parent)
    @OneToMany(mappedBy = "order") // mappedBy = 필드명
    private List<Item> items = new ArrayList<Item>();
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

## One-To-One
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

## Many-To-Many
- 관계형 데이터베이스는 정규화된 테이블 2개로 다대다 관계를 표현할 수 없음
- 보통 일대다, 다대일 관계로 풀어내는 연결 테이블 사용

<br/>

<img width="80%" src="https://user-images.githubusercontent.com/42172353/197381090-fdedd1e4-6d8e-49d7-aa0d-8ce36521c271.png">
<h5>[출처 - 스프링과 JPA를 이용한 웹개발] http://www.kocw.net/home/cview.do?cid=5e6aec4a9ae2dd45 </h5>
<br/><br/><br/>





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

<br/><br/><br/>




## 비식별 관계
- 받아온 식별자는 외래 키로만 사용, 새로운 식별자 추가
- 데이터베이스에서 자동으로 생성해주는 대리 키를 Long 값으로 사용하는 것
- 장점
  - 간편
  - 영구히 쓸 수 있음
  - 비즈니스에 의존하지 않음
  - ORM 매핑 시에 복합 키를 만들지 않아도 되므로 간단히 매핑 가능

<br/>

```java
@Entity
@Table(name = "MEMBER_T")
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

<br/>

```java
public static void testSave(EntityManager em) {
    Product productA = new Product();
    productA.setId("productA");
    productA.setName("상품A");
    em.persist(productA);

    Member member1 = new Member("member1", "회원1");
    member1.getProducts().add(productA);
    em.persist(member1);
}

public static void find(EntityManager em) {
    Member member = em.find(Member.class, "member1");
    List<Product> products = member.getProducts();
    for (Product product : products) {
        System.out.println("product.getName() = " + product.getName());
    }
}

public static void findInverse(EntityManager em) {
    Product product = em.find(Product.class, "productA");
    List<Member> members = product.getMembers();
    for (Member member : members) {
        System.out.println("member.getUserName() = " + member.getUserName());
    }
}
```
<h5>[출처 - 자바 ORM 표준 JPA 프로그래밍 (김영한 저)]</h5>
<br/><br/><br/><br/><br/>
<h5>









#### ManyToMany -> OneToMany, ManyToOne 식별 관계 예시

```java
@Entity
@Table(name = "MEMBER")
public class Member {

    @Id
    @Column(name = "MEMBER_ID")
    private String id;


    @OneToMany(mappedBy = "member") // FK (일 parent)
    private List<MemberProduct> memberProducts;

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

```java
public static void testSave(EntityManager em) {
  Member member1 = new Member("member1", "회원1");
  em.persist(member1);

  Product productA = new Product();
  productA.setId("productA");
  productA.setName("상품1");
  em.persist(productA);

  MemberProduct memberProduct = new MemberProduct();
  memberProduct.setMember(member1);
  memberProduct.setProduct(productA);
  memberProduct.setOrderAmount(2);
  em.persist(memberProduct);
}

public static void find(EntityManager em) {
  MemberProductId memberProductId = new MemberProductId();
  memberProductId.setMember("member1");
  memberProductId.setProduct("productA");

  MemberProduct memberProduct = em.find(MemberProduct.class, memberProductId);

  Member member = memberProduct.getMember();
  Product product = memberProduct.getProduct();

  System.out.println("member = " + member.getUserName());
  System.out.println("product = " + product.getName());
  System.out.println("orderAmount = " + memberProduct.getOrderAmount());
}
```

<h5>[출처 - 자바 ORM 표준 JPA 프로그래밍 (김영한 저)]</h5>
<br/><br/><br/><br/><br/>
<h5>






#### ManyToMany -> OneToMany, ManyToOne 비식별 관계 예시

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

<br/>

```java
public static void testSave(EntityManager em) {
    Member member1 = new Member("member1", "회원1");
    em.persist(member1);

    Product productA = new Product();
    productA.setId("productA");
    productA.setName("상품1");
    em.persist(productA);

    Order order = new Order();
    order.setMember(member1);
    order.setProduct(productA);
    order.setOrderAmount(2);
    em.persist(order);
}

public static void find(EntityManager em) {
    Long orderId = 1L;
    Order order = em.find(Order.class, orderId);

    Member member = order.getMember();
    Product product = order.getProduct();

    System.out.println("product.getName() = " + product.getName());
    System.out.println("member.getUserName() = " + member.getUserName());
    System.out.println("orderAmount = " + order.getOrderAmount());
}
```

<br/><br/><br/><br/><br/>
<h5>

[참고 강의 : 스프링과 JPA를 이용한 웹개발](http://www.kocw.net/home/cview.do?cid=5e6aec4a9ae2dd45)   
[참고 서적 : 김영한 - 자바 ORM 표준 JPA 프로그래밍](https://product.kyobobook.co.kr/detail/S000000935744)

</h5>