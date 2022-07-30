# 서블릿(Servlet) 클래스 작성하기

* javax.servlet.http.HttpServlet 클래스를 상속받는다.
* public으로 선언해준다.
* public으로 선언해야 하는 이유는 웹 컨테이너가 서블릿 객체를 만들 때 이 접근 권한이 필요하기 때문이다.

~~~java
package first;

import javax.servlet.http.HttpServlet;

public class FirstServelt extends HttpServlet{

	
}

~~~java


### 서블릿 안에 doFet , doPost 선언
* doGet(HttpServletRequest req, HttpServletResponse resp) //doGet 의 매개변수
* doPost(HttpServletRequest req, HttpServletResponse resp) //doPost 의 매개변수
* javax.servlet.ServletException과 java.io.IOException을 던질 수 있도록 선언

~~~java
package first;

import java.io.IOException;

import javax.servlet.ServletException;
import javax.servlet.http.HttpServlet;
import javax.servlet.http.HttpServletRequest;
import javax.servlet.http.HttpServletResponse;

public class FirstServelt extends HttpServlet{

	@Override
	protected void doGet(HttpServletRequest req, HttpServletResponse resp) throws ServletException, IOException {
		
	}

	
}

~~~

~~~jsp
response.setContentType("text/html; charset=utf-8");
<!--response.setContentType("출력된 문서타입; charset=인코딩");
서블릿에서 한글은 출력되지 않고 깨져 보이게 되는데 이때 인코딩(charset=utf-8)과 출력될 문서 타입(text/html)을 추가하면 한글이 제대로 출력이 됩니다.-->
~~~

* HttpServletResponse resp 이 매개변수의 객체에서 getWriter 메서드를 호출

~~~java
PrintWriter out = resp.getWriter();
~~~

~~~java
package first;

import java.io.IOException;
import java.io.PrintWriter;

import javax.servlet.ServletException;
import javax.servlet.http.HttpServlet;
import javax.servlet.http.HttpServletRequest;
import javax.servlet.http.HttpServletResponse;

public class FirstServelt extends HttpServlet{

	@Override
	protected void doGet(HttpServletRequest req, HttpServletResponse resp) throws ServletException, IOException {
		int  total= 0;
		for(int i = i; i <= 10; i++) {
				total += i;
		}
		
		resp.setContentType("text/html; charset=utf-8");
		PrintWriter out = resp.getWriter();
		out.println("<html>");
		out.println("<body>");
		out.printf("1부터 10까지 숫자의 곱은 = %d", total);
		out.println("</body>");
		out.println("</html>");
	}

	
}
~~~

### web.xml 파일에 서블릿(Servlet) 등록하기
* web.xml 파일은 웹 애플리케이션 디렉토리마다 딱 하나씩만 만들수 있습니다. 경로는 webapps/WEB-INF/web.xml 입니다.
* web.xml 파일을 새로 만들 때는 루트 요소인 을 만드는 일부터 시작하는 것이 좋습니다.

~~~jsp
<web-app>
	<servlet>
		<!-- 서블릿 클래스의 이름이 들어갈 부분 -->
	</servlet>
	<servlet-mapping>
		<!-- 서블릿 클래스를 호출할 때 사용할 URL이 들어갈 부분 -->
	</servlet-mapping>
</web-app>  
		<servlet>
			<servlet-name>FirstServelt-servlet</servlet-name>
			<servlet-class>first.FirstServlet</servlet-class>
		</servlet>

		<servlet-mapping>
			<servlet-name>FirstServelt-servlet</servlet-name>
			<url-pattern>/FirstServelt</url-pattern>
		</servlet-mapping>
	</web-app>
  
  ~~~
> 모든 web.xml파일 안에는 반드시 써 넣어야 하는 두 가지 정보가 포함되어 있는데, 하나는 web.xml 파일의 작성에 사용된 문법의 식발자이고, 다른 하나는 그 문법의 버전입니다.

~~~jsp
<?xml version="1.0" encoding="UTF-8"?>
<web-app version="4.0"
	xmlns="http://xmlns.jcp.org/xml/ns/javaee"
	xmlns:xsi="http://www.w3.org/2001/XMLSchema-instance"
	xsi:schemaLocation="http://xmlns.jcp.org/xml/ns/javaee                      
	 http://xmlns.jcp.org/xml/ns/javaee/web-app_4_0.xsd">


		<servlet>
			<servlet-name>FirstServelt-servlet</servlet-name>
			<servlet-class>first.FirstServlet</servlet-class>
		</servlet>

		<servlet-mapping>
			<servlet-name>FirstServelt-servlet</servlet-name>
			<url-pattern>/FirstServelt</url-pattern>
		</servlet-mapping>
	</web-app>
  ~~~
  
> Servers 탭의 tomcat v9.0 Server at localhost를 클릭 하고 오른쪽 끝에 위치한 실행 버튼을 클릭하면 웹서버가 실행

~~~html
실행경과 1부터 10까지 숫자의 합은 = 55
~~~


~~~java
package first;

import java.io.IOException;
import java.io.PrintWriter;

import javax.servlet.ServletException;
import javax.servlet.annotation.WebServlet;
import javax.servlet.http.HttpServlet;
import javax.servlet.http.HttpServletRequest;
import javax.servlet.http.HttpServletResponse;

@WebServlet("/alswo")
public class FirstServlet extends HttpServlet{

	@Override
	protected void doGet(HttpServletRequest req, HttpServletResponse resp) throws ServletException, IOException {
		int  total= 0;
		for(int i = 0; i <= 10; i++) 
				total += i;
		
		
		resp.setContentType("text/html; charset=utf-8");
		PrintWriter out = resp.getWriter();
		out.println("<html>");
		out.println("<body>");
		out.printf("1부터 10까지 숫자의 곱은 = %d", total);
		out.println("</body>");
		out.println("</html>");
	}
}
~~~

### 웹 브라우저로부터 데이터 입력 받기
* XML 문법의 기초

~~~jsp
<?xml version="1.0" encoding="UTF-8"?>
<!--첫째. XML 선언은 XML 문서 작성에 사용된 XML 규격서의 버전과 XML 문서를 저장하는 데 사용된 문자 코드의 인코딩 방식을 표시하는 역할을 합니다.-->
~~~

* 요소를 통해 입력된 데이터는 doGet, doPost 메서드의 첫 번째 매개변수인 HttpServletRequest 객체에서 getParameter 메서드를 호출해서 가져오기

~~~java
package myservlet;

import java.io.IOException;
import java.io.PrintWriter;

import javax.servlet.ServletException;
import javax.servlet.annotation.WebServlet;
import javax.servlet.http.HttpServlet;
import javax.servlet.http.HttpServletRequest;
import javax.servlet.http.HttpServletResponse;


public class AdderServlet extends HttpServlet{

	@Override
	protected void doGet(HttpServletRequest req, HttpServletResponse resp) throws ServletException, IOException {
		String str1 = req.getParameter("num1");
		String str2 = req.getParameter("num2");
		int num1 = Integer.parseInt(str1);
		int num2 = Integer.parseInt(str2);
		int sum = num1 + num2;
		resp.setContentType("text/html;  charset=utf-8");
		PrintWriter out= resp.getWriter();
		out.println("<html>");
		out.println("<head><title>덧셈 프로그램 - 결과화면</title></head>");
		out.println("<body>");
		out.printf("%d + %d = %d", num1, num2, sum);
		out.println("</body>");
		out.println("</html>");
	}
	

}  
~~~

~~~jsp
 <?xml version="1.0" encoding="UTF-8"?>
<web-app version="4.0"
   xmlns="http://xmlns.jcp.org/xml/ns/javaee"
   xmlns:xsi="http://www.w3.org/2001/XMLSchema-instance"
   xsi:schemaLocation="http://xmlns.jcp.org/xml/ns/javaee                      
    http://xmlns.jcp.org/xml/ns/javaee/web-app_4_0.xsd">
<servlet>
   <servlet-name>AdderServlet-Servlet</servlet-name>
   <servlet-class>myservlet.AdderServlet</servlet-class>
   </servlet>
   <servlet-mapping>
   <servlet-name>AdderServlet-servlet</servlet-name>
   <url-pattern>/Adderservlet</url-pattern>
   </servlet-mapping>
   </web-app>
 ~~~
