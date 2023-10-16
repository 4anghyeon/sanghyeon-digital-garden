---
{"tags":["JavaScript"],"dg-publish":true,"dg-hide":true,"permalink":"/language/java-script/promise/","hide":true,"dgPassFrontmatter":true,"noteIcon":""}
---

ES6에서 비동기 처리를 위한 패턴으로 프로미스(Promise)를 도입했다. 프로미스는 전통적인 콜백 패턴이 가진 단점을 보완하며 비동기 처리 시점을 명확하게 표현할 수 있다.

# 비동기 처리를 위한 콜백 패턴의 단점
## Callback Hell
![Pasted image 20231016175428.png](/img/user/Language/JavaScript/Pasted%20image%2020231016175428.png)
비동기 함수는 비동기 처리 결과를 외부에 반환할 수 없고, 상위 스코프의 변수에 할당할 수도 없다.

따라서 비동기 함수의 처리 결과(서버의 응답 등)에 대한 후속 처리는 비동기 함수 내부에서 수행해야 한다. 이때 후속 처리를 수행하는 콜백 함수를 전달하는 것이 일반적이다.

콜백 함수를 통해 비동기 처리 결과를 처리해야 한다면 콜백 함수가 호출이 중첩되어 복잡도가 높아지는 현상이 발생한다. 이를 **콜백 헬**이라 한다.
```js
setTimeout(
  function (name) {
    var coffeeList = name;
    console.log(coffeeList);

    setTimeout(
      function (name) {
        coffeeList += ", " + name;
        console.log(coffeeList);

        setTimeout(
          function (name) {
            coffeeList += ", " + name;
            console.log(coffeeList);

            setTimeout(
              function (name) {
                coffeeList += ", " + name;
                console.log(coffeeList);
              },
              500,
              "카페라떼"
            );
          },
          500,
          "카페모카"
        );
      },
      500,
      "아메리카노"
    );
  },
  500,
  "에스프레소"
);
```

## 에러 처리의 한계
콜백 패턴의 심각한 문제점 중 하나는 에러 처리가 곤란하다는 것이다.
```js
try {
  setTimeout(() => { throw new Error('Error!'); }, 1000);
} catch (e) {
  // 에러를 캐치하지 못한다
  console.error('캐치한 에러', e);
}
```

try 코드 블록은 `setTimoeut` 함수의 [[Language/JavaScript/실행 컨텍스트\|실행 컨텍스트]]가 콜 스택에 푸시되어 실행 될 때 캐치한다. 콜백 함수는 태스크 큐로 푸시되어 콜 스택이 비어졌을 때 콜스택에 푸시되어 실행된다.

즉, try 가 캐치하는 것은 콜백 함수가 아닌 `setTimeout` 함수가 콜백 함수를 태스크 큐로 푸시하는 과정이므로 에러가 잡히지 않는다.

이처럼 콜백 패턴은 콜백 헬이나 에러 처리가 곤란하다. 이를 극복하기 위해 ES6에서 프로미스가 도입되었다.

# 프로미스(Promise)의 생성
`Promise` 생성자 함수를 `new` 연산자와 함께 호출하면 프로미스를 생성한다.

`Promise` 생성자 함수는 비동기 처리를 수행할 콜백 함수를 인수로 전달받는데 이 콜백 함수는 `resolve`와 `reject` 함수를 인수로 전달받는다.

```js
const promise = new Promise((resolve, reject) => {
  // Promise 함수의 콜백 함수 내부에서 비동기 처리를 수행한다.
  if (/* 비동기 처리 성공 */) {
    resolve('result');
  } else { /* 비동기 처리 실패 */
    reject('failure reason');
  }
});
```

`Promise` 생성자 함수가 전달받은 콜백 함수 내부에서 비동기 처리를 수행하고, 비동기 처리가 성공하면 콜백 함수의 인수로 전달받은 `resolve` 함수를, 실패하면 `reject` 함수를 호출한다.

상단 콜백 헬 함수를 `Promise`를 이용해 다음과 같이 구현할 수 있다.
```js
new Promise(function (resolve) {  
  setTimeout(function () {  
    let name = "espresso";  
    console.log(name);  
    resolve(name);  
  }, 500);  
})  
  .then(function (prevName) {  
    return new Promise(function (resolve) {  
      setTimeout(function () {  
        let name = `${prevName}, americano`;  
        console.log(name);  
        resolve(name);  
      }, 500);  
    });  
  })  
  .then(function (prevName) {  
    return new Promise(function (resolve) {  
      setTimeout(function () {  
        let name = `${prevName}, mocha`;  
        console.log(name);  
        resolve(name);  
      }, 500);  
    });  
  })  
  .then(function (prevName) {  
    return new Promise(function (resolve) {  
      setTimeout(function () {  
        let name = `${prevName}, latte`;  
        console.log(name);  
        resolve(name);  
      }, 500);  
    });  
  });
```

반복되는 부분이 많으므로 공통적인 부분을 빼내어 리팩토링 해보자.
```js
let addCoffee = function (name) {  
  return function (prevName) {  
    return new Promise(function (resolve) {  
      setTimeout(function () {  
        let newName = prevName ? `${prevName}, ${name}` : name;  
        console.log(newName);  
        resolve(newName);  
      }, 500);  
    });  
  };  
};  
  
addCoffee("espresso")()  
  .then(addCoffee("americano"))  
  .then(addCoffee("mocha"))  
  .then(addCoffee("latte"));
```


## 프로미스 상태
프로미스는 현재 비동기 처리가 어떻게 진행되고 있는지를 나타내는 상태정보를 갖는다.

|프로미스의 상태 정보|의미|상태 변경 조건|
|---|---|---|
|pending|비동기 처리가 아직 수행되지 않은 상태|프로미스가 생성된 직후 기본 상태|
|fulfilled|비동기 처리가 성공된 상태|resolve 함수 호출|
|rejected|비동기 처리가 실패된 상태|reject 함수 호출|
프로미스의 상태는 `resolve` 또는 `reject` 함수를 호출하는 것으로 결정된다.

# 프로미스의 후속 처리 메서드
프로미스의 비동기 처리 상태가 변화하면 이에 따른 후속 처리를 해야 한다. 이를 위해 프로미스는 후속 메서드 `then`, `catch`, `finally`를 제공한다.

**프로미스의 비동기 처리 상태가 변화하면 후속 처리 메서드에 인수로 전달한 콜백 함수가 선택적으로 호출된다.**

## then
`then` 메서드는 두 개의 콜백 함수를 인수로 전달받는다.

- 첫 번째 콜백 함수는 프로미스가 `fulfilled` 상태가 되면 호출된다. 이때 콜백 함수는 비동기 처리 결과를 인수로 전달받는다.
- 두 번째 콜백 함수는 프로미스가 `rejected` 상태가 되면 호출된다. 이때 콜백 함수는 프로미스의 에러를 인수로 전달받는다.

즉, 첫 번째 콜백 함수는 비동기 처리의 성공 처리 콜백 함수이며, 두 번째 콜백 함수는 실패했을 때 실패 처리 콜백 함수이다.

```js
// fulfilled
new Promise(resolve => resolve('fulfilled'))
  .then(v => console.log(v), e => console.error(e)); // fulfilled

// rejected
new Promise((_, reject) => reject(new Error('rejected')))
  .then(v => console.log(v), e => console.error(e)); // Error: rejected
```

`then` 메서드는 언제나 프로미스를 반환한다. `then` 메서드의 콜백 함수가 프로미스를 반환하면 그 프로미스를 그대로 반환하고, 프로미스가 아닌 것을 반환하면 그 값을 암묵적으로 `resolve` 또는 `reject` 하여 프로미스를 생성해 반환한다.

## catch
`catch` 메서드는 한 개의 콜백 함수를 인수로 전달받는다. `catch` 메서드의 콜백 함수는 프로미스가 `rejected` 상태인 경우만 호출된다.

```js
// rejected
new Promise((_, reject) => reject(new Error('rejected')))
  .catch(e => console.log(e)); // Error: rejected

// then에서도 catch 할 수 있긴 하다.
new Promise((_, reject) => reject(new Error('rejected')))
  .then(undefined, e => console.log(e)); // Error: rejected
```

## finally
`finally` 메서드는 한 개의 콜백 함수를 인수로 전달받는다. `finally` 메서드의 콜백 함수는 프로미스의 성공 여부와 상관없이 무조건 한 번 호출된다. 따라서 프로미스의 상태와 상관없이 공통적으로 수행해야할 처리 내용이 있을 때 유용하다.

```js
new Promise(() => {})
  .finally(() => console.log('finally')); // finally
```

# 프로미스 체이닝
```js
const url = 'https://jsonplaceholder.typicode.com';

// id가 1인 post의 userId를 취득
promiseGet(`${url}/posts/1`)
  // 취득한 post의 userId로 user 정보를 취득
  .then(({ userId }) => promiseGet(`${url}/users/${userId}`))
  .then(userInfo => console.log(userInfo))
  .catch(err => console.error(err));
```

위 예제에서 `then` → `then` → `catch` 순서로 후속 처리 메서드를 호출했다. <mark style='background:#f7b731'>후속 처리 메서드는 언제나 프로미스를 반환하므로 연속적으로 호출할 수 있다. 이를 프로미스 체이닝이라 한다.</mark>

위 예제에서 후속 처리 메서드의 콜백 함수는 다음과 같이 인수를 전달받으면서 호출된다.

|후속 처리 메서드|콜백 함수의 인수|후속 처리 메서드의 반환값|
|---|---|---|
|then|promiseGet 함수가 반환한 프로미스가 resolve한 값(id가 1인 post의 userId)|콜백 함수가 반환한 프로미스|
|then|첫 번째 then 메서드가 반환한 프로미스가 resolve한 값(userId가 1인 유저 정보)|콜백 함수가 반환한 값을 resolve한 프로미스|
|catch|||
|(에러가 발생하지 않으면 호출되지 않는다.)|promiseGet 또는 앞선 훗속 처리 메서드가 반환한 프로미스가 reject한 값|콜백 함수가 반환한 값을 resolve한 프로미스|

프로미스는 프로미스 체이닝을 통해 비동기 처리 결과를 전달받아 후속 처리를 하므로 callback hell이 발생하지 않는다.
다만 프로미스도 콜백 패턴을 사용하므로 콜백 함수를 사용하지 않는 것은 아니다.

# 프로미스의 정적 메서드
## Promise.resolve / Promise.reject
`Promise.resolve`와 `Promise.reject` 메서드는 이미 존재하는 값을 래핑하여 프로미스를 생성하기 위해 사용한다.

`Promise.resolve` 메서드는 인수로 전달받은 값을 `resolve`하는 프로미스를 생성한다.
```js
// 배열을 resolve하는 프로미스를 생성
const resolvedPromise = Promise.resolve([1, 2, 3]);
resolvedPromise.then(console.log); // [1, 2, 3]


// 위와 아래의 코드는 똑같은 동작을 한다.

const resolvedPromise = new Promise(resolve => resolve([1, 2, 3]));
resolvedPromise.then(console.log); // [1, 2, 3]
```

`Promise.reject` 메서드는 인수로 전달받은 값을 `reject`하는 프로미스를 생성한다.

```js
// 에러 객체를 reject하는 프로미스를 생성
const rejectedPromise = Promise.reject(new Error('Error!'));
rejectedPromise.catch(console.log); // Error: Error!

// 위와 아래의 코드는 똑같은 동작을 한다.

const rejectedPromise = new Promise((_, reject) => reject(new Error('Error!')));
rejectedPromise.catch(console.log); // Error: Error!
```

## Promise.all
```js
const requestData1 = () => new Promise(resolve => setTimeout(() => resolve(1), 3000));
const requestData2 = () => new Promise(resolve => setTimeout(() => resolve(2), 2000));
const requestData3 = () => new Promise(resolve => setTimeout(() => resolve(3), 1000));

// 세 개의 비동기 처리를 순차적으로 처리
const res = [];
requestData1()
  .then(data => {
    res.push(data);
    return requestData2();
  })
  .then(data => {
    res.push(data);
    return requestData3();
  })
  .then(data => {
    res.push(data);
    console.log(res); // [1, 2, 3] ⇒ 약 6초 소요
  })
  .catch(console.error);
```
위 예제는 세 개의 비동기 처리를 순차적으로 처리한다. 앞선 비동기 처리가 완료하면 다음 비동기 처리를 수행한다. 따라서 위 예제는 총 6초 이상이 소요된다.

그런데 위 예제의 경우 세 개의 비동기 처리가 서로 의존하지 않고 개별적으로 수행된다. <mark style='background:#f7b731'>즉, 위의 비동기 처리는 순차적으로 처리할 필요가 없다.</mark>

`Promise.all` 메서드는 여러 개의 비동기 처리를 모두 **병렬 처리**할 때 사용한다.

```js
const requestData1 = () => new Promise(resolve => setTimeout(() => resolve(1), 3000));
const requestData2 = () => new Promise(resolve => setTimeout(() => resolve(2), 2000));
const requestData3 = () => new Promise(resolve => setTimeout(() => resolve(3), 1000));

Promise.all([requestData1(), requestData2(), requestData3()])
  .then(console.log) // [ 1, 2, 3 ] ⇒ 약 3초 소요
  .catch(console.error);
```

모든 프로미스가 `fulfilled` 상태가 되면 `resolve`된 처리 결과를 모두 배열에 저장해 새로운 프로미스를 반환한다. 이때 첫 번째 프로미스가 가장 나중에 `fulfilled` 되어도 `Promise.all` 메서드는 첫 번째 프로미스가 `resolve` 한 처리 결과부터 차례대로 배열에 저장한다. **즉, 처리 순서가 보장된다.**

`Promise.all` 메서드는 인수로 전달받은 배열의 프로미스가 하나라도 `rejected` 상태가 되면 나머지 프로미스가 `fulfilled` 상태가 되는 것을 기다리지 않고 즉시 종료한다.

## Promise.race
`Promise.race` 메서드는 `Promise.all` 메서드와 동일하게 프로미스를 요소로 갖는 배열 등의 이터러블을 인수로 전달받는다. 

`Promise.race` 메서드는 모든 프로미스가 `fulfilled` 상태가 되는 것을 기다리는 것이 아니라 가장 먼저 `fulfilled` 상태가 된 프로미스의 처리 결과를 `resolve` 하는 새로운 프로미스를 반환한다.

```js
Promise.race([
  new Promise(resolve => setTimeout(() => resolve(1), 3000)), // 1
  new Promise(resolve => setTimeout(() => resolve(2), 2000)), // 2
  new Promise(resolve => setTimeout(() => resolve(3), 1000)) // 3
])
  .then(console.log) // 3
  .catch(console.log);
```

프로미스가 하나라도 `rejected` 되면 에러를 `reject`하는 프로미스를 즉시 반환한다.

## Promise.allSettled
`Promise.allSettled` 메서드는 프로미스를 요소로 갖는 배열 등의 이터러블을 인수로 전달받는다. 그리고 전달받은 프로미스가 모두 `settled` 상태(비동기 처리가 수행된 상태, `fulfilled` 또는 `rejected`)가 되면 처리 결과를 배열로 반환한다.

```js
Promise.allSettled([
  new Promise(resolve => setTimeout(() => resolve(1), 2000)),
  new Promise((_, reject) => setTimeout(() => reject(new Error('Error!')), 1000))
]).then(console.log);
```

`Promise.allSettled` 메서드가 반환한 배열에는 `fulfilled` 또는 `rejected` 상태와는 상관없이 `Promise.allSettled` 메서드가 인수로 전달받은 모든 프로미스들의 처리 결과가 모두 담겨 있다. 프로미스의 처리 결과를 나타내는 객체는 다음과 같다.

- 프로미스가 `fulfilled` 상태인 경우 `value` 프로퍼티를 갖는다.
- 프로미스가 `rejected` 상태인 경우 `reason` 프로퍼티를 갖는다.

```js
[
  // 프로미스가 fulfilled 상태인 경우
  {status: "fulfilled", value: 1},
  // 프로미스가 rejected 상태인 경우
  {status: "rejected", reason: Error: Error! at <anonymous>:3:60}
]
```



---
> **참조**
> - 모던자바스크립트 DeepDive
> - 내일배움캠프 JS 문법 종합반 강의자료