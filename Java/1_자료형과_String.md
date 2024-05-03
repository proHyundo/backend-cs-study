
<!-- vscode-markdown-toc-config
	numbering=false
	autoSave=true
	/vscode-markdown-toc-config -->
※ 면접 대비 겸 학습의 목적으로 질문과 답변을 작성했습니다. 따라서 키워드 중심의 간결한 문체를 사용했음을 양해바랍니다. 잘못된 내용 또는 보강이 필요한 부분을 댓글로 말씀해주시면 정말 감사드리겠습니다. 답변의 출처는 대게 전문서적이며, 검색에 기반한 답변은 가급적 출처를 달았습니다.

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
- 컴퓨터의 최소 단위 bit는 2진수(0, 1) 값을 저장 가능
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

메서드를 호출할 때, 파라미터를 통해 `값`과 `참조` 무엇을 전달하는지에 따라 구분된다.

**pass by value**
- 매개된 실제 값을 복사하여 `복사된 값`만 전달되는 방식
- 기본 자료형은 항상 이에 해당

**pass by reference**
- 값이 아닌 객체에 대한 참조가 전달되는 방식
- 호출된 메서드에서 다른 객체로 대체하여 처리하면 기존 값은 바뀌지 않는다.
- 단, 매개변수로 받은 참조 자료형 안에 있는 변수를 변경하면, 데이터가 바뀐다.
    - 예제코드
    ```java
    public class PassbySample {
        public static void main(String[] args) {
            PassbySample passbySample = new PassbySample();
            ObjectA objectA = new ObjectA("origin name");

            passbySample.passByReference(objectA);
            System.out.println("objectA = " + objectA);

            passbySample.passByReferenceWithAccessOperator(objectA);
            System.out.println("objectA = " + objectA);
        }

        public void passByReference(ObjectA objectA) {
            objectA = new ObjectA("passByReference");
        }

        public void passByReferenceWithAccessOperator(ObjectA objectA) {
            objectA.name = "passByReferenceWithAccessOperator";
        }
    }
    ```
    ```bash
    출력결과 >> objectA = ObjectA(name=origin name)
                objectA = ObjectA(name=passByReferenceWithAccessOperator)
    ```

이렇게 두 가지 방식으로 구분되지만, 실질적으로 Java는 모든 메서드 호출에 있어 pass by value 방식을 사용하고 있다.
    
</details>

#### 꼬리질문1) Java는 왜 pass by value 만 존재하나요?

<details>
    <summary>답변</summary>

- 참조 자료형을 매개변수로 전달할 때, 직접적인 참조를 넘긴 게 아닌, 주소 값을 복사해서 넘긴다. 즉, Stack 영역에 새로운 파라미터에 선언한 변수를 생성하고, 주소 값을 복사해 할당한다. 때문에 pass by value 이다.

|![image](https://github.com/proHyundo/backend-cs-study/assets/128882585/dabbfe69-9071-49de-9aba-5e8145702bb6) | ![image](https://github.com/proHyundo/backend-cs-study/assets/128882585/b3bab7e5-a468-41f5-a846-3b62ba88675f) |
| --- | --- |
- run 메서드가 실행되며 매개변수를 전달받으면 다음과 같은 상황이 됩니다. arg1은 a1이 가지고 있는 주소값을 복사하여 독자적으로 가지게 됩니다. arg2도 마찬가지로 a2가 가지고 있는 주소 값을 복사하여 독자적으로 가지고 있게 됩니다. 주소 값을 복사하여 가져 가는 call by value가 발생한 것이죠.
- arg2에 arg1의 값을 저장한다고 해도 이는 run 메서드 내에 존재하는 arg2가 arg1이 가진 주소값을 복사하여 저장하는 것일 뿐 원본 a2와는 독립된 변수이기 때문에 원본 a2는 변경되지 않습니다.
- 이미지 출처 및 참고 링크 : [Java는 Call by reference가 없다](https://deveric.tistory.com/92)

|![image](https://github.com/proHyundo/backend-cs-study/assets/128882585/6a67859b-4f06-4fc7-ae65-9d25d3f22f38)|![image](https://github.com/proHyundo/backend-cs-study/assets/128882585/893c44ab-8117-4947-ba78-3d98969a18fa)|
|:---:|:---:|
|원시타입|참조타입|

- 이미지 출처 및 참고 링크 : [Pass By Value, Pass by Reference-항해일지:티스토리](https://internet-craft.tistory.com/2)


![백엔드_데브코스_2기_유도진](https://github.com/proHyundo/backend-cs-study/assets/128882585/7f350f0a-de0e-4ddc-8947-0f2eece42177)
- 이미지 출처 및 참고 링크 : [call by value vs call by reference - 유도진 | 백엔드 데브코스 2기 | 백둥이Deview 220329](https://youtu.be/34RAc5gdl54?si=J_yTUzFxmtjXrbXG) 



</details>

#### 꼬리질문2) String은 Primitive type(기본 자료형)이 아님에도 pass by value 방식을 따르고 있습니다. 어떤 특징 때문일까요?

<details>
    <summary>답변</summary>

- String pool을 통해 immutable(불변객체)로 관리되기 때문
- String의 경우 `""` 쌍따옴표로 값을 할당하면 `new` 연산자로 객체를 생성한 것과 같다.
```java
String strByDoubleQuotation = "string pool";
String strByNewOperator = new String("string pool");
String strByNewOperator2 = new String("string pool");
System.out.println("strByDoubleQuotation == strByNewOperator: " + (strByDoubleQuotation == strByNewOperator));
System.out.println("strByNewOperator == strByNewOperator2: " + (strByNewOperator == strByNewOperator2));
```

|![image](https://github.com/proHyundo/backend-cs-study/assets/128882585/a0940b7e-4e4e-44ed-ad34-f2b4a7e339f2)|
|---|
- 예제코드
```java
public class StringPassBy {
    public static void main(String[] args) {
        StringPassBy stringPassBy = new StringPassBy();
        String str = "origin name";
        
        stringPassBy.passByReferenceWithNewOperator(str);
        System.out.println("passByReferenceWithNewOperator str = " + str);
        
        stringPassBy.passByReferenceDoubleQuotation(str);
        System.out.println(" passByReferenceDoubleQuotation str = " + str);
    }

    public void passByReferenceWithNewOperator(String str) {
        str = new String("passByReferenceWithNewOperator");
    }

    public void passByReferenceDoubleQuotation(String str) {
        str = "passByReference";
    }
}
```
```bash
>> passByReferenceDoubleQuotation str = origin name
   passByReferenceWithNewOperator str = origin name
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
- equlas() : hashCode() 메서드를 호출하여 값을 비교한다.
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

- 객체의 동일 여부를 판단할 때
객체에 정의된 멤버의 값이 동일할 때 동일하다고 여기고 싶다면
equals() 메서드를 재정의 하게 될 것이다.

equals를 재정의 할 것이라면, 해당 객체를 key 값으로 사용하는 Collection에서 논리적으로 같은지 비교하기 위해 hashCode()도 같이 재정의 해야 한다

그런데 이때 hashCode를 잘못 재정의하게 되면 HashMap 의 버켓 들에 균등하게 요소들이 들어가지 않아 검색시간이 느려질 수 있음.

HashMap의 버켓은 배열로 되어 있고 각 요소는 링크드리스트로 구성되어 있음.
만약 같은 버켓에 많은 요소가 저장되면, 검색시간이 느린 링크드리스트 때문에 전체 조회 성능 떨어짐

해쉬값이 오버플로우나서 같아지면 안됨
- 참고 : 자바의 정석 2권 651 ~ 653

</details>

---
</br>

### 질문) String 클래스의 단점이 있다면 무엇일까요?

<details>
    <summary>답변</summary>

- String 클래스는 불변객체, 불변(Immutable)하다.
- 따라서 하나의 객체에 변경 작업이 계속되면, 변경(재할당)마다 새로운 객체를 Heap 영역의 String Pool 에 저장.
    1. String을 계속 더하는 작업을 하면 매번 new 연산을 수행해야 한다.
    2. 매번 기존 객체는 버려지고, 쓰레기가 됨. 저장되는 영역은 한계가 있다.
    3. 
    - JDK 1.5 부터 컴파일 과정에서 `+` 연산의 경우 StringBuilder으로 자동변환되어 성능최적화에 도움이 되지만, 반복문 내에서 new 연산자를 사용하기 때문에 명시적으로 반복문 외부에서 StringBuilder를 생성하는 것이 좋다.

</details>

#### 꼬리질문1) 불변이란 무엇이고 왜 String을 불변성을 띄게 설계되었을까?

<details>
    <summary>답변</summary>
</br>

불변성은 스레드 안전성을 제공하며, JVM 성능의 지속적인 개선으로 이전 객체를 변경하는 대신 새 객체를 더 빠르게 생성하는 경우가 많습니다.

1) 병렬 프로그래밍 환경에서 String Pool에 적재된 String 을 안전하게 공유하기 위함.
  - String은 Java 애플리케이션에서 가장 많이 사용될 데이터 타입이었다. 따라서 String pool에 String 객체를 공유해 임시로 생성된 String 객체를 줄여줌. 
1) String 객체를 수정할 수 있다면 Java의 전체 보안 모델이 깨질 것이기 때문.

- 참고 링크 : [Oracle Java Magazine, Why is Java making so many things immutable?](https://blogs.oracle.com/javamagazine/post/java-immutable-objects-strings-date-time-records)
- 참고 링크 : [Why String is Immutable or final in Java - 5 Reasons
](https://www.mimul.com/blog/why-string-class-has-made-immutable-or-final-java/)

</details>

#### 꼬리질문2) 불변한 String의 단점을 보완하는 방법이 있나요?

<details>
    <summary>답변</summary>

불변한 String의 단점을 보완하기 위해 가변적인 `StringBuffer`와 `StringBuilder` 클래스가 등장

**StringBuffer**
- 멀티쓰레드에 안전(thread safe)하도록 동기화 되어 있다

**StringBuilder**
- StringBuffer에서 쓰레드 동기화만 뺀 StringBuilder가 새로 추가

추가 키워드 : 동기화

참고 자료
- [Java Compiler Optimization for String Concatenation](https://medium.com/javarevisited/java-compiler-optimization-for-string-concatenation-7f5237e5e6ed)
- [jdk1.5에서 String 더하기의 컴파일시의 최적화](https://gist.github.com/benelog/b81b4434fb8f2220cd0e900be1634753)
- [String은 항상 StringBuilder로 변환될까?](https://siyoon210.tistory.com/160)

</details>

#### 꼬리질문3) 왜 동기화(synchronized)가 걸려있으면 느린걸까요?

<details>
    <summary>답변</summary>

싱글 스레드로 접근한다는 가정하에선 "StringBuilder" 와 "StringBuffer" 의 성능이 똑같을까요?
함정 질문입니다. synchronized 키워드를 달면 내부적으로 어떤 일이 벌어지는지 동작원리에 대해 잘 알아보세요!

</details>

#### 꼬리질문4) String 변수에 값을 할당할 때, `new` 연산자와 쌍따옴표`""`의 차이점은?

<details>
    <summary>답변</summary>

`new`
- 매번 heap 영역에 재생성

`""`
- 힙 영역 상수풀에서 재사용하여 불필요한 재생성 비용 X

</details>

#### 꼬리질문5) String은 참조자료형임에도 불구하고, 동일한 값을 갖는 두 객체(변수)에 `==` 연산의 결과가 True 입니다. 그 이유는 무엇일까요?

<details>
    <summary>답변</summary>

- equals()를 재정의해 주소 값이 아닌, 값을 비교하도록 구현되었기 때문
- Constant Pool
- 참고 링크 : [Java String Pool](https://junhyunny.github.io/java/java-string-pool/)


</details>

#### 꼬리질문6) String tokenizer와 split의 차이

<details>
    <summary>답변</summary>

- split : 분리의 기준이 되는 대상이 포함되지 않는다.
- tokenizer : 포함된다.

참고링크 : [https://velog.io/@junho5336/String.split의-limit](https://velog.io/@junho5336/String.split%EC%9D%98-limit)

</details>

---
</br>