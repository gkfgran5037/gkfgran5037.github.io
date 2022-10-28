---
title:  "SpringBoot Annotation"
categories:
  - Spring Boot
---

## 목차

- [Annotation](#annotation)
  - [@ServletComponentScan](#servletcomponentscan)
  - [@SpringBootApplication](#springbootapplication)
- [반환 형식](#반환-형식)
  - [@RestController](#restcontroller)
- [API](#api)
  - [@WebServlet](#webservlet)
  - [@GetMapping](#getmapping)
  - [@RequestParam](#requestparam)
- [롬복 Lombok](#롬복-lombok)
  - [@Getter @Setter](#getter-setter)
  - [@RequiredArgsConstructor](#requiredargsconstructor)
  - [@NoArgsConstructor](#noargsconstructor)
  - [Builder](#builder)
- [JPA](#jpa)
  - [@Entity](#entity)
  - [@Table](#table)
  - [@OrderBy](#orderby)
  - [@Id](#id)
  - [@GeneratedValue](#generatedvalue)
    - [GenerationType.IDENTITY](#generationtypeidentity)
    - [GenerationType.SEQUENCE](#generationtypesequence)
    - [@SequenceGenerator](#sequencegenerator)
    - [Table](#table-1)
    - [@TableGenerator](#tablegenerator)
    - [AUTO](#auto)
  - [@Column](#column)
  - [@Enumerated](#enumerated)
  - [@Temporal](#temporal)
  - [@Lob](#lob)
  - [@Transient](#transient)
  - [@Access](#access)
  - [@JoinColumn](#joincolumn)
  - [@OneToMany](#onetomany)
  - [@ManyToOne](#manytoone)
- [Spring Boot Test](#spring-boot-test)
  - [@RunWith](#runwith)
  - [@WebMvcTest](#webmvctest)
  - [@Autiwired](#autiwired)
  - [MockMvc](#mockmvc)
  - [param](#param)
  - [jsonPath](#jsonpath)
  - [is](#is)
- [JUnit, assertj](#junit-assertj)
  - [assertj](#assertj)
  - [@After](#after)




<br/><br/><br/><br/><br/>





# Annotation
- 어노테이션 순서 : 주요 어노테이션을 클래스에 가깝게 둠

<br/>
<br/>

## @ServletComponentScan
서블릿 자동 등록 어노테이션

<br/>
<br/>

## @SpringBootApplication
- 스프링 부트의 자동 설정, 스프링 Bean 읽기와 생성을 모두 자동으로 설정
  - @SpringBootApplication 이 있는 위치부터 설정을 읽어감
  - 프로젝트의 최상단에 위치해야 함
  - 해당 파일은 프로젝트의 메인 클래스
- SpringApplication.run : 내장 WAS 실행
  
<br/>

```java
@ServletComponentScan
@SpringBootApplication
public class ServletApplication {

	public static void main(String[] args) {
		SpringApplication.run(ServletApplication.class, args);
	}

}
```

<br/><br/><br/><br/><br/>









---
# 반환 형식
<br/>

## @RestController
컨트롤러를 JSON을 반환하는 컨트롤러로 만들어 줌

```java
@RestController
public class HelloController {
	
}
```

<br/><br/><br/><br/><br/>
<br/><br/><br/><br/><br/>
<br/><br/><br/><br/><br/>









---
# API
<br/>

## @WebServlet
서블릿 어노테이션  
* name: 서블릿 이름  
* urlPatterns: URL 매핑

<br/>

```java
@WebServlet(name = "helloServlet", urlPatterns = "/hello")
public class HelloServlet extends HttpServlet {
	@Override
	protected void service(HttpServletRequest req, HttpServletResponse resp) throws ServletException, IOException {
	}
}
```
<br/>
<br/>

## @GetMapping
Http Method인 Get의 요청을 받을 수 있는 API를 만들어줌
- 구버전 ) @RequestMapping(method = RequestMethod.GET)

```java
public class HelloController {

    @GetMapping("/hello")
    public String hello() {
        return "hello";
    }
}
```

<br/>
<br/>

## @RequestParam
외부에서 API로 넘긴 파라미터를 가져옴

```java
@GetMapping("/hello/dto")
public HelloResponseDto helloDto(@RequestParam("name") String name, @RequestParam("amount") int amount) {
	return new HelloResponseDto(name, amount);
}
```

<br/><br/><br/><br/><br/>
<br/><br/><br/><br/><br/>
<br/><br/><br/><br/><br/>


















---
# 롬복 Lombok
<br/>

## @Getter @Setter
- getter : 선언한 모든 필드의 get 메소드를 생성
- setter : 선언한 모든 필드의 set 메소드를 생성
  - JPA Entity 클래스에서는 Setter 메소드를 만들지 않고 목적과 의도를 나타낼 수 있는 메소드를 추가해야 함

<br/>
<br/>

## @RequiredArgsConstructor
선언된 모든 final 필드가 포함된 생성자를 생성

<br/>

```java
import lombok.Getter;
import lombok.Setter;

@Getter // 선언한 모든 필드의 get 메소드를 생성
@Setter // 선언한 모든 필드의 set 메소드를 생성
@RequiredArgsConstructor // 선언된 모든 final 필드가 포함된 생성자를 생성
public class HelloData {
	private String username;
	private int age;
}
```

<br/>
<br/>

## @NoArgsConstructor
기본 생성자 자동 추가
  - public Posts() {} 와 같은 효과

## Builder
- 해당 클래스의 빌더 패턴 클래스를 생성
- 해당 클래스 상단에 선언 시 생성자에 포함된 필드만 빌더에 포함

```java
@Getter
@NoArgsConstructor // 기본 생성자 자동 추가
public class Posts {
  private String title;
  private String content;
  
  @Builder // 해당 클래스의 빌더 패턴 클래스를 생성
  public Posts(String title, String content) {
      this.title = title;
      this.content = content;
  }
}
```

<br/><br/><br/><br/><br/>
<br/><br/><br/><br/><br/>
<br/><br/><br/><br/><br/>




















---
# JPA
<br/>

| 어노테이션           | 설명                                                        |
|-----------------|-----------------------------------------------------------|
| [@Entity](#entity)         | 테이블과 링크될 클래스임을 나타냄                                        |
| [@Table](#table)          | 엔티티와 매핑되는 테이블을 지정                                         |
| [@OrderBy](#orderBy)        | 정렬 수행                                                     |
| [@Transient](#transient)      | 테이블의 컬럼에 매핑되지는 않지만 비즈니스 로직 수행 등의 이유로 유지해야 하는 상태(값)가 있을 경우 |
| [@Column](#column)         | 테이블의 칼럼을 나타내며 굳이 선언하지 않더라도 해당 클래스의 필드는 모두 컬럼이 됨           |
| [@Id](#id)             | 해당 테이블의 PK 필드                                             |
| [@GeneratedValue](#generatedValue) | PK의 생성 규칙                                                 |

<br/><br/><br/>



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


### GenerationType.IDENTITY
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




### GenerationType.SEQUENCE
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




### Table
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



### AUTO
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
<br/><br/><br/><br/><br/>
<br/><br/><br/><br/><br/>




















---
# Spring Boot Test
<br/>

## @RunWith
- 테스트를 진행할 때 JUnit에 내장된 실행자 외에 다른 실행자를 실행시킴
- 스프링 부트 테스트와 JUnit 사이에 연결자 역할
	- SpringRunner : 스프링 실행자

<br/>
<br/>

## @WebMvcTest
Web(Spring MVC)에 집중할 수 있는 어노테이션
  - controllers : @Controller, @ControllerAdvice 등 사용 가능

<br/>
<br/>

## @Autiwired
스프링이 관리하는 Bean을 주입받음

<br/>
<br/>

## MockMvc
HTTP GET, POST 등 웹 API를 테스트
  - perform : uri로 API 요청
  - andExcept : perform의 결과 검증

<br/>

```java
import org.junit.Test;
import org.junit.runner.RunWith;
import org.springframework.test.context.junit4.SpringRunner;
import org.springframework.boot.test.autoconfigure.web.servlet.WebMvcTest;
import org.springframework.beans.factory.annotation.Autowired;
import org.springframework.test.web.servlet.MockMvc;
import static org.springframework.test.web.servlet.request.*;


@RunWith(SpringRunner.class) // 스프링 부트 테스트와 JUnit 사이에 연결자 역할
@WebMvcTest(controllers = HelloController.class) // Web에 집중할 수 있는 어노테이션
public class HelloControllerTest {
	@Autowired // 빈 주입 받음
    private MockMvc mvc; // HTTP GET, POST 등 웹 API를 테스트

	@Test
    public void hello_rtn() throws Exception {
		String hello = "hello";

		mvc.perform(get("/hello"))	// uri로 HTTP GET 요청
			.andExpect(status().isOk())	// HTTP Header의 Status 검증
			.andExpect(content().string(hello)); // 리턴 값 검증
	}
}
```

<br/><br/><br/><br/><br/>







## param
- API 테스트할 때 사용될 요청 파라미터를 설정
  - get(url).param("파라미터명", 변수)
- 값은 String만 허용
  - 숫자/날짜 등의 데이터 등록 시 문자열로 변경 필요
  - .param("파라미터명", String.valueOf(숫자형 변수))

<br/>
<br/>

## jsonPath
JSON 응답값을 필드별로 검증할 수 있는 메소드
  - jsonPath("$.필드명", 검증메소드)

<br/>
<br/>

## is
동일 여부 확인
  - org.hamcrest.Matchers.is

<br/>

```java
@Test
public void rtnHelloDto() throws Exception {
    String name = "hello";
    int amount = 1000;

    mvc.perform(get("/hello/dto")
                    .param("name", name)
                    .param("amount", String.valueOf(amount))
                ).andExpect(status().isOk())
                .andExpect(jsonPath("$.name", is(name)))
                .andExpect(jsonPath("$.amount", is(amount)));
}
```
<br/><br/><br/><br/><br/>
<br/><br/><br/><br/><br/>
<br/><br/><br/><br/><br/>


















---
# JUnit, assertj
<br/>

## assertj
- CoreMathers와 달리 추가적으로 라이브러리가 필요하지 않음
  - JUnit의 seertThat을 쓰게 되면 is()와 같이 CoreMatchers 라이브러리가 필요함
- 자동완성이 좀 더 확실하게 지원됨
  - IDE에서는 CoreMathcers와 같은 Matcher 라이브러리의 자동완성 지원이 약함
- 메소드
  - assertThat() : 검증하고 싶은 대상을 메소드 인자로 받음
  - isEqualTo() : 동일 여부 확인

<br/>

```java
@Test
public void lobok_fn_test() {
	// given
	String name = "test";
	int amount = 1000;

	//when
	HelloResponseDto dto = new HelloResponseDto(name, amount);

	//then
	assertThat(dto.getName()).isEqualTo(name);
	assertThat(dto.getAmount()).isEqualTo(amount);
}
```

<br/>
<br/>
<br/>
<br/>
<br/>

## @After
- JUnit에서 단위 테스트가 끝날 떄마다 수행되는 메소드를 지정
- 보통은 배포 전 전체 테스트를 수행할 떄 테스트간 데이터 침범을 막기 위해 사용
- 여러 테스트가 동시에 수행되면 테스트용 데이터베이스인 H2에 데이터가 그대로 남아 있어 다음 테스트 실행 시 테스트가 실패할 수 있음
<br/>

```java
@After // 단위 테스트가 끝날 떄마다 수행되는 메소드 지정
public void cleanup() {
    postsRepository.deleteAll();
}
```


<br/><br/><br/><br/><br/>
<h5>

[참고 서적 : 스프링 부트와 AWS로 혼자 구현하는 웹 서비스](http://www.yes24.com/Product/Goods/83849117)   
[참고 강의 : 김영한 - 스프링 MVC](https://www.inflearn.com/course/%EC%8A%A4%ED%94%84%EB%A7%81-mvc-1/dashboard)   
[참고 강의 : 스프링과 JPA를 이용한 웹개발](http://www.kocw.net/home/cview.do?cid=5e6aec4a9ae2dd45)

</h5>