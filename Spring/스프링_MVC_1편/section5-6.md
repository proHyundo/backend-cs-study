> ※ 면접 대비 및 학습의 목적으로 질문과 답변을 작성했습니다. 따라서 키워드 중심의 간결한 문체를 사용했음을 양해바랍니다. 잘못된 내용 또는 보강이 필요한 부분을 말씀해주시면 정말 감사드리겠습니다. 답변의 출처는 대게 인프런 김영한 강사님의 '스프링 MVC 1편'이며, 검색에 기반한 답변은 가급적 출처를 달았습니다.

## 질문) Spring MVC의 동작 과정을 설명해주세요.

<details>
    <summary>답변</summary>
</br>

![출처:https://velog.io/@hyunjong96/posts](https://velog.velcdn.com/images/hyunjong96/post/addaf48b-9d54-4b5c-9068-0df95d3c26f1/image.png)

DispatcherServlet의 doDispatch() 메서드의 수행과정을 살펴보면 MVC 동작방식을 파악할 수 있다.




`DispatcherServlet.doDispatch()`
```java
protected void doDispatch(HttpServletRequest request, HttpServletResponse
response) throws Exception {

    HttpServletRequest processedRequest = request;
    HandlerExecutionChain mappedHandler = null;

    ModelAndView mv = null;
    // 1. 핸들러 조회 : 핸들러 매핑을 통해 요청 URL에 매핑된 핸들러(컨트롤러)를 조회한다.
    mappedHandler = getHandler(processedRequest);
    if (mappedHandler == null) {
        noHandlerFound(processedRequest, response);
        return;
    }

    // 2. 핸들러 어댑터 조회 - 핸들러를 처리(실행)할 수 있는 어댑터를 조회한다.
    HandlerAdapter ha = getHandlerAdapter(mappedHandler.getHandler());
    
    // 3. 핸들러 어댑터 실행 
    // -> 4. 핸들러 어댑터를 통해 핸들러 실행 
    // -> 5. 핸들러 어댑터는 핸들러가 반환하는 정보를 ModelAndView로 변환해서 반환한다.
    mv = ha.handle(processedRequest, response, mappedHandler.getHandler());
    // 6. 해당 메서드를 호출해 render 처리
    processDispatchResult(processedRequest, response, mappedHandler, mv,
    dispatchException);
}
```
`HandlerAdapter Interface`
![image](https://github.com/proHyundo/backend-cs-study/assets/128882585/aa1b5a8c-1c35-4485-8df3-0beafd4f0ca1)
```java
public interface HandlerAdapter {
    boolean supports(Object handler);

    @Nullable
    ModelAndView handle(HttpServletRequest request, HttpServletResponse response, Object handler) throws Exception;
}
```

`DispatcherServlet.processDispatchResult()`
```java
private void processDispatchResult(HttpServletRequest request,
HttpServletResponse response, HandlerExecutionChain mappedHandler, ModelAndView
mv, Exception exception) throws Exception {
    if (mv != null && !mv.wasCleared()) {
            // 뷰 렌더링 호출
            this.render(mv, request, response);
            if (errorView) {
                WebUtils.clearErrorRequestAttributes(request);
            }
        }
}
```

`DispatcherServlet.render()`
```java
protected void render(ModelAndView mv, HttpServletRequest request,
HttpServletResponse response) throws Exception {
    View view;
    String viewName = mv.getViewName();
    // 6. 뷰 리졸버들이 담긴 viewResolvers 리스트를 순회하며 ModelAndView에 저장된 viewName과 일치하는 View를 찾아 반환한다.
    view = resolveViewName(viewName, mv.getModelInternal(), locale, request);
    // 8. 뷰 렌더링
    // JSP의 경우 InternalResourceView.render()
    view.render(mv.getModelInternal(), request, response);
}
```
6. viewResolver 호출: 뷰 리졸버를 찾고 실행한다.
JSP의 경우: `InternalResourceViewResolver` 가 자동 등록되고, 사용된다.
7. View 반환: 뷰 리졸버는 뷰의 논리 이름을 물리 이름으로 바꾸고, 렌더링 역할을 담당하는 뷰 객체를 반환한다.
JSP의 경우 `InternalResourceView(JstlView)` 를 반환하는데, 내부에 `forward()` 로직이 있다.
8. 뷰 렌더링: 뷰를 통해서 뷰를 렌더링 한다.

</details>

### 꼬리질문) Handler 매핑은 어떻게 할 수 있나요?

<details>
    <summary>답변</summary>
</br>

**스프링 부트가 자동 등록하는 HandlerMapping과 HandlerAdapter**

HandlerMapping
- RequestMappingHanlderMapping : 어노테이션 `@RequestMapping` 사용
- BeanNameUrlHandlerMapping : 스프링 빈 이름으로 핸들러 찾음, `@Component` 어노테이션 또는 포함된 어노테이션들을 사용.

HandlerAdapter
- RequestMappingHandlerAdapter : 어노테이션 `@RequestMapping` 사용
- HttpRequestHandlerAdapter : HttpRequestHandler 인터페이스 처리
- SimpleControllerHandlerAdapter : Controller 인터페이스 처리



가능한 조합
1. 핸들러매핑(@Component), 핸들러(Controller 인터페이스 구현), 핸들러 어댑터(SimpleControllerHandlerAdapter)
```java
@Component(value = "/example") // BeanNameUrlHandlerMapping
public class Combi1 implements Controller {
    @Override
    public ModelAndView handleRequest(HttpServletRequest request, HttpServletResponse response) throws Exception {
        System.out.println("Combi1.handleRequest");
        return new ModelAndView("jsp-file-name");
    }
}
```
2. 핸들러매핑(@Component), 핸들러(HttpRequestHandler 인터페이스 구현), 핸들러 어댑터(HttpRequestHandlerAdapter)
```java
@Component(value = "/example2")
public class Combi2 implements HttpRequestHandler {

    @Override
    public void handleRequest(HttpServletRequest request, HttpServletResponse response) throws ServletException, IOException {
        System.out.println("Combi2.handleRequest");
    }
}
```

</details>

### 꼬리질문) 왜 Handler는 Component Scan 대상이 되어야 하나요?

<details>
    <summary>답변</summary>
</br>

DispatcherServlet이 핸들러를 매핑해야 하는데, 

</details>

### 꼬리질문) View를 만들 필요 없는 즉, 정적 리소스나 뷰 템플릿이 아닌 Json과 같은 데이터를 응답할 때는 MVC의 동작 과정이 어떻게 되나요?

응답 과정

@ResponseBody를 사용하는 경우 ViewResolver 대신에 HttpMessageConverter 인터페이스의 구현체가 동작.

- 기본 문자 처리 : `StringHttpMessageConverter`
- 기본 객체 처리 : `MappingJasckson2HttpMessageConverter`


HttpMessageConverter가 response 객체에 해당 값을 넣어두고, 흐름이 다시 DispatcherServlet으로 가서(mv는 null) 내부 로직에 의해 view를 만드는 과정이 생략되고 http 응답 메시지가 생성



### 꼬리질문) 핸들러의 메서드 파라미터에는 어떤 어노테이션들이 올 수 있나요?

---
<br />

전체 흐름 정리

(0) 스프링 컨테이너 기본 빈 생성, 초기화, DI

(1) Client URL 호출, 서블릿 컨테이너가 request를 파싱해, HttpServletRequest와 HttpServletResponse 객체 생성

(2) 쓰레드 풀의 쓰레드가 DispatcherServlet.service() 호출

(3) service() 내부에서 DispatcherServlet.doDispatch() 호출

(4) doDispathc() 내부에서 DispatcherServlet.getHandler(processedRequest) 호출. 미리 초기화 해둔 handlerMappings의 handlerMapping 구현 클래스를 우선 순위가 높은 순서대로 하나씩 꺼내와서 검사.
- RequestMappingHandlerMapping 클래스의 조회 전략 : @Controller 클래스 중, 메서드 레벨에 @RequestMapping이 붙어 있고, request의 URL 정보를 토대로 일치하는지 검사. 

(4-1) 일치하는 Handler가 있다면 해당 핸들러를 반환(컨트롤러). 없다면 noHandlerFound() 호출해 서블릿 예외 발생.

(5) 해당 핸들러를 this.getHandlerAdapter(mappedHandler.getHandler())를 통해서 해당 핸들러를 지원하는 HandlerAdapter가 있다면 해당 어댑터를 반환하여 HandlerAdapter ha에 할당. 지원하는 어댑터가 없는 경우 서블릿 예외 발생. 
- 메서드에 @RequestMapping 어노테이션을 사용하는 경우가 99% 일테니, HandlerAdapter 인터페이스의 구현체로 RequestMappingHandlerAdapter 가 사용될 것이다.

(6) HandlerAdapter.handle()을 수행하면 구현체 AbstractHandlerMethodAdapter 추상클래스의 handle()이 수행되어, 해당 추상클래스의 구현체인 RequestMappingHandlerAdapter.handleInternal() 메서드가 수행된다.
(6-1) RequestMappingHandlerAdapter.handleInternal() 내부에서 this.checkRequest(request)를 수행해 HttpServletRequest의 HTTP메서드 등을 검증.
검증 후 this.invokeHandlerMethod(request, response, handlerMethod) 수행.

(7) invokeHandlerMethod() 는 ServletInvocableHandlerMethod.setHandlerMethodArgumentResolvers() 를 호출하여 핸들러의 해당 메서드가 필요로 하는 매개변수 정보를 (@RequestBody인지, HttpEntity인지) ArgumentResolver에 제공하고, 각각에 특화된 Http메세지 컨버터를 사용해서 필요한 객체를 생성 

(8) HandlerAdapter(RequestMappingHandlerAdapter)가 invokeHandlerMethod() 메서드 내부에서 ServletInvocableHandlerMethod 클래스의  invokeAndHandle 메서드를 수행. `invocableMethod.invokeAndHandle(webRequest, mavContainer, new Object[0]);`

(9) ServletInvocableHandlerMethod.invokeAndHandle() 내부에서 `Object returnValue = this.invokeForRequest(webRequest, mavContainer, providedArgs);` 를 수행해 컨트롤러 로직 수행 결과 값을 반환받음. 

(10) 반환 받은 결과 값으로 
HandlerMethodReturnValueHandlerComposite.handleReturnValue() 를 수행

(11) 컨트롤러 메서의 응답 타입에 따라 HandlerMethodReturnValueHandler의 구현체 중 하나가 동작. 

(12-1) View를 응답하는 경우
ReturnValueHandler는 반환 값의 타입에 따라 적절한 HttpMessengerConverter를 통해 응답 메세지를 생성 후, ViewResolver가 동작하는데 이는 View 객체를 생성하고 DispatcherServlet가 이를 이용하여 render()하고 최종적으로 클라이언트에게 반환. 


(12-2) String, Object, HttpEntity 와 같은 데이터를 응답할 경우

HttpEntity를 응답한다는 가정하에, HttpEntityMethodProcessor.handleReturnValue() 가 수행된다.

RetrunValueHandler가 HttpMessengerConverter를 이용해서 반환값을 응답 메세지 바디부에 실어서 ViewResolver등을 거치지 않고 즉시 요청 송신자에 반환

참고 : https://jerry92k.tistory.com/70

===

안녕하세요 !!

[1] 요청 시에 @RequestBody와 HttpEntity를 안 쓰는 경우 , 응답 시에 @ResponseBody와 HttpEntity를 안 쓰는 경우

[2] 요청 시에 @RequestBody와 HttpEntity를 쓰는 경우 , 응답 시에 @ResponseBody와 HttpEntity를 쓰는 경우


[1]

요청 : 파라메터 타입에 @RequestBody X 이거나 HttpEntity X의 경우
응답 : 반환 값에 @ResponseBody X 이거나 HttpEntity X의 경우

1. 클라이언트의 요청

2. DispatcherServlet를 호출(urlPatterns = /* 경로이기 때문)

3. HandlerMapping의 가장 우선순위에 있는 구현체인 RequestMappingHandlerMapping을 통해 @Controller이 붙은 클래스의 객체를 매핑 정보로 활용

4. doDispatch()의 getHandler()에서 해당 매핑 정보의 컨트롤러 객체 반환

5. doDispatch()의 getHandlerAdapter()을 통해 HandlerAdapter을 호출 후 컨트롤러를 처리할 수 있는 어댑터 있는지 검증(supports()) 후 어댑터 호출(handle())

6. 이때 컨트롤러는 @Controller의 @RequestMapping이 붙어있으므로 HandlerAdapter의 가장 우선순위의 RequestMappingHandlerAdapter 어댑터 구현체가 호출되는 것!

7. 어댑터는 컨트롤러의 파라메터에 해당되는 객체가 ArgumentResolver의 구현체로 존재하는지 검증(supportsParameter()) 후 존재하면 생성(resolveArgument())

8. 어댑터는 생성된 객체를 컨트롤러의 파라메터에 주입하면서 컨트롤러 호출

9. 컨트롤러는 로직 수행 후 return 값(객체)을 어댑터에 반환

10. 어댑터는 ResultValueHandler 호출하면서 반환값에 해당되는 객체가 존재하는지 검증(supportsReturnType()) 후 존재하면 생성(handleReturnValue())

11. 어댑터는 생성된 객체 및 논리적 뷰 이름으로 초기화된 ModelAndView 객체 생성 후 DispatchServlet에 반환

12. DispatchServlet에서 ViewResolver에 논리적 뷰 이름을 넘겨주면서 호출

13. ViewResolver에선 논리적 뷰 이름을 물리적 뷰 경로로 바꿔주고 그 값으로 초기화된 View 객체 반환

14. DispatchServlet에서 View객체 이용해서 render() 호출

15. JSP 뷰 템플릿이었으면 render()에서 JSP 코드로 포워드 후 랜더링하고 나머지 타임리프 같은 뷰 템플릿이면 render() 받자마자 바로 화면 랜더링


[2]

요청 : 파라메터 타입에 @RequestBody O 이거나 HttpEntity O의 경우
응답 : 반환 값에 @ResponseBody O 이거나 HttpEntity O의 경우

1. 클라이언트의 요청

2. DispatcherServlet를 호출(urlPatterns = /* 경로이기 때문)

3. HandlerMapping의 가장 우선순위에 있는 구현체인 RequestMappingHandlerMapping을 통해 @Controller이 붙은 클래스의 객체를 매핑 정보로 활용

4. doDispatch()의 getHandler()에서 해당 매핑 정보의 컨트롤러 객체 반환

5. doDispatch()의 getHandlerAdapter()을 통해 HandlerAdapter을 호출 후 컨트롤러를 처리할 수 있는 어댑터 있는지 검증(supports()) 후 어댑터 호출(handle())

6. 이때 컨트롤러는 @Controller의 @RequestMapping이 붙어있으므로 HandlerAdapter의 가장 우선순위의 RequestMappingHandlerAdapter 어댑터 구현체가 호출되는 것!

7. 어댑터는 컨트롤러의 파라메터에 해당되는 타입의 객체가 ArgumentResolver의 구현체로 존재하는지 검증(supportsParameter())

8. ArgumentResovler이 검증하던 중 컨트롤러의 파라메터 타입이 @RequestBody 혹은 HttpEntity임을 감지하고 RequestResponseBodyMethodProcessor 구현체가 동작하며 HTTP 메시지 컨버터 호출

9. RequestResponseBodyMethodProcessor 구현체는 HTTP 메시지 컨버터의 canRead()를 통하여 파라메터의 클래스 타입과 미디어 타입(Content-Type)을 검증하고 조건 만족하면 read()를 통해 HTTP 메세지 바디에 있는 데이터 변환

10. RequestResponseBodyMethodProcessor 구현체는 변환된 객체를 어댑터에 반환

11. 어댑터에선 반환된 객체를 컨트롤러의 파라메터에 주입하면서 컨트롤러 호출

12. 컨트롤러는 로직 수행 후 return 값(객체)을 어댑터에 반환

13. 어댑터는 ResultValueHandler 호출하면서 해당 반환값이 존재하는지 검증(supportsReturnType())

14. 이때 ResultValueHandler에선 컨트롤러의 반환값이 @ResponseBody , HttpEntity임을 감지 후 RequestResponseBodyMethodProcessor 구현체가 동작하며 HTTP 메시지 컨버터를 호출

15. RequestResponseBodyMethodProcessor구현체는 HTTP 메시지 컨버터의 canWrite()를 통하여 파라메터 클래스 타입과 HTTP 요청 메시지에서의 미디어 타입(Accept)을 검증하고 조건 만족하면 write() 수행

16. AbstractHttpMesageConverter.write()에서 각 컨버터의 wirte를 호출하여 데이터를 변환하여 HttpResponse에 스트림으로 응답 데이터를 전달