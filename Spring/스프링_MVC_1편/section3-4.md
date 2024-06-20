> ※ 면접 대비 및 학습의 목적으로 질문과 답변을 작성했습니다. 따라서 키워드 중심의 간결한 문체를 사용했음을 양해바랍니다. 잘못된 내용 또는 보강이 필요한 부분을 말씀해주시면 정말 감사드리겠습니다. 답변의 출처는 대게 인프런 김영한 강사님의 '스프링 MVC 1편'이며, 검색에 기반한 답변은 가급적 출처를 달았습니다.

---
<br />

## 질문) 템플릿 엔진이 등장한 배경은 무엇인가요?

<details>
    <summary>답변</summary>
</br>

- 서블릿 덕분에 동적으로 원하는 HTML을 마음껏 만들 수 있으나, 매우 복잡하고 비효율적.
- HTML 문서에 동적으로 변경해야 하는 부분만 자바 코드를 넣을 수 있다면 더 편리.
- 종류 : JSP, Thymeleaf, Mustache, Freemarker, Velocity 등

</details>

### 꼬리질문) 가장 선호하는 템플릿 엔진이 있다면 무엇이고 왜 선호하는가?

<details>
    <summary>답변</summary>
</br>

사용 경험 有 : JSP, Mustache, Thymleaf

</details>

---
<br />

## 질문) MVC 패턴이란 무엇인가요?

<details>
    <summary>답변</summary>
</br>

웹 애플리케이션 개발 방법론으로, Model / View / Controller 세 가지 요소가 각 기능에 특화하여 역할을 분리하여 개발하는 디자인패턴이다.

등장배경
- 서블렛 또는 JSP 만으로 비즈니스 로직과 View 렌더링 까지 모두 처리하면
    1. 너무 많은 역할로 인해 유지보수 측면에서 비효율적.
    2. 비즈니스/뷰렌더링 두 측면은 변경의 라이프 사이클이 다름
- 특정 업무(기능)에 특화하여 역할을 분리하기 위해  MVC 패턴이 등장

구성
- Model : 뷰에 출력할 데이터을 담음. 
- View : 모델의 데이터를 사용해 화면을 그리는 일에 집중. 즉, HTML을 생성.
- Controller : HTTP 요청을 받아서 파라미터를 검증. 비즈니스 로직 호출. 모델에 데이터 적재.

|![image](https://github.com/proHyundo/backend-cs-study/assets/128882585/98f0cd5f-0b86-4dd1-93ee-1c0706b86227)|![image](https://github.com/proHyundo/backend-cs-study/assets/128882585/6e7e3ac5-dd1d-473e-9ebd-e40ce9e4ef20)|![image](https://github.com/proHyundo/backend-cs-study/assets/128882585/01d161c1-5040-40f6-8724-3e4675e8389e)|
|--|--|--|

</details>

### 꼬리질문) `WEB-INF` 디렉터리의 특징을 알고 계신가요?

<details>
    <summary>답변</summary>
</br>

- `WEB-INF` 디렉터리는 외부에서 직접 호출하여 제공받을 수 없다.
- RequestDispatcher의 include() 또는 forward() 메서드를 사용해 응답하는 것이 일반적이다.

</details>

### 꼬리질문) RequestDispatcher의 `redirect()` 와 `forward()` 메서드의 차이점은 무엇인가요?

<details>
    <summary>답변</summary>
</br>

redirect()
- 실제 클라이언트(웹 브라우저)에 응답이 나갔다가, 클라이언트가 redirect 경로로 다시 요청
- 따라서 클라이언트가 인지할 수 있고, URL 경로도 실제로 변경

forward()
- 서버 내부에서 일어나는 호출이기 때문에 클라이언트가 전혀 인지하지 못한다.

</details>

---
<br />

## 질문) FrontController 패턴이란 무엇인가요?

<details>
    <summary>답변</summary>
</br>

![image](https://github.com/proHyundo/backend-cs-study/assets/128882585/01d161c1-5040-40f6-8724-3e4675e8389e)

위 구조의 문제점
- Controller에 중복된 코드(forward, viewpath)가 존재한다.
- Controller에 response 객체가 사용되고 있지 않다.
- 로그 출력 등 **공통처리가 어렵다.**
    - 유틸리티 메서드로 분리하여도 항상 호출해야 하기 때문에 중복이 발생.
    

FrontController 패턴
- 서블릿 하나를 도입해 공통로직을 처리하도록 한다.(수문장 역할)
- 해당 서블릿 하나로 클라이언트의 요청을 받고, 요청에 맞는 컨트롤러가 호출되도록 한다. (입구를 하나로) 즉, **`진입점이 하나로`** 모인다.
- 나머지 컨트롤러는 서블릿으로 생성하지 않아도 된다.

스프링 MVC의 핵심도 FrontController 패턴이다. DispatcherServelt이 FrontController 패턴을 구현하고 있다.

참고링크
- https://velog.io/@neity16/6-스프링-MVC-4-프론트-컨트롤러FrontController-패턴-V1-V2-V3
- https://jeong-pro.tistory.com/96

</details>

### 꼬리질문1) DispatcherServelt 에 대해 대해 더 자세히 설명해주세요.

<details>
    <summary>답변</summary>
</br>

![DispatcherServlet-diagram](https://github.com/proHyundo/backend-cs-study/assets/128882585/58b11bbe-c913-4ebd-839b-0db413865ea7)

0. SpringBoot는 DispatcherServlet을 서블릿으로 자동 등록. 루트 경로 `/` 에 대해 매핑
1. Client HTTP 요청 발생
2. 서블릿이 호출되면, HttpServelt의 service메서드를 오버라이딩한 FrameworkServlet의 service 메서드를 시작으로 DispatcherServlet의 doDispatch() 메서드가 수행됨

</details>

---
<br />

## 질문) Adapter 패턴과 적용 사례에 대해 말씀해주세요.

<details>
    <summary>답변</summary>
</br>

A와 C타입이 서로 같지 않아 서로를 직접호출 할 수 없을 때 중간에 B 타입(Adapter)을 통해 호출한다.

어댑터 패턴의 간단한 예제를 들어볼게요. 상황은 이렇습니다: 이미 작성된 코드가 있고, 이 코드에서 사용하는 인터페이스와 새로운 시스템에서 사용하는 인터페이스가 서로 다릅니다. 어댑터 패턴을 이용해 기존 코드를 수정하지 않고 새로운 시스템에 적용해봅시다.

예를 들어, 다음과 같은 두 가지 인터페이스가 있다고 가정해봅시다.
```java
// 기존 인터페이스
interface OldInterface {
    void oldMethod();
}

// 새로운 인터페이스
interface NewInterface {
    void newMethod();
}
```
기존 시스템에서는 OldInterface를 사용한 클래스가 있습니다.
```java
class OldSystemClass implements OldInterface {
    @Override
    public void oldMethod() {
        System.out.println("Old method executed.");
    }
}
```
이제 새로운 시스템에서는 NewInterface를 사용해야 합니다. 이때 어댑터 패턴을 사용하여 기존의 OldSystemClass를 새로운 시스템에 적용할 수 있습니다. 어댑터 클래스를 다음과 같이 작성해봅시다.
```java
class Adapter implements NewInterface {
    private OldInterface oldInterface;

    public Adapter(OldInterface oldInterface) {
        this.oldInterface = oldInterface;
    }

    @Override
    public void newMethod() {
        oldInterface.oldMethod();
    }
}
```
이제 새로운 시스템에서 어댑터를 사용하여 기존의 OldSystemClass를 활용할 수 있습니다.
```java
public class Main {
    public static void main(String[] args) {
        OldSystemClass oldSystemClass = new OldSystemClass();
        NewInterface adapted = new Adapter(oldSystemClass);
        adapted.newMethod(); // 출력: Old method executed.
    }
}
```
위 예제에서 볼 수 있듯이, 어댑터 패턴을 사용하면 기존 코드를 수정하지 않고도 새로운 시스템에 적용할 수 있습니다. 이를 통해 코드 재사용성과 유지 보수성을 향상시킬 수 있습니다.

 

추가로 스프링 MVC에서 어댑터 패턴은 다양한 컨트롤러 구현체를 동일한 인터페이스로 처리할 수 있도록 돕는 역할을 합니다. 어댑터 패턴은 기존 클래스의 인터페이스를 변경하지 않고도 그 클래스와 다른 인터페이스를 가진 클래스와 협업할 수 있게 해주는 구조 패턴입니다. 스프링 MVC에서는 이를 통해 다양한 종류의 컨트롤러를 하나의 인터페이스로 통합하여 관리할 수 있습니다.

실무에서 어댑터 패턴은 다음과 같은 상황에서 유용하게 활용됩니다:

라이브러리나 외부 시스템과의 호환성: 외부 라이브러리나 시스템과 연동할 때 해당 라이브러리나 시스템이 제공하는 인터페이스와 호환되지 않는 경우가 있습니다. 이때 어댑터 패턴을 사용하여 다양한 인터페이스를 통합하여 사용할 수 있습니다.
기존 코드 재사용: 기존에 작성된 코드가 있고, 이를 새로운 시스템에서 사용하고자 할 때 인터페이스가 다른 경우가 발생할 수 있습니다. 어댑터 패턴을 사용하여 기존 코드를 수정하지 않고도 새로운 시스템에 적용할 수 있습니다.
기능 확장: 기능 확장이 필요한 경우, 어댑터 패턴을 통해 추가적인 기능을 구현할 수 있습니다. 즉, 기존 코드를 수정하지 않고 새로운 기능을 추가할 수 있게 도와줍니다.
스프링 MVC에서 어댑터 패턴의 대표적인 예는 HandlerAdapter 인터페이스입니다. 이 인터페이스를 구현하는 클래스들은 서로 다른 종류의 컨트롤러를 처리할 수 있게 해주며, 어플리케이션에 등록된 여러 가지 HandlerAdapter들 중 적절한 것을 선택하여 요청을 처리할 수 있습니다. 이를 통해 프레임워크의 유연성이 높아지며, 다양한 컨트롤러 구현체를 효율적으로 관리할 수 있습니다.

인터페이스로 딱 맞게 떨어지면 어댑터가 필요하지 않는데, 인터페이스로 딱 맞아 떨어지지 않을 때 어댑터 패턴으로 끼워 맞출 수 있습니다.

예를 들어서 외부 라이브러리를 사용하는데, 해당 라이브러리가 우리쪽 기준과 맞지 않을 때 어댑터를 통해서 우리쪽 기준에 맞도록 맞출 수 있습니다.

중간에 어댑터가 복잡한 역할을 대신 해주는 것이지요.

쉽게 설명드리자면 인터페이스가 220V에 맞는 전원 코드라고 한다면 서로 인터페이스를 맞추기 때문에 국내에서는 아무런 문제 없이 전기를 사용할 수 있습니다. 그런데 해외 제품을 구매하거나 해외에 나가면 110V 제품이 존재하는데요. 이 경우에는 중간에 전원 어댑터(돼지코 등이 떠오르네요)를 사용해서 문제를 해결할 수 있습니다.

이런 패턴은 정해진 용도가 딱 있다기 보다는 실무에서는 이렇게 잘 맞지 않은 경우에 어댑터를 사용해서 문제를 해결할 수 있습니다.

답변 출처 : https://www.inflearn.com/questions/847008

</details>

---
<br />