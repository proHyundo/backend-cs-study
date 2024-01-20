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
- Java8 부터 함수형 프로그래밍을 위해 메소드를 일급 객체로 취급, 함수를 새로운 값의 형식으로 추가.
- 즉, 메소드 참조와 람다 문법을 활용해 메소드/함수 그 자체를 값으로 매개할 수 있다.
- 메서드 참조와 람다 문법을 활용하여 메서드/함수 자체를 값으로 매개변수화할 수 있다.
- Stream 기능의 토대를 제공

</details>

#### 꼬리질문) 일급 시민(객체)란 무엇이고, 왜 중요한가요?

<details>
    <summary>답변</summary>

일급 객체

- 메소드 같은 구조체를 값으로 취급하여 다른 메소드로 자유롭게 전달하는 것
- 자바에서는 함수들이 일급 객체에 포함되지 않지만 코틀린,자바스크립트 등의 언어에서는 변수에 함수를 할당하고 사용할 수 있다.

장점
1. 비즈니스 로직과 데이터구조를 분리할 수 있다.
2. 컬렉션의 상태의 일관성을 보장할 수 있다.
3. 컬렉션의 상태와 행동을 한 시점에 관리할 수 있다.
4. 가독성이 높아진다.

</details>

#### 꼬리질문) 일급 객체가 되기 위한 조건은 무엇인가요?

<details>
    <summary>답변</summary>

일급 시민의 조건 3가지

1. 변수에 할당 할 수 있어야 한다.
2. 함수의 인자로 넘길 수 있어야 한다.
3. 함수의 리턴값으로 리턴 할수 있어야 한다.

참고 링크
- https://github.com/backend-deep-dive/modern-java-in-action/issues?q=is%3Aissue+is%3Aclosed
- https://medium.com/ryanjang-devnotes/who-is-first-class-citizen-in-programming-world-b92c67b32635

</details>

---
</br>

### 질문) 동작 파라미터화에 대해 설명해주세요

<details>
    <summary>답변</summary>

- 메소드의 인수로 코드블록을 전달하는 방법.
- 메소드가 다양한 동작 중 하나를 받아서 내부적으로 특정 동작을 수행.(전략 디자인 패턴)
- 실행 시점에 동작이 결정된다.

이점
- 변화하는 요구사항에 유연하게 대응하기 용이하다.
- 동작과 로직을 구분(분리)할 수 있다.

종류
1. 전략 디자인 패턴 / 다형성 / 구현체 인스턴스를 생성해서 주입
2. 익명클래스
3. 람다

</details>

#### 꼬리질문) 전략 디자인 패턴을 통해서도, 익명클래스를 통해서도 동작과 로직을 구분하고, 메소드를 값으로 전달받을 수 있음에도 사용되지 않는 이유는 무엇인가요?

<details>
    <summary>답변</summary>

- 전략디자인패턴 : 클래스를 구현해 인스턴스화 하는 과정이 필요
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

---
</br>


### 질문) Java8에 등장한 람다 표현식이란 무엇이고, 어떤 경우에 유용한가요?

<details>
    <summary>답변</summary>

- 메서드로 전달할 수 있는 익명 클래스를 단순화한 것
- 자바 8 이전에 불가능 했던 기능은 아니지만, 코드를 간결하게 다른 메소드에게 전달할 수 있다.

</details>

#### 꼬리질문) 람다표현식은 어떻게 구성되어 있나요?

<details>
    <summary>답변</summary>

- 파라미터, 화살표, 바디로 구분된다.
- 람다는 함수디스크립터를 통해 형식추론이 가능하기 때문에 파라미터의 타입은 생략 가능
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

1. 내부 동작 코드를 재사용하고 싶을 때
2. 반쪽짜리 클로저 지원(모던자바인액션 112~114p)
    - 클로저 : 클로저 함수 외부에 정의된 변수의 값에 접근하고, 그 값을 바꿀 수 있다.
    - 그러나 람다와 익명클래스는 정의된 메소드의 지역변수에 접근은 가능하지만 값은 바꿀 수 없다. final이거나 사실상 final 으로 취급되어야 한다.
3. 재귀 람다식 호출이 까다로움
    - 참고 링크 : https://dzone.com/articles/do-it-java-8-recursive-lambdas

</details>

#### 꼬리질문) 람다 표현식을 메소드 참조 형식으로 축약할 수 있는데, 메소드 참조의 유형 3가지를 말씀해주세요.

<details>
    <summary>답변</summary>

1. 정적 메소드 참조
2. 인스턴스 메소드 참조
    - 인스턴스 메소드 : 람다 표현식의 파라미터로 전달된 객체의 메소드
3. 기존 객체의 인스턴스 메소드 참조
    - 비공개 헬퍼 메소드 정의한 상황에서 활용 가능
    - this 키워드 또는 매개받은 인스턴스로 접근.
    - 비공개 헬퍼 메소드 : 클래스/인터페이스 private 으로 선언되어 내부에서만 사용되는 메소드

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

#### 꼬리질문) 종단연산의 종류 중 collect() 메소드가 존재하는데, 

---
</br>

### 질문) 스트림의 작동 방식을 설명해주세요. 주로 어떤 상황에서 쓰이나요?

<details>
    <summary>답변</summary>

- 컬렉션, 배열, I/O 등 으로 부터 제공된 소스로 스트림을 얻을 수 있다.
- 스트림의 중간 연산들과 중단 연산으로 파이프라인을 형성할 수 있다.

참고 링크
- [동작원리](https://velog.io/@kakdark/Stream)
- [[Java] 자바 스트림(Stream) API 내부 동작 알아보기](https://transferhwang.tistory.com/688)
- https://velog.io/@mangoo/Java-Stream-%EC%88%9C%EC%B0%A8-%EC%B2%98%EB%A6%AC-Part1-3zrmb5in

</details>

---
</br>

### 질문) 컬렉션이 이미 존재하는데 스트림을 사용하는 이유가 무엇인가요?

<details>
    <summary>답변</summary>

(모던 자바 인 액션 143p~)

**컬렉션 스트림 공통점**
- 연속된 값을 저장하는 자료구조의 인터페이스를 제공
- '연속된'이란, 순차적으로 값에 접근

**컬렉션 스트림 차이점**
1. 데이터를 언제 계산하는지
    - 최종연산이 수행되기 전 까지 중간연산을 실행하지 않는다. lazy 연산
2. 한 번만 탐색(소비)할 수 있다, 한 번 닫히면 재사용이 불가능.
3. 외부 반복을 사용하는 컬렉션과 달리 내부 반복(반복을 알아서 처리하고 결과 스트림 값을 어딘가에 저장해주는)을 사용
    - 손쉬운 병렬처리를 지원. &larr; ParallerStream()

**사용 이유**
- 외부 반복 대신 내부 반복을 지원함으로써 두 가지 이점을 가져올 수 있다.
1. 손쉬운 병렬처리를 지원
    - 외부 반복은 `synchronized`키워드 등을 사용해 병렬성을 스스로 관리해야 함.
2. 가독성이 좋음
    - 복잡한 데이터 처리 질의를 표현할 수 있도록 최적화된 연산을 미리 정의해줌.
    - 스트림이 고수준의 추상화를 제공하기 때문에, 스트림의 소스의 데이터 구조에 대한 세부 사항을 몰라도 최적화된 처리를 수행 가능.


</details>

---
</br>

### 질문) 병렬 스트림에 대해 설명해주세요

<details>
    <summary>답변</summary>

- 

</details>

#### 꼬리질문) 병렬 스트림은 내부적으로 어떻게 처리되나요?

<details>
    <summary>답변</summary>

- ForkJoinPool 을 사용한다. availableProcessors() 메소드에서 JVM에서 이용가능한 코어 개수를 반환받아 생성할 스레드의 개수를 결정한다.

</details>

#### 꼬리질문) ForkJoinPool에 대해서 조금 더 자세히 설명해주실 수 있나요? ForkJoinPool이란 무엇이며 ThreadPool과의 차이점은 무엇일까요?

<details>
    <summary>답변</summary>

- ForkJoinPool은 병렬 스트림(Parallel Stream)이 병렬 작업을 수행할 때 사용하는 스레드를 
- Task(작업)을 스레드에 분산 할당하는 방법은 여러가지가 존재한다.
- 병렬 스트림은 ExecutorService 인터페이스를 구현한 ForkJoinPool 클래스를 통해 

참고링크
- https://m.blog.naver.com/tmondev/220945933678
- https://hamait.tistory.com/612

</details>

#### 꼬리질문) 병렬 스트림을 사용할 땐 어떤 점에 주의하며 사용해야 할까요?

<details>
    <summary>답변</summary>

- 공유된 가변 상태를 피해야 한다.
- 병렬화가 공짜가 아니다
- 스트림을 청크로 분할할 수 있어야 한다.
- 박싱 언박싱 비용에 주의, 기본형 특화 스트림을 사용.
- 참고 : 모던자바인 액션 249 ~ 252    

</details>


---
</br>

### 질문) 옵저버 디자인 패턴에 대해 설명해주세요

<details>
    <summary>답변</summary>

- 어떤 이벤트가 발생하여 특정 객체(주체,Subject)가 변화하였을 때 이를 Observer들에게 알리는 패턴.
- 발행자와 구독자들의 관계에 비유할 수 있음.
- 상태가 변화하였을 때 Observer들이 어떤 행동을 취할지를 람다식으로 표현하여 옵저버를 명시적으로 인스턴스화 하지 않을 수 있다.

</details>

---
</br>

### 질문) 리팩터링, 테스팅, 디버깅에 대해 설명해주세요

<details>
    <summary>답변</summary>

테스트(참고 : 모던자바313~315)
- 람다 표현식은 함수형 인터페이스의 인스턴스를 생성한다
- 따라서, 인스턴스의 동작으로 람다표현식을 테스트할 수 있다.
- 복잡한 람다 표현식은 일반 메소드로 선언해 메서드 참조로 테스트 가능.

디버깅
- 람다 표현식에는 이름이 없다. 따라서 컴파일러가 람다를 참조하는 이름을 생성하기 때문에 `$` 와 같은 기호로 에러의 발생 지점을 가리킨다.
- peek() 메소드를 사용해 파이프라인 동작 전후의 중간값을 출력할 수 있다. 이 메소드는 스트림 요소를 소비하지 않는다.

</details>
