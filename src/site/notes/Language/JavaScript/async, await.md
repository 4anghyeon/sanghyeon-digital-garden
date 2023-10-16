---
{"tags":["JavaScript"],"dg-publish":true,"dg-hide":true,"permalink":"/language/java-script/async-await/","hide":true,"dgPassFrontmatter":true,"noteIcon":""}
---

ES8에서 [[Language/JavaScript/제네레이터 (Generator)\|제네레이터 (Generator)]]보다 간단하고 가독성 좋게 비동기 처리를 동기 처리처럼 동작하도록 구현할 수 있는 `async` / `await`가 도입되었다.
`async` / `await` 는 프로미스를 기반으로 동작한다.

## async 함수
`await` 키워드는 반드시 `async` 함수 내부에서 사용해야 한다. `async` 함수는 `async` 키워드를 사용해 정의하며 언제나 프로미스를 반환한다. 
`async` 함수가 명시적으로 프로미스를 반환하지 않더라도 암묵적으로 반환값을 `resolve` 하는 프로미스를 반환한다.

```js
// async 함수 선언문
async function foo(n) { return n; }
foo(1).then(v => console.log(v)); // 1

// async 함수 표현식
const bar = async function (n) { return n; };
bar(2).then(v => console.log(v)); // 2

// async 화살표 함수
const baz = async n => n;
baz(3).then(v => console.log(v)); // 3

// async 메서드
const obj = {
  async foo(n) { return n; }
};
obj.foo(4).then(v => console.log(v)); // 4

// async 클래스 메서드
class MyClass {
  async bar(n) { return n; }
}
const myClass = new MyClass();
myClass.bar(5).then(v => console.log(v)); // 5
```

## await 키워드
`await` 키워드는 프로미스가 `settled` 상태가 될 때까지 대기하다가 `settled` 상태가 되면 프로미스가 `resolve`한 처리 결과를 반환한다. `await` 키워드는 반드시 프로미스 앞에서 사용해야 한다.

```js
const getGithubUserName = async id => {
  const res = await fetch(`https://api.github.com/users/${id}`); // ①
  const { name } = await res.json(); // ②
  console.log(name); // Sanghyeon Lee
};
```

`await` 키워드는 ①의 `fetch` 함수의 응답이 도착해서 반환한 프로미스가 `settled` 될 때까지 대기한다. **이후 프로미스 `settled` 상태가 되면 프로미스가 `resolve` 한 처리가 결과가 `res` 변수에 할당된다.**

이처럼 `await` 키워드는 다음 실행을 일시 중지시켰다가 프로미스가 `settled` 상태가 되면 다시 재개한다.

```js
async function foo() {
  const a = await new Promise(resolve => setTimeout(() => resolve(1), 3000));
  const b = await new Promise(resolve => setTimeout(() => resolve(2), 2000));
  const c = await new Promise(resolve => setTimeout(() => resolve(3), 1000));

  console.log([a, b, c]); // [1, 2, 3]
}

foo(); // 약 6초 소요된다.
```

[[Language/JavaScript/프로미스 (Promise)#프로미스(Promise)의 생성\|프로미스]]에서 살펴봤던 예제를 `async/await`으로 구현해보자.

```js
let addCoffee = function (name) {  
  return new Promise(function (resolve) {  
    setTimeout(function () {  
      resolve(name);  
    }, 500);  
  });  
};  
  
let coffeeMaker = async function () {  
  let coffeeList = "";  
  let _addCoffee = async function (name) {  
    coffeeList += (coffeeList ? ", " : "") + (await addCoffee(name));  
  };  
  
  // promise를 반환하는 함수의 경우, await를 만나면 무조건 끝날 때 까지 기다린다.  
  await _addCoffee("espresso");  
  console.log(coffeeList);  
  await _addCoffee("americano");  
  console.log(coffeeList);  
  await _addCoffee("mocha");  
  console.log(coffeeList);  
  await _addCoffee("latte");  
  console.log(coffeeList);  
};  
  
coffeeMaker();
```
## 에러처리
`async/await` 에서는 에러처리에 `try...catch`를 이용할 수 있다.

```js
const foo = async () => {
  try {
    const wrongUrl = 'https://wrong.url';

    const response = await fetch(wrongUrl);
    const data = await response.json();
    console.log(data);
  } catch (err) {
    console.error(err); // TypeError: Failed to fetch
  }
};

foo();
```

`async`함수 내에서 `try...catch`를 이용해 에러처리를 하지 않으면 `async` 함수는 발생한 에러를 `reject`하는 프로미스를 반환한다.
```js
const foo = async () => {
  const wrongUrl = 'https://wrong.url';

  const response = await fetch(wrongUrl);
  const data = await response.json();
  return data;
};

foo()
  .then(console.log)
  .catch(console.error); // TypeError: Failed to fetch
```


---
> **참조**
> - 모던자바스크립트 DeepDive
> - 내일배움캠프 JS 문법 종합반 강의자료