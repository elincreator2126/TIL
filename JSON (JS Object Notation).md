# JSON (JS Object Notation)

모바일에서 XML 형식 대신 

**name/value 쌍**으로 이루어진 **데이터 객체 전달하기 위해** 

사람이 읽을 수 있는 텍스트를 사용하는 

**개방형 표준 데이터 형식** 

=> 비동기 브라우저/서버 통신 (ajax)을 위해 **xml을 대체하는 데이터 전송형식** 중 하나 

=> 근본은 js에서 파행, js의 구문형식을 따르지만, **프로그래밍 언어나 플랫폼에 독립적**



## json 의 기능 

- 메서드

 **parse**( ) : 내가 원하는 데이터를 특정패턴이나 순서로 추출하고 가공하는 것 



### json의 자료형 

**객체 - { }**로 표현 / 콤마를 사용해 **여러 프로퍼티**를 포함 가능 

**name/value 쌍**을 콤마로 구분해서 나열 

```jsp
{
"name": "홍길동"
"age": 16
"weight": 67 
} //객체 작성법
```



> **json1.jsp** 

**문자열 자료형 화면에 출력** 

시작은 함수로! $(function() {

});  //여기 안에 담기 

시작과 끝 메서드 활용 

시작: click( ) -- 안에 함수 --> .click( function( ) ) { } ; //클래스 

끝:  html( )  -- 안에 매개변수 --> .html( output ) ;

- 배열(json 자료형) 선언과 parse 메서드 적용 
- 최종 출력 변수를 parse한 변수와 배열 속성을 이용하여 가공 
- for 반복문 과 누적(+=) 활용 
- 최종적으로 id 속성 선택자를 활용하는 JQuery 형태로 출력 $("#____").html(  ); 

html메서드 활용 => 웹 페이지로 출력 

(html 대체 )

```jsp
<script>
$(function () {
    $("#checkjson").click(function(){
        var jsonStr = '{ "name": ["홍길동", "이순신", "임꺽정"]}'; //이름을 저장하는 배열을 name으로 선언 
        var jsonInfo = JSON.parse(jsonStr); //parse 메서드로 json자료형(배열)을 가져와
        var output = "회원 이름<br>"; //최종 출력 변수 선언 
        output += "=========<br>";
        for(var i in jsonInfo.name){ //json 자료형(배열)을 저장한 변수.선언한 속성
            //자료형 변수와 속성을 활용하여 출력변수에 활용 
            output += jsonInfo.name[i] + "<br>"; //누적출력
        }
        $("#output").html(output);
    })
});
</script>
```

> **json2.jsp** 

**정수 자료형 화면에 출력** 

내용 위와 동일 

```jsp
<script> 
$(function () {
    $("#checkjson").click(function(){
        var jsonStr = '{ "age": [22, 33, 44]}'; 
        var jsonInfo = JSON.parse(jsonStr);
        var output = "회원 나이<br>";
        output += "=========<br>";
        for(var i in jsonInfo.age){
            output += jsonInfo.name[i] + "<br>";
        }
        $("#output").html(output);
    })
});
</script>
```



> **json3. jsp** 

json  객체에 회원정보 저장 - 회원정보 재출력 

```jsp
<script> 
$(function () {
    $("#checkjson").click(function(){
        var jsonStr = '{ "name": "박지성", "age":25, "gender":"남자", "nickname":"날쌘돌이" }'; 
        var jsonObj = JSON.parse(jsonStr);
        var output = "회원 정보<br>";
        output += "=========<br>";
        output += "이름: " + jsonObj.name + "<br>";
        output += "나이: " + jsonObj.age + "<br>";
        output += "성별: " + jsonObj.gender + "<br>";
        output += "별명: " + jsonObj.nickname + "<br>";
        $("#output").html(output);
    })
});
</script>
```



> **json4.jsp**

JSON 배열의 요소에 JSON 객체를 저장 

배열에 접근 -> json객체의 속성 값을 출력 

```jsp 
<script> 
$(function () {
    $("#checkjson").click(function(){
        var jsonStr = '{"members" : [{ "name": "박지성", "age":25, "gender":"남자", "nickname":"날쌘돌이" }'
        + ',{"name": "손흥민", "age":20, "gender":"남자", "nickname":"탱크"}] }'; 
        var jsonInfo = JSON.parse(jsonStr);
        var output = "회원 정보<br>";
        output += "=========<br>";
        for(var i in jsonInfo.members){
        output += "이름: " + jsonInfo.members[i].name + "<br>";
        output += "나이: " + jsonInfo.members[i].age + "<br>";
        output += "성별: " + jsonInfo.members[i].gender + "<br>";
        output += "별명: " + jsonInfo.members[i].nickname + "<br><br><br>";
        }//for end 
        $("#output").html(output);
    });
});
</script>
```



## JSON 데이터 주고받기

### 1단계 : web (jsp) -----> servlet

**JsonServlet1.java**

1) json 문자열을 가져와 (getParameter( )  이용 )

2) JSONObject 가져와 ( parse( ) 이용 )

3) json 데이터의 name 속성으로 value 출력 

```java
@WebServlet("/json")
public class JsonServlet1 extends HttpServlet {
    private void doService(HttpServletRequest request, HttpServletResponse response) throws ... {
        :
    String jsonInfo = request.getParameter("jsonInfo");
    try{
        JSONParser jsonParser = new JSONParser(); //json 데이터 처리 위해 JSONParser 객체 생성 
        JSONObject jsonObject = (JSONObject) jsonParser.parse(jsonInfo); //전송된 JSON 데이터를 파싱
        system.out.println(jsonObject.get("name"));
        system.out.println(jsonObject.get("age"));
        system.out.println(jsonObject.get("gender"));
        system.out.println(jsonObject.get("nickname"));
    } catch(Exception e) {
        e.printStackTrace();
    }//catch end 
}//throws end 
}//class end 
```



**json5.jsp** 

1) js 에서 회원정보를 JSON 객체로 만들어 

2) 매개변수 이름 jsonInfo 로 ajax 이용해 서블릿으로 전송 

```jsp
<script> 
$(function () {
    $("#checkjson").click(function(){
        var _jsonInfo = '{ "name": "박지성", "age":25, "gender":"남자", "nickname":"날쌘돌이" }'; 
    $.ajax({
        type: "post", 
        async: false, 
        url: "$(contextPath)/json"
        data: {jsonInfo: _jsonInfo} //매개변수 이름 jsonInfo로 JSON 데이터(_jsonInfo)를 ajax로 전송 
         success: function (data, textStatus) {
    	}, 
        error: function (data, textStatus) {
            alert("에러가 발생했습니다.");
        }, 
        complete : function (data, textStatus) {
        }
         }); //ajax end 
    }); //click(function()) end 
 }); //function() end 
</script>
```



### 2단계 : servlet -----> web (jsp)

**JsonServlet2.java**

<json 배열에 회원정보를 저장 -> jsp 로 전송 > 

1) json 객체 생성 -> 회원 정보를 name/value 쌍으로 저장 

2) 객체 생성 -> json 객체를 차례로 저장 

3) 배열에 회원정보를 저장 

4) 객체 생성 -> name에 ~  , value에~ 저장 

시작: printWriter writer = response.getWriter();

끝: writer.print(jsonInfo);

```java
@WebServlet("/json")
public class JsonServlet1 extends HttpServlet {
    private void doService(HttpServletRequest request, HttpServletResponse response) throws ... {
        :
        printWriter writer = response.getWriter();
        JSONObject totalObject = new JSONObject();//배열을 저장 객체 선언 
        JSONArray membersArray =  new JSONArray(); //json객체 저장할 배열 선언  -- 최종 저장되는 value
        JSONObject memberInfo = new JSONObject(); //한 명의 정보가 들어갈 객체 생성 
        memberInfo.put("name", "박지성");
        memberInfo.put("age", "35");
        memberInfo.put("gender", "남자");
        memberInfo.put("nickname", "날쌘돌이");
        membersArray.add(memberInfo); //회원 정보 배열에 다시 저장 
        memberInfo = new JSONObject();
        memberInfo.put("name", "김연아");
        memberInfo.put("age", "21");
        memberInfo.put("gender", "여자");
        memberInfo.put("nickname", "스핀");
        membersArray.add(memberInfo); //다른 정보 추가 후, 배열에 재저장 
        
        totalObject.put("members", membersArray); //members 라는 name으로 , membersArray라는 value로 . 배열을 저장할 객체의 변수에 저장 
        
        String jsonInfo = totalObject.toJSONString(); //json객체를 json의 toString()을 이용해서 문자열로 반환 
        System.out.println(jsonInfo); //반환한 문자열 출력
        writer.print(jsonInfo); //json데이터를 브라우저로 전송
        //PrintWriter 매개변수.메서드(문자열 변수)형식으로 작성
        
```



**json6.jsp**

$(contextPath) ????

servlet -> web 

data: {jsonInfo: _jsonInfo}  => 제거 ; json데이터가 저장된 servlet에서 web으로 전송임. data 보낼 필요 없어서 삭제 . 데이터를 받아와서 출력하면 되므로

_//매개변수 이름 jsonInfo로 JSON 데이터(_jsonInfo)를 ajax로 전송   

```jsp
<script> 
$(function () {
    $("#checkjson").click(function(){
        var _jsonInfo = '{ "name": "박지성", "age":25, "gender":"남자", "nickname":"날쌘돌이" }'; 
    $.ajax({
        type: "post", 
        async: false, 
        url: "$(contextPath)/json2"
        success: function (data, textStatus) {
        	var jsonInfo = JSON.parse(data);
        	var memberInfo = "회원 정보<br>";
        	memberInfo += "==========<br>";
        	for (var i in jsonInfo.members){
                membersInfo += "이름: " + jsonInfo.members[i].name + "<br>";
                membersInfo += "나이: " + jsonInfo.members[i].age + "<br>";
                membersInfo += "성별: " + jsonInfo.members[i].gender + "<br>";
                membersInfo += "별명: " + jsonInfo.members[i].nickname + "<br><br><br>";
            }
        	$("#output").html(membersInfo);
    	}, 
        error: function (data, textStatus) {
            alert("에러가 발생했습니다.");
        }, 
        complete : function (data, textStatus) {
        }
         }); //ajax end 
    }); //click(function()) end 
 }); //function() end 
</script>
```

서블릿 -> 웹 

이 코드를 ajax의  success 안에 넣음 (위치이동)

서블릿에서 json 데이터를 받아서 화면에출력하는 것이므로 

ajax 의 data 속성이 없으므로 

        	var jsonInfo = JSON.parse(data);
        	var memberInfo = "회원 정보<br>";
        	memberInfo += "==========<br>";
        	for (var i in jsonInfo.members){
                membersInfo += "이름: " + jsonInfo.members[i].name + "<br>";
                membersInfo += "나이: " + jsonInfo.members[i].age + "<br>";
                membersInfo += "성별: " + jsonInfo.members[i].gender + "<br>";
                membersInfo += "별명: " + jsonInfo.members[i].nickname + "<br><br><br>";
            }
        	$("#output").html(membersInfo);

=> json 데이터를 전달받아 화면에 회원 정보를 출력 