> ※ 면접 대비 겸 학습의 목적으로 질문과 답변을 작성했습니다. 따라서 키워드 중심의 간결한 문체를 사용했음을 양해바랍니다. 잘못된 내용 또는 보강이 필요한 부분을 말씀해주시면 정말 감사드리겠습니다. 답변의 출처는 대게 인프런 김영한 강사님의 '스프링 MVC 1편'이며, 검색에 기반한 답변은 가급적 출처를 달았습니다.

---
<br />

## 질문) 템플릿 엔진이 등장한 배경은 무엇인가?

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

## 질문) FrontController 패턴

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

</details>

참고링크
- https://velog.io/@neity16/6-스프링-MVC-4-프론트-컨트롤러FrontController-패턴-V1-V2-V3
- https://jeong-pro.tistory.com/96

### Adapter 패턴이란 무엇인가요? Spring MVC 를 예로 들어 설명해주세요.

A와 C타입이 서로 같지 않아 서로를 직접호출 할 수 없을 때 중간에 B 타입(Adapter)을 통해 호출한다.