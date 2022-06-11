# setState

## setState 사용시 주의점

리액트는 렌더링 최적화를 위해 state의 변경 발생 시 batching 처리를 한다.

batching : 여러 state 업데이트를 하나의 리렌더링으로 묶는 것을 의미한다.

state는 다음과 같은 특징을 가진다는 점을 기억하자.

1. setState는 비동기로 처리된다.
2. setState를 연속적으로 호출하면 batching 처리를 한다.
3. state는 객체이다.

### setState는 비동기이다.

흔히 setState로 상태를 변경하고 변경된 상태를 확인하려 하지만 변경된 값을 얻을 수 없다.

```js
const [state, setState] = useState(0);

const onClick = () => {
  setState(state + 1);
  console.log(state); // 1이라고 생각할 수 있지만 0을 출력한다.
};
```

위와 같이 동작하는 이유는 setState가 batching 처리를 위해 비동기로 동작하기 때문이다.

### setState의 batching

아래 예시를 보면 동일한 state를 연속적으로 변경했을 때 마지막 setState만 실행되는 것처럼 동작한다. 다음과 같이 동작하는 이유는 batching 처리를 위해 전달 받은 각각의 state를 합치는 merging 작업을 수행한 뒤 한번에 setState 처리를 하기 때문이다.

```js
const App = () => {
  const [state, setState] = useState(0);

  const onClick = () => {
    setState(state + 1);
    setState(state + 2);
    setState(state + 3);
  };

  console.log(state); // 3

  return <button onClick={onClick}>Click</button>;
};
```

그렇다면 리액트는 어떻게 merging 작업을 수행할까?

```js
const [state, setState] = useState(0);

const newState = Object.assign(
  { state },
  { state: state + 1 },
  { state: state + 2 },
  { state: state + 3 }
);

console.log(newState); // { state: 3 }
setState(newState);
```

리액트는 Object.assign 메서드를 사용해 여러 객체를 합치는 작업을 수행하기 때문에 같은 프로퍼티를 갖는다면 이전 값이 덮어씌워지기 때문에 마지막 setState 명령어만 실행되는 것처럼 동작한다.

### setState의 함수형 업데이트

위와 같은 문제를 해결하기 위해서는 setState의 인자로 함수를 전달할 수 있다.

함수를 전달할 경우에도 setState가 비동기 처리되는 것은 변함 없지만 인자로 전달받은 함수들은 Queue에 저장되어 순차적으로 실행된다.

따라서 첫번째 함수가 반환한 state가 두번째 함수의 인자로 전달되기 때문에 state의 최신 상태 유지가 가능해진다.

```js
const App = () => {
  const [state, setState] = useState(0);

  const onClick = () => {
    setState(prev => prev + 1);
    setState(prev => prev + 2);
    setState(prev => prev + 3);
  };

  console.log(state); // 6

  return <button onClick={onClick}>Click</button>;
};
```

리액트는 이벤트 핸들러 내에서만 batching 처리를 했지만 createRoot가 포함된 리액트 v18.0.0 부터는 Promise, timeout등에서의 모든 setState는 automatic batching 처리된다.

automatic batching 처리하고 싶지 않은 경우 react-dom의 flushSync 메서드를 활용할 수 있다.

<br>

## 참고 자료

https://leehwarang.github.io/docs/tech/2020-07-28-setState.html
https://github.com/reactwg/react-18/discussions/21
