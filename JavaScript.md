# JavaScript

## Variables

### 자료형

원시타입 (Primitive Type)

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

참조타입

* 종류: Object, Array, Function

* 변수에 할당할 때 값이 아닌 데이터의 참조만 저장된다.

  ```js
  const person = { age: 27 };
  const person2 = person1;
  person.age = 28;
  console.log(person2.age);		// 28
  ```



### const, let, var

**var**

* 함수 스코프
* 호이스팅이 일어남
* 변수의 재선언 / 재할당 가능

**const & let**

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

  

