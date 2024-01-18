```
### 질문)

<details>
    <summary>답변</summary>

-

</details>
```

### 질문) 함수형 프로그래밍이란 무엇이고, Java에 어떤 영향을 미쳤나요?

<details>
    <summary>답변</summary>

- 함수형 프로그래밍이란, 메소드 그 자체를 값으로 취급하여 다른 메소드에게 전달하는 프로그래밍 기법
- Java8 부터 함수형 프로그래밍을 위해 메소드를 일급 객체로 취급, 새로운 값의 형식으로 추가.
- 즉, 메소드 참조와 람다 문법을 활용해 메소드/함수 그 자체를 값으로 매개할 수 있다.
- Stream 기능의 토대를 제공

</details>

#### 꼬리질문) 일급 객체란 무엇이고, 왜 중요한가요?

<details>
    <summary>답변</summary>

일급 객체

- 자바에서는 함수들이 일급 객체에 포함되지 않지만 코틀린,자바스크립트 등의 언어에서는 변수에 함수를 할당하고 사용할 수 있다.

</details>

#### 꼬리질문) 일급 객체가 되기 위한 조건은 무엇인가요?

<details>
    <summary>답변</summary>

일급 시민의 조건 3가지

1. 변수나 데이터에 할당 할 수 있어야 한다.
2. 객체의 인자로 넘길 수 있어야 한다.
3. 객체의 리턴값으로 리턴 할수 있어야 한다.

- 참고 링크 : https://github.com/backend-deep-dive/modern-java-in-action/issues?q=is%3Aissue+is%3Aclosed

</details>

---

</br>

### 질문) Java8에 등장한 람다 표현식이란 무엇이고, 어떤 경우에 유용한가요?

<details>
    <summary>답변</summary>

- 메서드로 전달할 수 있는 익명 함수를 단순화한 것
- 자바 8 이전에 불가능 했던 기능은 아니지만, 코드를 간결하게 다른 메소드에게 전달할 수 있다.

</details>

#### 꼬리질문) 동작파라미터를 통해서도, 익명클래스를 통해서도 동작과 로직을 구분하고, 메소드를 값으로 전달받을 수 있음에도 사용되지 않는 이유는 무엇인가요?

<details>
    <summary>답변</summary>

- 동작파라미터 : 클래스를 구현해 인스턴스화 하는 과정이 필요
- 예제코드

```java

public static List<Apple> filterApples(List<Apple> inventory, ApplePredicate p){
    List<Apple> result = new ArrayList<>();
    for(Apple apple : inventory){
        if(p.test(apple)){
            result.add(apple);
        }
    }
    return result;
}

public interface ApplePredicate {
    boolean test (Apple apple);
}

public class AppleRedAndHeavyPredicate implements ApplePredicate {
    public boolean test(Apple apple){
        return RED.equals(apple.getColor()) && apple.getWeight() > 150;
    }
}

public static void main(){
    List<Apple> readAndHeavyApples = filterApples(inventory, new AppleRedAndHeavyPredicate);
}
```

- 익명 클래스 : 클래스 선언과 인스턴스화를 동시에 할 수 있지만, 가독성이 떨어짐

</details>

#### 꼬리질문) 람다표현식은 어떻게 구성되어 있나요?

<details>
    <summary>답변</summary>

- 파라미터, 화살표, 바디로 구분된다.
- 파라미터의 타입은 생략 가능
- 람다 표현식에 한 문장만 존재할 경우 중괄호, return 문 생략 가능

</details>

#### 꼬리질문) 자바가 제공하는 함수형 인터페이스는 예외를 던질 수 없습니다. 람다 표현식에서 예외를 던지려면 어떻게 해야하나요?

<details>
    <summary>답변</summary>

- checked exception을 함수형 인터페이스에서 던질 수 없다.
1. 확인된 예외를 선언하는 함수형 인터페이스를 직접 정의
2. 람다를 tyr catch 블록으로 surround 하기

</details>

#### 꼬리질문) 구문(statement)과 표현식(expression)의 차이점에 대해 알고 계신가요?

<details>
    <summary>답변</summary>

- Expression : 하나 이상의 값으로 표현될 수 있는 코드, 즉 평가 가능
  - 산술식, 객체 할당, 함수 호출
- Statement : 실행 가능한 최소의 독립적 코드, 동작을 기술
  - 변수 선언, 할당, 조건문, 반복문
- 개념적 집합 관계가 아닌 구성 가능 관계, 즉 문장 속에 식이 존재할 수 있음을 의미

</details>

#### 꼬리질문) 어떤 상황에서 람다 표현식의 단점이 나타날까요?

<details>
    <summary>답변</summary>

- 내부 동작 코드를 재사용하고 싶을 때
- 반쪽짜리 클로저 지원(모던자바인액션 112~114p)
- 재귀 람다식 호출이 까다로움

</details>

#### 꼬리질문) 람다 표현식을 메소드 참조 형식으로 축약할 수 있는데, 메소드 참조의 유형 3가지를 말씀해주세요.

<details>
    <summary>답변</summary>

1. 정적 메소드 참조
2. 인스턴스 메소드 참조
3. 기존 객체의 인스턴스 메소드 참조
    - 비공개 헬퍼 메소드 정의한 상황에서 활용 가능
    - this 키워드 또는 매개받은 인스턴스로 접근.
    - 비공개 헬퍼 메소드 : 클래스/인터페이스 내부에서만 사용되는 메소드

</details>

---

</br>

### 질문) Java 8에서 추상화와 관련된 변화 요소를 하나 설명해주세요.

<details>
    <summary>답변</summary>

- 인터페이스에 추상메소드 외에 default 메소드와 static 메소드가 추가되었습니다.
- Collection 하위의 자료구조를 Stream으로 변환하기 위해 추상메소드를 추가하면, 하위의 모든 구현체를 수정해야 하는 문제를 해결하기 위해 등장.

참고 링크 : https://cloudstudying.kr/questions/72

</details>

---
</br>

### 질문) 스트림이란 무엇인지 설명해주세요

<details>
    <summary>답변</summary>

- 컬렉션 데이터를 처리할 수 있는 방법
- 다양한 연산을 SQL과 같이 선언형 질의로 표현하여 직관적이고 간결하게 표현 가능. 이를 통해 코드의 가독성 향상.
- 내부 동작의 구현을 신경쓰지 않고 데이터를 처리 가능.

</details>

#### 꼬리질문) 중간 연산과 종단 연산에 대해 설명해주세요

<details>
    <summary>답변</summary>

중간 연산
- 연결할 수 있는 스트림
- 다른 스트림을 반환
- 단말 연산을 실행해야 연산 수행 &rarr; lazy
- E.g.) filter, map, sorted, distinct, limit

최종(단말) 연산
- 스트림을 닫는 연산
- 스트림 이외의 결과가 반환
- E.g.) collect, count, forEach

</details>

#### 꼬리질문) Stream의 중간 연산 중 map과 flatMap 메소드의 차이점에 대해 설명해주실 수 있나요?

<details>
    <summary>답변</summary>

map
- 스트림 각 요소에 적용할 함수를 map 메소드 인수로 제공
- 각 요소를 다른 요소로 변환(매핑)하거나 정보를 추출하는데 사용

flatMap
- 하나의 평면화된 스트림을 반환
- 스트림의 각 값을 다른 스트림으로 만들어 모든 스트림을 하나의 스트림으로 연결

</details>

#### 꼬리질문) 

---
</br>

### 질문) 컬렉션이 이미 존재하는데 스트림을 사용하는 이유가 무엇인가요?

<details>
    <summary>답변</summary>

(모던 자바 인 액션 143p~)

컬렉션 스트림 공통점
- 연속된 값을 저장하는 자료구조의 인터페이스를 제공
- '연속된'이란, 순차적으로 값에 접근

차이점
- 데이터를 언제 계산하는지
- 한 번만 탐색(소비)할 수 있다
- 외부 반복을 사용하는 컬렉션과 달리 내부 반복을 사용

</details>

