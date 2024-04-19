### 질문) Java 기본 자료형의 종류는 무엇이 있나요?

<details>
    <summary>답변</summary>

- 정수형 : byte, short, int, long (aka. bsil)
- 실수형 : float, double (aka. fd)
- 그 외 : boolean
![image](https://github.com/proHyundo/backend-cs-study/assets/128882585/197cc15e-fe2b-4635-9240-5aa7be08b624)

</details>

#### 꼬리질문1) byte 자료형은 왜 -128~127 범위를 가지는지 아시나요?

<details>
    <summary>답변</summary>

- 1byte 는 8bit
- 컴퓨터의 최소 단위 bit는 0, 1 2진수 값을 저장 가능
- 그런데, 자바의 정수형 기본 자료형은 모두 signed 타입
- 가장 첫 번째 비트는 부호를 나타내는 비트.
- 따라서 1000_0000 ~ 0111_111 &rarr; -128~127

</details>

#### 꼬리질문2) 그럼 Java에는 unsigned 타입의 자료형에는 무엇이 있나요?

<details>
    <summary>답변</summary>

- char자료형. '\u0000'(공백) ~ '\uffff'(물음표) 데이터 범위를 가지며, 16진법을 10진수로 변경하면 0 ~ 65535이다. (2byte == 16bit)
- 참고 링크 : [is-java-char-signed-or-unsigned-for-arithmetic](https://stackoverflow.com/questions/54924058/is-java-char-signed-or-unsigned-for-arithmetic)

</details>

#### 꼬리질문3) float 이나 double 자료형은 돈을 위한 값을 저장하기에는 부적절해요. 왜 부적절하고 대안은 무엇이 있을까요?

<details>
    <summary>답변</summary>

부적절한 이유
- 자바는 IEEE 754 부동 소수점 방식을 사용해서, 정확한 실수를 저장하지 않고 최대한 완벽에 가깝기를 바라는 근사치 값을 저장한다.
- 따라서 정확한 돈과 소수점을 다루기에는 부적절하다.

대안
- 정수를 이용해 실수를 표현하는 java.math.BigDecimal 클래스
- 참고 링크 : [@new_wisdom/Java-BigDecimal](https://velog.io/@new_wisdom/Java-BigDecimal과-함께하는-아마찌의-너드짓)
- 참고 링크 : [BigDecimal A to Z: 정확한 계산을 위한 숫자 처리 클래스](https://dev.gmarket.com/75)

</details>

#### 꼬리질문4) new 연산자로 객체를 생성하는 것과 valueOf()메서드로 객체를 생성하는 방법의 차이점이 있을까요?

<details>
    <summary>답변</summary>

- new 연산자 : 새로운 메모리 주소 할당
- Java의 Wrapper 클래스에는 valueOf() 메서드가 정의되어 있음. : -128 ~ 127 사이의 정수인 경우 캐싱된 데이터를 가져옴

```java
Integer integer = new Integer(10);
System.out.println("Integer: " + integer);

Integer integer2 = Integer.valueOf(10);
System.out.println("Integer2: " + integer2);

// == operator compare the references of two objects in memory.
if (integer == integer2) {
    System.out.println("integer == integer2");
} else {
    System.out.println("integer != integer2");
}

// Integer class overrides the equals method of Object class to compare the actual values of two objects.
if (integer.equals(integer2)) {
    System.out.println("integer.equals(integer2)");
} else {
    System.out.println("!integer.equals(integer2)");
}
```
- 참고 링크 : https://meetup.nhncloud.com/posts/185

</details>

---
</br>

### 질문) pass by value, pass by reference 에 대해 설명해주세요.

<details>
    <summary>답변</summary>
</br>

두 방식 모두 메서드를 호출할 때, 파라미터를 통해 값을 전달하는 방식.

- pass by value : 복사된 값만 전달되는 방식으로, 기본 자료형은 항상 이에 해당된다.
- pass by reference : 값이 아닌 객체에 대한 참조가 전달되는 방식
    - 호출된 메서드에서 다른 객체로 대체하여 처리하면 기존 값은 바뀌지 않는다.
    - 그러나, 매개변수로 받은 참조 자료형 안에 있는 변수를 변경하면, 데이터가 바뀐다.
    - String의 경우 `""` 쌍따옴표로 값을 할당하면 `new` 연산자로 객체를 생성한 것과 같다.

- 그러나, 실질적으로 Java는 모든 메서드 호출에 있어 pass by value 방식을 사용하고 있다.
    

</details>

#### 꼬리질문1) Java는 왜 pass by value 만 존재하나요?

<details>
    <summary>답변</summary>

- 참조 자료형의 경우, 매개변수를 넘기는 과정에서 직접적인 참조를 넘긴 게 아닌, 주소 값을 복사해서 넘기기 때문에


|![image](https://github.com/proHyundo/backend-cs-study/assets/128882585/dabbfe69-9071-49de-9aba-5e8145702bb6) | ![image](https://github.com/proHyundo/backend-cs-study/assets/128882585/b3bab7e5-a468-41f5-a846-3b62ba88675f) |
| --- | --- |
- run 메서드가 실행되며 매개변수를 전달받으면 다음과 같은 상황이 됩니다. arg1은 a1이 가지고 있는 주소값을 복사하여 독자적으로 가지게 됩니다. arg2도 마찬가지로 a2가 가지고 있는 주소 값을 복사하여 독자적으로 가지고 있게 됩니다. 주소 값을 복사하여 가져 가는 call by value가 발생한 것이죠.
- run 메서드가 실행되며 매개변수를 전달받으면 다음과 같은 상황이 됩니다. arg1은 a1이 가지고 있는 주소값을 복사하여 독자적으로 가지게 됩니다. arg2도 마찬가지로 a2가 가지고 있는 주소 값을 복사하여 독자적으로 가지고 있게 됩니다. 주소 값을 복사하여 가져 가는 call by value가 발생한 것이죠.
- 참고 링크 : [Java는 Call by reference가 없다](https://deveric.tistory.com/92)

- 참고 링크 : [Pass By Value, Pass by Reference-항해일지:티스토리](https://internet-craft.tistory.com/2)
- 참고 링크 : [call by value vs call by reference - 유도진 | 백엔드 데브코스 2기 | 백둥이Deview 220329](https://youtu.be/34RAc5gdl54?si=J_yTUzFxmtjXrbXG) ![백엔드_데브코스_2기_유도진](https://github.com/proHyundo/backend-cs-study/assets/128882585/7f350f0a-de0e-4ddc-8947-0f2eece42177)
- 참고 링크 : [JAVA) 자바에서는 Call By Reference가 불가능 합니다.](https://shanepark.tistory.com/380)


</details>

#### 꼬리질문2) String은 Primitive type(기본 자료형)이 아님에도 pass by value 방식을 따르고 있습니다. 어떤 특징 때문일까요?

<details>
    <summary>답변</summary>

- String pool을 통해 immutable로 관리되기 때문

```java
public class StringPoolSampleCode {
    
    public static void main(String[] args) {
        String a = "Hello";
        String b = new String("Hello");

        if (a == b) {
            System.out.println("a and b are the same object");
        } else {
            System.out.println("a and b are different objects");
        }

        if (a.equals(b)) {
            System.out.println("a and b have the same value");
        } else {
            System.out.println("a and b have different values");
        }

        String c = "Hello";
        String d = "Hello";

        if (c == d) {
            System.out.println("c and d are the same object");
        } else {
            System.out.println("c and d are different objects");
        }

        if (c.equals(d)) {
            System.out.println("c and d have the same value");
        } else {
            System.out.println("c and d have different values");
        }

        String e = new String("Hello");
        String f = new String("Hello");

        if (e == f) {
            System.out.println("e and f are the same object");
        } else {
            System.out.println("e and f are different objects");
        }

        if (e.equals(f)) {
            System.out.println("e and f have the same value");
        } else {
            System.out.println("e and f have different values");
        }
    }
}
```
</details>


---
</br>

### 질문) "java.lang.Object" 클래스에 대해서 아는 내용을 설명해주세요.

<details>
    <summary>답변</summary>

- 모든 클래스의 최상위 클래스 이다.
- Object 클래스를 제외한 모든 클래스들은 묵시적으로 Object 클래스를 상속받고 있다.
- Object 클래스 상속을 통해 모든 클래스의 기본적인 행동을 정의할 수 있다.

</details>

#### 꼬리질문1) Object 클래스에는 어떤 메서드가 정의되어 있나요?

<details>
    <summary>답변</summary>

- 객체 처리 메서드 :
    - `public String toString()` : 객체를 문자열로 표현. println() 메서드에 매개변수로 객체가 들어갈 경우와 객체에 `+` 연산을 수행할 경우 자동으로 `toString()` 메서드가 호출된다.
    - `public boolean equals(Object obj)`
    - hasCode()
- 쓰레드 관련 메서드 : 
    - `notify()`, `notifyAll()`, `wait()`

</details>

#### 꼬리질문2) 등위연산자`==`, equals(), hashCode() 차이점은 무엇인가요?

<details>
    <summary>답변</summary>

- 등위 연산자 : 기본자료형을 비교할 땐 값을, 참조자료형을 비교할 땐 주소값을 비교하게 된다. &rarr; 그럼 결국 stack에 저장된 값을 비교한다는거 아닌가?
- equlas() : hasCode() 메서드를 호출하여 값을 비교한다.
- hasCode() : 객체의 주소 값을 이용해서 해싱(hashing) 기법을 통해 해시 코드를 만든 후 반환. 엄밀히 말하면 해시코드는 메모리상 주소값은 아니고, 주소값으로 만든 고유한 숫자값.

</details>

#### 꼬리질문3) 그럼 우리가 "hashCode"를 오버라이드 했을때에도 메모리 주소를 리턴하게 할 수 있을까요? 자바에서는 개발자가 직접 메모리에 접근할 수 있나요?

<details>
    <summary>답변</summary>

- hashCode() 메서드를 오버라이딩한 이후에도 객체의 메모리 주소값(해시코드)를 가져오기 위해서는 `System.identityHashCode()` 메서드를 사용하여 가져올 수 있다.
- Java는 개발자가 직접 메모리에 접근할 수 없다.
    - 그럼 다음 링크들에서 다루는 내용은 메모리 접근이 아닌건가 🤔 : https://homoefficio.github.io/2020/08/10/Java-NIO-FileChannel-%EA%B3%BC-DirectByteBuffer/
    - https://forl.tistory.com/137

</details>

#### 꼬리질문4) equals() 메서드 오버라이딩이 필요한 경우와 불필요한 경우를 알고 있으신가요?

<details>
    <summary>답변</summary>

- 동일한 객체간의 (멤버변수 등) 상태 비교가 필요한 경우 오버라이딩이 필요.
- 비교를 제외한 기능위주의 클래스인 경우 오버라이딩 불필요.
    - 메서드만 있는 클래스인 경우 유틸 클래스이다. static 하게 필요한 상태를 관리하는 것이 좋다.
- 참고링크 : [자바 equals / hashCode 오버라이딩 - 완벽 이해하기
출처: https://inpa.tistory.com/entry/JAVA-☕-equals-hashCode-메서드-개념-활용-파헤치기 [Inpa Dev 👨‍💻:티스토리]](https://inpa.tistory.com/entry/JAVA-%E2%98%95-equals-hashCode-%EB%A9%94%EC%84%9C%EB%93%9C-%EA%B0%9C%EB%85%90-%ED%99%9C%EC%9A%A9-%ED%8C%8C%ED%97%A4%EC%B9%98%EA%B8%B0)

</details>

#### 꼬리질문5) equals 메서드를 재정의 할 때 주의할 점을 알고 있나요?

<details>
    <summary>답변</summary>

- 자신과 비교할 땐 true (반사성), hasCode도 같이 재정의 해야한다.
- 그렇지 않은 경우 hash를 활용하는 자료구조에서 문제가 발생한다.  hash 값을 사용하는 Collection(HashMap, HashSet, HashTable)은 객체가 논리적으로 같은지 비교할 때 아래 그림과 같은 과정을 거치기 때문.

<p align="center">
  <img src="https://img1.daumcdn.net/thumb/R1280x0/?scode=mtistory2&fname=https%3A%2F%2Fblog.kakaocdn.net%2Fdn%2FsMxPO%2FbtrNAyvpegC%2FUPc1EBboouzu0WZnc9bUfk%2Fimg.png" alt="equals-hash-img" width="400" />
</p>

```java
import java.util.HashMap;
import java.util.Objects;

public class HashCodeSample {

    public static void main(String[] args) {
        SampleDto s1 = new SampleDto("song");
        SampleDto s2 = new SampleDto("song");

        HashMap<SampleDto, String> map = new HashMap<>();
        map.put(s1, "fisrt song");
        map.put(s2, "second song");

        System.out.println(map.size());
        // OUTPUT : 1
    }
}

class SampleDto {
    public String name;

    public SampleDto(String name) {
        this.name = name;
    }

    @Override
    public int hashCode() {
        return Objects.hash(name);
    }

    @Override
    public boolean equals(Object obj) {
        if (obj instanceof SampleDto) {
            SampleDto s = (SampleDto) obj;
            return name.equals(s.name);
        } else {
            return false;
        }
    }
    
}
```
→ 서로 다른 객체 임에도 hashCode가 같으면서 equlas() 메소드에서 실제 메모리 주소를 비교하지 않으면, Map에서 의도한 value 를 올바르게 찾지 못한다.

</details>

#### 꼬리질문6) "hashCode" 를 잘못 오버라이딩하면 "HashMap" 등 hash 콜렉션의 성능이 떨어질 수가 있습니다. 어떤 케이스일 때 그럴 수 있을까요?

<details>
    <summary>답변</summary>

- 내용

</details>

---
</br>

### 질문) String 클래스의 단점이 있다면 무엇일까요?

<details>
    <summary>답변</summary>

- String 클래스는 불변객체 Immutable하다.
- 따라서 하나의 객체에 변경 작업이 계속되면, 변경(재할당)마다 새로운 객체를 Heap 영역의 String Pool 이라는 공간에 저장한다.        
    - 저장되는 영역은 한계가 있다.
    - JDK 1.5 부터 컴파일 과정에서 `+` 연산의 경우 StringBuilder으로 자동변환되어 성능최적화

</details>

#### 꼬리질문) 불변이란 무엇이고 왜 String을 불변성을 띄게 설계되었을까?

<details>
    <summary>답변</summary>
</br>

1) 보안

- 참고 링크 : [Oracle Java Magazine, Why is Java making so many things immutable?](https://blogs.oracle.com/javamagazine/post/java-immutable-objects-strings-date-time-records)

</details>

#### 꼬리질문1) 불변한 String의 단점을 보완하는 방법이 있나요?

<details>
    <summary>답변</summary>

불변한 String과 달리 StringBuffer와 StringBuilder는 가변적이다.

StringBuffer
- 멀티쓰레드에 안전(thread safe)하도록 동기화 되어 있다

StringBuilder
- StringBuffer에서 쓰레드 동기화만 뺀 StringBuilder가 새로 추가

추가 키워드 : 동기화

참고 자료
- [Java Compiler Optimization for String Concatenation](https://medium.com/javarevisited/java-compiler-optimization-for-string-concatenation-7f5237e5e6ed)
- [jdk1.5에서 String 더하기의 컴파일시의 최적화](https://gist.github.com/benelog/b81b4434fb8f2220cd0e900be1634753)
- [String은 항상 StringBuilder로 변환될까?](https://siyoon210.tistory.com/160)

</details>

#### 꼬리질문) 왜 동기화(synchronized)가 걸려있으면 느린걸까요?


싱글 스레드로 접근한다는 가정하에선 "StringBuilder" 와 "StringBuffer" 의 성능이 똑같을까요?
함정 질문입니다. synchronized 키워드를 달면 내부적으로 어떤 일이 벌어지는지 동작원리에 대해 잘 알아보세요!

#### 꼬리질문) String 변수에 값을 할당할 때, `new` 연산자와 쌍따옴표`""`의 차이점은?

<details>
    <summary>답변</summary>

`new`
- 매번 heap 영역에 재생성

`""`
- 힙 영역 상수풀에서 재사용하여 불필요한 재생성 비용 X

</details>

#### 꼬리질문2) String은 참조자료형임에도 불구하고, 동일한 값을 갖는 두 객체(변수)에 `==` 연산의 결과가 True 입니다. 그 이유는 무엇일까요?

<details>
    <summary>답변</summary>

- equals()를 재정의해 주소 값이 아닌, 값을 비교하도록 구현되었기 때문
- Constant Pool
- 참고 링크 : [Java String Pool](https://junhyunny.github.io/java/java-string-pool/)


</details>

#### 꼬리질문) String tokenizer와 split의 차이

<details>
    <summary>답변</summary>

- split : 분리의 기준이 되는 대상이 포함되지 않는다.
- tokenizer : 포함된다.

참고링크 : [https://velog.io/@junho5336/String.split의-limit](https://velog.io/@junho5336/String.split%EC%9D%98-limit)

</details>

---
</br>