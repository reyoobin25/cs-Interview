# JAVA 기술 면접 질문 정리

> 지금까지 기술 면접에서 받은 질문들과 자바를 공부하며 중요한 개념이라고 생각한 키워드를 정리하고 설명은 추가해 나가기

- [자바 언어의 특징](#user-content-자바-언어의-특징)
- [자바 언어 == 객체지향](#user-content-자바-언어와-객체-지향) 
- [객체 지향 설명 및 절차지향, 함수적 프로그래밍에 대한 설명](#user-content-함수형-프로그래밍)
- [JAVA SE와 EE의 차이점](#user-content-java-se-vs-java-ee) 
- [JRE와 JDK 차이 설명](#user-content-jre-vs-jdk)
- [Garbage Collection](#user-content-garbage-collection)
- [컴파일 과정에 대한 설명](#user-content-java-compile)
- [자바 1.8버전에서 달라진 점](#user-content-java-se-8)
  - 람다 표현식
  - 스트림 api
  - java.time 패키지
- 추상화, 인터페이스 설명
- 접근제한자 설명
- 다형성 설명
- 상속 설명
- 오버로딩, 오버라이딩 
- 직렬화, 역직렬화
- 메모리 영역 설명
- [String, StringBuffer, StringBuilder](#user-content-string-stringbuffer-stringbuilder)
- final, finalize, finally
- [call by value, call by reference](#user-content-call-by-value-vs-call-by-reference)
- Collection api설명
- Synchronized 
- Equals메소드
- 리플렉션
- 제네릭 
- 쓰레드





## 자바 언어의 특징

- 객체 지향 프로그래밍

  - 자바는 객체 지향 프로그래밍 언어로서 객체 지향의 특징을 가지게 된다.
- 플랫폼에 독립적

  - C,C++ 등과 같은 다른 언어와 다르게 플랫폼에 독립적이다.

  - 한번 컴파일된 바이트 코드는 여러 플랫폼에서 실행될 수 있다. (이식성이 뛰어나다)

- 단순하다
  - 자바는 매우 쉽게 배울 수 있다.
  - 문법이 간단 명료하여 이해하기 쉽다.
- 멀티 스레드
  - 여러 스레드를 정의하여 한 번에 많은 작업을 처리하는 Java 프로그램을 작성할 수 있다.
- 메모리 관리가 필요없다
  - JVM의 가비지 컬렉션
- 동적 로딩
  - 각 객체는 필요한 시점에 클래스를 동적으로 생성된다.



## 자바 언어와 객체 지향

자바는 객체 지향 프로그래밍이다.

> 객체 지향 프로그래밍
> : 프로그램을 단순히 데이터와 처리 방법으로 나누는 것이 아니라, 프로그램을 수많은 '객체'라는 기본 단위로 나누고 이 객체들의 상호작용으로 서술하는 방식(나무위키:[https://namu.wiki/w/객체 지향 프로그래밍](https://namu.wiki/w/객체 지향 프로그래밍))



객체 지향 프로그래밍의 특징 (상추캡이다)

- 상속

  - 어떠한 클래스를 상속받게 되면 해당 클래스의 필드와 메소드를 사용할 수 있게 되는 것.

- 추상화

  - 객체들의 공통적인 속성들과 행동들을 묶어 놓는 것.

    ```java
    public abstract class Car{
        Handle handle;
        Tire tire;
        abstract void run();
    }
    ```

- 캡슐화

  - 변수와 함수를 하나의 단위로 묶는 것.

  - 정보 은닉 : 내부의 로직을 외부에 공개하지 않고 기능만을 제공하는 것.

    ```java
    /** 외부에 공개 **/
    public Result process(){
       ...
       secret();
       return result;
    }
    /** 비공개 **/
    private void secret(){
        ...
    }
    ```

- 다형성

  - 같은 이름을 가진 메서드이지만 서로 다른 객체에서 다르게 작동하는 것.



## 함수형 프로그래밍

### 함수형 프로그래밍?

자료 처리를 수학적 함수의 계산으로 취급하는 프로그래밍 패러다임.

`P(x) = f º g º h º ...`  이런 모양으로 



### 함수형 프로그래밍 키워드

- Immutable DataStructure 

  - 원본 데이터에 대해 변경을 일으키지 않는다.

  - 데이터 변경이 필요한 경우, 원본 데이터에서 복사를 한 후 변경한다.

    ```java
    Number number = new Number(3);
    
    System.out.println(addThree(number).getNum());//6
    
    System.out.println(number.getNum()); //3
    
    public Number addThree(Number number){
    	return new Number(number.getNum()+3);
    }
    ```

- Pure Function

  - 언제 어디서든 같은 입력에 같은 결과를 출력하는 함수

    ```java
    String name = "Minsoo";
    System.out.println(helloMethod(name)); //helloo Minsoo
    System.out.println(helloMethod(name)); //hello Minsoo
    System.out.println(name); //Minsoo
    ...
        
    public String helloMethod(String name) {
            return "hello " + name;
    }
    ```

  - 순수함수가 아닌 경우

    ```java
    System.out.println(getSecond());
    System.out.println(getSecond());
    System.out.println(getSecond());
    
      
    public int getSecond() {
        return new Date().getSeconds();
    }
    ```

    

### 왜 함수형 프로그래밍을 배우는가

1. 불필요한 코드를 줄일 수 있다 / 코드의 가독성이 좋아진다 

   ```java
   // 명령형 (어떻게에 집중)
   public Map<Integer, Integer> numberOfWordsPerLengthImperative(List<String> words){
       Map<Integer,Integer> result = new HashMap<>();
       for(int i=0;i<words.size();i++){
           int key = words.get(i).length();
           result.put(key,result.getOrDefault(key,0)+1);
       }
   }
   // 함수형 (무엇에 집중)
   public Map<Integer,Integer> numberOfWordsPerLengthFunctional(List<String> words){
       return words.stream().collect(Collectors.groupingBy(String::length,Collecors.counting()));
   }
   ```

2. 순수 함수는 테스트를 용이하게 만든다. (같은 입력에는 항상 같은 결과를 배출하므로)

   



## Java SE vs Java EE

Java 기술은 프로그래밍 언어이자 플랫폼이다.

Java 플랫폼은 Java 프로그래밍 언어 응용 프로그램이 실행되는 특정 환경이다.

플랫폼 종류

- Java SE (Standard Edition)

  - 가장 기본이 되는 플랫폼
  - SE의 API는 자바 언어의 핵심 기능 들을 제공한다
  - 기본적인 type부터 object, 네트워킹, 보안, 데이터베이스 엑세스, GUI 등 고급 클래스까지 모든 것을 정의
  - 핵심 API 외에도 가상 머신, 개발 툴, 배포 기술 및 자바 기술 애플리케이션에서 일반적으로 사용되는 기타 클래스 라이브러리 및 툴킷으로 구성

- Java EE(Enterprise Edition)

  - Java SE 플랫폼이 기초가 된다.

  - 대규모 멀티 계층, 확장성, 신뢰성 및 보안 네트워크 애플리케이션을 개발하고 실행할 수 있는 API 및 런타임 환경이 추가됨.

    > 대규모 멀티 계층이란?
    >
    > 응용 프로그램을 클라이언트 계층,웹 계층,비즈니스 계층 등으로 나누는 것.
    >
    > 클라이언트 계층 = Java EE 서버에 요청하는 곳(웹 브라우저, 독립형 응용 프로그램 등)
    >
    > 웹 계층 = 클라이언트 계층과 비즈니스 계층 간의 상호 작용을 처리하는 곳
    >
    > - 클라이언트를 위한 동적인 컨텐츠를 생성
    > - 클라이언트의 사용자로부터 입력을 수집하고 비즈니스 계층에 리턴
    > - 클라이언트의 화면 또는 페이지 흐름 제어
    > - 사용자 세션 데이터 유지
    > - Servlet, JSP, JSP Expression Language 등의 기술이 사용됨.
    >
    > 비즈니스 계층 = 비즈니스 로직이 만들어진 곳
    >
    > - EJB, JAX-RS RESTful web service, Java Persistence API Entity 등의 기술이 사용
    >
    > 엔터프라이즈 정보 시스템(ELS) 계층
    >
    > - 데이터베이스 서버같은 Java EE 서버가 아닌 별도의 시스템
    > - JDBC, Java Persistence API, Java Transaction API 등의 기술이 사용



## JRE vs JDK

JRE (Java Runtime Environment) : 자바 **실행 환경**

JVM이 자바 프로그램을 동작시킬 때 필요한 라이브러리 파일들과 기타 파일들을 가지고 있다.



JDK (Java Development Kit) : 자바 **개발 도구**

JRE + 개발을 위해 필요한 javac나 java 등을 포함한다 



## Garbage Collection

> 'Stop the World'
>
> - GC를 실행할 때 , GC를 실행하는 쓰레드를 제외한 나머지 쓰레드는 모두 작업을 멈춘다.
>
> - 어떤 GC 알고리즘을 사용하더라도 stop the world는 발생
>
> - 대개 GC 튜닝이란 이 stop the world의 시간을 줄이는 것




- 가비지 컬렉션은 Heap 영역에 존재
- Heap 영역은 3개의 영역으로 나뉜다.
  - Young 영역
  - Old 영역
  - Permanent Generation 영역


### Young 영역

- Young 영역은 3개의 영역으로 나뉜다.
  - Eden 
    - 새로 생성된 대부분의 객체들이 저장
    - 대부분의 객체는 금방 접근 불가능 상태가 된다. = 앞으로 사용되지 않을 객체
    - 접근 불가능 상태가 된 객체는 이 영역에서 제거된다 = Minor GC
  - Suvivor 1
    - Eden 영역에서 GC가 일어난 후 살아남은 객체들이 저장
    - Suvivor 1의 영역이 가득차게 되면 그 중에서 살아남은 객체만 다른쪽 Suvivor 영역으로 옮긴다.
    - Eden 영역에서는 이제 Suvivor 2 영역을 바라본다
  - Suvivor 2
    - 마찬가지로 이 영역이 가득차게 되면 Suvivor 1영역으로 다시 객체를 이동 시킨다. 
    - Eden 영역은 Suvivor 1 영역을 바라본다.
- 위의 과정을 반복하다가 계속해서 살아남아 있는 객체는 Old 영역으로 이동한다.

### Old 영역

- 접근 불가능 상태로 되지 않아 Young 영역에서 살아남은 객체가 여기로 복사된다.
- 대부분 Young 영역보다 메모리를 크게 할당
  - 크기가 커진만큼 Young 영역보다 GC가 덜 발생한다.
- 기본적으로 데이터가 가득차면 GC를 실행한다.
  - Major GC가 발생한다고 말한다.



### Permanent Generation 영역

...



## Java Compile

<img src="https://t1.daumcdn.net/cfile/tistory/9919763E5C8FB79424"/>

#### 컴파일

프로그래머가 구현한 소스 코드를 컴퓨터가 이해할 수 있는 기계어로 변환시키는 과정을 말한다.

#### 자바 컴파일러

자바 소스를 <u>JVM이 읽을 수 있도록</u> 변형시키는 것

#### 클래스 파일 

자바 컴파일러를 통해서 나온 결과로 JVM이 읽을 수 있도록 <u>번역된 바이트코드</u>

#### 코드가 실행되기 까지의 과정

1. 자바 소스 코드가 컴파일러에 의해서 바이트코드(클래스파일)로 번역됨
2. JVM의 Class Loader가 해당 클래스 파일을 메모리에 로딩(적재)
3. 실행 엔진이 메모리(런타임 데이터 영역)에 배치된 바이트코드를 해석하고 실행



## Java SE 8

Java SE 8에서 변경되거나 새롭게 추가된 사항들

1. 람다 

   -  함수형 프로그래밍을 제공하기 위해 만듬.

   - 사용하면서 느낀 장점

     - 보일러 플레이트 코드(불필요한 선언, 변수)가 필요없다.

       ```java
       new Thread(new Runnable() {
           public void run() {
               System.out.println("전통적인 방식의 일회용 스레드 생성");}}).start();
       
       new Thread(()->{
           System.out.println("람다 표현식을 사용한 일회용 스레드 생성");}).start();
       ```

     - 코드가 간결해지고 의도를 파악하기 쉽다.

2. 스트림 API : 데이터의 추상화

   - 배열이나 컬렉션 등의 데이터에 접근하기 위해 반복문이나 iterator를 사용해야 함.
   - 가독성이 떨어지고, 불필요한 선언이 많아짐

   ```java
   List<String> list = new ArrayList<String>();
   //리스트 요소 중 길이가 3이상인 요소의 개수 구하기
   int cnt = 0;		//중간에 변수가 필요
   for(int i=0;i<list.size();i++){ //i라는 변수가 필요
       String temp = list.get(i);	//temp라는 변수가 필요
       if(temp.length() >= 3)
           cnt++;
   }
   System.out.println(cnt);
   //람다로 표현하면
   long count = list.stream().filter(s->s.length() > 3).count();
   System.out.println(count);
   ```

3. java.time 패키지 : Joda-Time을 이용한 새로운 날짜와 시간 API

   - 기존의 Date나 Calendar 클래스의 단점
     - Calendat 인스턴스는 가변 객체라서 도중에 값이 수정될 수 있다.
     - 윤초와 같은 특별한 상황이 고려되지 않음
     - 월을 나타낼 때 0~11로 표현함
     - 일관성 없는 요일 상수(1~7), Date는 (0~6)
   - time 패키지는 이러한 단점들을 보완함.
     - 불변 객체로 생성
     - 월을 1~12로 표현
     - plusDays , minusSeconds 등의 메소드가 있음.

## Interface vs Abstract

|            | interface                                                 | abstract class                                      |
| ---------- | --------------------------------------------------------- | --------------------------------------------------- |
| 상속(구현) | 다중 구현 가능                                            | 다중 상속 불가능                                    |
| 접근제한자 | public, default                                           | public, default, protected                          |
| 목적       | 서로 관련성이 없는 클래스들을 인터페이스로 묶고 싶은 경우 | 관련성이 높은 클래스 간에 코드를 공유하고 싶은 경우 |
|            |                                                           | 일반 메소드, 필드 선언 가능                         |
|            |                                                           |                                                     |
|            |                                                           |                                                     |

### interface

- public, default 접근 제한자 가능

  - java 8 버전에서는 default가 가능해지면서 interface안에 메소드를 구현할 수 있게 됨.

    ```java
    public interface Calculator{
        default int add(int x, int y){
            return x+y;
        }
    }
    ```

- static 메서드가 가능해짐

  ```java
  public interface Calculator{
      public static int add(int x, int y){
          return x+y;
      }
  }
  ```

- 다중 상속이 가능함



### abstract class

- public , default, protected , private 가능

## String StringBuffer StringBuilder

String

- 불변 객체이다.

- ex) abc -> abcd로 바꿀경우

  ```java
  String s1 = "abc";
  String s1 = s1+"d";//기존의 객체를 변경시키는게 아니라 새로 생성
  ```

- 메모리 오버

  ```java
  String s1 = "Abc";
  for(int i=0;i<1000000;i++){
      s1 += "a";
  }
  //GC
  ```

StringBuilder,StringBuffer

- 가변 객체
- 연산이 필요할 때 크기를 변경시켜서 문자열을 변경
  - 문자열 연산이 자주 있을 때 사용하면 성능이 좋음
- 이 둘의 차이점?
  - 동기화 여부
  - StringBuilder(동기화 처리가 x) 
    - 멀티 쓰레드 환경에 적합하지 않음
    - 싱글 쓰레드 환경에서는 더 나은 성능을 보임
  - StringBuffer(동기화 처리가 됨)
    - 멀티 쓰레드 환경에 안정적
    - 싱글 쓰레드 환경에서는 StringBuilder에 비해 낮은 성능



## Call by Value vs Call by Reference

|        | Call by value                                                | Call by reference                                            |
| :----: | :----------------------------------------------------------- | :----------------------------------------------------------- |
|  정의  | 함수를 호출하는 동안, 복사한 변수들의 값을 전달하는 것       | 함수를 호출하는 동안, 변수들의 주소를 복사하여 전달하는 것.  |
|  효과  | 변수 사본에서 변경된 사항은 함수 외부의 변수 값을 수정하지 않는다. | 변수의 변경은 함수 외부의 변수 값에 영향을 준다.             |
|  전달  |                                                              | 변수의 주소를 저장하기 위한 포인터 변수 필요                 |
| 메모리 | 복사하기 위한 메모리 공간 필요                               | 기존의 메모리 공간                                           |
|  장점  | 실제 변수에 어떠한 영향도 미치지 않는다                      | 변수 값을 변경할 수 있다. <br />복사하기 위한 메모리 공간이 필요없다.<br /> |
|  단점  | 변수에 대해 복사를 해야 하므로 메모리가 효율적이지 않다.     | Null 검사를 해야한다.<br />순수 함수가 아니다.<br />멀티스레드 프로그램에서 위험 요소가 될 수 있다. |

