# 질문 리스트

- [자바스크립트란?](#user-content-자바스크립트란)
- [프로토타입](#user-content-prototype)
- [자바 vs 자바스크립트](#user-content-자바-vs-자바스크립트)







## 자바스크립트란

> 프로그래밍 언어 = 소프트웨어를 작성하기 위한 언어(C, C++, Java ...)
>
> 스크립트 언어 = 소프트웨어를 제어하는 컴퓨터 프로그래밍 언어(Javascript, Ruby, Shell script ...)

자바스크립트의 특징

- 인터프리터 언어
  - 컴파일 언어와 같이 전체 소스를 컴파일하고 링크 하는 등의 과정없이 즉시 실행이 가능하다.
- 웹페이지의 스크립트 언어
  - 웹 브라우저 위에서 실행
    - 운영체제나 플랫폼에 영향을 받지 않는다.
  - 페이지에서 이벤트 발생시 어떻게 작동하는지 디자인
  - 웹 페이지를 동적으로 제어할 수 있게 해준다
- 브라우저가 아닌 환경에서도 사용(node.js, mongoDB, couchDB 등)
- 프로토타입 기반 언어
  - 객체 지향 프로그래밍의 한 형태
  - 클래스가 없다
    - 상속 기능이 없다. 
    - ES6부터 Class 문법이 추가됨. 하지만 문법이 추가된 것이지, 자바스크립트가 클래스 기반으로 바뀌었다는 뜻은 아니다.
  - 프로토타입(객체 원형)을 기반으로 상속을 흉내내도록 구현
  - [https://medium.com/@bluesh55/javascript-prototype-이해하기-f8e67c286b67](https://medium.com/@bluesh55/javascript-prototype-이해하기-f8e67c286b67)
- 다중 패러다임 스크립트 언어
  - 객체 지향 프로그래밍 , 함수형 프로그래밍

- 단점
  - 소스코드가 그대로 보인다.
  - 디버깅도구와 개발도구의 부족



## Prototype


### Prototype = Prototype Link + Prototype Object

객체는 언제나 `함수`로 생성된다.

```javascript
function Person(first,last,age,eyecolor){
    this.firstname = first;
    this.lastname = last;
    this.age = age;
    this.eyecolor = eyecolor;
} // 함수
var personObject = new Person("John", "Doe", 50, "blue"); // 객체


var car = {
    type:"sedan",
    run : function(){
    }
};
//이런 모습도 사실은
var car = new Object(){
    this.type = "sedan";
    this.run = function(){};
}
```

객체를 정의할 때 (=함수를 정의할 때)에는 2가지 일이 동시에 이루어진다.

1. 해당 함수에 Constructor 자격 부여

   ```javascript
   var obj = {}; //undefined
   var a = new obj(); // Error -> is not a consturctor
   ```

   생성자가 아니라면 객체를 만드는일(new 연산자)를 할 수 없다.

2. 해당 함수의 **Prototype Object** 생성 및 연결

   함수를 정의하면  `Prototype Object`그리고 `해당 함수`가 생성되며 각각의 속성에 서로를 가리키도록 함.

   - 해당 함수
     - prototype : Prototype Object을 가리킨다.
   - Prototype Object
     - constructor : 해당 함수를 가리킨다.
     - \__proto__ : **Prototype Link.**

이러한 구조로 인해서 Person의 객체는 모두 prototype이라는 속성을 가지게 되고 Prototype Object에 접근할 수 있게됩니다. 

#### 데이터를 변경하면?

데이터를 변경하게 되면 어떻게 될까?

```javascript
kim = new Person();
lee = new Person();
kim.eyes // 2
lee.eyes // 2
kim.eyes=3
lee.eyes // 2
```

이렇게 값을 변경하게 되었을 때 다른 객체에 영향을 미칠 것 같지만 kim.eyes를 3이라는 값으로 선언하게 되었을 때는 Person 함수에 eyes:3 이라는 값을 설정하게 된다.

```javascript
> kim
  Person {eyes: 3}
	eyes: 3
    prototye:
		eyes: 2
		nose: 1
		constructor: f Person()
		__proto__: Object
```

### 결론

- 자바 스크립트는 프로토 타입 기반 언어

- 프로토 타입 프로그래밍 
  - 객체 지향 프로그래밍의 한 부분
  - 클래스가 없음 -> 상속 기능도 없음
    - ES6부터 class 문법이 추가되었지만, 자바스크립트가 클래스 기반이 된 건 아님
  - 프로토 타입 프로그래밍은 상속을 흉내내도록 프로토타입을 사용한다.
    - 객체를 생성하면 속성과 메서드를 인스턴스마다 추가해야함(메모리가 쭈욱 쭈욱)
    - 프로토타입을 이용하게 되면 공유 속성, 메서드를 이용하기 때문에 (메모리 절약의 효과?)

## 자바 vs 자바스크립트

