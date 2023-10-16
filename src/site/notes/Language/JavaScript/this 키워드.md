---
{"tags":["JavaScript"],"dg-publish":true,"dg-hide":true,"permalink":"/language/java-script/this/","hide":true,"dgPassFrontmatter":true,"noteIcon":""}
---

동작을 나타나내는 메서드는 자신이 속한 객체의 상태, 즉 프로퍼티를 참조하고 변경할 수 있어야 한다. 이때 메서드가 자신이 속한 객체의 프로퍼티를 참조하려면 자신이 속한 객체를 가리키는 식별자를 참조할 수 있어야 한다.

```js
const circle = {
  radius: 5,
  getDiameter() {
    return 2 * circle.radius;
  }
}
```
위 예제에서 `getDiameter` 메서드가 호출되는 시점에는 이미 객체 리터럴의 평가가 완료되어 객체가 생성되었고 `circle` 식별자에 객체가 할당된 이후다. 따라서 메서드 내부에서 `circle` 식별자를 참조할 수 있다. 하지만 자기 자신이 속한 객체를 재귀적으로 참조하는 방식은 일반적이지 않으며 바람직하지도 않다.

따라서 자신이 속한 객체 또는 자신이 생성할 인스턴스를 가리키는 특수한 식별자가 필요하다. 자바스크립트는 이를 위해 `this`라는 특수한 식별자를 제공한다.

`this`**는 자신이 속한 객체 또는 자신이 생성할 인스턴스를 가리키는 자기 참조 변수다.** `this`**를 통해 자신이 속한 객체 또는 자신이 생성할 인스턴스의 프로퍼티나 메소드를 참조할 수 있다.**

```js
// 객체 리터럴
const circle = {
  radius: 5,
  getDiameter() {
    // this는 메서드를 호출한 객체를 가리킨다.
    return 2 * this.radius;
  }
};

// 생성자 함수
function Circle(radius) {
  this.radius = radius;
}

Circle.prototype.getDiameter = function() {
  // this는 생성자 함수가 생성할 인스턴스를 가리킨다.
  return 2 * this.radius;
}
```
이처럼 `this`는 상황에 따라 가리키는 대상이 다르다.


`this`는 객체의 프로퍼티나 메서드를 참조하기 위한 자기 참조 변수이므로 일반적으로 객체의 메서드 내부 또는 생성자 함수 내부에서만 의미가 있다. 따라서 일반 함수 내부의 `this`는 일반적으로 전역 객체를 가리킨다. `strict mode`가 적용된 일반 함수 내부의 `this`에는 `undefined`가 바인딩 된다. 일반 함수에서는 this를 사용할 필요가 없기 때문이다.

```js
let func = function (a) {  
  console.log(this); // window or global
};
```

```js
"use strict";

let func = function (a) {  
  console.log(this); // undefined
};
```

# 함수 호출 방식과 this 바인딩
`this`는 자바스크립트 엔진에 의해 암묵적으로 생성되며 `this`가 가리키는 값, `this` 바인딩(식별자와 값을 연결하는 과정)은 함수 호출 방식에 의해 동적으로 결정된다.

## 일반 함수 호출
기본적으로 `this`에는 전역 객체가 바인딩 된다. (브라우저: window, Node.js: global)
```js
let func = function (a, b, c, d) {  
  console.log(this);  
};

func(); // window or global
```

메서드 내에서 정의한 중첩 함수도 일반 함수로 호출되면 중첩 함수 내부의 `this`도 전역 객체가 바인딩 된다.
```js
var obj1 = {  
  outer: function () {  
    var innerFunc = function () {  
      console.log(this); // widnow or global
    };  
    innerFunc();  
  
    var obj2 = {  
      innerMethod: innerFunc,  
    };  
    obj2.innerMethod();  
  },  
};  
obj1.outer();
```
<mark style='background:#f7b731'>이처럼 일반 함수로 호출된 모든 함수(중첩 함수, 콜백 함수...) 내부의 this에는 전역 객체가 바인딩 된다.</mark>

## 메서드 호출
```js
const person = {
  name: 'Lee',
  getName() {
    // 메서드 내부의 this는 메서드를 호출한 객체에 바인딩 된다.
    return this.name;
  }
};

console.log(person.getName()); // Lee

const getName = person.getName;

console.log(getName()); // ''
// 일반 함수로 호출된 getName 함수 내부의 this.name은 window.name과 같다.
```
따라서 메서드 내부의 `this`는 프로퍼티로 메서드를 가리키고 있는 객체와는 관계가 없고 메서드를 호출한 객체에 바인딩 된다.

## 생성자 함수 호출
생성자 함수 내부의 `this`에는 생성자 함수가 생성할 인스턴스가 바인딩 된다.
```js
// 생성자 함수
function Circle(radius) {
  this.radius = radius;
  this.getDiameter = function () {
    return 2 * this.radius;
  }
}

const circle1 = new Circle(5);
console.log(circle1.getDiameter()); // 10

const circle2 = Circle(10); // 만약 new를 붙이지 않으면 생성자 함수가 아니라 일반 함수로 동작한다.
console.log(circle2); // undefined

// Circle 내부의 this는 전역 객체를 가리킨다.
console.log(radius); // 15
```

## apply, call, bind 메서드에 의한 간접 호출

### apply, call
`apply`와 `call` 메서드의 본질적 기능은 함수를 호출하는 것이다. 첫 번째 인수로 전달한 객체를 호출한 함수의 `this`에 바인딩 한다. `apply`와 `call` 메서드는 인수를 전달하는 방식만 다를 뿐 동일하게 동작한다.

```js
function getThisBinding() {
  console.log(arguments);
  return this;
}

const thisArg = { a: 1 };

// apply 메서드는 인수를 배열로 묶어 전달
console.log(getThisBinding.apply(thisArgs, [1, 2, 3])); // {a: 1}

// call 메서드는 인수를 쉼표로 구분한 리스트 형식으로 전달
console.log(getThisBinding.call(thisArgs, 1, 2, 3)); // {a: 1}
```

`apply`와 `call` 메서드의 대표적인 용도는 `arguments` 객체와 같은 유사 배열 객체에 배열 메서드를 사용하는 경우다. `arguments` 객체는 유사 배열이기 때문에 배열의 메서드를 사용할 수 없으나 `apply`와 `call` 메서드를 이용하면 가능하다.

```js
function convertArgsToArray() {
  const arr = Array.prototype.slice.call(arguments);

  return arr;
}
convertArgsToArray(1, 2, 3);
```


### bind
`bind` 메서드는 `apply`, `call` 메서드와 달리 함수를 호출하지 않는다. 단지 `this` 바인딩이 교체된 함수를 새롭게 생성해 반환한다. 콜백 함수 와 외부 함수의 `this`를 일치시킬 때 주로 사용한다.
```js
let func = function (a, b, c, d) {  
  console.log(this, a, b, c, d);  
};  
  
// 함수에 this를 미리 적용!  
let bindFunc1 = func.bind({ x: 1 });  
bindFunc1(5, 6, 7, 8);  // { x: 1}, 5, 6, 7, 8
  
// 부분 적용 함수  
let bindFunc2 = func.bind({ x: 1 }, 4, 5);  
bindFunc2(6, 7); // {x: 1}, 4, 5, 6, 7
```

---
> **참조**
> - 모던자바스크립트 DeepDive
> - 내일배움캠프 JS 문법 종합반 강의자료