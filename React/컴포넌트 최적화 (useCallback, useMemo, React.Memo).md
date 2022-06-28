# 컴포넌트 최적화 (useCallback, useMemo, React.Memo)

메모이제이션이란 이전 값을 저장해두어 동일한 계산의 반복 수행을 제거하는 기술이다.

리액트에서는 메모이제이션을 위한 세가지 api를 제공한다.

## useCallback

useCallback은 반환된 콜백 함수를 메모이제이션해서 함수의 재선언을 방지하는 Hook이다.

useCallback은 콜백 함수와 의존성 배열을 인자로 받는다. 메모이제이션된 콜백은 의존성이 변경되었을때만 새로운 함수를 생성한다.

```js
// a 또는 b가 변경되었을때만 새로운 함수를 생성
const memoizedCallback = useCallback(() => {
  doSomething(a, b);
}, [a, b]);
```

useCallback은 다음과 같은 경우에 유용하게 사용할 수 있다.

1. React.memo로 래핑된 자식 컴포넌트에 함수를 전달할 경우
2. useEffect Hook이 함수에 의존하는 경우 => 이 경우 useEffect 내부에서만 사용되는 함수라면 effect 내부에 선언하는것이 좋다.
3.

## useMemo

useMemo는 값을 메모이제이션하는 Hook이다.

useMemo는 의존성이 변경되었을때만 다시 값을 계산한다.

값을 계산하는 로직이 복잡한 경우 성능의 이점을 가져올 수 있다.

```js
const memoizedValue = useMemo(() => computeExpensiveValue(a, b), [a, b]);
```

주의할 점은 useCallback, useMemo Hook 또한 계산 비용이 든다는 점이다.

무분별한 사용보다는 성능에 이점이 있는지 측정 후 사용하는 것이 좋다.

## React.memo

React.memo는 고차 컴포넌트(HOC)로 컴포넌트를 메모이제이션한다.

동일한 props로 동일한 결과를 렌더링한다면, React.memo를 사용해 컴포넌트를 리렌더링하지 않고 재사용 할 수 있다.

```js
const Memoized = React.memo(Component);

// 새로 렌더링
<Memoized num={1} />
// 컴포넌트를 재사용
<Memoized num={1} />
// 새로 렌더링
<Memoized num={2} />
```

## 참고 자료

https://ko.reactjs.org/docs/hooks-faq.html#performance-optimizations <br>
https://kentcdodds.com/blog/usememo-and-usecallback <br>
https://dmitripavlutin.com/dont-overuse-react-usecallback/ <br>
