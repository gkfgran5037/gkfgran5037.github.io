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

<br/>
<br/>
<br/>
<br/>
<br/>










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

<br/>
<br/>
<br/>
<br/>
<br/>










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

<br/>
<br/>
<br/>
<br/>
<br/>










---
# 롬복 Lombok
<br/>

## @Getter @Setter
- getter : 선언한 모든 필드의 get 메소드를 생성
- setter : 선언한 모든 필드의 set 메소드를 생성

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
<br/>
<br/>
<br/>











---
# Spring Boot Test
<br/>

## @RunWith
- 테스트를 진행할 때 JUnit에 내장된 실행자 외에 다른 실행자를 실행시킴
- 스프링 부트 테스트와 JUnut 사이에 연결자 역할
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


@RunWith(SpringRunner.class) // 스프링 부트 테스트와 JUnut 사이에 연결자 역할
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

<br/>
<br/>
<br/>
<br/>
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