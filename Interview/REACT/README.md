# REACT

## 1. Virtual DOM에 대해 설명하세요.

Virtual DOM은 리액트에서 DOM 처리 횟수를 최소화하고 효율적으로 진행하기 위한 방식으로 사용되는 패턴이다.

Virtual DOM의 동작 방식은 다음과 같다.

1. 데이터를 업데이트하면 전체 UI를 Virtual DOM에 리렌더링한다.
2. 이전 Virtual DOM에 있던 내용과 현재 내용을 비교한다.
3. 바뀐 부분만 실제 DOM에 적용한다.

위 과정을 재조정이라고 하며, 리액트는 비교 알고리즘을 통해 효율적으로 재조정을 진행한다.

비교 알고리즘은 다음 두 가지 가정을 기반으로 구현되어 있다.

1. 서로 다른 타입의 두 엘리먼트는 서로 다른 트리를 만들어낸다.
2. 개발자가 key prop을 통해 여러 렌더링 사이에서 어떤 자식 엘리먼트가 변경되지 않아야 할 지 표시해 줄 수 있다.

✨ 자식 요소에 대한 재귀 처리의 경우 key prop을 통해 기존 트리와 변경된 트리의 자식 요소들이 일치하는지 확인함으로서 효율적인 처리를 구현한다. 이때 key prop은 형제 요소 사이에서 변하지 않는 유일한 값을 사용해야 한다.

## 2. useCallback과 useMemo에 대해 설명하세요.

useCallback과 useMemo는 메모이제이션을 통해 컴포넌트를 최적화 하기 위한 hook이다.

메모이제이션이란 이전 값을 저장해두어 동일한 계산의 반복 수행을 제거하는 기술이다.

useCallback은 함수를 메모이제이션해서 함수의 재선언을 방지한다.

하위 컴포넌트가 React.memo로 최적화 되어 있고 함수를 prop으로 전달할 경우 유용하게 사용할 수 있다.

useMemo는 값을 메모이제이션한다.

값을 계산하는 로직이 복잡한 경우 성능의 이점을 가져올 수 있다.

## 3. state와 props의 차이는?

state는 컴포넌트에서 변경 가능한 데이터이고 props는 컴포넌트 외부에서 컴포넌트에 전달하는 데이터로 변경 불가능(읽기 전용)하다.

## 4. state 변경 시 setState를 사용하는 이유는?

setState를 사용하지 않고 state를 직접 변경할 경우 리액트는 state의 변경을 인지하지 못하기 때문에 render 메서드를 다시 호출하지 않기 때문에 DOM을 업데이트 할 수 없다. 따라서 state 변경 시 setState를 사용해야 정상적인 UI 업데이트가 가능하다.

## 5. 컴포넌트 생명주기에 대해 설명하세요.

컴포넌트는 렌더링 되기 전인 준비 과정에서부터 제거까지의 생명주기를 갖는다.

컴포넌트의 생명주기는 크게 마운트(Mount), 업데이트(Update), 언마운트(Unmount) 세가지 경우로 구분할 수 있다.

1. 마운트(Mount) : 컴포넌트가 생성되는 과정
2. 업데이트(Update) : props, state의 변경이 발생하면 컴포넌트가 업데이트 된다.
3. 언마운트(Unmount) : 컴포넌트가 제거되는 과정

클래스형 컴포넌트는 특정 시점에 작업을 처리하기 위해 생명주기 메서드를 제공한다.

주로 사용되는 메서드는 아래와 같다.

1. componentDidMount : 컴포넌트가 렌더링 된 후 호출되는 메서드
2. componentDidUpdate : 컴포넌트의 업데이트가 끝난 후 호출되는 메서드
3. componentWillUnmount : 컴포넌트가 제거되기 전에 호출되는 메서드

함수형 컴포넌트의 경우 useEffect Hook을 사용해 생명주기 메서드처럼 특정 시점의 작업을 가능하게 한다.

```js
useEffect(() => {
  // 컴포넌트가 마운트, 업데이트될 때 실행
  console.log('mount and update');
});

useEffect(() => {
  // 컴포넌트가 마운트될 때 실행
  console.log('mount');
}, []);

useEffect(() => {
  // 컴포넌트가 마운트, 특정 상태가 업데이트될 때 실행
  console.log('mount and state update');
}, [state]);

useEffect(() => {
  return () => {
    console.log('before unmount and update');
  };
});
```
