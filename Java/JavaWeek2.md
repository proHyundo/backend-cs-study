```
### 질문) 

<details>
    <summary>답변</summary>

- 

</details>
```


### 질문) Java의 실행과정에 대해 설명해주세요

<details>
    <summary>답변</summary>

1. 소스파일 작성 (.java)
2. 컴파일러(javac.exe)로 소스파일을 컴파일하여 바이트코드(.class) 파일 생성
3. JVM 구동 명령어(java.exe)로 바이트코드를 운영체제에 맞게 기계어로 번역

</details>

#### 꼬리질문1) 인터프리터와 컴파일러의 특징을 비교하며 설명해주세요

<details>
    <summary>답변</summary>

- 인터프리터 :
- 정적 컴파일러 :

</details>


#### 꼬리질문2) Java는 어떤 실행 방식을 채택하고 있나요?

<details>
    <summary>답변</summary>

- 정적 컴파일러와 Jit 컴파일러를 모두 사용하고 있다.
- 소스코드 &rarr; 바이트코드 : 자바 정적 컴파일러
- 바이트코드 &rarr; 기계코드 : JVM Jit 컴파일러

</details>

---
</br>

### 질문) JVM 이란 무엇이며 구성요소는 어떻게 되는지 설명해주세요

<details>
    <summary>답변</summary>

- 

</details>

#### 꼬리질문1) JVM은 메모리 관리를 어떻게 수행하나요?

<details>
    <summary>답변</summary>

- Grabage Collection 을 통해 메모리 관리를 수행한다.
- 

</details>

#### 꼬리질문) 기업에서는 왜 Java를 사용할까요? 면접자의 의견이 궁금합니다.

<details>
    <summary>답변</summary>

- 
</details>


---
</br>

### 질문) JDK에 대해서 설명해주세요.

<details>
    <summary>답변</summary>

- JDK는 Java Development Kit의 약자
- 현재의 자바 표준 버전은 Java SE(standard edition)이지만, 일반적으로 JDK라고 불린다.
- Oracle이 Sun 사를 인수하며, Java SE 11 부터 유료화 되었다. Oracle에서 제공하는 OpenJDK는 오픈소스 버전의 JDK 이다.

</details>

#### 꼬리질문1) JDK와 JRE의 차이점에 대해 설명해주세요.

<details>
    <summary>답변</summary>

- JRE는 실행만을 위한 환경.
- JRE만 설치할 경우 컴파일러 등이 제외 됨.
- JDK는 개발에 필요한 JVM, ~~라이브러리 API~~, javac(컴파일러), ~~jheap, jconsole~~ 등이 포함된다.
    - 확인필요
- Java9 버전 부터 JDK 안에 JRE를 포함하고 있으며, JDK만 배포되고 있다.

</details>

#### 꼬리질문2) GraalVM JDK 

<details>
    <summary>답변</summary>
    
- 출처 : https://www.oracle.com/java/graalvm/what-is-graalvm/
</details>

#### 꼬리질문3) JDK 1.5, 1.7, 1.8 은 다른 버전 업데이트에 비해 많은 변화가 일어났습니다. 버전별 주요 특징을 아는만큼 설명해주세요.

<details>
    <summary>답변</summary>
    
- 
</details>

---
</br>

### 질문) 제네릭에 대해 설명해주세요.

<details>
    <summary>답변</summary>

- 명시적으로 타입을 지정하기 위해 사용된다.
- 형 변환에서 발생할 수 있는 불편함을 보완하기 위해 Java5 버전에서 추가된 기능.
    - 런타임 시점에 잘못된 형 변환으로 인해 예외 발생을 방지.
    ```java
    dto1.setObject(new String());
    dto2.setObject(new StringBuffer());
    dto3.setObject(new StringBuilder());

    String temp1 = (String)dto1.getObject();
    StringBuffer temp2 = (StringBuffer)dto2.getObject();
    String temp3 = (String)dto3.getObject(); // 잘못된 형 변환
    ```

    - 의도하는 타입으로 반환받기 위해 instaceof 연산자를 사용하는 과정이 없어 코드의 간결성 향상
    ```java
    // 타입 점검을 위한 추가적인 코드
    if(tempObject instanceof String){
        ...
    }else if(tempObject instanceof StringBuffer){
        ...
    }
    ```

</details>

#### 꼬리질문) Java의 Covariant(공변성)에 대해 알고 계신가요? 공변성이란, ~ 를 의미합니다. 제네릭은 이를 지원하지 않습니다. 지원하지 않는 이유, 이로 인해 발생하는 문제, Java의 해결방안에 대해서 설명해주세요.

<details>
    <summary>답변</summary>

- 출처 : https://inpa.tistory.com/entry/JAVA-%E2%98%95-%EC%A0%9C%EB%84%A4%EB%A6%AD-%EC%99%80%EC%9D%BC%EB%93%9C-%EC%B9%B4%EB%93%9C-extends-super-T-%EC%99%84%EB%B2%BD-%EC%9D%B4%ED%95%B4

</details>

#### 꼬리질문) 제네릭의 와이들카드 종류와 각 특징에 대해 설명해주세요.

<details>
    <summary>답변</summary>

- 

</details>

---
</br>

### 질문) Java의 Collection에 대해서 설명해주세요

<details>
    <summary>답변</summary>

- 컬렉션은 목록성 데이터를 처리하는 자료구조를 통칭.
- 자바의 자료구조는 Map과 Collection 인터페이스를 구현.
![Alt text](./assets/java-data-structure.png)

</details>

#### 꼬리질문) 어떻게 Collection 인터페이스를 구현한 모든 클래스는 for-each 구문을 사용할 수 있을까요?

<details>
    <summary>답변</summary>

- 컴파일러가 for-loop 구문을 아래와 같이 변환한다.
- Collection 인터페이스는 Iterable 인터페이스를 구현하기 때문.
- Iterable 인터페이스를 구현한 클래스는 for-loop 구문을 사용할 수 있다.
- Iterable 인터페이스에는 Iterator<T> iterator(); 메소드가 정의되어 있는데, Iterator 를 반환하고, Iterator 에는 hasNext()와 next() 메소드가 정의되어 있다.

</details>

#### 꼬리질문1) Vector와 ArrayList는 모두 AbstractList 추상클래스를 구현하는 클래스 이지만, 대게 ArrayList를 많이 사용합니다. 어떤 이유 때문일까요?


<details>
    <summary>답변</summary>

- thread safe
    - Unlike the new collection implementations, Vector is synchronized. If a thread-safe implementation is not needed, it is recommended to use ArrayList in place of Vector.(출처 : https://docs.oracle.com/javase/7/docs/api/java/util/Vector.html)
- 내부 동작 차이
    - 새 요소를 추가할 때, 여유공간이 없으면 vector는 사이즈의 2배 / arraylist는 1/2배 증가

</details>

#### 꼬리질문3) Collection 객체를 복사할 때, deep copy 를 수행하기 위한 방법에 대해 설명해주세요. 

- addAll() 메소드


---
</br>

### 질문) Map, Set

<details>
    <summary>답변</summary>

- 

</details>

### 꼬리질문)

- HashMap 의 시간 복잡도
- 항상 O(1) 인가요?
- 해시 충돌에 대해서 아시나요?
- 그렇다면 그걸 자바에서는 해시 충돌에 대해 어떻게 대응하나요?
- TreeBin 방식은 어떨 때 사용하나요?
- HashMap 이외에 다른 Map 종류들을 알고 계신게 있으신가요?