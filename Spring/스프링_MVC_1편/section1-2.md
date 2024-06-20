> ※ 면접 대비 겸 학습의 목적으로 질문과 답변을 작성했습니다. 따라서 키워드 중심의 간결한 문체를 사용했음을 양해바랍니다. 잘못된 내용 또는 보강이 필요한 부분을 말씀해주시면 정말 감사드리겠습니다. 답변의 출처는 대게 인프런 김영한 강사님의 '스프링 MVC 1편'이며, 검색에 기반한 답변은 가급적 출처를 달았습니다.

---
<br />

## 질문) WebServer와 WAS에 대해 설명해주세요

<details>
    <summary>답변</summary>
</br>

**공통점**
- HTTP 프로토콜을 기반으로 동작하는 서버.

**웹 서버(Web Server)**
- 정적 컨텐츠(이미지, 파일, 텍스트) 제공, HTTP 요청 메시지를 연결하고 받아들이는 웹 서버의 역할
- E.g.) Nginx, Apache HTTP Server

**웹 애플리케이션 서버(Web Application Server, WAS)**

- WAS는 `Web Server` + `Web Container(Sevlet Container)` 으로 구성되어 있다.
- 따라서 WebServer의 역할을 기본적으로 수행가능하며,  웹 컨테이너를 이용해 내부(비즈니스) 로직을 거쳐 동적 페이지/데이터/컨텐츠를 제공(응답)한다. (E.g.동적 HTML, JSON)
- E.g.) Tomcat, Jetty

</details>

### 꼬리질문1) 그렇다면 웹 시스템을 구성할 때, WAS와 DB만 존재하면 되는것인가요?

<details>
    <summary>답변</summary>
</br>

**WAS & DB 으로만 구성된 경우**
- 정적 자원 응답을 포함해 WAS가 너무 많은 역할을 담당. 서버 과부화, 응답 지연 우려.
- WAS 장애시 오류 화면도 응답이 불가.

**WebServer & WAS & DB 구성**
![image](https://github.com/proHyundo/backend-cs-study/assets/128882585/076417b7-5051-422f-ac02-3567ecdb261f)
- 단순한 정적 컨텐츠 응답은 웹서버에게 맡기고, WAS는 중요한 애플리케이션 로직에 집중. 필요 시 WAS 서버 증설도 가능
- WAS, DB 장애 시, WEB 서버가 오류화면 제공 가능

**그 외 구성**
- 특정 구조가 정답은 아님. API 서버를 운영할 것이면 WAS만 있어도 되고, WebServer 앞에 CDN 서버를 둬서 정적 자원을 캐싱하기도 함

</details>

### 꼬리질문2) WebContainer와 ServletContainer의 차이점은 무엇인가요?

<details>
    <summary>답변</summary>
</br>

자바의 경우 웹 구현 기술로 Servlet을 사용하는데, 이때 자바 기반인 경우 WAS의 WebContainer는 ServletContainer 이다.

따라서 Java 웹 애플리케이션의 경우 두 용어는 동일한 의미이다.

HttpServletRequest, HttpServletReponse를 생성하고, 우리가 만든 서블릿을 호출하는 서블릿 컨테이너의 역할. 이 Servlet을 관리하고 jsp파일을 실행할 수 있게 해주는 것이 Servlet Container.

</details>

---
</br>

## 질문) Servlet에 대해 설명해주세요

<details>
    <summary>답변</summary>
</br>

**정의**

- 다양한 유형의 요청에 응답할 수 있으며, 일반적으로 웹 서버 또는 웹 애플리케이션을 위한 기능/사양을 갖춘 자바 클래스를 `Servlet` 이라 한다. 
    - 출처 : [17.1 What Is a Servlet?
](https://docs.oracle.com/javaee/7/tutorial/servlets001.htm)

|![image](https://github.com/proHyundo/backend-cs-study/assets/128882585/59de63fa-709a-45c8-87de-71fe5106544b)|
|---|
-  TCP/IP 연결이나 HTTP 메세지 파싱, HTTP 응답 메시지 생성(Body에 HTML 생성) 등 비즈니스 로직 외의 것들을 대신 해줘서 개발자는 비즈니스(애플리케이션) 로직에만 집중하여 개발할 수 있다.

**특징**
- Servlet 객체는 싱글톤으로 관리된다.
    - 따라서 공유 변수는 조심해서 사용하라.
- 서블릿 객체는 WAS 안에 존재하는 서블릿 컨테이너에 의해 자동으로 생성, 호출, 생명주기에 따라 관리된다.



- Spring MVC는 Servlet 위에서 동작한다.



</details>

### 꼬리질문) Servlet Container 란 무엇인가요?

<details>
    <summary>답변</summary>
</br>

![servlet-http-request-response-flow](https://github.com/proHyundo/backend-cs-study/assets/128882585/855a09b1-7a18-4702-a0b0-e27b793ee78e)

서블릿 컨테이너는 서블릿을 관리하기 위한 것

참고 링크 :
- [ServletContainer와 SpringContainer는 무엇이 다른가?](https://sigridjin.medium.com/servletcontainer%EC%99%80-springcontainer%EB%8A%94-%EB%AC%B4%EC%97%87%EC%9D%B4-%EB%8B%A4%EB%A5%B8%EA%B0%80-626d27a80fe5)

</details>

### 꼬리질문) Servlet Context는 Application Context와 어떤 차이가 있나요?

<details>
    <summary>답변</summary>
</br>


- [급하게 알아보는 스프링 기반 기술 Servlet, Servlet Context, Application Context](https://jeong-pro.tistory.com/222)

</details>

### 꼬리질문) 서블릿 컨테이너와 스프링 컨테이너는 어떻게 다른가?

<details>
    <summary>답변</summary>
</br>

서블릿 컨테이너에 서블릿 객체를 등록하는 것과 서블릿 객체를 스프링 빈으로 스프링 컨테이너에 등록하는 것은 유사한 듯 보이지만, 기본적으로 서로 다른 과정을 의미합니다. 각각의 컨테이너가 하는 일과 그 목적을 이해하면 이 차이점이 명확해집니다.

서블릿 컨테이너

서블릿 컨테이너(예: Apache Tomcat, Jetty)는 서블릿 생명 주기를 관리하고, HTTP 요청을 받아서 서블릿에 전달한 후, 서블릿이 생성하는 HTTP 응답을 클라이언트에게 전송하는 역할을 합니다. 서블릿 컨테이너에 서블릿 객체를 등록한다는 것은 이러한 서블릿 컨테이너가 관리할 수 있도록 서블릿 클래스를 등록하고, URL 패턴과 연결하는 과정을 의미합니다. 이 등록 과정은 대개 web.xml 파일에 서블릿과 서블릿 매핑을 정의하거나, 서블릿 3.0 이상에서 제공하는 @WebServlet과 같은 애노테이션을 사용하여 수행됩니다.

스프링 컨테이너

반면, 스프링 프레임워크는 자체 컨테이너(ApplicationContext 등)를 통해 스프링 빈의 생명 주기를 관리합니다. 스프링 컨테이너에 객체를 스프링 빈으로 등록한다는 것은 그 객체가 스프링 프레임워크의 관리 아래에 들어간다는 의미이며, 이를 통해 의존성 주입, AOP, 트랜잭션 관리 등 스프링이 제공하는 다양한 기능을 활용할 수 있습니다.

답변 출처 : https://www.inflearn.com/questions/1226415

</details>

### 꼬리질문2) Servlet의 생명주기에 대해 설명해주세요

<details>
    <summary>답변</summary>
</br>

![servlet-life-cycle](https://github.com/proHyundo/backend-cs-study/assets/128882585/da528ddc-d468-424b-a534-8a53ae2f61a1)

요청이 오면, Servlet 클래스가 로딩되어 요청에 대한 Servlet 객체가 생성됩니다.
서버는 init() 메소드를 호출해서 Servlet을  초기화 합니다.
service() 메소드를 호출해서 Servlet이 브라우저의 요청을 처리하도록 합니다.
service() 메소드는 특정 HTTP 요청(GET, POST 등)을 처리하는 메서드 (doGet(), doPost() 등)를 호출합니다.
서버는 destroy() 메소드를 호출하여 Servlet을 제거합니다.

Servlet 객체를 생성하고 초기화하는 작업은 비용이 많은 작업이므로, 다음에 또 요청이 올 때를 대비하여 이미 생성된 Servlet 객체는 메모리에 남겨둡니다.
또 톰캣이 종료되기 전이나 reload 전에 모든 Servlet을 제거하게 됩니다.
이렇게 톰캣은 자원을 아끼면서 Servlet을 사용하고 있습니다.

</details>

### 꼬리질문3) HttpServletRequest 의 역할은 무엇인가요?

<details>
    <summary>답변</summary>
</br>

1. HTTP 요청 메시지를 편리하게 파싱/사용 가능.
2. HTTP 요청 메시지 안에 임시저장소 제공.
   - `request.setAttribute(key, value);`
   - 요청이 서버에 들어와 응답되는 시점까지 사용가능
3. 세션 관리 기능
   - `request.getSession();`

![image](https://github.com/proHyundo/backend-cs-study/assets/128882585/2ab275ae-7dbd-4ff4-98ee-38d51dfb503f)

![image](https://github.com/proHyundo/backend-cs-study/assets/128882585/f21dae90-db1f-49db-9d10-6a579b113bf2)

인터페이스 이다.
참고 링크 : https://acafela.github.io/tomcat/2021/01/18/tomcat-facade.html

</details>

### 꼬리질문3) HTTP 요청을 보낼때 마다 request 객체의 주소값이 변하지 않는 이유는 무엇인가요?

<details>
    <summary>답변</summary>
</br>

`protected service` 메서드의 매개변수 `HttpServletRequest` 는 인터페이스이며, 톰캣은 `RequestFacade` 구현체를 사용하고 있습니다.
클라이언트의 매 요청마다 해당 인터페이스의 구현체인 RequestFacade 클래스 인스턴스를 새로 생성할 것이라는 추측과 다르게 실제로는 동일한 객체를 재사용하고 있습니다.
이는 톰캣의 GC 성능 최적화 정책에 의해 `RequestFacade` 객체의 상시 새로운 객체를 생성을 결정하는 `discardFacades` flag 값이 false 으로 설정되어 있기 때문입니다.
톰캣 8.5 버전 공식문서의 설명에 의하면 이는 컨테이너 내부 요청 처리 객체를 격리하는 파사드 객체의 재활용을 활성화 또는 비활성화하는 데 사용할 수 있는 부울 값입니다. true로 설정하면 모든 요청 후 가비지 수집을 위해 파사드가 설정되고, 그렇지 않으면 재사용됩니다. 보안 관리자가 활성화된 경우에는 이 설정이 적용되지 않습니다. 지정하지 않으면 이 속성은 org.apache.catalina.connector.RECYCLE_FACADES 시스템 속성의 값으로 설정되고, 설정하지 않으면 false로 설정됩니다.

우분투의 경우, 이 설정은 /etc/systemd/system/tomcat.service 파일에서 CATALINA_OPTS 변수를 통해 관리할 수 있습니다. 이 설정은 파일에 정의되어 있으며 Tomcat 시작 시 참조되어 Tomcat 환경 변수를 로드합니다.

각 요청에 대해 매번 새 파사드 객체를 생성할 경우, 애플리케이션의 버그로 인해 한 요청의 데이터가 다른 요청에 노출될 가능성이 줄어드는 이점이 있지만,

Tomcat은 GC Churning(즉, 많은 개체를 반복적으로 생성하고 폐기하는 것)을 줄이기 위해 요청 전반에 걸쳐 가능한 한 많은 개체를 재사용합니다. 이러한 객체 사용으로 인한 성능 차이는 없으며, 단지 폐기 및 재생성 주기로 인해 성능이 저하될 뿐입니다.

기본 설정 자체가 "안전하지 않은" 것은 아니지만 버그가 있는 애플리케이션으로 인해 Tomcat이 매우 이상한 작업을 수행하는 것처럼 보일 수 있습니다. 따라서 애플리케이션이 규칙을 위반하지 않는다면 더 높은 성능의 구성을 선호해야 합니다.

질문 출처 : [http 요청을 보낼때마다 request, response 객체의 주소값이 변하지 않는 이유?](https://www.inflearn.com/questions/307198/http-%EC%9A%94%EC%B2%AD%EC%9D%84-%EB%B3%B4%EB%82%BC%EB%95%8C%EB%A7%88%EB%8B%A4-request-response-%EA%B0%9D%EC%B2%B4%EC%9D%98-%EC%A3%BC%EC%86%8C%EA%B0%92%EC%9D%B4-%EB%B3%80%ED%95%98%EC%A7%80-%EC%95%8A%EB%8A%94-%EC%9D%B4%EC%9C%A0)
답변 참고 링크
- https://tomcat.apache.org/tomcat-8.5-doc/config/http.html
- https://stackoverflow.com/questions/57777941/disadvantages-of-setting-tomcats-recycle-facades-true
- https://knowledge.broadcom.com/external/article/219611/how-to-configure-tomcat-and-enable-recyc.html
- https://mangkyu.tistory.com/14

# https://codinglog.tistory.com/118

---
</br>

### 질문) Controller 1개는 어떻게 수십 만개의 요청을 처리하는가

<details>
    <summary>답변</summary>
</br>

참고 링크 : [servletcontainer와-springcontainer는-무엇이-다른가](https://sigridjin.medium.com/servletcontainer%EC%99%80-springcontainer%EB%8A%94-%EB%AC%B4%EC%97%87%EC%9D%B4-%EB%8B%A4%EB%A5%B8%EA%B0%80-626d27a80fe5)

</details>

### 꼬리질문1) 하나의 클라이언트에 하나의 쓰레드가 부여되는 것 아닌가요? 각각의 클라이언트는 자신만의 쓰레드에서 동작하는데요?

<details>
    <summary>답변</summary>
</br>

</details>

### 꼬리질문2) Thread-per-request를 Thread-per-connection에 우선해서 쓰는 이유는 뭐죠?

<details>
    <summary>답변</summary>
</br>

</details>

---
</br>

### 질문) HTTP Post 요청 시, `x-www-form-urlencoded` 형식을 서버에서 어떻게 파싱할(꺼낼) 수 있나요?

<details>
    <summary>답변</summary>
</br>

1) `request.getParameter()`

- 클라이언트(웹 브라우저) 입장에서는 두 방식에 차이가 있지만, 서버 입장에서는 둘의 형식이 동일하므로, `request.getParameter()` 로 편리하게 구분없이 조회할 수 있다. 정리하면 `request.getParameter()` 는 GET URL 쿼리 파라미터 형식도 지원하고, POST HTML Form 형식도 둘 다 지원한다.
- 따라서 두 방식 모두 `요청 파라미터를 읽는다` 라고 표현한다.

2) request.getInputStream()

</details>

#### 꼬리질문) JSON request -> Obj & Obj result -> JSON response

<details>
    <summary>답변</summary>
</br>

```java
ObjectMapper objMapper = new ObjectMapper();
// request -> Obj
HelloDto helloDto = objMapper.readValue(
    StreamUtils.copyToString(request.getInputStream(), StandardCharsets.UTF_8),
    HelloDto.class
);

// obj -> json
ResultDto resultDto = new ResultDto("result value");
String jsonResult = objMapper.writeValueAsString(resultDto);
```

</details>

### 질문) Spring MVC의 흐름을 말씀해주세요


질문 목록

Servlet의 동작 원리는 무엇인지
Servlet의 라이프사이클은 어떻게 동작하는지
web.xml이 각 Servlet과 매핑되는 URI를 어떻게 관리하는지
DispatcherServlet 구조가 web.xml을 어떻게 대체하는지

Q.jsp와 servlet의 차이점을 설명해보아라.
A.두 기술 모두 자바 기반으로 웹 프로그래밍을 수행하기 위해 존재하는 기술이다. servlet은 java 코드안에 html 언어를 작성하는 방식이지만, jsp는 반대로 html 언어 안에 java 코드를 작성한다. jsp는 servlet의 불편함을 극복하기 위해 고안된 기술이고, jsp는 컴파일될 때 서블릿으로 한 번 변환된 후 컴파일 된다는 특성이 있다.

---
</br>




https://russell-seo.tistory.com/9

https://ksh-coding.tistory.com/116

## 질문) GET 요청 시, 중복된 파라미터 Key 값으로 데이터를 전송하면 어떤 값이 ?

참고로 이렇게 중복일 때 `request.getParameter()` 를 사용하면 `request.getParameterValues()` 의 첫
번째 값을 반환한다.

중복으로 보내는 경우는 많지는 않지만 아래와 같은 상황에서 사용될 수 있음
- HTML checkbox 태그의 name 이 같을때
- 단순히 같은 이름으로 여러 데이터를 동적으로 추가하면서 넘길때


더 공부할 링크

https://jeong-pro.tistory.com/222
https://sigridjin.medium.com/servletcontainer%EC%99%80-springcontainer%EB%8A%94-%EB%AC%B4%EC%97%87%EC%9D%B4-%EB%8B%A4%EB%A5%B8%EA%B0%80-626d27a80fe5