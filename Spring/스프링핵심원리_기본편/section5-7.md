### 질문) 싱글톤 이란 무엇인가요?

<details>
    <summary>답변</summary>

- 

</details>

#### 꼬리질문) 웹 애플리케이션에서 왜 싱글톤이 많이 사용되나요?

<details>
    <summary>답변</summary>

- 웹 애플리케이션은 여러 고객이 동시에 요청하는 경우가 빈번하다
- 요청마다 객체를 새로 생성하는 것은 메모리 낭비를 유발한다.
- 따라서 생성된 하나의 인스턴스를 공유하는 것이 효율적이다.

</details>

#### 꼬리질문) 싱글톤 패턴이란 무엇인가요?

<details>
    <summary>답변</summary>

- 클래스의 인스턴스가 딱 1만 생성되는 것을 보장하는 디자인 패턴

**싱글톤 패턴 구현 방법은 여러가지 있다.**
1. private 생성자
    - 이를 위해 private생성자를 사용해 외부에서 new 연산자를 사용불가하도록 해 의도하지 않은 곳에서 객체 인스턴스가 생성되는 것을 막는다.
    - 가장 단순하고 안전한 방법이다

**싱글톤 패턴 문제점** &rarr; 안티패턴
- 싱글톤 구현을 위해 필요한 코드가 추가되어야 한다.
- DIP 원칙을 위반한다.
    - 클라이언트가 구체 클래스에 의존하기 때문
        - 구체클래스.getInstance() 해서 인스턴스를 불러와야 하기 때문
    ```java
    public static SingletonService getInstance() {
        return instance;
    }
    ```
- OCP 원칙 위반 가능성이 높다
- 유연하게 테스트하기 어렵다
- private 생성자 때문에 자식 클래스를 만들기 어렵다

이러한 문제점을 Spring Container가 해결한다.

</details>


#### 꼬리질문) 싱글톤은 메모리를 효율적으로 사용할 수 있는 장점이 있다고 하셨는데, 그렇다면 모든 인스턴스를 싱글톤으로 유지하는 것이 좋은가요?

<details>
    <summary>답변</summary>

- 공유변수가 필요 없는 인스턴스는 싱글톤으로 유지하면 좋지 않을까?
- 아래 꼬리질문이 답변이 될 수 있다.

</details>


#### 꼬리질문) 스프링 프레임워크에서 싱글톤 방식을 사용할 때 주의해야할 점이 있나요?

<details>
    <summary>답변</summary>

- 싱글톤 객체(스프링 빈)는 Stateful(상태유지) 하게 설계해서는 안된다. 즉, Stateless(무상태) 해야 한다.
    - 특정 클라이언트가 값을 변경할 수 있는 필드가 있으면 안된다
    ```java
    public void order(String name, int price) {
            System.out.println("name = " + name + " price = " + price);
            this.price = price; // 여기가 문제!
    }
    ```
    - 가급적 읽기만 가능해야 한다
    - 필드 대신, 공유되지 않는 지역변수/파라미터/ThreadLocal 등을 사용해야 한다.
        ThreadLocal 이란 무엇인가?

</details>


#### 꼬리질문) 스프링 컨테이너는 싱글톤 인스턴스만 꺼낼 수 있나요?

<details>
    <summary>답변</summary>

- 기본 빈 등록 방식은 싱글톤이지만, 설정을 통해 변경 가능

</details>

#### 꼬리질문) Spring은 어떻게 `@Configuration` 어노테이션 이 붙은 설정파일을 보고 싱글톤 객체를 만들 수 있는가?

```
// 예상 출력 결과 :
// 1. AppConfig.memberService
// 2. AppConfig.memberRepository
// 3. AppConfig.orderService
// 4. AppConfig.memberRepository
// 5. AppConfig.memberRepository

// 실제 출력 결과 :
// 1. AppConfig.memberService
// 2. AppConfig.memberRepository
// 3. AppConfig.orderService
```

<details>
    <summary>답변</summary>

- CGLIB proxy가 intercept 해서 bean으로 등록할 클래스를 상속받은 임의의 클래스를 만들어, 해당 클래스를 스프링 빈으로 등록.
- 상속, 오버라이딩
    - 부모타입으로 조회 가능

참고 링크 : [Understanding Configuration and Proxy in the Spring Framework: A Beginner's Guide (CGLIB, JDK Libs)](https://youtu.be/2HS6LSAESKw?si=Xksrf4LL-qpUhICc)

</details>


#### 꼬리질문) `@Configuration` 이 아니라 `@Component` 어노테이션이 붙은 구체 클래스들은 싱글톤일까?

<details>
    <summary>답변</summary>

- 참고 링크 : [[Spring] @Configuration vs @Component](https://kth990303.tistory.com/395)
- 이 부분에서 혼동이 온다
```
new MemberServiceConfig()를 반환하는 메서드를 @Bean으로 등록했다고 하더라도,

@Configuration이 아닌 @Component를 사용한다면 Java의 new 키워드때문에 매번 힙 인스턴스로 생성되어 메모리에 부하가 가게 된다. 외부 라이브러리 또는 설정 정보 클래스를 반환하는 클래스일 경우 @Configuration를 꼭 붙여주도록 하자.
```
```
Ok, so @Component and @Configuration annotations make beans singleton instance always as default. But only @Configuration beans get override by CGLIB ?

그렇습니다. @Component와 @Configuration 애노테이션이 붙은 빈은 기본적으로 싱글톤입니다. 하지만 @Configuration 애노테이션은 추가적인 특별한 동작을 합니다.

Spring은 @Configuration이 붙은 클래스를 CGLIB를 통해 상속받은 프록시 클래스를 만들어 사용합니다. 이렇게 만들어진 프록시 클래스는 @Bean 메서드가 호출될 때마다 새로운 객체를 생성하는 것이 아니라, 이미 생성된 객체가 있다면 그것을 반환합니다. 이렇게 함으로써 싱글톤이 보장됩니다.

반면에, @Component 애노테이션은 이러한 추가적인 동작을 하지 않습니다. @Component 애노테이션은 단순히 해당 클래스의 인스턴스가 싱글톤으로 관리되도록 Spring에 지시합니다. 그러므로 @Component 애노테이션이 붙은 클래스의 메서드는 호출될 때마다 새로운 결과를 반환할 수 있습니다.
```

- 참고 링크 : [Spring @Component @Bean 알고 쓰기](https://dev.to/composite/spring-component-bean-3hj)

</details>

---
</br>

### 질문) Spring 의 빈 등록 방법에는 무엇이 있나요?

<details>
    <summary>답변</summary>

1. 설정 정보(파일) 
    - `@Configuration` Annotation
    - xml 파일 bean 태그
2. 컴포넌트 스캔
    - `@ComponentScan` 방식은 `@Component` 어노테이션이 붙은 모든 클래스를 스프링 빈으로 등록한다.
    - 스프링 빈의 이름은 기본값으로 클래스 명에서 가장 앞글자만 소문자로 변경해 지정된다.

</details>

#### 꼬리질문) `@ComponentScan`의 스캔 범위에 대해 아시나요? 

<details>
    <summary>답변</summary>

- basePackages 으로 탐색할 패키지의 시작 위치 지정. 해당 패키지를 기준으로 하위 패키지 모두 탐색
- 생략 시, `@ComponentScan` 어노테이션이 사용된 클래스의 패키지를 디폴트로 사용.
- 관례적으로 프로젝트 메인 설정 정보는 프로젝트를 대표하는 정보이기 때문에, 시작 루트 위치에 설정파일을 둔다.
    - `@ComponentScan` 어노테이션은 `@SpringBootApplication` 어노테이션 내부에 존재한다.
    - 스프링부트 서버를 실행할 때 컴포넌트 스캔이 되어, 별도의 설정파일을 생성할 필요 없으나, 따로 관리하고 싶다면 생성하기도 한다.
- 컴포넌트 스캔의 기본 대상 : @Component, @Controller, @Service, @Repository, @Configuration
- 여러 옵션을 통해 스캔 범위를 제어할 수 있다.
    - excludeFilters, includeFilters b


</details>

#### 꼬리질문) ComponentScan 과정에서 중복된 빈 이름이 존재하면 어떻게 되나요?

<details>
    <summary>답변</summary>

- 자동 빈 등록되는 이름이 중복되는 경우 (E.g. `@Component("service")`)
    - ConflictingBeanDefinitionException 발생

- 자동 빈 등록과 수동 빈 등록된 이름이 중복되는 경우
    - 과거에는, 수동 빈 등록이 우선권을 가져 수동 빈이 자동 빈을 오버라이딩 한다.
    - 최신 스프링부트에서는 오류가 발생하게 변경되었다. 기본 값이 오버라이딩 False 이다.
        - `spring.main.allow-bean-definition-overrding = true`

</details>

---
</br>

### 질문) ComponentScan 방식으로 빈을 등록할 때 의존관계를 주입하는 방법에는 무엇이 있나요?

<details>
    <summary>답변</summary>

1. 생성자 주입 : 인자 생성자 + `@Autowired` 자동 주입
    - `@Autowired`의 스프링 빈 기본 조회 전략은, 타입 조회 이다.
    - 불변 : 생성자 호출시점에 단 1번만 호출(값 세팅)되는 것을 보장한다
    - 필수 의존 관계에 사용된다 : `final` == 값이 반드시 존재해야 함을 의미
    - 생성자가 오직 1개일 때만 `@Autowired` 생략가능.

2. 수정자 주입
    ```java
    // setter 주입
    private MemberRepository memberRepository;
    private DiscountPolicy discountPolicy;

    @Autowired(required = false)
    public void setMemberRepository(MemberRepository memberRepository) {
        this.memberRepository = memberRepository;
    }
    @Autowired
    public void setDiscountPolicy(DiscountPolicy discountPolicy) {
        this.discountPolicy = discountPolicy;
    }
    ```
    - 만약 생성자 주입과 수정자 주입이 모두 존재하면, 모두 수행된다.
    - 선택적(의존이 있어도 되고 없어도 되고)이거나 변경가능성이 있을 때(외부에서 호출하여 의존을 변경) 수정자 주입을 사용한다.
        - 선택적 : `@Autowired(required = false)`

3. 필드 주입
    ```java
    @Autowired private MemberRepository memberRepository;
    ```
    - 코드는 간결하지만, 스프링 컨테이너를 띄우는 것이 아닌, 순수한 테스트코드를 작성할 때 `new` 연산자로 구체클래스를 생성하면 의존이 필요한 빈들이 존재하지 않기 때문에 `NullPointerException` 오류가 발생한다.
        - 이를 해결하기 위해선 결국 setter 메서드를 생성해야 하는데, 그렇다면 굳이 필드주입을 사용할 이유가 없다. 수정자 주입 방식으로 대체하는 것이 낫다.
    - 테스트 코드를 작성할 땐, 해당 테스트 클래스 파일 내에서만 의존을 주입하면 되기 때문에 필드주입을 주로 사용한다.

4. 일반 메서드 주입
    - 수정자 주입과 동일하게 동작한다.
    - 수정자 주입과 달리 여러 인자를 받을 수 있는 특징이 있다.

</details>


#### 꼬리질문) 왜 필드 주입은 IDE에서조차 권장하지 않는 방법이라고 하는 것일까요?

<details>
    <summary>답변</summary>


1. 필드 주입은 단위 테스트 작성 시 어려움이 있다.
    - 단위 테스트에는 `@SpringBootTest`를 사용하지 않아, 테스트를 수행할 때 스프링 컨테이너가 의존성 주입을 해주지 않는다. 그래서 이 경우 의존 객체 주입을 직접 해야 하지만, 필드 주입 방법을 사용하면 의존 관계 주입을 수동으로 해줄 방법이 없다. Mocking 하거나 다른 의존성으로 대체할 수 있는 방법이 없다.
    - 이를 `외부에서 변경이 불가능하다` 라고 표현한다.

2. 스프링 프레임워크에 의존적이다.
    - 생성자 주입은 POJO 방식으로 의존성을 주입할 수 있음.
    - `@Autowired` 는 스프링 프레임워크에서 제공하는 빈 자동주입 방법. 즉 DI를 제공하는 프레임워크에 의존적이다.
    - @Autowired를 붙여주면 애플리케이션 실행시 스프링 컨테이너가 자동으로 의존관계 대상이 빈으로 등록되어있는지 찾고, 해당 빈을 주입해줍니다! 그래서 @Autowired를 사용해서 스프링 컨테이너가 의존 객체를 주입해주는 모든 방법이 스프링 의존적이라고 볼 수 있습니다. 다만 다른 방법은 일반적인 POJO(자바 객체)로 사용했을 때도 의존성을 주입해줄 방법이 있습니다. 그러나 필드 주입은 스프링 컨테이너 없이 의존 객체를 주입해줄 방법이 없습니다. 필드 주입을 제외한 다른 방법은 기본적인 자바 객체 상태일 때도 주입해줄 수 있습니다.

</details>


#### 꼬리질문) 생성자 주입과 수정자(세터) 주입 방식 중 더 올바른 방법이 있다면 무엇이고 왜 그렇게 생각하시나요?

<details>
    <summary>답변</summary>

수정자 주입의 단점
1. 불변
    - 대부분의 의존관계는 App 종료시점까지 변경될 일이 없다. 오히려 불변을 유지하는 것이 권장된다.
    - 또한, 수정자 주입을 사용하면 setter를 public 접근제어자로 열어두어야 한다. 변경하면 안되는 메서드를 열어두는 것은 좋은 설계 방법이 아니다.

2. 누락
    - 단위테스트를 작성하는 과정에서, 생성자 주입은 컴파일 단계에서 누락된 의존관계를 확인할 수 있는 반면, 수정자 주입은 테스트 대상 내부 코드를 확인해야 필요한 의존관계를 확인할 수 있다. 
    ```java
    // 수정자 주입일 때는 컴파일 에러 발생 X, 생성자 주입일 때는 파라미터 내부에 빨간 밑줄

    @Test
    void createOrder() {
        OrderServiceImpl orderService = new OrderServiceImpl(
            new MemberRepository(), new DiscountPolicy() // 의존 주입
        );
    }

    ```

생성자 주입의 장점
1. final 키워드 활용
    - 누락 방지
2. 불변 보장
3. 프레임워크에 의존 X
    - 순수 자바 코드로 의존관계 설정 가능 POJO

참고 링크 : https://spring.io/blog/2007/07/11/setter-injection-versus-constructor-injection-and-the-use-of-required


</details>

### 질문) 같은 타입의 빈이 2개 이상 중복되는 경우, 어떻게 특정 빈을 선택할 수 있나요?

<details>
    <summary>답변</summary>

1. `@Autowired`가 붙은 필드 명 또는 파라미터 변수 명을 구체타입 빈의 이름으로 설정한다.

2. `@Qualifier("name")`
    - 구체클래스에 식별할 수 있는 이름을 붙이고, 클라이언트에서 해당 식별 값으로 구분할 수 있다. 
    - 즉, `@Qualifier` 끼리 매칭한다.
    - 만약 매칭된 빈을 찾을 수 없다면, 스프링 빈 컨테이너에서 해당 이름의 빈을 찾는다.
    - 필드, 생성자, 수정자에서 모두 사용할 수 있다.

3. `@Primary`
    - 우선순위를 1개 지정한다.
    - 가장 자주 사용되는 방법.
    - 상황) Main DB Connection 이 95%, Sub DB Connection 이 5% 인 경우에, Main DB에 `@Primary`를 붙이고, Sub DB가 필요한 로직에만 `@Qualifier` 어노테이션을 붙인다.
    - 즉, `@Primary`와 `@Qualifier` 모두 존재할 경우 `@Qualifier`의 우선순위가 더 높다는 것을 알 수 있다.



</details>

#### 꼬리질문) `@Qualifier`의 단점은 무엇이 있을까요? 그리고 어떻게 해결할 수 있을까요? (반드시 사용해야 한다는 가정하에)

<details>
    <summary>답변</summary>

단점
- name을 컴파일 단계에서 검증할 수 없다

대안 1. Custom Annotation 만들기 
    - 
    - 단, 스프링이 `@Qualifier` 기능을 제공함에도 같은 역할을 하는 어노테이션을 재정의 하는 것은 유지보수 측면에서 복잡성이 증가될 수 있다. 이를 개선하기 위해 대안 2를 생각해볼 수 있다.

대안 2. `@Qualifier`와 상수 결합해서 사용하기
```java
public class QualifierConst {
    public static final String MAIN_DISCOUNT_POLICY = "mainDiscountPolicy";
}

// 사용하는 곳에서 
@Qualifier(QualifierConst.MAIN_DISCOUNT_POLICY)
```
</details>

#### 꼬리질문) 위의 방법들 처럼 정적으로 구현클래스를 결정하는 것이 아닌 동적인 방법은 없을까요?

<details>
    <summary>답변</summary>

```java
static class DiscountService {
    private final Map<String, DiscountPolicy> policyMap;
    private final List<DiscountPolicy> policies;

    public DiscountService(Map<String, DiscountPolicy> policyMap, List<DiscountPolicy> policies) {
        this.policyMap = policyMap;
        this.policies = policies;
        System.out.println("policyMap = " + policyMap);
        System.out.println("policies = " + policies);
    }
```

- 이렇게 빈을 교체하는 성격이라면 수동 등록을 더 선호합니다.

</details>