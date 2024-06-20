> ※ 면접 대비 겸 학습의 목적으로 질문과 답변을 작성했습니다. 따라서 키워드 중심의 간결한 문체를 사용했음을 양해바랍니다. 잘못된 내용 또는 보강이 필요한 부분을 말씀해주시면 정말 감사드리겠습니다. 답변의 출처는 대게 인프런 김영한 강사님의 '스프링 MVC 2편'이며, 검색에 기반한 답변은 가급적 출처를 달았습니다.

---
<br />

## 질문) 스프링에서 제공하는 BindingResult 에 대해 설명

<details>
    <summary>답변</summary>
</br>

HTTP 요청이 정상인지 검증하는 것.
클라이언트 검증은 신뢰할 수 없음. 조작 가능하기 때문. 

</details>

BindingResult

- 스프링이 제공하는 검증 오류를 보관하는 객체
- BindingResult` 가 있으면 `@ModelAttribute` 에 데이터 바인딩 시 오류가 발생해도 컨트롤러가 호출된다!
    - `BindingResult` 가 없으면 400 오류가 발생하면서 컨트롤러가 호출되지 않고, 오류 페이지로 이동한다.

BindingResult에 오류가 적용되는 방법 3가지
1. `@ModelAttribute` 의 객체에 타입 오류 등으로 바인딩이 실패하는 경우 스프링이 `FieldError` 생성해서 `BindingResult` 에 넣어준다.
2. 개발자가 직접 넣어준다.
    ```java
    bindingResult.addError(new FieldError("item", "itemName", "ControllerV2.addItemV1 상품 이름은 필수 입니다."));
    ```
3. `Validator` 사용

BindingResult 주의사항
- 파라미터 매개변수의 순서가 검증 대상 바로 다음에 위치해야 한다.

### 꼬리질문)

`MessageCodesResolver`를 통해 오류 메시지를 계층적으로 관리할 수 있다.
오류 메시지는 포괄적 범위의 일반적인 메시지를 먼저 만들고, 구체적인 메시지를 나중에 만드는 것을 권장한다.

1. errors.properties 파일 생성
2. application 설정파일 설정 추가
```properties
spring.messages.basename=errors
```

DefaultMessageCodesResolver의 기본 메시지 생성 규칙

**객체 오류**
```
객체 오류의 경우 다음 순서로 2가지 생성
1.: code + "." + object name
2.: code
예) 오류 코드: required, object name: item
1.: required.item
2.: required
```

**필드 오류**
```
필드 오류의 경우 다음 순서로 4가지 메시지 코드 생성
1.: code + "." + object name + "." + field
2.: code + "." + field
3.: code + "." + field type
4.: code
예) 오류 코드: typeMismatch, object name "user", field "age", field type: int
1. "typeMismatch.user.age"
2. "typeMismatch.age"
3. "typeMismatch.int"
4. "typeMismatch"
```


---
<br />

## 질문) Bean Validation 에 대해 설명해주세요

<details>
    <summary>답변</summary>
</br>

- Bean Validation은 자바 기술 표준. 따라서 검증 어노테이션과 인터페이스의 모음.
- 대표적인 구현체로 하이버네이트 Validator 가 있음.

![image](https://github.com/proHyundo/projects-for-study/assets/128882585/88566b5a-e1bd-4425-b1a3-fb33dfb2da69)
- Bean Validation에서 제공하는 어노테이션들을 위와 같이 찾아볼 수 있다.

```gradle
implementation 'org.springframework.boot:spring-boot-starter-validation'
```
스프링 부트에서 위 의존성을 주입하면, hibernate validator 를 사용한다.
</details>