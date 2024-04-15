```
---
</br>

### 질문)

<details>
    <summary>답변</summary>

-

</details>
```

---
</br>

### 질문) Optional에 대해 설명해주세요

<details>
    <summary>답변</summary>

- 선택형 반환값을 지원하기 위한 용도이다. (모던자바인액션 377)
- 객체의 일부 또는 전체가 null이 될 수 있는 상황에서 사용.

</details>

#### 꼬리질문) 모든 Null 참조를 Optional로 감싸는 것이 좋을까?

<details>
    <summary>답변</summary>

- No, 값이 있을 수도 없을 수도 있는 선택형 값에서 Optional을 적용하는 것이 의미상 전달이 된다.
참고 : 모던자바인액션 370p

</details>

#### 꼬리질문) Optional의 map() flatMap() 메서드는 어떤 역할을 하나요?

<details>
    <summary>답변</summary>

- 호출 체인 메서드
- map : map 메서드는 Optional 객체가 값을 포함하고 있다면, 그 값을 주어진 함수를 적용하여 변환합니다. 그리고 변환된 값을 다시 Optional로 감싸서 반환합니다. 만약 원래 Optional 객체가 비어있다면, 변환을 시도하지 않고 빈 Optional을 반환합니다.
- flatMap : Optional 이 Person을 감싸고 있다면 flatMap에 전달된 Function이 Person에 적용된다.
flatMap 메서드는 map 메서드와 유사하지만, 주요 차이점은 변환 함수가 직접 Optional 객체를 반환한다는 것입니다. flatMap은 반환된 Optional에서 값을 꺼내어 "평탄화"하고, 최종적으로 하나의 Optional 객체로 만듭니다. 이는 중첩된 Optional 구조를 단일 Optional로 간단하게 만들 때 유용합니다.
- 참고 서적 : 모던자바인액션 376
- 참고 링크 : https://velog.io/@hksdpr/JAVA-Optional%EC%9D%98-%EC%B6%A9%EA%B2%A9%EC%A0%81%EC%9D%B8-%EC%82%AC%EC%9A%A9%EB%B2%95-map%EC%9D%84-%EC%9D%B4%EC%9A%A9%ED%95%9C-%EC%B2%B4%EC%9D%B4%EB%8B%9D#map-%ED%95%A8%EC%88%98

</details>

#### 꼬리질문) 다음 코드의 동작을 설명해주세요.

```Java
public Optional<Insurance> nullSafeFindCheapestInsurance(
    Optional<Person> person, Optional<Car> car){
        return person.flatMap(p->car.map(c->findCheapestInsurance(p,c)));
}
```

<details>
    <summary>답변</summary>

- flatMap() : `Optional<person>`이 비어있다면 표현식 실행되지 않고 빈 Optional반환. 비어있지 않다면 `Function` 인자로 Optional 벗긴 person을 전달한다.
- map() : `Optional<car>`이 비어있다면 표현식 실행되지 않고 빈 Optional반환. 비어있지 않다면 findCheapestInsurance 메서드에 Optional 벗긴 person, car 전달
- 
person.flatMap(...): person이 Optional<Person> 객체를 포함하고 있다면, 이 flatMap 호출은 Person 객체 p를 사용하여 내부 람다 표현식을 실행합니다. person이 비어있다면, 이 메서드는 바로 빈 Optional<Insurance>를 반환합니다.
car.map(c -> findCheapestInsurance(p, c)): car가 값 Car 객체를 포함하고 있다면 이 map 호출은 Car 객체 c와 함께 findCheapestInsurance(p, c) 메서드를 실행하여 Insurance 객체를 찾습니다. 그리고 이 Insurance 객체는 Optional로 감싸져 반환됩니다. car가 비어있다면, map은 빈 Optional을 반환합니다.
결과적으로 person.flatMap은 car.map이 반환하는 Optional<Insurance>를 "평탄화"하여, 결국 하나의 Optional<Insurance> 객체를 반환합니다. 이는 person과 car가 모두 존재할 때만 Insurance 객체를 포함하고, 그렇지 않은 경우에는 빈 Optional을 반환합니다.
</details>


---
</br>

### 질문) DSL을 왜 사용하나요. dsl의 예는 뭐가 있을까요?

<details>
    <summary>답변</summary>

- DSL 은 도메인 전용 언어의 약자로, 특정 비즈니스(도메인)에 특화된 인터페이스로 만든 API를 뜻합니다.
- 저수준 레벨의 언어가 아닌 고수준 언어로 추상화 되어 있으며, 이를 통해 가독성과 유지보수에 이점을 얻을 수 있다.
- DSL의 종류로는 내부 DSL, 외부 DSL, 다중 DSL으로 구분.

</details>

#### 꼬리질문1) DSL을 설계할 때 고려해야할 주요 사항은 무엇인가요?

---
</br>

### 질문) 인터페이스에 새로운 기능을 추가할 때 어떤 문제가 발생할 수 있을까요

<details>
    <summary>답변</summary>

- 인터페이스에 메서드를 추가하는 등 수정 하면, 인터페이스를 구현하는 모든 클래스의 구현을 수정해야 한다.
- default 메서드를 사용하면 호환성을 유지하면서 인터페이스를 수정할 수 있다.

</details>

#### 꼬리질문) 인터페이스에 새로운 메서드를 추가해도 바이너리 호환성은 유지되는데, 바이너리 호환성이란 무엇인지 설명해주세요

<details>
    <summary>답변</summary>

- 새로 추가된 메서드를 호출하지 않으면 새로운 메서드 구현 없이도 기존 클래스 파일 구현이 동작하는 것.
- 모든 환경에서 자동 재컴파일이 지원되는 것은 아니다.
- 참고 링크 : https://velog.io/@kms8571/JAVA-%EB%B0%94%EC%9D%B4%EB%84%88%EB%A6%AC-%ED%98%B8%ED%99%98%EC%84%B1-%EA%B4%80%EB%A0%A8-%EC%9D%B4%EC%8A%88

</details>

#### 꼬리질문) default 메서드가 사용된 예시를 하나 말씀해주세요

<details>
    <summary>답변</summary>

- List 인터페이스에 sort() default 메서드가 추가됨.
- Collection 인터페이스에 stream() default 메서드가 추가됨.

</details>

---
</br>

### 질문) 자바 모듈 시스템에 대해 설명해주세요

<details>
    <summary>답변</summary>

- 자바 모듈 시스템은 자바 9 버전에서 도입된 새로운 프로그래밍 기능으로, 이를 통해 개발자들은 큰 애플리케이션을 더 작은, 관리 가능한 부분들로 분리할 수 있게 되었습니다. 이 시스템의 주요 목표는 코드의 재사용성을 증가시키고, 유지보수를 용이하게 하며, 소프트웨어 아키텍처를 개선하는 것입니다.

모듈은 관련된 패키지, 리소스, 그리고 이들 사이의 의존성을 포함하는 자바 컴포넌트입니다. 각 모듈은 'module-info.java'라는 특별한 파일을 통해 정의되며, 이 파일은 해당 모듈이 어떤 다른 모듈에 의존하는지, 그리고 자신의 어떤 부분을 다른 모듈에게 공개하는지를 명시합니다.

자바 모듈 시스템의 핵심 요소는 다음과 같습니다:

'requires': 이 키워드는 해당 모듈이 다른 모듈에 의존한다는 것을 나타냅니다. 이를 통해 모듈 간의 의존성이 명확하게 정의됩니다.
'exports': 이 키워드는 해당 모듈에서 다른 모듈에게 특정 패키지를 공개하겠다는 것을 나타냅니다. 이를 통해 모듈의 노출 범위를 제한하고, 캡슐화를 강화할 수 있습니다.
'uses': 이 키워드는 해당 모듈이 특정 서비스를 사용하겠다는 것을 나타냅니다.
'provides': 이 키워드는 해당 모듈에서 특정 서비스를 제공하겠다는 것을 나타냅니다.
이러한 기능들을 통해 자바 모듈 시스템은 높은 수준의 캡슐화와 의존성 관리를 제공하며, 이를 통해 개발자는 더욱 견고하고 관리하기 쉬운 코드를 작성할 수 있습니다.

</details>

---
</br>

### 질문) 스레드풀에 대해 설명해주세요

<details>
    <summary>답변</summary>

-

</details>