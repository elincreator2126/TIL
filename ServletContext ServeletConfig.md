# 서블릿 확장 API 사용하기 (8장)



## ServletContext 와 ServeletConfig

ServletContext는 컨텍스트(웹 애플리케이션) 당 생성, ServletConfig는 각 서블릿에 대해 생성 



### ServletContext 클래스

**정의 (설명)**

톰캣 컨테이너 실행 시, **각 컨텍스트(웹 어플리케이션)마다 한 개의 ServletContext 객체를 생성** 

**ServletContext 객체**는 웹 애플리케이션이 실행되면서 애플리케이션 전체의 공통 자원이나 정보를 **미리 바인딩해서 서블릿들이 공유하여 사용** 



**요약** 

- 전역 객체
- **톰캣 컨테이너 실행 시 하나의 컨텍스트가 생성, 컨테이너 종료 시 소멸.**

(톰캣 컨테이너에 따라 좌지우지되는 클래스)

- 어플리케이션 전체 공동 자원



**ServletContext 클래스의 특징** 

- javax.servlet.ServletContext 로 정의 
- 서블릿과 컨텍스트 간의 연동위해 사용 
- **컨텍스트(웹 애플리케이션)마다 하나의 ServletContext가 생성** 
- 서블릿끼리 **데이터를 공유하는데 사용** 



**ServletContext 클래스의 제공 기능** 

서블릿에서 파일 접근 기능 -=> 모든 웹 브라우저에서 공유하면서 접근 가능 

 / 데이터 바인딩 / 로그파일 기능 / 컨텍스트에서 제공하는 설정 정보 제공 



### **ServletContext에서 제공하는 메서드** 

**---------------바인딩 --------------**

getAttribute(String name) :  주어진 name을 이용해 바인딩된 value를 가져와. name 미 존재 시 null 반환 

getAttributeNames( ) : 바인딩된 속성들의 name을 반환

getContext(String uripath) : 지정한 uripath에 해당하는 객체를 반환 



**--------- 매개변수 초기화 --------**

getInitParameter(String name) : name에 해당되는 매개변수의 초기화 값을 반환. 해당되는 매개변수가 미 존재 시, null을 반환. 

getInitParameterNames( ) : 컨텍스트의 초기화 관련 매개변수들의 이름들을 String 객체가 저장된 Enumeration 타입으로 반환. 매개변수가 미존재 시, null 반환 

 

 **URI (식별자; 구분자)= URL (위치)+ URN (이름)****

URI는 Uniform Resource Identifier ; 자원의 식별자 ; 리소스의 식별은 리소스의 위치를 표시하거나 unique한 이름으로 가능 

URL은 Uniform Resource Locator + URN은 Uniform Resource Name

***url의 구성:** protocol(necessary) + domain name(necessary) + port + path to the file + parameters + anchor



***parameter(매개변수):** 범용(汎用) 프로그램의 개개의 작업에 적용할 경우에 필요한 수치 정보. 함수의 특정한 성질을 나타내는 변수.

변수의 특별한 한 종류

인풋으로 제공되는 여러 데이터들을 **전달인자(argument)**

매개변수는 **변수(variable)**로, 전달인자는 **값(value)**

매개변수는 함수의 정의 부분에서 볼 수 있으며, 전달인자는 함수를 호출하는 부분에서 볼 수 있다. `f(x) = x*x`와 같은 함수 정의 부분에서 변수 'x'가 매개변수가 되며, `f(2)`와 같은 함수 호출 부분에서 값 '2' 가 함수의 전달인자가 된다. (전달인자는 함수가 호출될때 제공되는 값)



### web.xml 

파라미터 설정

- context-param 태그안에 컨텍스트 생성 시, **초기화할 파라미터**를 설정

  ```java
  <?xml version="1.0" encoding="UTF-8"?>
  <web-app xmlns:xsi="http://www.w3.org/2001/XMLSchema-instance" xmlns="http://java.sun.com/xml/ns/javaee" xsi:schemaLocation="http://java.sun.com/xml/ns/javaee http://java.sun.com/xml/ns/javaee/web-app_3_0.xsd" id="WebApp_ID" version="3.0">
  
    <context-param>
        <param-name>menu_member</param-name>
        <param-value>회원등록 회원조회 회원수정</param-value> //초기값 설정 
    </context-param>
  
    <context-param>
        <param-name>menu_order</param-name>
        <param-value>주문조회 주문등록 주문수정 주문취소</param-value>
    </context-param>
  
    <context-param>
        <param-name>menu_goods</param-name>
        <param-value>상품조회 상품등록 상품수정 상품삭제</param-value>
    </context-param>    
  
    <display-name>pro08</display-name>
    <welcome-file-list>
      <welcome-file>index.html</welcome-file>
      <welcome-file>index.htm</welcome-file>
      <welcome-file>index.jsp</welcome-file>
      <welcome-file>default.html</welcome-file>
      <welcome-file>default.htm</welcome-file>
      <welcome-file>default.jsp</welcome-file>
    </welcome-file-list>
  </web-app>
  ```

  

  ### ContextParamServlet

- web.xml 에서 서블릿 컨텍스트의 초기 파라미터를 설정하였으므로, 바로 컨텍스트에 접근하여 값을 가져올 수 있다.

``` java
package sec05.ex02;

import java.io.IOException;
import java.io.PrintWriter;

import javax.servlet.ServletContext;
import javax.servlet.ServletException;
import javax.servlet.annotation.WebServlet;
import javax.servlet.http.HttpServlet;
import javax.servlet.http.HttpServletRequest;
import javax.servlet.http.HttpServletResponse;


@WebServlet("/initMenu")
public class ContextParamServlet extends HttpServlet{

    @Override
    protected void doGet(HttpServletRequest req, HttpServletResponse resp) throws ServletException, IOException {
        req.setCharacterEncoding("utf-8");
        resp.setContentType("text/html;charset=utf-8");
        PrintWriter out = resp.getWriter();
        ServletContext context = getServletContext(); // web.xml 에 <context-param> 에 있는 변수를 가져옴.

        String menu_member = context.getInitParameter("menu_member");
        String menu_order = context.getInitParameter("menu_order");
        String menu_goods = context.getInitParameter("menu_goods");

        out.print("<html><body>");
        out.print("<table border=1 cellspacing=0><tr>메뉴 이름</tr>");
        out.print("<tr><td>" + menu_member + "</td></tr>");
        out.print("<tr><td>" + menu_order + "</td></tr>");
        out.print("<tr><td>" + menu_goods + "</td></tr>");
        out.print("</tr></table></body></html>");            

    }
}
```

![img](md-images/99424B445E0A8B951A)

### 1) ServletContext 의 바인딩 기능 



**SetServletContext** (/cest)

getServletContext() 메서드를 이용해 ServletContext 객체에 접근 -> ArrayList에 데이터 저장 -> ServletContext 객체에 바인딩 (setAttribute( ) 메서드 이용) 

컨텍스트 객체에 List 세팅 (설정)

```java
package sec05.ex01;

import java.io.IOException;
import java.io.PrintWriter;
import java.util.ArrayList;
import java.util.List;

import javax.servlet.ServletContext;
import javax.servlet.ServletException;
import javax.servlet.annotation.WebServlet;
import javax.servlet.http.HttpServlet;
import javax.servlet.http.HttpServletRequest;
import javax.servlet.http.HttpServletResponse;

@WebServlet("/cset")
public class SetServletContext extends HttpServlet{
    @Override
    protected void doGet(HttpServletRequest req, HttpServletResponse resp) throws ServletException, IOException {
        resp.setContentType("text/html;charset=utf-8");
        PrintWriter out = resp.getWriter();        
        ServletContext context = getServletContext(); // 서블릿 컨테이너 생성.getServletContext()메서드를 이용해 ServletContext객체에 접근

        List member = new ArrayList(); //리스트 객체 생성 
        member.add("이순신"); //리스트 객체에 데이터 저장 
        member.add(30);
        context.setAttribute("member", member);
        out.print("<html><body>");
        out.print("이순신과 30 설정");
        out.print("</body></html>");

    }
}
```

![img](md-images/9996773E5E0A884115)

=> ServletContext 객체에 데이터를 바인딩 



**GetServletContext** (/cget)

(위와 동일) getServletContext() 메서드를 이용해 ServletContext 객체에 접근 -> getAttribute( ) 메서드로 바인딩한 ArrayList를 가져와 -> 회원정보 출력 

컨텍스트(웹 애플리케이션)에 저장된 데이터 불러오기 

```java
package sec05.ex01;

import java.io.IOException;
import java.io.PrintWriter;
import java.util.ArrayList;
import java.util.List;

import javax.servlet.ServletContext;
import javax.servlet.ServletException;
import javax.servlet.annotation.WebServlet;
import javax.servlet.http.HttpServlet;
import javax.servlet.http.HttpServletRequest;
import javax.servlet.http.HttpServletResponse;

@WebServlet("/cget")
public class GetServletContext extends HttpServlet {
    @Override
    protected void doGet(HttpServletRequest req, HttpServletResponse resp) throws ServletException, IOException {
        resp.setContentType("text/html;charset=utf-8");
        PrintWriter out = resp.getWriter();        
        ServletContext context = getServletContext(); // 서블릿 컨테이너 생성. ServletContext 객체를 가져온다. 

        List member = (ArrayList) context.getAttribute("member"); //바인딩된 회원정보를 가져와
        String name = (String)member.get(0); // 이순신
        int age = (Integer)member.get(1); // 30

        out.print("<html><body>"); //브라우저에 출력 
        out.print(name + "<br>");
        out.print(age + "<br>");
        out.print("</body></html>");

    }
}
```

![img](md-images/9931B94E5E0A885018)

=> 바인딩된 데이터를 브라우저에 표시 

최종 의의 : ServletContext에 바인딩된 데이터는 **모든** 서블릿들(사용자)이 접근 가능. 웹 애플리케이션에서 모든 사용자가 **공통으로 사용하는 데이터는 ServletContext에 바인딩**해놓고 사용하면 편리. 



## 2) ServletContext 의 매개변수 기능 

**web.xml 파일**

- 초기화 설정 (메뉴항목 설정)

<context-param> 태그 안에 

<param-name> <param-value> 태그로 초기값 설정 

```java
<context-param>
      <param-name>menu_member</param-name>
      <param-value>회원등록 회원조회 회원수정</param-value> //초기값 설정 
  </context-param>

  <context-param>
      <param-name>menu_order</param-name>
      <param-value>주문조회 주문등록 주문수정 주문취소</param-value>
  </context-param>

  <context-param>
      <param-name>menu_goods</param-name>
      <param-value>상품조회 상품등록 상품수정 상품삭제</param-value>
  </context-param>    
    </web-app>
```



**ContextParamServlet 클래스** 

(위와 동일) getServletContext() 메서드를 이용해 ServletContext 객체에 접근 -> getInitParameter( ) 메서드의 인자로 각각 메뉴의 이름을 전달 -> 메뉴 항목들 가져와 -> 브라우저에 출력 

```java
@WebServlet(/initMenu)
:
ServletContext context = getServletContext(); 
String menu_member = context.getInitParameter("menu_member"); //
String menu_member = context.getInitParameter("menu_order");
String menu_member = context.getInitParameter("menu_goods");
:

```

=> 모든 브라우저에서 같은 메뉴를 출력. 메뉴는 **ServletContext 객체를 통해 접근**하므로 모든 웹 브라우저에서 공유하면서 접근 가능. 



## 3) ServletContext 의 파일 입출력 기능 

:

```java
@WebServlet(/cfile)
:
ServletContext context = getServletContext();
InputStream is = context.getResourceAsStream("WEB-INF/bin/init.txt");
:
```

