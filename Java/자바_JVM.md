### 질문) Java의 실행 과정에 대해 설명해주세요

<details>
    <summary>답변</summary>

1. 소스파일 작성 (.java)
2. 자바 컴파일러(javac.exe)로 소스파일을 컴파일하여 바이트코드(.class) 파일 생성
3. JVM 구동 명령어(java.exe)로 바이트코드를 운영체제에 맞게 기계어로 번역

- 소스 코드를 자바 컴파일러가 클래스 파일로 생성, 생성된 클래스 파일을 클래스 로더가 jvm 메모리에 올림, 메모리가 올린 bytecote를 실행 엔진이 하드웨어가 읽을 수 있는 jit이 바이트로더 → 어셈블리로 변환해서 실행

</details>

#### 꼬리질문1) 자바 컴파일러, 인터프리터, JIT 에 대해 설명해주세요

<details>
    <summary>답변</summary>

- 인터프리터 방식
    - 한 줄씩 동적으로 컴파일해 실행, 빠른 실행이지만, 실행 속도가 느린 단점이 있음
    - 고급언어를 미리 컴파일 하지 않고, 프로그램 실행 시점에 번역과 실행
- 컴파일 방식
    - 소스 코드를 한 번에 컴파일해서 동적으로 실행하는 것이 아니기 때문에 인터프리터 방식보다 빠르고, 일반적인 언어에서 사용되는 방식
- JIT
    - 바이트코드를 어셈블리어로 변환해 실행하는 방식

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

Java7 &rarr; Java8
- JVM의 여러 메모리 영역 중에 PermGen 메모리 영역이 없어지고 Metaspace 영역이 생겼다.
- Metaspace는 클래스 메타데이터를 담는 영역인데, Default으로 제한된 크기를 가지고 있지 않고 동적으로 클래스를 생성할 경우 필요한 만큼 계속 늘어난다. 따라서 불필요한 클래스가 계속 생겨나면 추후 서버가 다운되는 상황이 발생할 수 있으니 모니터링을 통해 적절한 max 값을 찾는 것이 좋다.

![image](https://github.com/proHyundo/backend-cs-study/assets/128882585/acca98b0-942c-4a6e-9bf6-619d5d2893f2)

![image](https://github.com/proHyundo/backend-cs-study/assets/128882585/5410a9ad-1c01-419d-8b1a-cbdea4fb7c78)

참고
- 더 자바 8 백기선

</details>

#### 꼬리질문) 기업에서는 왜 Java를 사용할까요?

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