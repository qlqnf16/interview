# JavaScript

## JavaScript의 동작원리

### JavaScript Runtime의 구성 요소

* JS Engine: Chrome, Node.js에서 사용하는 `Google V8` 엔진
  * Memory heap: 메모리 할당이 일어나는 곳
  * Call Stack: 코드 실행에 따라 호출 스택이 쌓이는 곳
* Web API
* Event Loop & Callback Queue



JavaScript는 기본적으로 싱글스레드. 따라서 콜스택에서 함수가 수행되는 동안 다른 작업은 불가능함 => 작업시간이 긴 요청이 들어올 경우, 브라우저가 멈춘 것 처럼 보이는 현상이 일어나게 된다.

이를 해결하기 위한 방법이 비동기 콜백(Asynchronous Callback). 

![img](https://media.vlpt.us/post-images/yejinh/839084c0-2743-11ea-988d-bf9260599c22/1FA9NGxNB6-v1oI2qGEtlRQ.png)

1. 함수가 실행되면 Call Stack에 쌓임
2. Web API를 이용하는 경우, 그 함수가 Web API를 요청, Web API 공간으로 옮겨져 수행됨.
3. Web API에서 수행이 완료되면 해당 함수의 콜백함수가 Callback Queue로 이동
4. Event Loop는 Call Stack과 Callback Queue를 바라보고 있다가, Call Stack이 비면 Callback Queue의 함수를 Call Stack으로 쌓고, 함수가 실행됨



## Variables

### 자료형

==원시타입== 

* 종류: String, Number, Boolean, Null, Undefined, Symbol

  * Symbol: ES6에서 추가됨. 유일한 객체 키를 만들기 위해 사용

* Literal로 표현 가능하다.

  ```js
  const sampleString = 'This is string variable';
  const sampleNumber = 50;
  const sampleBoolean = true;
  const sampleNull = null;
  const sampleUndefined = undefined;
  ```

* 변수에 할당할 때 값 자체가 그대로 저장된다. (메모리 참조가 아닌 값의 복사)

  ```js
  let age1 = 27;
  let age2 = age1;
  age1 = 28;
  console.log(age2); 				// 27
  ```

==참조타입==

* 종류: Object, Array, Function

* 변수에 할당할 때 값이 아닌 데이터의 참조만 저장된다.

  ```js
  const person = { age: 27 };
  const person2 = person1;
  person.age = 28;
  console.log(person2.age);		// 28
  ```



### const, let, var

==var==

* 함수 스코프
* 호이스팅이 일어남
* 변수의 재선언 / 재할당 가능

==const & let==

* 블록 스코프

* 호이스팅이 일어남. 단, 값이 대입되기 이전 변수는 TDZ(Temporal Dead Zone)에 위치해 접근할 수 없음

  ```js
  console.log(a);
  const a = 5;			// ReferenceError: Cannot access 'a' before initialization
  ```

* 변수의 재선언 불가능, const 는 재할당 불가능

  ```js
  const a = 5;
  a = 4;					// TypeError: Assignment to constant variable.
  ```

  ```js
  let b = 'hello';
  b = 'world';
  console.log(b);	      // world
  let b = 'hi';			// SyntaxError: Identifier 'b' has already been declared
  ```




### 호이스팅

변수가 선언, 할당됐을 때 변수의 선언부가 코드의 최상단으로 끌어올려지는 현상. 

함수의 경우, **함수 선언식은 함수 통째로 호이스팅**되어 사용할 수 있다. 반면, 함수 표현식은 선언부만 호이스팅되어 사실 undefined 상태이기 때문에, 선언부분 이전에 호출하면 에러가 발생한다.

```js
// 이 코드는 
console.log(num);				// undefined
var num = 5;

// 사실 이 코드처럼 작동한다
var num;
console.log(num);
num = 5;
```



## 비동기처리

### Promise

JavaScript는 기본적으로 싱글스레드로 작동한다. 하지만 Web API, Callback queue와 Event loop의 도움으로 마치 멀티스레드인 것처럼 비동기처리가 가능하다. 비동기처리 과정에서 일반적으로 콜백함수가 사용되는데, 몇 번만 콜백이 중첩돼도 코드를 매우 알아보기 힘들어진다. (Callback Hell...👿)

이를 해결하고 콜백을 쉽게 사용하기 위한 객체가 Promise인데, 다음 세가지 상태를 가진다.

* pending(대기): 요청이 수행되지 않은 상태
* fulfilled(이행): 요청이 완료되고 Promise가 결과값을 반환한 상태
* rejected(실패): 요청이 수행되는 도중에 오류가 발생한 상태



Promise는 `.then()`과 `.catch()`를 이용해 사용가능하다.

* then: resolve가 호출됐을 때 수행된다. resolve에 인자로 값을 넘겨주면, then에서 받아와서 사용할 수 있다.
* catch: reject가 호출됐을 때 수행된다.

```js
function countdown(seconds) {
  return new Promise((resolve, reject) => {
    setTimeout(() => resolve("done"), seconds * 1000);
  });
}

countdown(2).then((result) => console.log(`${result} successful!`));				// done successfull!
```



### async & await

Promise 객체를 더 편리하게 사용하기 위한 방법. 함수에 붙여서 사용한다. async가 붙은 함수는 요청을 처리하다가 await를 만나면 그 구문의 Promise가 수행되고 resolve를 반환할 때까지 기다린다.

아래 코드는 Promise를 반환하는 fetch 요청을 이용해 결과값을 가져오고, 그 결과값을 다시 이용해 새로운 fetch 요청을 하는 예시다. `.then()`을 이용해 체이닝을 하던 것보다 `async`, `await` 을 이용하면 코드의 가독성이 훨씬 높아진다. async function은 반드시 Promise 객체를 반환한다.

```js
function fetchExample() {
  fetch(`${END_POINT}/user`)
  .then((user) => fetch(`${END_POINT}/user/${user.id}`))
  .then((result) => console.log(result));
}

async function fetchExample() {
  const user = await fetch(`${END_POINT}/user`);
  const result = await fetch(`${END_POINT}/user/${user.id}`);
  console.log(result);
}
```

async, await 패턴의 에러처리는 `try.. catch` 구문을 이용한다.

```js
async function fetchExample() {
  try {
    const user = await fetch(`${END_POINT}/user`);
    const result = await fetch(`${END_POINT}/user/${user.id}`);
    console.log(result);
  } catch (error) {
    console.log(error);
  }
}
```



## Scope & Closure

### Scope

함수를 실행할 때 변수를 찾는 유효한 범위. JS는 기본적으로 함수단위 스코프를 가지고, 스코프 체인을 따라 올라가며 변수를 찾는다.

스코프 체인은 함수를 감싸고 있는 부모함수의 스코프를 참조하며 전역 스코프(Global Scope)까지 올라가는 것을 말한다.



스코프는 함수를 호출할 때가 아닌, **함수를 선언할 때** 생성된다. 이 특징을 ==Lexical Scope==라 부른다. 아래 코드에서 `log()` 가 선언될 때 `log()` 의 스코프는 자기 자신과 가장 가까운 전역 스코프가 된다. `wrapper()` 내에서 호출된다고 해서 `wrapper()` 의 스코프도 참조하는 것이 아닌, **선언될 때의 부모인 전역 스코프만을 참조**하는 것이다. 따라서 출력값은 nero가 아닌 zero가 된다.

```js
const name = "zero";
function log() {
  console.log(name);
}

function wrapper() {
  const name = "nero";
  log();
}
wrapper();							// zero
```



### Closure

함수와 Lexical Environment의 조합으로, 함수의 실행이 끝난 뒤에도 함수 내의 변수를 기억하고 사용할 수 있게 하는 방법이다.

```js
function counterFunc() {
  let num = 0;
  function increase() {
    num++;
  }
  function show() {
    console.log(num);
  }

  return { increase, decrease, show };
}

const count = counterFunc();
count.show();				// 0
count.increase();
count.show();				// 1
```

함수 내에서 내부함수를 만들어 반환하면, 그 내부함수는 선언될 때의 스코프, lexical environment를 기억하게 된다.

1. `counterFunc()`는 실행이 완료되고 종료된다. 
2. 이 때 이 함수가 반환한 함수를 전역 스코프에 있는 `count` 변수에 저장된다.
3. `count`가 `show()`, `increase()`를 실행하면, 우선 **내부에서** `num` 변수를 찾는다.
4. 내부에 `num`이 존재하지 않으므로 기억하고 있던 **lexical scope에서** `num`을 찾아 수행한다. 

`show()`, `increase()`는 자신이 생성된 환경을 벗어나 global 환경에서 실행되었고, 실행 컨텍스트와 상관 없는 `counterFunc()`의 환경을 참조한 것



## 실행 컨텍스트 (Execution Context)

코드가 실행되는 환경, 구역. 

실행 컨텍스트는 실행 스택에 쌓이고, JS Engine은 실행 스택의 가장 상단 함수부터 실행한다.

* 전역 컨텍스트: JS 코드가 브라우저/Node에서 처음 실행될 때 생성
* 함수 컨텍스트: 함수를 호출할 때마다 각 함수에 대해 생기는 실행 컨텍스트.



1. 컨텍스트 생성 시 **변수객체**(variable, arguments)**, Scope chain** 생성 및 **this binding** 이 이루어짐
2. 함수가 실행되면 변수 객체 내에서 변수를 찾고, 없으면 스코프체인을 따라 올라가며 변수를 찾아 할당
3. 함수가 종료되면 컨텍스트도 함께 사라짐



## 메모리관리

JS는 객체가 생성될 때 자동으로 메모리를 할당하고, 쓸모가 없어질 때 메모리를 해제한다. 이를 ==가비지 컬렉션(Garbage Collection)==이라 한다.



### 메모리의 생성주기

1. 필요시 할당
2. 사용 (읽기, 쓰기)
3. 필요 없어지면 해제 



### GC 알고리즘

메모리는 더이상 필요하지 않으면 해제된다. 메모리의 값이 필요하지 않다는 것을 판단하는 것이 GC의 알고리즘

* reference-counting: 참조를 이용. **어떤 다른 오브젝트도 참조하지 않는 오브젝트**를 제거
* **mark-and-sweep**: reference-counting의 순환참조 문제를 해결한 방법. **닿을 수 없는 오브젝트**를 제거



### JS에서 메모리 누수가 발생할 수 있는 경우

* 전역변수
* Closure: 일반적으로 lexical environment는 함수 실행이 끝나는 시점에 할당이 해제된다. 하지만 클로저를 사용하면 계속 함수의 환경에 접근할 수 있어야 하기 때문에 메모리가 해제되지 않는다
* DOM 외부에서 참조: 부모 Element부터 통째로 DOM에서 삭제돼도 자식 Element를 참조하는 JS코드가 있다면 메모리에서 해제되지 않는다.

### 



## This

JS는 함수의 실행을 바탕으로 동작하는 언어. 함수가 실행될 때마다 각 함수에 대해 실행컨텍스트가 생김. 컨텍스트가 생성될 때 변수객체, Scope Chain을 생성하고 **this binding**이 이루어진다.

1. JS에서 this는 기본적으로 **글로벌 객체**. (브라우저에서는 `window`, node에서는 `global`)
2. 객체 내부의 메소드를 호출할 경우 this는 **객체**를 가리킨다.
3. 생성자 함수를 호출할 경우 this는 **인스턴스**를 가리킨다.
4. 이벤트리스너의 콜백함수에서 this는 **이벤트리스너에 바인딩된 element**이다.
5. `.apply`, `.call`, `.bind` 를 이용해 호출할경우 this는 **인자로 넘겨주는 객체**를 가리킨다. (명시적 바인딩)



### Arrow Function의 this

일반함수의 this는 함수를 호출하는 방식에 따라 가리키는 객체가 달라졌다. 반면 화살표 함수의 this 는 **언제나 상위 스코프의 this**를 가리키며, 이를 ==lexical this==라 한다.



## Prototype & Class

### Prototype

JS에서 같은 원형으로 생긴 객체가 공통으로 참조하는 공간. JS의 객체는 모두 부모 객체를 가지고, **부모 객체의 프로퍼티와 메소드를 상속**받을 수 있다. 이 때 부모 객체를 Prototype 이라 한다.

```js
function Person() { };

Person.prototype.eyes = 2;
Person.prototype.nose = 1;

const lee = new Person();
const kim = new Person();
console.log(lee.eyes);			// 2
console.log(kim.eyes);			// 2
```



객체를 생성하는 **방법**에 따라 그 객체의 Prototype이 달라진다.

* 객체 리터럴로 생성 or `new Object()` 로 생성: Object.prototype

* 생성자 함수로 생성: [Function_Name].prototype

  ```js
  function Person() {}
  const lee = new Person();				// lee의 Prototype: Person.prototype
  ```



**Prototype Chain**

객체는 일단 자기자신으로부터 프로퍼티나 메소드를 찾고, 찾기를 실패하면 Prototype 을 타고 올라가 부모에서 찾는다. 이를 Prototype Chain이라 하고, 이 체인은 가장 최상단 Prototype인 `Object.prototype` 에 도달할 때까지 이어진다.



### Class

다른 언어에 존재하는 클래스 기능을 구현한 것으로, ES6에 추가됨.

클래스는 함수의 일종으로, `constructor()` 내부의 내용이 함수의 본문으로 사용된다. 클래스 내부에 정의한 메소드는 prototype에 추가된다.

상속 및 메소드의 오버라이딩이 가능하다.

```js
class Person {
    constructor(name, age) {
        this.name = name,
        this.age = age
    }
    sayHi() {
        console.log('Hi'+this.name)
    }
}
```

<img src="/Users/jeongminlee/Library/Application Support/typora-user-images/Screen Shot 2021-05-29 at 12.30.51 AM.png" alt="Screen Shot 2021-05-29 at 12.30.51 AM" style="zoom:150%;" />





### 