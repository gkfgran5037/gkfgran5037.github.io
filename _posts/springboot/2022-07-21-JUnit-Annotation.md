---
title:  "JUnit Annotation"
categories:
  - springBoot
---

## 목차

- [목차](#목차)
- [@RunWith](#runwith)
- [@ContextConfiguration](#contextconfiguration)
- [@Transactional](#transactional)
- [@Autiwired](#autiwired)
- [MockMvc](#mockmvc)
- [param](#param)
- [jsonPath](#jsonpath)
- [is](#is)
- [assertj](#assertj)
- [@After](#after)

<br/><br/><br/><br/><br/>






---

<br/>

## @RunWith
- 테스트를 진행할 때 JUnit에 내장된 실행자 외에 다른 실행자를 실행시킴
- 스프링 부트 테스트와 JUnit 사이에 연결자 역할
	- SpringRunner : 스프링 실행자
  - SpringJUnit4ClassRunner.class 지정 시 JUnit으로 작성한 테스트 케이스를 스프링 프레임워크와 통합

<br/>

```java
@RunWith(SpringJUnit4ClassRunner.class)
@ContextConfiguration(locations = "classpath:appConfig.xml")
@Transactional
class MemberServiceTest {
}
```

```java
@RunWith(SpringRunner.class)
@WebMvcTest(controllers = HelloController.class)
class HelloControllerTest {
}
```
<br/><br/>



## @ContextConfiguration
- 테스트 케이스를 실행할 때 사용할 스프링 설정 정보 지정

<br/><br/>



## @Transactional
- 외부에서 이 클래스의 메소드를 호출할 때 트랜잭션을 시작하고 메소드를 종료할 때 트랜잭션을 커밋
- 예외 발생 시 트랜잭션 롤백
  - RuntimeException과 그 자식들인 Unchecked 예외만 롤백함
  - 만약 체크 예외가 발생해도 롤백하고 싶다면 rollbackOn = Exception.class처럼 롤백할 예외를 지정해야 함
- 스프링프레임워크는 @Transactional이 붙어 있는 클래스나 메소드에 트랜잭션을 적용함
- 테스트에서 사용 시 각각의 테스트를 실행할 떄마다 트랜잭션을 시작하고 테스트가 끝나면 트랜잭션을 강제로 롤백함

<br/><br/>





## @Autiwired
스프링이 관리하는 Bean을 주입받음

<br/><br/>



## MockMvc
HTTP GET, POST 등 웹 API를 테스트
  - perform : uri로 API 요청
  - andExcept : perform의 결과 검증

<br/>

```java
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




