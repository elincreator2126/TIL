# 서블릿 확장 API 사용하기 (8장)

**API ; Application Programming Interface** 

애플리케이션 소프트웨어를 구축하고 통합하기 위한 정의 및 프로토콜 세트

API는 손님(프로그램)이 주문할 수 있게 메뉴(명령 목록)를 정리하고, 주문(명령)을 받으면 요리사(응용프로그램)와 상호작용하여 요청된 메뉴(명령에 대한 값)를 전달

 **API**는 **프로그램**들이 서로 **상호작용**하는 것을 도와주는 **매개체**

![API, 쉽게 알아보기](md-images/API-%EC%89%BD%EA%B2%8C-%EC%95%8C%EC%95%84%EB%B3%B4%EA%B8%B0.png)

### **API의 역할**

**1. API는 서버와 데이터베이스에 대한 출입구 역할**

API는 이를 방지하기 위해 여러분이 가진 서버와 데이터베이스에 대한 출입구 역할을 하며, 허용된 사람들에게만 접근성을 부여

**2. API는 애플리케이션과 기기가 원활하게 통신할 수 있도록** 

애플리케이션; 스마트폰 어플이나 프로그램. API는 애플리케이션과 기기가 데이터를 원활히 주고받을 수 있도록 돕는 역할

**3. API는 모든 접속을 표준화**

API는 모든 접속을 표준화하기 때문에 기계/ 운영체제 등과 상관없이 누구나 동일한 액세스를 얻을 수 있다. 쉽게 말해, API는 범용 플러그처럼 작동



## API 이점

- Private API를 이용: 개발자들이 애플리케이션 코드를 작성하는 방법을 표준화함으로써, 간소화되고 빠른 프로세스 처리를 가능하게 합니다. 소프트 웨어를 통합하고자 할 때는 개발자들 간의 협업을 용이하게.
- public API와 partner API 를 사용:  기업은 타사 데이터를 활용하여 브랜드 인지도를 높일 수 있습니다. 뿐만 아니라 고객 데이터베이스를 확장하여 전환율 증대 가능



![img](md-images/API-page-graphic.png)

## 포워딩과 바인딩의 개념 

서블릿 프로그래밍 개발 시, 사용하는 기능 : 바인딩, 포워딩, 에너테이션 

##### 바인딩 후 포워딩 코드) 

```java
		MemberDAO dao = new MemberDAO();        
        List memberList = dao.listMembers(); 

        request.setAttribute("membersList", memberList); //데이터를 전달받아와 
        RequestDispatcher dispatch = request.getRequestDispatcher("viewMembers"); //" " 는 서블릿 이름 
        dispatch.forward(request, response); //데이터를 전달해 

```

- 서블릿 요청이 들어오면 DAO 객체를 생성

- 회원정보를 조회해 setAttribute 의 "membersList" 객체에 담아 

- dispatch를 활용해 viewMembers로 서블릿을 요청한다. 

  (ViewServlet의 @WebServlet("/viewMembers") )

  

  #### 포워딩

  ##### 기능 설명

  하나의 서블릿  => 다른 서블릿 혹은 jsp 로 연동 ; 요청과 데이터를 전달할 때 사용 

  원래 요청이 들어온 url을 다른 곳으로 이동시켜주는 기능 

*웹 어플리케이션: 여러 기능을 합쳐 하나의 프로그램을 실행. 

=> 서블릿끼리 혹은 서블릿이나 jsp를 연동해서 작업해야 하는 경우 생길 때, 이용하는 기능

- dispatcher 방법

서블릿이 직접 요청하는 방식: 서버에서 바로 데이터를 넘겨줄 때 사용. 

​												(서블릿에서 클라이언트를 거치지 않고 )

RequestDispatcher 클래스의 forward 메서드를 이용 

전달 후에도 웹 브라우저 주소창의 URL 미변경

(클라이언트 측에서는 포워드 진행되었는지 여부를 알 수 없음 ) 



#### 바인딩

서블릿 => 다른 서블릿 또는 jsp로 대량의 데이터를 공유하거나 전달하고 싶을 때 사용 

: 데이터 묶음 (하나 이상의 데이터들을 전달할 때, 하나로 묶어서 전달하는 기법)

웹 프로그램 실행 시, 데이터를 서블릿 관련 객체에 저장하는 방법 

- HttpServletRequest, HttpSession, ServletContext 객체에서 사용

- 바인딩 관련 메서드 : 

  - **setAttribute(String name, Object obj)** : **데이터를 각 객체에 바인딩**; 전달하는 쪽 (설정) 
  - **getAttribute(String name)** : 각 객체에 **바인딩된 데이터**를 name으로 가져옴 ; 전달받는 쪽 (조회)

  서버에서만 데이터 바인딩이 가능

  단순한 요청은 브라우저를 통해서 가능

  그러나, DB 정보를 조회하거나 보안에 관련된 사항들은 dispatch를 이용하는 것이 바람직



**회원정보 바인딩)** 

##### MemberSerlvet => ViewServlet

서버에서 바로 데이터를 넘겨줄 때는 dispatch를 이용

 DB에 있는 정보를 조회해 화면으로 데이터를 바인딩 시켜보자.

``` java
List membersList = (List) request.getAttribute("membersList"); 
```

=> MemberServlet에서 넘어온 리스트를 받아서 회원정보를 출력

"브라우저에서 전달된 request에 **주소를 바인딩**"

**회원정보 조회 바인딩 실습 코드))** 

**MemberSerlvet** ; 첫 번째 서블릿 

바인딩 후 포워딩 (다른 서블릿으로 데이터 전달용)

```java
package sec04.ex03;

import java.io.IOException;
import java.io.PrintWriter;
import java.sql.Date;
import java.util.List;

import javax.servlet.RequestDispatcher;
import javax.servlet.ServletException;
import javax.servlet.annotation.WebServlet;
import javax.servlet.http.HttpServlet;
import javax.servlet.http.HttpServletRequest;
import javax.servlet.http.HttpServletResponse;

@WebServlet("/member")
public class MemberServlet extends HttpServlet {

    protected void doGet(HttpServletRequest request, HttpServletResponse response) throws ServletException, IOException {
        doHandle(request, response);
    }

    protected void doPost(HttpServletRequest request, HttpServletResponse response) throws ServletException, IOException {
        doHandle(request, response);
    }

    private void doHandle(HttpServletRequest request, HttpServletResponse response) throws ServletException, IOException {
        request.setCharacterEncoding("utf-8");
        response.setContentType("text/html;charset=utf-8");
        MemberDAO dao = new MemberDAO();        
        List memberList = dao.listMembers(); //dao회원정보를 list로 받아와 

        request.setAttribute("membersList", memberList); //데이터를 각 객체에 바인딩 
        RequestDispatcher dispatch = request.getRequestDispatcher("viewMembers"); //ViewServlet으로 포워딩 
        dispatch.forward(request, response); 

    }

}
```



**ViewServlet** ; 두 번째 서블릿 

요청되고 바인딩된 데이터를 list로 가져옴 

=> List membersList = (List) request.getAttribute("membersList"); 

MemberServlet에서 넘어온 리스트를 받아서 회원정보를 출력

브라우저에 출력용 

``` java
package sec04.ex03;

import java.io.IOException;
import java.io.PrintWriter;
import java.sql.Date;
import java.util.List;

import javax.servlet.ServletException;
import javax.servlet.annotation.WebServlet;
import javax.servlet.http.HttpServlet;
import javax.servlet.http.HttpServletRequest;
import javax.servlet.http.HttpServletResponse;

@WebServlet("/viewMembers")
public class ViewServlet extends HttpServlet {

    protected void doGet(HttpServletRequest request, HttpServletResponse response) throws ServletException, IOException {
          request.setCharacterEncoding("utf-8");        
          response.setContentType("text/html;charset=utf-8");
          PrintWriter out = response.getWriter();    

          List membersList = (List) request.getAttribute("membersList"); //첫번째 서블릿에서 바인딩한 회원정보를 List로 가져옴 

          out.print("<html><body>"); //브라우저에 출력
          out.print("<table border=1><tr align='center' bgcolor='lightgreen'>");
          out.print("<td>아이디</td><td>비밀번호</td><td>이름</td><td>이메일</td><td>가입일</td><td >삭제</td></tr>");

          for(int i=0; i < membersList.size(); i++) {
              MemberVO memberVO = (MemberVO) membersList.get(i);
              String id = memberVO.getId();
              String pwd = memberVO.getPwd();
              String name = memberVO.getName();
              String email = memberVO.getEmail();
              Date joinDate = memberVO.getJoinDate();
              out.print("<tr><td>" + id + "</td><td>" + pwd + "</td><td>" + name + "</td><td>" + email + "</td><td>"
                    + joinDate + "</td><td>" + "<a href='/pro07/member3?command=delMember&id=" + id
                    + "'>삭제 </a></td></tr>");              
          }

          out.print("</table></body></html>");
          out.print("<a href='/pro07/memberForm.html'>새 회원 등록하기</a");        
    }

}



