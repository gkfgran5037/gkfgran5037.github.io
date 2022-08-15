## @ServletComponentScan
서블릿 자동 등록 어노테이션

<br/>

```
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

## @WebServlet
서블릿 어노테이션  
* name: 서블릿 이름  
* urlPatterns: URL 매핑

<br/>

```
@WebServlet(name = "helloServlet", urlPatterns = "/hello")
public class HelloServlet extends HttpServlet {
	@Override
	protected void service(HttpServletRequest req, HttpServletResponse resp) throws ServletException, IOException {
	}
}
```
<br/>
<br/>
<br/>
<br/>
<br/>

## @Getter @Setter
getter, setter 어노테이션

<br/>

```
import lombok.Getter;
import lombok.Setter;

@Getter @Setter
public class HelloData {
	private String username;
	private int age;
}
```
