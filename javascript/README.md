# 질문 리스트

- [자바스크립트란?](#user-content-자바스크립트란)
- [프로토타입](#user-content-prototype)
- [자바스크립트 엔진](#user-content-자바스크립트-엔진)
  - 자바스크립트 엔진의 영역
    - [Call Stack](#user-content-call-stack)
    - [Web API](#user-content-web-api)
    - [Task Queue](#user-content-task-queue)
- [함수의 범위](#user-content-함수의-범위)
  - [전역 변수와 지역 변수](#user-content-전역-변수와-지역-변수)
  - [Scope Chain](#user-content-scope-chain)
  - [Lexical Scoping](#user-content-lexical-scoping)
- [실행 컨텍스트](#user-content-실행-컨텍스트)
- [호이스팅](#user-content-호이스팅)
- [strict mode](#user-content-strict-mode)
- [this Keyword](#user-content-this-keyword)
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

이러한 구조로 인해서 Person의 객체는 모두 prototype이라는 속성을 가지게 되고 Prototype Object에 접근할 수 있게됩니다. 그리고 이러한 Prototype Object는 다시 \__poroto__으로 자신보다 더 깊은 곳에 있는 객체에 접근할 수 있습니다. 최종적으로 Object가 그 예입니다.

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



## 자바스크립트 엔진

자바스크립트 엔진이란 JS 코드를 실행하는 프로그램 또는 인터프리터를 말한다. 자바스크립트 엔진은 전통적인 인터프리터일 수도 있고, 특정한 방식으로 바이트코드로 JIT 컴파일을 할 수 있다. ([위키]([https://ko.wikipedia.org/wiki/%EC%9E%90%EB%B0%94%EC%8A%A4%ED%81%AC%EB%A6%BD%ED%8A%B8_%EC%97%94%EC%A7%84](https://ko.wikipedia.org/wiki/자바스크립트_엔진)))

예) 웹 브라우저

 

### 자바스크립트 엔진의 종류

- 스파이더몽키(파이어폭스)

- V8(크롬, node.js 등에서 사용)

- 웹킷(사파리, React Native 등에서 사용)

- 차크라(익스플로러)

- 그 외 다수의 프로젝트

  

### 자바스크립트 엔진 파이프라인

자바스크립트 엔진들이 소스 코드를 기계어로 만들기까지 공통적을 수행하는 과정

![](../img/js-engine-pipeline.PNG)

1. 자바스크립트코드가 자바스크립트 엔진(parser)에 의해 추상 구문 트리(AST)로 만들어진다.
2. 인터프리터가 AST를 기반으로 바이트 코드를 생성 

### 자바스크립트 엔진의 영역

자바스크립트 엔진은 여러가지 영역으로 나뉜다,

#### Call Stack

- 자바스크립트는 단 하나의 호출 스택을 사용한다 
- 함수가 실행되면 이 함수의 실행이 끝날 때까지 다른 어떤 task도 수행될 수 없다.
- Run on Completion
- 자바스크립트는 어떻게 Ajax로 데이터를 불러오면서 mouseover 이벤트를 처리하고 많은 일들을 동시적으로 처리할 수 있는 것인가?

#### Web API

- 브라우저에서 제공되는 API
- ajax나 timeout 등의 비동기 작업을 실행
- 자바스크립트 엔진이 Web API에 함수를 요청하고 동시에 콜백함수까지 넘겨준다.
- Web API는 요청받은 함수를 완료하고 전달받은 콜백함수를 Task Queue에 넘겨준다.

#### Task Queue(Event Queue)

- 자바스크립트의 런타임 환경에서는 처리해야 하는 Task들을 임시 저장하는 대기 큐가 존재한다.

- 이 대기 큐를 Task Queue 또는 Event Queue라고 한다.

- Call Stack이 비어졌을 때 Task Queue 대기열의 Task를 다시 꺼내서 실행한다.

- 자바스크립트에서 비동기로 호출되는 함수들은 Call Stack에 쌓이지 않고 Task Queue에 넣어지게 된다. 

  - 이벤트(click, mouseover, ...)에 의해 실행되는 함수들이 비동기로 실행.
  - 자바스크립트 엔진이 아닌 Web API 영역에 정의된 함수들도 비동기로 실행.

- 예제

  ```javascript
  function test1(){
      console.log("test1"); //2. 출력
      test2();	//3. call stack에 추가
  }
  function test2(){
      setTimeout(function(){	//4. 이벤트 큐에 추가
          console.log("test2");
      },0);
      test3(); //5.call stack에 추가
  }
  function test3(){
      console.log("test3");//출력
  }
  
  test1();	// 1. call stack에 추가
  
  --------------
  결과
  --------------
  test1
  test3
  test2
  ```




## 함수의 범위

### 전역 변수와 지역 변수

**전역 변수**란 자바스크립트에서 제일 바깥 범위에 변수를 만드는 것 = [window 객체](https://www.zerocho.com/category/Javascript/post/573b321aa54b5e8427432946)에 변수를 만드는 것

**지역 변수**란 변수 영역이 특정한 영역으로 정해진 변수.

```javascript
var name = "kim"; //전역 변수  window.name or name 으로 어디서든 접근가능.
function call(){
    var name="mo"; //지역 변수
    alert(name);
}
call();
alert(name); 	//kim
```

### Scope Chain

내부 범위에서 외부 범위의 변수에 접근 가능하지만, 외부에서는 내부 범위의 변수에 접근할 수 없다.

내부 범위에서 변수를 찾기 위해 먼저 자기 자신 그 후에는 외부의 범위에 선언된 변수를 찾고, 또 그마저도 없으면 한 단계씩 범위를 벗어나 바깥쪽에서 찾는 이러한 것을 ''**스코프 체인**''이라고 한다.

### Lexical Scoping

스코프는 함수를 호출할 때가 아니라 선언의 위치에 따라 정해진다.

```javascript
var name="kim";
function log(){
    console.log(name); //자신의 영역에 name이라는 변수가 없어 자신의 외부영역(전역)에서 name을 찾음
}
function wrapper(){
    var name = "mo";
    log();
}
wrapper(); // "kim"
```

### 전역변수는 지양하라

한 사람이 개발하는 것이 아니라 여려 명과 협동을 해야하고, 다른 사람의 라이브러리를 사용하는 일도 있다. 때문에 전역변수를 계속사용하다 보면 이름이 overwrite되어 내용이 사라지는 일이 발생할 수도 있다. 

해결 방법 : 네임스페이스

```javascript
//name.js
var name={
    name: "kim"
}

//person.js
var person={
    name: "person";
}

//main.js
console.log(name.name);
console.log(person.name);
```



## 실행 컨텍스트

코드를 실행하게 되면 아래와 같은 원칙에 의해 컨텍스트가 생성된다

1. 전역 컨텍스트 하나를 생성 후, 함수 호출 시마다 함수 컨텍스트가 생성
2. 컨텍스트 생성 시 컨텍스트 안에 변수객체,  스코프 체인, this가 생성
3. 컨텍스트 생성 후 함수가 실행되는데, 사용되는 변수들은 변수 객체 안에서 값을 찾고, 없다면 스코프 체인을 따라 검색한다.
4. 함수 실행이 마무리 되면 해당 함수 컨텍스트는 사라진다. 

### 전역 컨텍스트

- 전역 변수, 전역 변수 객체(함수)
- scope chain은 없다. 자기 자신이 제일 바깥 쪽의 범위이기 때문
- this 는 window 객체를 나타난다.

### 함수 컨텍스트

- 함수가 호출될 때 컨텍스트가 생성
- arguments(함수 호출 시 전달된 인자값), 지역 변수
- scope chain 
- this는 따로 설정하지 않았다면 window 객체

## 호이스팅

자바스크립트에서는 변수는 사용된 후에 선언할 수 있다. 다른 말로 변수는 선언되기 전에 사용할 수 있다.

```javascript
x = 5;
elem = document.getElementById("demo");
elem.innerHTML = x;

var x;
```

- 호이스팅이란 현재 scope에 해당되는 모든 선언을 상단으로 이동시키는 javascript의 기본 동작입니다.

- 초기화 된 것이 아닌 선언된 것만 호이스팅합니다.

  ```javascript
  var x = 5; // Initialize x
  
  elem = document.getElementById("demo");
  elem.innerHTML = x + " " + y;           // 5 undefined
  
  var y = 7; // Initialize y
  ```

> let과 const  키워드는 호이스팅 되지 않습니다.

## Strict Mode

`"use strict"l;`가 선언된 자바스크립트는 엄격 모드로 실행한다

- 선언되지 않은 변수에 대해 에러를 발생시킨다.

  ```javascript
  "use strict";
  x = 3.14; // Error
  ```

- 함수 안에 선언할 수도 있으며, 함수 안에 선언하면 오직 그 범위 안에서만 엄격 모드를 실행한다.

  ```javascript
  x = 3.14;       // This will not cause an error.
  myFunction();
  
  function myFunction() {
    "use strict";
    y = 3.14;   // This will cause an error
  }
  ```

- 엄격 모드에서는 변수 또는 객체, 함수를 지우는 것을 허락하지 않는다

  ```javascript
  "use strict";
  var x = 3.14;
  delete x;  //error
  ```

- 중복되는 파라미터 이름을 잡을 수 있다

  ```javascript
  "use strict";
  function x(p1,p1){};
  ```

- read-only 프로퍼티를 허락하지 않는다.

  ```javascript
  "use strict";
  var obj = {};
  Object.defineProperty(obj, "x", {value:0, writable:false});
  
  obj.x = 3.14;            // This will cause an error
  ```

- get 프로퍼티만 사용하는 것을 허락하지 않는다.

  ```javascript
  "use strict";
  var obj = {get x() {return 0} };
  
  obj.x = 3.14;            // This will cause an error
  ```

- 지우지 말아야 할 것들 지우는 행동을 허락하지 않는다.

  ```javascript
  "use strict";
  delete Object.prototype; // This will cause an error
  ```

- 자바스크립트 예약 키워드를 변수 이름으로 사용하는 것을 허락하지 않는다.

  - let, package, private, public, static, implements  ...



### 왜 Strict mode를 사용하는가

strict mode를 사용하면 javascript를 좀 더 안전하게 작성할 수 있다.

예를 들어, 일반적으로 javascript에서 이름을 잘못 입력하면 새로운 전역 변수가 만들어지게 된다. strict mode에서는 이를 에러로 반환시켜 예상치 못한 전역 변수가 만들어지는 것을 방지한다.

## this Keyword

- 객체의 this 키워드는 그 메소드의 owner로 지정

  ```javascript
  var person = {
    firstName: "John",
    lastName : "Doe",
    id     : 5566,
    fullName : function() {
      return this.firstName + " " + this.lastName;//John Doe
    }
  };
  ```

- 단독 사용일 때에는 글로벌 객체(window)를 나타낸다.

  ```javascript
  var x = this; // object Window
  ```

- 함수 안에서 this 키워드는 글로벌 객체(window), 'strict mode'에서는 'undefined'

  ```javascript
  function myFunction() {
    return this;	//object Window
  }
  ```

  ```javascript
  "use strict";
  function myFunction() {
    return this;	// undefined
  }
  ```

- 이벤트 핸들러에서는 HTML 엘리먼트를 나타낸다.

  ```html
  <button onclick="this.style.display='none'"> <!-- Element -->
    Click to Remove Me!
  </button>
  ```

- 

## 자바 vs 자바스크립트

