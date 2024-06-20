섹션 4까지 듣고, IOC/DI/스프링컨테이너/스프링 빈 개념 정리 해오기
```
final 키워드 변수는, 생성자를 통해 반드시 초기화 되어야 한다.
```
### 스프링 생태계

필수
- Spring Framework
- Spring boot

선택
- Spring Data : E.g Spring Data JPA
- Spring Session
- Spring Security
- Spring Rest Docs
- Spring Batch
- Spring Cloud

### 스프링 프레임워크
![Alt text](../assets/image.png)

### 스프링 부트
- 스프링을 편리하게 사용할 수 있도록 지원, 최근에는 기본으로 사용
- 단독으로 실행할 수 있는 스프링 애플리케이션을 쉽게 생성
- Tomcat 같은 웹 서버를 내장해서 별도의 웹 서버를 설치하지 않아도 됨
- 손쉬운 빌드 구성을 위한 starter 종속성 제공
    - 필요한 빌드 구성을 통으로 땡겨옴
- 스프링과 3rd party(외부) 라이브러리 자동 구성
    - 라이브러리 간의 버전을 자동으로 구성해줌
- 메트릭, 상태 확인, 외부 구성 같은 프로덕션 준비 기능 제공
    - 기본 모니터링 기능
- 관례에 의한 간결한 설정

---

### 질문) 스프링 프레임워크의 핵심 컨셉은 무엇이길래 사람들이 열광하는 것인가요?

<details>
    <summary>답변</summary>

- 스프링은 객체지향 언어 기반의 프레임워크이다.
- 객체지향의 특징을 극대화하는 프레임워크이다.
- 즉, 좋은 객체지향 애플리케이션을 개발할 수 있게 도와주는 도구(프레임워크)이다.

</details>


#### 꼬리질문) 좋은 객체지향 프로그래밍이란 무엇인가요?

<details>
    <summary>답변</summary>

1. 컴퓨터 프로그램을 객체들의 모임으로 파악
2. 각 객체는 서로 협력하여 메시지를 주고받고, 데이터를 처리
3. 프로그램을 유연하고 변경에 용이하게 한다.
    - 이를 가능하게 하는 객체지향의 핵심은 다형성 Polymorphism이다.
    - 클라이언트는 역할(인터페이스)만 알면 된다.
    - 클라이언트는 구현 대상의 내부 구조를 몰라도 된다.
    - 클라이언트는 구현 대상 자체 또는 내부가 변경되어도 영향 받지 않는다.

- 스프링은 다형성을 활용한 IoC, DI 방법을 통해 역할과 구현을 편리하게 다룰 수 있도록 지원한다.

</details>

#### 꼬리질문) 다형성에 대해 설명해주세요

<details>
    <summary>답변</summary>

- 

</details>

#### 꼬리질문) 좋은 객체 지향 설계의 5가지 원칙 SOLID에 대해 설명해주세요.

<details>
    <summary>답변</summary>

1. SRP: 단일 책임 원칙(single responsibility principle)
    - 문맥과 상황에 따라 다르지만, 책임 분리의 기준은 `변경`이다. 변경이 있을 때 파급이 적으면 SRP 원칙을 잘 따른 것

2. **OCP: 개방-폐쇄 원칙 (Open/closed principle)**
    - 확장에 열려있고 변경에 닫혀있다.
    - 다형성을 활용해 역할과 구현을 분리한 것을 의미.
    - 아래 코드는 OCP 원칙을 따르지 않는다. 이를 스프링 컨테이너가 해결해준다.
    ![Alt text](../assets/OCP원칙.png)

3. LSP: 리스코프 치환 원칙 (Liskov substitution principle)
    - 다형성에서 구현체를 믿고 사용하기 위해서는 인터페이스 규약을 지켜야 한다.

4. ISP: 인터페이스 분리 원칙 (Interface segregation principle)
    - 범용 인터페이스 보다 특정 클라이언트를 위한 인터페이스 여러개로 분리하는 것이 좋다.
    - 추상 메서드가 많은 범용 인터페이스를 모두 구현하는데 어려움이 있기 때문

5. **DIP: 의존관계 역전 원칙 (Dependency inversion principle)**
    - 클라이언트 코드(Service)가 구현 클래스를 바라보지 말고 인터페이스를 바라볼것.
    - 구현(구체화)이 아닌 역할(추상화)에 의존해야 한다.
    - 이를 통해 유연한 구현체 변경이 가능

Spring Framework를 통해 다형성 + OCP, DIP 를 지킬 수 있음.

</details>


#### 꼬리질문) 인터페이스에 의존하고, 인터페이스로 분리하는 것을 상당히 강조하는데, 그렇다면 항상 인터페이스를 도입해 추상화 하는 것이 좋은 방식인가?

<details>
    <summary>답변</summary>

- 추상화는 런타임에 어떤 구현체를 사용하게 될지 결정되기 때문에, 어떤 구현체를 사용하는지 개발자가 찾는데 비용이 발생한다.
- 따라서 기능 확장의 가능성이 없다면 구체 클래스를 직접 사용하고, 추후 리팩토링 과정을 통해 인터페이스를 도입하는 것도 하나의 방법이다.

</details>


#### 꼬리질문) 다음 코드는 OCP / DIP 원칙을 따르지 않는 코드이다. Spring 은 이를 어떻게 해결하는가?

```java
public class OrderServiceImpl implements OrderService{

    private final MemberRepository memberRepository = new MemberMemoryRepository();
    
//    private final DiscountPolicy discountPolicy = new FixDiscountPolicy();
    private final DiscountPolicy discountPolicy = new RateDiscountPolicy();

    @Override
    public Order createOrder(Long memberId, String itemName, int itemPrice) {
        Member member = memberRepository.findById(memberId);
        int discountPrice = discountPolicy.discount(member, itemPrice);
        return new Order(memberId, itemName, itemPrice, discountPrice);
    }
}
```

<details>
    <summary>답변</summary>

- 구체에 의존하지 않고 추상화에만 의존하기 위해, 클라이언트가 인터페이스에만 의존하도록 변경해야 한다.
- 구현체를 할당하기 위해, Spring이 구현 객체를 대신 생성하고 주입해준다.
- 전체 구성을 담당할 별도의 설정파일(AppConfig)이 필요 &rarr; 구현객체 주입을 외부에서 결정한다. 클라이언트는 '의존관계에 대한 고민은 외부'에 맡기고 '실행에만 집중.

</details>

---
</br>

### 질문) 다음과 같이 생성자 주입을 통해 의존관계를 주입하는 상황을 가정하자. 어떻게 스프링에게 어떤 인터페이스의 구현체를 사용할지 명시할 수 있는가?

<details>
    <summary>답변</summary>

- `@Qualifier`, `@Primary`

</details>

#### 꼬리질문) 위 대안은 OCP 원칙을 위반하는 코드일까?

<details>
    <summary>답변</summary>

- OCP 원칙은 클라이언트 코드 즉, 사용영역의 변경에 닫혀 있어야 한다. 이는 '클래스 의존관계를 변경하지 않는다'라는 의미인데

</details>


---
</br>

### 질문) IoC와 DI에 대해 설명해주세요

<details>
    <summary>답변</summary>

**IoC**
- 프로그램 제어 흐름의 제어권이 클라이언트가 아닌 외부에서 관리하는 것을 말한다.
- 구현 객체는 자신의 로직을 실행하는 역할만 담당하며, (클라이언트의)외부 파일이 제어 흐름을 갖는다.

**DI**
- 의존관계 주입이란, 실행 시점에 외부에서 동적인 구현객체(인스턴스)를 생성해 클라이언트에 (참조값을)전달해, 클라이언트와 구현 객체의 실제 의존 관계를 연결하는 것을 의미한다.
- 의존관계 주입을 사용하면 정적인 클래스 의존관계를 변경하지 않고, 동적인 객체 인스턴스 의존관계를 쉽게 변경할 수 있다.

</details>

---
</br>

### 질문) Spring Container과 Spring Bean에 대해 설명해주세요

<details>
    <summary>답변</summary>

**spring container**
- 일반적으로 ApplicationContext를 스프링 컨테이너라 한다.
- 해당 인터페이스의 구현체에는 대표적으로 `@Configuration` 어노테이션 기반한 `AnnotationConfigApplicationContext`, XML 구성 파일을 읽을 수 있는 `ClassPathXmlApplicationContext`, `FileSystemXmlApplicationContext`, `XmlApplicationContext`등이 있다.

**Spring Bean**
- 어노테이션/XML 등 환경정보를 기반으로 스프링 컨테이너에 등록된 객체를 스프링 Bean 이라 한다.
- Default로 @Bean 어노테이션이 붙은 메서드의 이름을 스프링 빈 이름으로 사용하며, 임의로 명시할 수 있다.

</details>


#### 꼬리질문) SpringContainer와 연관짓어 SpringBean의 생명주기에 대해 설명해주세요

<details>
    <summary>답변</summary>

1. 스프링 IoC 컨테이너 생성
    - 설정(구성)정보를 기반으로 스프링 IoC 컨테이너 생성
2. 스프링 빈 생성
    - 스프링 컨테이너 내부에 스프링 빈 저장소가 존재. 구성(설정) 정보로 만들어진 BeanDefinition의 메타정보를 통해 메서드 명 또는 명시된 빈 이름을 등록. 인스턴스 참조값을 빈 객체에 등록
3. 스프링 의존관계 주입
    - 설정 정보를 참고해 스프링이 의존관계 주입
    - 실제 스프링 사이클은 빈 생성과 주입 단계가 나누어져 있으나, 두 과정이 동시에 되는 경우도 있다.
        - 생성자 주입 : 구체 클래스를 빈으로 등록할 때 생성자가 실행되기 때문에 빈 생성과 의존관계 주입이 동시에 진행된다. 만약 의존 주입에 필요한 빈이 생성되어 있지 않으면 생성해서 주입한다.
        - 그 외 수정자(setter) 주입 등과 같은 방법은 빈 생성과 주입 단계가 나누어져 있다.
4. 초기화 콜백 메서드 호출
5. 사용
6. 소멸 전 콜백 메서드 호출
7. 스프링 종료

</details>


#### 꼬리질문) AplicationContext와 상위 인터페이스인 BeanFactory인터페이스의 차이는 무엇인가요?

<details>
    <summary>답변</summary>

**BeanFactory**
- BeanFactory 인터페이스는 스프링 컨테이너의 최상위 인터페이스.
- `getBean()` 등의 메서드가 선언되어 있음

**ApplicationContext**
- 부가기능이 포함됨
    - 환경변수 분리, 국제화, 

</details>


#### 꼬리질문) Spring Container를 사용하면 어떤 장점이 있는가?

<details>
    <summary>답변</summary>

- `BeanFactory`는 
</details>
