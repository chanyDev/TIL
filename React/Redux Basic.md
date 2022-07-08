# Redux Basic

리덕스는 자바스크립트의 상태 관리를 위한 라이브러리이다.

리덕스의 장점은 다음과 같다.

1. 상태 관리의 중앙화 (전역 상태)
2. 데이터 흐름이 단방향이고 개발자 도구를 지원하여 디버깅에 용이하다.
3. 미들웨어를 제공하여 효율적인 비동기 작업이 가능하다.

물론 러닝커브가 다소 높고 작성해야 하는 코드가 많다는 단점 또한 존재한다.

## 전역 상태 관리의 필요성

리액트에서 전역 상태를 관리해야 할 떄는 최상위 컴포넌트의 state에서 관리하고 필요한 컴포넌트에게 props로 전달하는 방식을 사용한다. (prop drilling)

아래 예시를 살펴보면 App 컴포넌트에서 관리되고 있는 상태 value를 D, C 컴포넌트에서 사용하기 위해 props로 전달했다.

D 컴포넌트에서 value를 사용하기 위해서는 상위 컴포넌트인 B 컴포넌트를 거쳐서 전달된다.

더 복잡한 대규모 프로젝트라면 이런 전달 과정은 더욱 복잡해질 것이다. 이 때문에 유지보수성이 낮아지고 가독성이 떨어질 수 있다.

```js
import React, { useState } from 'react';

const App = () => {
  const [value, setvalue] = useState(1);

  const handleClick = () => {
    setvalue(prev => prev + 1);
  };

  return (
    <div>
      <B value={value} />
      <C value={value} handleClick={handleClick} />
    </div>
  );
};

const B = ({ value }) => {
  return (
    <div>
      <p>B</p>
      <D value={value} />
    </div>
  );
};

const C = ({ value, handleClick }) => {
  return (
    <div>
      <p>C: {value}</p>
      <button onClick={handleClick}>증가</button>
    </div>
  );
};

const D = ({ value }) => {
  return <div>D: {value}</div>;
};

export default App;
```

따라서 다음과 같은 경우 전역 상태관리를 고려해야 한다.

1. 여러 컴포넌트에서 사용되는 많은 양의 상태가 있다.
2. 상태가 자주 업데이트 된다.
3. 상태가 어떻게 업데이트 되는지 확인해야 한다.

## 리덕스 주요 개념

### 액션(Action)

상태에 변화가 필요하면 액션을 발생시켜야 한다.

액션은 무슨 일이 일어났는지 설명하는 평범한 객체이며, type 프로퍼티를 반드시 가지고 있어야 한다.

type 값은 대문자(컨벤션) 문자열로 작성해야 하고 고유해야 한다.

액션의 타입을 변수로 정의해서 사용하면 실수를 줄일 수 있다.

```js
// 액션 객체
{
  type: 'ADD_TODO',
}

// payload가 존재하는 액션 객체
{
  type: 'ADD_TODO',
  payload
}

// 액션 타입 정의
const ADD_TODO = 'ADD_TODO';
```

### 액션 생성 함수(Action Creator)

액션 생성 함수는 액션 객체를 만들어 주는 함수다.

상태에 변화를 일으킬 때마다 액션 객체를 만들어야 하는데, 이는 실수를 유발할 수 있고 번거롭기 때문에 함수로 만들어서 관리하는것이 좋다.

```js
const addTodo = () => ({
  type: 'ADD_TODO',
});
```

### 리듀서(Reducer)

리듀서는 변화를 일으키는 함수다.

액션을 발생시키면 리듀서가 현재 상태와 전달받은 액션 객체를 파라미터로 받아 새로운 상태를 만들어낸다.

이때 상태의 **불변성** 을 유지하면서 데이터에 변화를 일으켜야 한다.

```js
const ADD_TODO = 'ADD_TODO';

const addTodo = () => ({
  type: ADD_TODO,
});

const initialState = {
  todos: [],
};

const reducer = (state = initialState, action) => {
  switch (action.type) {
    case ADD_TODO:
      return {
        ...state, // 불변 유지
        todos: [...state.todos, action.payload],
      };
    default:
      return state;
  }
};
```

### 스토어(Store)

프로젝트의 전체 상태와 리듀서를 갖는 저장소로서, 몇가지 내장 메서드를 갖는 객체다.

리덕스는 단일 스토어만 가질 수 있다.

스토어의 상태를 변경할 수 있는 방법은 액션을 발생시키는 것 뿐이다.

redux의 createStore API를 사용해 생성할 수 있다.

```js
import { createStore } from 'redux';

const reducer = (state = [], action) => {
  switch (action.type) {
    case 'ADD_TODO':
      return [...state, action.payload];
    default:
      return state;
  }
};

// 스토어 생성 => createStore(reducer, [preloadedState], [enhancer])
let store = createStore(reducer);

// 액션을 디스패치
store.dispatch({
  type: 'ADD_TODO',
  payload: '쇼핑하기',
});
```

### 디스패치(dispatch)

디스패치는 스토어의 내장 메서드 중 하나로서, 액션을 보낸다.

디스패치가 호출되면 스토어는 해당 액션과 일치하는 리듀서 함수를 실행시켜서 새로운 상태를 만든다.

```js
// 다음과 같은 형태로 사용한다. => dispatch(action);
dispatch({ type: 'ADD_TODO', payload: '산책하기' });
```

### 구독(subscribe)

구독은 스토어의 내장 메서드 중 하나로서, 변경 사항에 대한 리스너를 추가한다.

액션이 보내져서 스토어의 상태가 변경될 때마다 호출된다.

구독 함수는 구독 취소 함수를 반환한다.

```js
// 다음과 같은 형태로 사용한다. subscribe(listener)
const listener = () => {
  console.log('상태 변경');
};

const unsubscribe = store.subscribe(listener);

unsubscribe(); // 구독을 비활성화 할 때 해당 함수를 호출
```

간단한 카운터 예제를 만들어보자.

```js
// redux/counter.js
import { createStore } from 'redux';

const initialState = {
  count: 0,
};

const INCREASE = 'INCREASE';
const DECREASE = 'DECREASE';

export const increase = () => ({ type: INCREASE });
export const decrease = () => ({ type: DECREASE });

const counterReducer = (state = initialState, action) => {
  switch (action.type) {
    case INCREASE:
      return {
        ...state,
        count: state.count + 1,
      };
    case DECREASE:
      return {
        ...state,
        count: state.count - 1,
      };
    default:
      return state;
  }
};

export const store = createStore(counterReducer);
```

```js
// App.jsx
import React from 'react';
import { useSelector } from 'react-redux';
import { useDispatch } from 'react-redux';
import { decrease, increase } from './redux/counter';

const App = () => {
  const dispatch = useDispatch();
  const { count } = useSelector(state => ({
    count: state.count,
  }));

  const increaseCount = () => {
    dispatch(increase());
  };

  const decreaseCount = () => {
    dispatch(decrease());
  };

  return (
    <div>
      <button onClick={decreaseCount}>-</button>
      <p>count: {count}</p>
      <button onClick={increaseCount}>+</button>
    </div>
  );
};

export default App;
```

```js
// index.js
import React from 'react';
import ReactDOM from 'react-dom';
import App from './App';
import { Provider } from 'react-redux';
import { store } from './redux/counter';

ReactDOM.render(
  <React.StrictMode>
    <Provider store={store}>
      <App />
    </Provider>
  </React.StrictMode>,
  document.getElementById('root')
);
```

## Reference

[Redux 공식 문서](https://redux.js.org/) <br>
📙 리액트를 다루는 기술 <br>
