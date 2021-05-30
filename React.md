# React

SPA(Single Page Application) 개발을 위한 UI 라이브러리.

* Real DOM이 아닌 virtual DOM을 이용한 UI 업데이트
* 상위컴포넌트에서 하위컴포넌트로의 단방향 데이터 흐름



## Life Cycle

<img width="1246" alt="Screen Shot 2021-05-30 at 9 55 39 PM" src="https://user-images.githubusercontent.com/37537216/120104922-c7b6d980-c191-11eb-8948-8e719bb4ecd9.png">


### 컴포넌트 마운트

* **constructor(props)**

  컴포넌트 생성자 함수. 초기 데이터값을 설정. state에 값을 직접 대입할 수 있는 유일한 시점 

* **getDerivedStateFromProps(props, state)**

  상위컴포넌트에서 받아온 props를 state에 새로 대입할 필요가 있을  때 사용.  state 갱신을 위한 객체를 반환하거나, null을 반환해서 아무것도 갱신하지 않을 수 있다.
  
  ```js
  static getDerivedStateFromProps(props, state) {
  	if (props.value !== state.value) {
  		return { value: props.value };
  	}
  	return null;
  }
  ```


* **render()**

  React Element를 생성하는 함수. 여기에서 리턴된 데이터를 이용해 DOM을 업데이트한다.

* **componentDidMount()**

  DOM 요소들이 모두 그려진 뒤에 호출. 주로 API요청, 이벤트리스너 삽입, 외부 라이브러리 연동 등이 이루어진다.



### 컴포넌트 업데이트

* **getDerivedStateFromProps()**

* **shouldComponentUpdate(nextProps, nextState)**

  현재 props, state와 비교를 통해 특정 상황에만 컴포넌트가 업데이트되도록 한다. `true`/`false`를 반환하며, `true`면 컴포넌트를 업데이트한다.

  ```js
  shouldComponentUpdate(nextProps, nextState) {
  	return nextProps.value !== this.props.value;
  }
  ```

* **render()**

* **getSnapshotBeforeUpdate(prevProps, prevState)**

  컴포넌트가 업데이트 되기 전에 상태정보를 저장할 수 있다. 여기에서 리턴된 값은 `componentDidMount`의 세번째 인자로 받아서 사용가능하다. 스크롤 위치, 인풋 포커스 등을 기억할 때 주로 사용된다.

  ```js
  getSnapshotBeforeUpdate(prevProps, prevState) {
    // 스크롤 위치 기억 로직
    return { scrollTop, scrollHeight };
  }
  ```

* **componentDidUpdate(prevProps, prevState, snapshot)**

  컴포넌트의 상태가 업데이트되어 DOM이 새로 그려진 뒤에 호출.



### 컴포넌트 마운트 해제

* **componentWillUnmount**

  컴포넌트가 완전히 DOM 트리, 화면에서 사라질 때 호출. 주로 타이머 해제, 이벤트리스너 해제 등의 작업이 일어난다.



### 에러 처리

* **getDerivedStateFromError(error)**

  컴포넌트에서 에러가 발생할 때, Render Phase에서 호출. 에러 정보를 state에 저장해 화면에 나타내는 용도

* **componentDidCatch(error, info)**

  컴포넌트에서 에러가 발생할 때, Commit Phase에서 호출. 에러정보를 서버로 전송하는 용도.



## Hooks

함수 컴포넌트에서도 클래스 컴포넌트의 기능을 사용할 수 있게 하는 기능. 컴포넌트의 state를 관리하고, lifecycle method를 사용할 수 있게 한다. 

**클래스 컴포넌트 대비 장점**

* 클래스 컴포넌트는 라이프 사이클 단위로 관리됨 
  → 관련 없는 로직들이 하나의 메서드에 섞이거나, 같은 로직이 여러 메서드에 중복으로 작성되는 문제
  → Hook을 이용하면 로직 단위로 관리 가능
* 기존 HOC를 대체 가능, 로직의 재사용성이 높아짐



### useState

함수형 컴포넌트에서 상태값을 관리할 수 있게 하는 훅. 인자로 default value를 설정할 수 있고, useState 훅 여러개를 써서 여러 state를 관리할 수 있다.

```js
const [name, setName] = useState('jeongmin');
const [age, setAge] = useState(27);
```



### useEffect

클래스 컴포넌트의 lifecycle 메소드를 이용할 수 있는 훅. DOM 업데이트가 완료된 후에 호출된다.

* return 되는 값은 componentWillUnmount와 같은 시점에 호출된다. 
* 두번째 인자로 배열을 넘겨주면, 그 배열의 값이 변경될때만 useEffect가 작동한다. 만약 빈배열`[]`을 넘겨주면, 초기 마운트 때만 단 한번 호출된다.

​```js
const [count, setCount] = useState(0);
useEffect(() => {
  document.title = `${count}번 업데이트 됨`;
  return () => {
    document.title = 'Reset';
  }
}, [count])
```



### Custom Hooks

React에 내장된 게 아니더라도, 자주 사용하는 로직을 custom hook으로 만들어 재사용할 수 있다.

```jsx
function useHasMounted() {
  const [hasMounted, setHasMounted] = useState(false);
  useEffect(() => setHasMounted(true), []);
  return hasMounted;
}

function Example() {
  const hasMounted = useHasMounted();
}
```



### 기타 React 내장 Hooks

**useRef**

DOM Element에 직접 접근하기 위해 사용

```jsx
function Example() {
  const inputEl = useRef(null);
  const onClick = () => {
    if (inputEl.current) {
      inputEl.current.focus();
    }
  }
  return <input ref={inputEl} />
}
```



**useContext**

Consumer 컴포넌트 없이 부모 컴포넌트의 context 이용.

```js
const UserContext = React.CreateContext();

function ChildComponent() {
	const user = useContext(UserContext);
}
```



**useMemo**

계산량이 많은 함수의 결과값을 저장하고, 같은 입력값이 들어왔을 때 바로 결과값을 반환

**useCallback**

<u>컴포넌트가 렌더링될 때마다 내부의 함수, 변수도 매번 다시 선언</u>된다. 함수는 자기자신이 아닌 이상 다르다고 인식하기 때문에 그냥 함수를 자식 컴포넌트에 넘겨주게 되면, 실질적으로 변화가 없다해도 자식 컴포넌트는 props가 바뀌었다고 인식한다. 이 경우 `React.memo()` 나 `PureComponent`를 이용한 최적화가 작동하지 제대로 않는다.

이 때 함수를 useCallback에 넣어주면 해결된다. 아래 코드에서는 name, age가 바뀌었을 때만 새 함수가 전달된다.

```jsx
function Example() {
	const onSave = useCallback(() => saveToServer(name, age), [name, age]);
	return (
    <div>
    	<form onSave={onSave}/>
		</div>);
}
```



