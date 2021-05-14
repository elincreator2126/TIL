# 쿠키와 세션 

**웹 페이지 연결 기능,  '세션 트래킹 (Session Tracking)'**

예) 사용자의 로그인 상태를 일정하게 유지시키는 기능 

웹 페이지 사이의 상태나 정보를 공유하기 위해 

-  '세션 트래킹 (Session Tracking)' 구현해야 



**웹 페이지에 연동하는 방법** 

1. <hidden> 태그, HTML 의 이 태그를 이용해, 웹 페이지들 사이의 정보 공유

2. URL Rewriting : GET 방식으로 URL 뒤에 정보를 붙여서 다른 페이지로 전송 

3. 쿠키: 클라이언트 PC의 Cookie 파일에 정보를 저장 후, 웹 페이지들이 공유

4. 세션 : 서버 메모리에 정보 저장 

   

   ### 1) html <hidden> 태그

   **login.html** 

```html
<input type="hidden" name=  value=  />
<input type="hidden" name=  value=  />
<input type="hidden" name=  value=  />
```



**LoginServlet 클래스** 

getParameter( ) 이용 -> 전송된 회원정보 가져와 -> 브라우저로 출력 

```java
@WebServlet("/login")
:
String user_address = request.getParameter("user_address");
String user_address = request.getParameter("user_email");
String user_address = request.getParameter("user_hp"); //요청된 데이터 받아오기(가져오기)

String data = "안녕하세요! <br> 로그인하셨습니다.<br><br>"
data += "<html><body>"
:
data += "휴대전화: "+user_hp;
data += "</body></html>";
out.print(data);
}
:
```



### 2)URL Rewriting을 이용

**LoginServlet** 

로그인!

로그인 창에서 입력받은 ID 와 PW를 <a> 태그의 두 번째 서블릿으로 보내기 클릭 - > 로그인 창에서 입력한 ID, PW 그리고 다른 정보들을 get 방식을 이용해 전송 (두 번째 서블릿으로)

 ```java
 @WebServlet("/login")
 :
 String user_address = request.getParameter("user_address");
 String user_address = request.getParameter("user_email");
 String user_address = request.getParameter("user_hp"); //요청된 데이터 받아오기(가져오기)
 
 String data = "안녕하세요! <br> 로그인하셨습니다.<br><br>"
 data += "<html><body>"
 :
 data += "휴대전화: "+user_hp;
 data += "<br>";
 out.print(data);
 
 user_address = //인코딩 
 out.print("<a href = ' / /second?user_id=" +user_id+ "&user_pw=" + user_pw + "&user_address="+user_address+"'> 두 번째 서블릿으로 보내기 </a>)
 data += "</body></html>";
 out.print(data);
 }
 :
 ```



**SecondServlet** 

로그인 상태 유지!

```java
@WebServlet("/second")
:
String user_id = request.getParameter("user_id");
String user_pw = request.getParameter("user_pw");
String user_address = request.getParameter("user_address");

out.println("<html><body>");
if (user_id != null && user_id.length()!=0) {
    out.println("이미 로그인 상태입니다. <br><br>");
    :
    out.prinln("</body></html>");
}else{
    out.println("로그인 하지 않았습니다.<br><br>");
    out.println("<a href=' / /login.html'> 로그인 창으로 이동하기 </a>"); 
}
}
```

