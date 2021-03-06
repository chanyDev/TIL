# Promise

프로미스는 ES6에서 추가된 비동기 처리를 위한 패턴으로 콜백 패턴이 가진 단점을 보완하며 비동기 처리 시점을 명확하게 표현할 수 있다.

## 콜백 패턴의 단점

### 1. 콜백 헬

XMLHttpRequest 객체를 활용한 GET 요청 함수 예시를 살펴보자

```js
const getRequest = url => {
  const xhr = new XMLHttpRequest();

  xhr.open('GET', url);
  xhr.send();

  xhr.onload = () => {
    if (xhr.status === 200) {
      return JSON.parse(xhr.response);
    } else {
      console.error(`${xhr.status}`);
    }
  };
};

const res = getRequest('https://jsonplaceholder.typicode.com/todos/1');
console.log(res); // undefined
```

위 코드의 getRequest 함수는 비동기로 동작하는 함수이기 때문에 처리 결과를 외부로 반환하거나 상위 스코프의 변수에 할당할 수 없다.

이러한 문제를 해결하기 위해 사용하는 것이 콜백 패턴으로 비동기 함수 처리 결과에 대해 후속 처리를 수행해야 하는 경우 콜백 함수를 전달하는 것이 일반적이다.

위 코드를 콜백 패턴으로 수정해보자.

```js
const getRequest = (url, successFunc, failureFunc) => {
  const xhr = new XMLHttpRequest();

  xhr.open('GET', url);
  xhr.send();

  xhr.onload = () => {
    if (xhr.status === 200) {
      successFunc(JSON.parse(xhr.response));
    } else {
      failureFunc(xhr.status);
    }
  };
};

getRequest(
  'https://jsonplaceholder.typicode.com/todos/1',
  console.log,
  console.error
);
```

콜백 함수가 비동기 함수의 후속 처리를 수행한 결과를 가지고 다시 비동기 함수를 호출해야 한다면 콜백 함수가 중첩되어 복잡한 구조를 갖게 된다는 문제를 지닌다, 이를 콜백 헬 이라고 한다.

### 2. 에러 처리

try...catch 문은 동기적으로 동작하기 때문에 비동기 함수 내에서의 에러를 캐치하기 위해서는 비동기 함수의 내부에 구현해야 한다.

```js
// 에러를 캐치할 수 없다. => Bad Case
try {
  setTimeout(() => {
    console.log(x); // x is not defined
  }, 1000);
} catch (e) {
  console.log('error');
}

// 에러를 캐치할 수 있다. => Good Case
setTimeout(() => {
  try {
    console.log(x);
  } catch (e) {
    console.log('error'); // 'error' 출력
  }
});
```

따라서 콜백 패턴을 사용하게 될 경우 비동기 함수에게 전달한 콜백 함수 내부에서 try...catch 문을 사용해야 하는데, 이는 처리하기 곤란하다는 문제를 가진다.

## 프로미스의 사용

Promise 생성자 함수를 new 연산자와 함께 호출하면 Promise 객체를 생성한다.

Promise 생성자 함수는 비동기 처리를 수행할 콜백 함수를 인수로 전달받는다. 이때 콜백 함수는 resolve와 reject 함수를 인수로 전달받는다.

비동기 처리가 성공하면 resolve 함수를 호출하고, 비동기 처리가 실패하면 reject 함수를 호출한다.

위 예제를 Promise로 구현해보자.

```js
const getPromise = url => {
  return new Promise((resolve, reject) => {
    const xhr = new XMLHttpRequest();

    xhr.open('GET', url);
    xhr.send();

    xhr.onload = () => {
      if (xhr.status === 200) {
        resolve(JSON.parse(xhr.response));
      } else {
        reject(console.error(`${xhr.status}`));
      }
    };
  });
};

// Promise { <pending> }
const result = getPromise('https://jsonplaceholder.typicode.com/todos/1');
```

프로미스는 비동기 처리 진행에 따른 세가지 상태를 갖는다.

1. pending : 비동기 처리가 아직 수행되지 않은 상태
2. fulfilled : 비동기 처리가 성공적으로 수행된 상태 => resolve 함수가 호출되면 fulfilled 상태로 변경된다.
3. rejected : 비동기 처리가 실패로 수행된 상태 => reject 함수가 호출되면 rejected 상태로 변경된다.

fulfilled, rejected 된 상태를 settled(처리) 되었다고 한다.

즉, 프로미스는 비동기 처리 상태와 처리 결과를 관리하는 객체다.

## 프로미스 후속 메서드

프로미스는 처리 결과(성공, 실패)에 따른 후속 처리를 위해 then, catch, finally 메서드를 제공한다.

프로미스 후속 메서드는 항상 프로미스를 반환한다. 따라서 then().then().catch() 와 같이 연속적으로 호출할 수 있다. 이를 프로미스 체이닝 이라고 한다.

1. then : then 메서드는 2개의 콜백 함수를 인수로 전달받는다. 첫번째 콜백 함수는 프로미스 객체의 상태가 fulfilled 상태가 되면 호출되고, 두번째 콜백 함수는 프로미스 객체의 상태가 rejected가 되면 호출된다. 이때 인수로 프로미스 객체의 처리 결과를 받는다.

2. catch : catch 메서드는 프로미스 객체의 상태가 rejected 되거나 then 메서드에서 에러가 발생하면 호출된다. 이때 인수로 프로미스 객체의 처리 결과를 받는다.

3. finally : finally 메서드는 프로미스 객체의 성공, 실패에 상관없이 pending 상태가 아니라면 호출된다. 인수를 전달받지 않는다 => 성공, 실패 처리 여부를 판단할수 없기 때문

```js
// fulfilled 상태
const promise = new Promise((resolve, reject) => resolve(1)).then(
  res => console.log(res) // 1;
);

// rejected 상태
const promise = new Promise((_, reject) => reject('error')).catch(
  e => console.log(e) // error
);

const promise = new Promise(resolve => resolve(1))
  .then(() => {
    throw new Error('error');
  })
  .catch(e => console.log(e)); // Error: error

// settled 상태
const promise = new Promise((resolve, reject) => resolve(1)).finally(() =>
  console.log('settled')
);
```

## 프로미스 정적 메서드

Promise.all : 프로미스로 이루어진 이터러블을 인수로 전달받으며, 비동기 처리를 모두 병렬 처리할 때 사용한다. 인수로 전달받은 프로미스가 모두 fulfilled 상태가 되면 순서대로 배열에 저장해 resolve 하는 프로미스를 반환하며 하나라도 rejected 상태가 될 경우 즉시 종료한다. => 처리 순서를 보장한다.

```js
// 전부 fulfilled 되는 경우
const promise1 = new Promise(resolve => setTimeout(() => resolve(1), 3000));
const promise2 = new Promise(resolve => setTimeout(() => resolve(2), 2000));
const promise3 = new Promise(resolve => setTimeout(() => resolve(3), 1000));

Promise.all([promise1, promise2, promise3]).then(console.log); // [1, 2, 3] => 약 3초 소요

// 하나라도 rejected 되는 경우
const promise1 = new Promise(resolve => setTimeout(() => resolve(1), 3000));
const promise2 = new Promise((_, reject) => setTimeout(() => reject('error'), 2000));
const promise3 = new Promise(resolve => setTimeout(() => resolve(3), 1000));

Promise.all([promise1, promise2, promise3])
  .then(console.log)
  .catch(e => console.log(e)); // error
```

Promise.race : 프로미스로 이루어진 이터러블을 인수로 전달받는다. 전달받은 프로미스중 가장 먼저 fulfilled 상태가 된 프로미스의 처리결과를 resolve 하는 프로미스를 반환하며 하나라도 rejected 상태가 될 경우 즉시 종료한다.

```js
// 먼저 처리된 프로미스의 결과를 resolve 한다
const promise1 = () => new Promise(resolve => setTimeout(() => resolve(1), 3000));
const promise2 = () => new Promise(resolve => setTimeout(() => resolve(2), 2000));
const promise3 = () => new Promise(resolve => setTimeout(() => resolve(3), 1000));

Promise.race([promise1(), promise2(), promise3()]).then(console.log); // 3
```

## 마이크로태스크 큐

프로미스의 후속 메서드의 콜백 함수는 마이크로태스크 큐에 저장된다.

마이크로태스크 큐는 태스크 큐보다 우선순위가 높다!

이전에 정리해 두었던 [브라우저 환경과 이벤트 루프](https://github.com/chanyDev/TIL/blob/main/JavaScript/%EB%B8%8C%EB%9D%BC%EC%9A%B0%EC%A0%80%20%ED%99%98%EA%B2%BD%EA%B3%BC%20%EC%9D%B4%EB%B2%A4%ED%8A%B8%20%EB%A3%A8%ED%94%84.md) 을 참고하자.

아래 코드의 실행 순서를 살펴보자.

```js
setTimeout(() => {
  console.log('task queue'); // 3
}, 0);

Promise.resolve()
  .then(() => console.log('microtask queue 1')) // 1
  .then(() => console.log('microtask queue 2')); // 2

// 1 => 2 => 3 순서로 출력된다.
```

## Reference

📙 모던 자바스크립트 Deep Dive <br>
https://developer.mozilla.org/ko/docs/Web/JavaScript/Reference/Global_Objects/Promise
