---
{"tags":["JavaScript"],"dg-publish":true,"dg-hide":true,"permalink":"/language/java-script/closure/","hide":true,"dgPassFrontmatter":true,"noteIcon":""}
---

클로저는 난해하기로 유명한 자바스크립트의 개념 중 하나이다. 그러나 클로저는 자바스크립트 고유의 개념이 아니다.
함수를 일급 객체로 취급하는 함수형 프로그래밍 언어(하스켈, 리스프, 스칼라 등)에서 사용하는 중요한 특성이다.

MDN에서는 클로저에 대해 다음과 같이 정의하고 있다.
> “A closure is the combination of a function and the lexical environment within which that function was declared” 클로저는 함수와 그 함수가 선언된 렉시컬 환경과의 조합이다.

위 정의에서 먼저 이해해야할 핵심 키워드는 “함수가 선언된 [[Language/JavaScript/렉시컬 환경 (Lexcial Environment)\|렉시컬 환경 (Lexcial Environment)]]" 이다.

```js
const x = 1;

function outer() {
	const x = 10;

	inner();
}

function inner() {
	console.log(x); // 1
}

outer();
```
만약 `inner` 함수가 `outer` 함수의 내부에서 정의된 중첩 함수가 아니면 `inner` 함수를 `outer` 함수 내부에서 호출하더라도 `outer`의 지역 변수(10)에 접근할 수 없다.

이는 자바스크립트가 렉시컬 스코프(정적 스코프)를 따르기 때문이다.

# 렉시컬 스코프
자바스크립트 엔진은 함수를 어디서 호출했는지가 아니라 함수를 어디서 정의했는지에 따라 상위 스코프를 결정하는 렉시컬 스코프(정적 스코프)를 따른다.

함수의 상위 스코프를 결정한다는 것은 ''[[Language/JavaScript/렉시컬 환경 (Lexcial Environment)#외부 렉시컬 환경에 대한 참조 결정\|렉시컬 환경의 외부 렉시컬 환경에 대한 참조]]에 저장할 참조값을 결정한다.” 와 같다.

**즉, 상위 스코프에 대한 참조는 함수 정의가 평가되는 시점에 정의된 위치에 의해 결정된다. 이것이 바로 렉시컬 스코프다.**

# 클로저와 렉시컬 환경
```js
const x = 1;

// ①
function outer() {
	const x = 10;
	const inner = function () { console.log(x) }; // ②
	return inner;
}

const innerFunc = outer(); // ③
innerFunc(); // ④ 10
```
`outer` 함수를 호출(③) 하면 `outer` 함수는 중첩 함수 `inner` 를 반환하고 생명 주기를 마감한다. 즉, `outer` 함수의 실행 컨텍스트는 실행 컨텍스트 스택에서 제거된다. 따라서 `outer` 함수의 지역 변수 `x` 도 생명 주기를 마감하여 이 `x` 에 접근하는 방법은 달리 없어 보인다.

그러나 ④ 코드의 실행 결과는 `outer` 함수의 지역 변수 `x` 의 값인 10이다. `outer` 함수의 생명주기는 종료되어 실행 컨텍스트 스택에서도 제거 되었는데 살아있는 것처럼 동작하고 있다.

이처럼 **외부 함수보다 중첩 함수가 더 오래 유지되는 경우 중첩 함수는 이미 생명 주기가 종료한 외부 함수의 변수를 참조할 수 있다. 이러한 함수를 클로저(Closure) 라고 부른다.**

다시 MDN이 설명한 클로저의 정의로 돌아가보면

> “A closure is the combination of a function and the lexical environment within which that function was declared” 클로저는 함수와 그 함수가 선언된 렉시컬 환경과의 조합이다.

그 함수가 선언된 렉시컬 환경이란 함수가 정의된 위치의 상위 스코프를 의미하는 실행 컨텍스트의 렉시컬 환경을 말한다.

**자바스크립트의 모든 함수는 자신의 상위 스코프를 기억하고, 모든 함수가 기억하는 상위 스코프는 어디서 호출하든 상관없이 유지된다**. 따라서 함수를 어디서 호출하든 함수 자신이 기억하는 상위 스코프의 식별자를 참조하고, 그 값을 변경할 수도 있다.

중첩 함수 `inner` 는 외부 함수 `outer` 보다 더 오래 생존했다. 이때 더 오래 생존한 중첩 함수는 외부 함수의 생존 여부(실행 컨텍스트의 생존 여부)와 상관없이 자신이 정의된 위치에 의해 결정된 상위 스코프를 기억한다.

자바스크립트의 모든 함수는 상위 스코프를 기억하므로 이론적으로 모든 함수는 클로저다. 하지만 모든 일반적으로 모든 함수를 클로저라고 하지는 않는다.

## 어떤게 클로저인 함수일까?
### 예제 1
```js
function foo() {  
    const x = 1;  
    const y = 2;  
  
    // 일반적으로 클로저라고 하지 않는다.  
    function bar() {  
        const z = 3;  
  
        debugger;  
        // 상위 스코프의 식별자를 참조하지 않는다.  
        console.log(z);  
    }  
  
    return bar;  
}  
  
const bar = foo();  
bar();
```
위의 코드를 개발자 도구에서 실행해보자.

![Pasted image 20231017200536.png](/img/user/Pasted%20image%2020231017200536.png)
`bar`함수는 `foo`보다 더 오래 유지되지만 상위 스코프의 어떤 식별자도 참조하지 않는다. 따라서 `bar`는 클로저라고 할 수 없다.

### 예제 2
```js
function foo() {  
    const x = 1;  
  
    // 일반적으로 클로저라고 하지 않는다.  
    // bar 함수는 클로저였지만 곧바로 소멸한다.    
    function bar() {  
        debugger;  
        // 상위 스코프의 식별자를 참조한다.  
        console.log(x);  
    }  
    bar();  
}  
  
foo();
```

![Pasted image 20231017200949.png](/img/user/Pasted%20image%2020231017200949.png)
위 예제는 중첩 함수 `bar` 가 상위 스코프의 식별자를 참조하고 있으므로 클로저다. 하지만 외부로 `bar` 함수가 반환되지 않으므로 외부 함수보다 일찍 소멸하기 때문에 클로저의 본질에 부합하지 않는다.

### 예제 3
```js
function foo() {  
    const x = 1;  
    const y = 2;  
  
    // 클로저  
    // 중첩 함수 bar는 외부 함수보다 더 오래 유지되며 상위 스코프의 식별자를 참조한다.    
    function bar() {  
        debugger;  
        console.log(x);  
    }  
    return bar;  
}  
  
const bar = foo();  
bar();
```

![Pasted image 20231017201220.png](/img/user/Pasted%20image%2020231017201220.png)

위 예제의 중첩 함수 `bar` 는 상위 스코프의 식별자를 참조하고 있으므로 클로저다. 그리고 외부 함수의 외부로 반환되어 외부 함수보다 더 오래 살아남는다.

이처럼 클로저는 중첩 함수가 상위 스코프의 식별자를 참조하고 있고 중첩 함수가 외부 함수보다 더 오래 유지되는 경우에 한정하는 것이 일반적이다.

# 클로저의 활용
**클로저는 상태(state)를 안전하게 변경하고 유지하기 위해 사용한다.** 다시 말해, **상태가 의도치 않게 변경되지 않도록 상태를 안전하게 은닉하고 특정 함수에게만 상태 변경을 허용**하기 위해 사용한다.

다음 예제는 호출된 횟수(`num`)이 안전하게 변경하고 유지해야 할 상태다.
```jsx
let num = 0; // 카운트 상태

const increase = function () {
	return ++num;
};

console.log(increase()); // 1
num = 100;
console.log(increase()); // 101 << 오류 발생!
console.log(increase()); // 102
```

위 예제가 바르게 동작하려면 다음과 같은 전제 조건이 지켜져야 한다.

1. `num` 은 `increase` 함수가 호출되기 전까지 변경되지 않고 유지되어야 한다.
2. 이를 위해 `num`은 `increase`만이 변경할 수 있어야 한다.

하지만 현재 `num`은 전역 변수를 통해 관리되고 있기 때문에 언제든지 누구나 접근할 수 있고 변경할 수 있다. 
따라서 카운트 상태를 안전하게 변경하고 유지하기 위해서는 `increase` 함수만이 `num` 변수를 참조하고 변경할 수 있게 하는 것이 바람직하다.

2번 조건을 지키기 위해 `num` 변수를 외부에서 접근하지 못하도록 `increase`함수 안으로 이동시켰다.
```js
const increase = function () {
	let num = 0; // 카운트 상태
	
	return ++num;
}

// 이전 상태를 유지하지 못한다.
console.log(increase()); // 1
console.log(increase()); // 1
console.log(increase()); // 1
```
전역 변수 `num`을 `increase` 함수의 지역 변수로 변경하여 의도치 않은 상태 변경은 방지 했다. 이제 `num` 변수의 상태는 `increase` 만이 변경할 수 있다.

하지만 `increase` 상태가 호출될 때마다 지역 변수가 초기화 되므로 이전 상태를 유지하지 못한다. 이전 상태를 유지할 수 있도록 클로저를 사용해보자.
```js
const increase = (function () {
	let num = 0;
	
	return function () {
		return ++num;
	};
}());

console.log(increase()); // 1
console.log(increase()); // 2
console.log(increase()); // 3
```
위 코드가 실행되면 즉시 실행 함수가 호출되어 반환된 함수가 `increase` 변수에 할당된다. `increase` 변수에 할당된 함수는 자신이 정의된 위치에 의해 결정된 상위 스코프(즉시 실행 함수)의 렉시컬 환경을 기억하는 클로저다.

**즉시 실행 함수는 호출 이후 즉시 소멸되지만 반환된 클로저는 `increase` 변수에 할당되어 호출된다. 이 클로저가 상위 스코프의 렉시컬 환경을 기억하고 있기 때문에 `num`을 언제 어디서 호출하든지 참조하고 변경할 수 있다.**

클로저는 상태(state)가 의도치 않게 변경되지 않도록 안전하게 은닉(information hiding) 하고 특정 함수에게만 상태 변경을 허용하여 상태를 안전하게 변경하고 유지하기 위해 사용한다.

# 캡슐화와 정보 은닉
캡슐화는 객체의 프로퍼티와 메서드를 하나로 묶는 것을 말한다. 캡슐화는 객체의 특정 프로퍼티나 메서드를 감출 목적으로 사용하기도 하는데 이를 정보 은닉이라 한다.

정보 은닉은 외부에 공개할 필요가 없는 일부를 공개되지 않도록 감추어 적절치 못한 접근으로부터 보호하고, 객체 간의 상호 의존성, 즉 결합도를 낮추는 효과가 있다.

대부분의 객체 지향 프로그래밍 언어는 `public`, `private`, `protected` 같은 접근 제한자를 선언하여 공개 범위를 한정할 수 있다.

자바스크립트는 이러한 접근 제한자를 제공하지 않는다. 따라서 자바스크립트 객체의 모든 프로퍼티와 메서드는 기본적으로 외부에 공개되어 있다. 즉, 기본적으로 `public` 하다.
```js
function Person(name, age) {
  this.name = name; // public
  let _age = age;   // private

  // 인스턴스 메서드
  this.sayHi = function () {
    console.log(`Hi! My name is ${this.name}. I am ${_age}.`);
  };
}

const me = new Person('Lee', 20);
me.sayHi(); // Hi! My name is Lee. I am 20.
console.log(me.name); // Lee
console.log(me._age); // undefined

const you = new Person('Kim', 30);
you.sayHi(); // Hi! My name is Kim. I am 30.
console.log(you.name); // Kim
console.log(you._age); // undefined
```
위 예제의 `name` 프로퍼티는 외부로 공개되어 있어 자유롭게 참조, 변경할 수 있다. 하지만 `_age` 변수는 생성자 함수의 지역 변수이므로 `Person` 생성자 함수 외부에서 참조하거나 변경할 수 없다.

하지만 위 예제의 `sayHi`는 인스턴스 메서드이므로 `Person` 객체가 생성될 때마다 중복 생성된다. `sayHi` 메서드를 프로토타입 메서드로 변경하여 중복 생성을 방지하자.

```js
function Person(name, age) {
  this.name = name; // public
  let _age = age;   // private
}

// 프로토타입 메서드
Person.prototype.sayHi = function () {
  // Person 생성자 함수의 지역 변수 _age를 참조할 수 없다
  console.log(`Hi! My name is ${this.name}. I am ${_age}.`);
};
```

이때 `Person.prototype.sayHi` 메서드 내에서 지역 변수 `_age` 를 참조할 수 없는 문제가 발생한다. 

따라서 다음과 같이 즉시 실행 함수를 사용하여 `Person` 생성자 함수와 `Person.prototype.sayHi` 메서드를 하나의 함수 내에 모아보자.
```js
const Person = (function () {
  let _age = 0; // private

  // 생성자 함수
  function Person(name, age) {
    this.name = name; // public
    _age = age;
  }

  // 프로토타입 메서드
  Person.prototype.sayHi = function () {
    console.log(`Hi! My name is ${this.name}. I am ${_age}.`);
  };

  // 생성자 함수를 반환
  return Person;
}());

const me = new Person('Lee', 20);
me.sayHi(); // Hi! My name is Lee. I am 20.
console.log(me.name); // Lee
console.log(me._age); // undefined

const you = new Person('Kim', 30);
you.sayHi(); // Hi! My name is Kim. I am 30.
console.log(you.name); // Kim
console.log(you._age); // undefined
```

위 패턴을 사용하면 접근 제한자를 제공하지 않는 자바스크립트에서도 정보 은닉이 가능한 것처럼 보인다. 하지만 위 코드도 문제가 있다. `Person` 생성자 함수가 여러 개의 인스턴스를 생성할 경우 `_age` 변수의 상태가 유지되지 않는다는 것이다.

```js
const me = new Person('Lee', 20);
me.sayHi(); // Hi! My name is Lee. I am 20.

const you = new Person('Kim', 30);
you.sayHi(); // Hi! My name is Kim. I am 30.

// _age 변수 값이 변경된다!
me.sayHi(); // Hi! My name is Lee. I am 30.
```

이는 `Person.prototype.sayHi` 메서드가 단 한 번 생성되는 클로저이기 때문에 발생하는 현상이다.

이처럼 자바스크립트는 정보 은닉을 완전하게 지원하지 않는다. 다행히도 현재 `private` 필드를 정의할 수 있는 새로운 표준 사양이 제안되어 있다. 표준 사양으로 승급이 확실시되는 이 제안은 현재 최신 브라우저에 이미 구현되어 있다. 

## 자주 발생하는 실수
```js
var funcs = [];

for (var i = 0; i < 3; i++) {
  funcs[i] = function () { return i; }; // ①
}

for (var j = 0; j < funcs.length; j++) {
  console.log(funcs[j]()); // ②
}
```
②의 결과로 1, 2, 3이 출력될 것을 기대하지만 결과는 그렇지 않다.

`for` 문 안에서 선언한 `i` 가 `var` 로 선언되었기 때문에 블록 레벨 스코프가 아닌 함수 레벨 스코프를 갖기 때문에 전역 변수로 취급된다.
따라서 `funcs` 에 들어가게 되는 함수는 같은 전역 변수 `i` 를 보게되고, 마지막으로 할당된 3이 3번 출력된다.

클로저를 사용해 위 예제를 바르게 동작하게 하면 다음과 같다.
```jsx
var funcs = [];

for (var i = 0; i < 3; i++){
  funcs[i] = (function (id) { // ①
    return function () {
      return id;
    };
  }(i));
}

for (var j = 0; j < funcs.length; j++) {
  console.log(funcs[j]());
}
```


사실 위 예제는 `for` 문 안에 `var` 키워드를 사용해 생긴 현상으로 `let` 을 사용하면 깔끔하게 해결된다.
```jsx
var funcs = [];

for (let i = 0; i < 3; i++) {
  funcs[i] = function () { return i; }; // ①
}

for (let j = 0; j < funcs.length; j++) {
  console.log(funcs[j]()); // ②
}
```

`let` 은 블록 레벨 스코프를 따르므로 코드 블록마다 새로운 렉시컬 환경을 생성한다.

---
> **참조**
> - 모던자바스크립트 DeepDive
> - 내일배움캠프 JS 문법 종합반 강의자료