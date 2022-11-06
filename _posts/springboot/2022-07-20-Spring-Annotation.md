---
title:  "SpringBoot Annotation"
categories:
  - springBoot
---

## 목차

- [Annotation](#annotation)
  - [@ServletComponentScan](#servletcomponentscan)
  - [@SpringBootApplication](#springbootapplication)
- [Web Application](#web-application)
  - [@Repository](#repository)
  - [@PersistenceContext](#persistencecontext)
  - [@PersistenceUnit](#persistenceunit)
  - [@Service](#service)
  - [@Transactional](#transactional)
  - [@Autowired](#autowired)
  - [@RequiredArgsConstructor](#requiredargsconstructor)
  - [@Controller](#controller)
  - [@RestController](#restcontroller)
  - [@JsonManagedReference](#jsonmanagedreference)
  - [@JsonBackReference](#jsonbackreference)
- [API](#api)
  - [@WebServlet](#webservlet)
  - [@GetMapping](#getmapping)
  - [@RequestParam](#requestparam)




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
<br/><br/><br/><br/><br/>
<br/><br/><br/><br/><br/>



























---
<br/>

# Web Application
<br/>

## @Repository
- 엔티티 메니저를 사용해서 엔티티를 저장하고 조회
- @Repository이 붙어 있으면 component-scan에 의해 스프링 빈으로 자동 등록됨
- JPA 전용 예외가 발생하면 스프링이 추상화한 예외로 변환해줌
  - ex) NoResultException -> EmptyResultDataAccessException
  - 서비스 계층은 JPA에 의존적인 예외를 처리하지 않아도 됨

<br/>

```java
@Repository
public class MemberRepository {

    @PersistenceContext
    EntityManager em;
    // ~
}
```
<br/><br/>



## @PersistenceContext
- 엔티티 매니저 주입
- 컨테이너가 관리하는 엔티티 매니저를 주입
- 컨테이너가 제공하는 트랜잭션 기능과 연계해서 컨테이너의 다양한 기능들을 사용 가능
  - 순수 자바 환경 ) 엔티티 메니저 팩토리에서 엔티티 매니저 직접 생성해서 사용
  - 스프링, J2EE 환경 ) 컨테이너가 엔티티 매니저를 관리하고 제공
<br/><br/>




## @PersistenceUnit
- 엔티티 매니저 팩토리를 주입 받음

<br/>

```java
@PersistenceUnit
EntityManagerFactory emf; 
```


<br/><br/><br/><br/><br/>
<br/><br/><br/><br/><br/>










---
<br/>

## @Service
- 비즈니스 로직이 있고 트랜잭션을 시작함
- 데이터 접근 계층인 리포지토리를 호출
- @Service이 붙어 있으면 component-scan에 의해 스프링 빈으로 등록됨

<br/>

```java
@Service
@Transactional
public class MemberService {
    @Autowired
    MemberRepository memberRepository;
    // ~
}
```

<br/><br/>



## @Transactional
- 외부에서 이 클래스의 메소드를 호출할 때 트랜잭션을 시작하고 메소드를 종료할 때 트랜잭션을 커밋
- 예외 발생 시 트랜잭션 롤백
  - RuntimeException과 그 자식들인 Unchecked 예외만 롤백함
  - 만약 체크 예외가 발생해도 롤백하고 싶다면 rollbackOn = Exception.class처럼 롤백할 예외를 지정해야 함
- 스프링프레임워크는 @Transactional이 붙어 있는 클래스나 메소드에 트랜잭션을 적용함
- 테스트에서 사용 시 각각의 테스트를 실행할 떄마다 트랜잭션을 시작하고 테스트가 끝나면 트랜잭션을 강제로 롤백함
<br/><br/>



## @Autowired
- @Autowired이 붙어 있으면 스프링 컨테이너가 적절한 스프링 빈을 주입해줌
- 지원 : 스프링 프레임워크 (타 프레임워크 호환 불가 )
- 빈 검색 방식 : 타입
- 특정 빈을 찾지 못하면 예외를 던짐 (단, required 속성으로 처리 가능)
- Autowired의 위치에 따른 주입 방법 구분 (생성자, 수정, 필드 주입)


## @RequiredArgsConstructor


## @Controller

## @RestController
컨트롤러를 JSON을 반환하는 컨트롤러로 만들어 줌

```java
@RestController
public class HelloController {
	
}
```

// ObjectMapper


## @JsonManagedReference

## @JsonBackReference

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




















<br/><br/><br/><br/><br/>
<h5>

[참고 서적 : 스프링 부트와 AWS로 혼자 구현하는 웹 서비스](http://www.yes24.com/Product/Goods/83849117)   
[참고 강의 : 김영한 - 스프링 MVC](https://www.inflearn.com/course/%EC%8A%A4%ED%94%84%EB%A7%81-mvc-1/dashboard)   
[참고 강의 : 스프링과 JPA를 이용한 웹개발](http://www.kocw.net/home/cview.do?cid=5e6aec4a9ae2dd45)

</h5>