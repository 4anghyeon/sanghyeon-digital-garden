---
{"tags":["JavaScript"],"dg-publish":true,"dg-hide":true,"permalink":"/language/java-script/generator/","hide":true,"dgPassFrontmatter":true,"noteIcon":""}
---

# 제네레이터란?
ES6에서 도입된 제네레이터는 코드 블록의 실행을 일시 중지했다가 필요한 시점에 재개할 수 있는 특수한 함수다. 제네레이터와 일반 함수의 차이는 다음과 같다.

1. **제네레이터는 함수 호출자에게 함수 실행의 제어권을 양도할 수 있다.**
    일반 함수를 호출하면 제어권이 함수에게 넘어가고 함수 코드를 일괄 실행한다. 제네레이터 함수는 함수 실행을 함수 호출자가 제어할 수 있다.
    
2. **제네레이터 함수는 함수 호출자와 함수의 상태를 주고받을 수 있다.**
    일반 함수는 함수가 실행되고 있는 동안에는 함수 외부에서 내부로 값을 전달하여 함수의 상태를 변경할 수 없다. 제네레이터 함수는 함수 호출자와 양방향으로 함수의 상태를 주고받을 수 있다.
    
3. **제네레이터 함수를 호출하면 제네레이터 객체를 반환한다.**
    <mark style='background:#f7b731'>제네레이터 함수를 호출하면 함수 코드를 실행하는 것이 아니라 이터러블이면서 동시에 이터레이터인 제네레이터 객체를 반환한다.</mark>


# 제네레이터 함수 정의
제네레이터 함수는 `*`가 붙은 `function*` 키워드로 선언한다. 그리고 하나 이상의 `yield` 표현식을 포함한다. 이것을 제외하면 일반 함수를 정의하는 방법과 같다.

```js
// 제너레이터 함수 선언문
function* genDecFunc() {
  yield 1;
}

// 제너레이터 함수 표현식
const genExpFunc = function* () {
  yield 1;
};

// 제너레이터 메서드
const obj = {
  * genObjMethod() {
    yield 1;
  }
};

// 제너레이터 클래스 메서드
class MyClass {
  * genClsMethod() {
    yield 1;
  }
}
```

애스터리스크(`*`)의 위치는 `function` 키워드와 함수 이름 사이라면 어디든지 상관없다. 하지만 일관성을 유지하기 위해 `function` 키워드 바로 뒤에 붙이는 것을 권장한다.

제네레이터 함수는 화살표 함수로 정의할 수 없다.
```js
const genArrowFunc = * () => {
  yield 1;
}; // SyntaxError: Unexpected token '*'
```

제네레이터 함수는 `new` 연산자와 함께 생성자 함수로 호출할 수 없다.
```js
function* genFunc() {
  yield 1;
}

new genFunc(); // TypeError: genFunc is not a constructor
```

# 제네레이터 객체
**제네레이터 함수를 호출하면 함수 코드를 실행하는 것이 아니라 이터러블(Iterable)이면서 동시에 이터레이터인 제네레이터 객체를 반환한다.**

```js
// 제너레이터 함수
function* genFunc() {
  yield 1;
  yield 2;
  yield 3;
}

// 제너레이터 함수를 호출하면 제너레이터 객체를 반환한다.
const generator = genFunc();

// 제너레이터 객체는 이터러블이면서 동시에 이터레이터다.
// 이터러블은 Symbol.iterator 메서드를 직접 구현하거나 프로토타입 체인을 통해 상속받은 객체다.
console.log(Symbol.iterator in generator); // true
// 이터레이터는 next 메서드를 갖는다.
console.log('next' in generator); // true
```

제네레이터 객체는 `next` 메서드를 갖는 이터레이터지만 이터레이터에는 없는 `return`, `throw` 메서드를 갖는다. 제네레이터 객체의 세 개의 메서드를 호출하면 다음과 같이 동작한다.

- `next` 메서드를 호출하면 제네레이터 함수의 `yield` 표현식까지 코드블록을 실행하고 `yield` 된 값을 `value` 프로퍼티 값으로, `false`를 `done` 프로퍼티 값으로 갖는 이터레이터 리절트 객체를 반환한다.
- `return` 메서드를 호출하면 인수로 전달받은 값을 `value` 프로퍼티 값으로, `true`를 `done` 프로퍼티 값으로 갖는 이터레이터 리절트 객체를 반환한다.
- `throw` 메서드를 호출하면 인수로 전달받은 에러를 발생시키고 `undefined`를 `value` 프로퍼티 값으로 `true`를 `done` 프로퍼티 값으로 갖는 객체를 반환한다.

# 제네레이터의 일시 중지와 재개
제네레이터는 `next` 메서드를 호출하면 제네레이터 함수의 코드 블록을 실행한다. 단, 일반 함수처럼 모든 코드를 실행하는 것이 아니라 `yield` 표현식까지만 실행한다.

`yield` 키워드는 제네레이터 함수의 실행을 일시 중지시키거나 `yield` 키워드 뒤에 오는 표현식의 평가 결과를 제네레이터 함수 호출자에게 반환한다.

```js
// 제너레이터 함수
function* genFunc() {
  yield 1;
  yield 2;
  yield 3;
}

// 제너레이터 함수를 호출하면 제너레이터 객체를 반환한다.
// 이터러블이면서 동시에 이터레이터인 제너레이터 객체는 next 메서드를 갖는다.
const generator = genFunc();

console.log(generator.next()); // {value: 1, done: false}
console.log(generator.next()); // {value: 2, done: false}
console.log(generator.next()); // {value: 3, done: false}
console.log(generator.next()); // {value: undefined, done: true}
```

이터레이터의 `next` 메서드와 달리 제네레이터 객체의 `next` 메서드에는 인수를 전달할 수 있다. 제네레이터 객체의 `next` 메서드에 전달한 인수는 제네레이터 함수의 `yield` 표현식을 할당받는 변수에 할당된다.
```js
function* genFunc() {
  const x = yield 1;
  const y = yield (x + 10);
  return x + y;
}

const generator = genFunc(0);

// 처음 호출하는 next 메서드에는 인수를 전달하지 않는다.
// 만약 처음 호출하는 next 메서드에 인수를 전달하면 무시된다.
// next 메서드가 반환한 이터레이터 리절트 객체의 value 프로퍼티에는 첫 번째 yield된 값 1이 할당된다.
let res = generator.next();
console.log(res); // {value: 1, done: false}

// next 메서드에 인수로 전달한 10은 genFunc 함수의 x 변수에 할당된다.
// next 메서드가 반환한 이터레이터 리절트 객체의 value 프로퍼티에는 두 번째 yield된 값 20이 할당된다.
res = generator.next(10);
console.log(res); // {value: 20, done: false}

// next 메서드에 인수로 전달한 20은 genFunc 함수의 y 변수에 할당된다.
// next 메서드가 반환한 이터레이터 리절트 객체의 value 프로퍼티에는 제너레이터 함수의 반환값 30이 할당된다.
res = generator.next(20);
console.log(res); // {value: 30, done: true}
```
이처럼 제네레이터 함수는 `next` 메서드와 `yield` 표현식을 통해 함수 호출자와 함수의 상태를 주고받을 수 있다.

# 제네레이터의 활용
## 비동기 처리
[[Language/JavaScript/프로미스 (Promise)#프로미스(Promise)의 생성\|프로미스]]에서 살펴봤던 비동기 처리를 제네레이터를 이용해 구현해보자.
```js
let addCoffee = function (prevName, name) {  
  setTimeout(function () {  
    coffeeMaker.next(prevName ? prevName + ", " + name : name);  
  }, 500);  
};  
  
let coffeeGenerator = function* () {  
  let espresso = yield addCoffee("", "espresso");  
  console.log(espresso);  
  let americano = yield addCoffee(espresso, "americano");  
  console.log(americano);  
  let mocha = yield addCoffee(americano, "mocha");  
  console.log(mocha);  
  let latte = yield addCoffee(mocha, "latte");  
  console.log(latte);  
};  
  
let coffeeMaker = coffeeGenerator();  
coffeeMaker.next();
```

제네레이터 함수는 `next` 메서드와 `yield` 표현식을 통해 함수 호출자와 함수의 상태를 주고받을 수 있다. 이러한 특성을 활용하여 프로미스를 사용한 비동기 처리를 동기 처리처럼 구현할 수 있다.

다시 말해, 프로미스의 후속 처리 메서드 `then` / `catch` / `finally` 없이 비동기 처리 결과를 반환하도록 구현할 수 있다.


---
> **참조**
> - 모던자바스크립트 DeepDive
> - 내일배움캠프 JS 문법 종합반 강의자료