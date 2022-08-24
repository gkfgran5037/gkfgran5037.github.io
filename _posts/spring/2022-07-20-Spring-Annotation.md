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

## @RestController
Http Method인 Get의 요청을 받을 수 있는 API를 만들어줌
- 구버전 ) @RequestMapping(method = RequestMethod.GET)

```java
@RestController
public class HelloController {
    @GetMapping("/hello")
    public String hello() {
        return "hello";
    }
}
```

<br/>
<br/>
<br/>
<br/>
<br/>

---
<br/>

## @Getter @Setter
getter, setter 어노테이션

<br/>

```java
import lombok.Getter;
import lombok.Setter;

@Getter @Setter
public class HelloData {
	private String username;
	private int age;
}
```
