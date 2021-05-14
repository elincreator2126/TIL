# Spring

## 메서드

- **contextPath( )** 

  : JSP에서 현재 경로를 가져오기

**js 이용** 

 <%=request.getRequestURL() %>

 <%=request.getContextPath()  %>



**el  표현식** 

${pageContext.request.requestURL}

${pageContext.request.requestURI}

${pageContext.request.contextPath}



효율적인 코딩 - contextPath가 변경되어도 jsp 소스 변경하지 않아도 됨

```jsp
<a href="${pageContext.request.contextPath}/a/b/c/d.do"> 이동하려는 페이지 </a> 

<a href="<c:url value="/a/b/c/d.do" /> //jstl의 core의 url이용
```

MVC모델에서 pageContext.request.requestURL 이용 

-> WEB-INF의 실제 JSP 경로 가져와  

표현식으로 현재 URL -> ${requestScope["javax.servlet.forward.request_uri"]}



- **ModelAndVIew**( )

: 데이터를 전송시킬 수 있는 리턴 타입

​	(cf. String 타입은 단순하게 페이지만 열어주는 역할을 함)

1. **setViewName** : 어떤 페이지를 보여줄 것인지

2. **addObject** : key와 value를 담아 보낼 수 있는 메서드

3. 1. setAttribute를 여러개 썼던 것 처럼, addObject를 여러개 쓸 수 있다.




## 에너테이션 (Annotation)

- **@RequestMapping** 

: 전송하기. 전송방식 지정하기 

**장점**

@RequestParam으로 form 안의 데이터를 직접 받을 수도 있다.

@PathVariable과 @RequestParam을 혼합해서 사용할 수 있다.

@RequestParam을 여러 개 쓸 수도 있다. 



- **@RequestParam**

: 매개변수에 값 자동 설정 기능 



**사용 계기** 

브라우저에서 매개변수 전송 -> getParameter( ) 이용해 값을 얻음 

단점: 전송되어 온 매개변수의 수가 많아지면 일일이 메소드 이용 불편 

=> @RequestParam을 메서드에 적용해 값을 얻음 

1) @RequestParam을 이용, 서버에서 전송받은 매개변수 설정 

=>  브라우저에서 매개변수 전달 - 지정한 변수에 자동으로 값 설정 

```java
@Controller("loginController")
public class LoginController {
    @RequestMapping(value = "/test/login2.do", method=~)
    
    public ModelAndView login2(@RequestParam("userID") String userID, @RequestParam("userName") String userName, )
        ModelAndView mv = new ModelAndView();
    	mv.setViewName("result");
    	mv.addObject("userID", userID);
     	mv.addObject("userName", userName);
    	return mv; 
}
```



- **@Autowired** 

: 클래스들의 빈을 직접 자바 코드에서 생성하여 사용 

장점 

별도의 setter나 생성자 없이 속성에 빈 주입 가능 



- **@Controller / @Service / @Repository / @Component**

: 컨트롤러 / 서비스 / Impl / VO(DTO) 빈을 자동 생성 



