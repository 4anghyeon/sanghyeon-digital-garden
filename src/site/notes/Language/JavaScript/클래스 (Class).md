---
{"tags":["JavaScript"],"dg-publish":true,"dg-hide":true,"permalink":"/language/java-script/class/","hide":true,"dgPassFrontmatter":true,"noteIcon":""}
---

# 클래스
ES6에서 도입된 `Class`는 자바나 C#과 같은 클래스 기반 객체지향 프로그래밍에 익숙한 프로그래머가 더 빠르게 학습할 수 있도록 클래스 기반 객체지향 프로그램이 언어와 매우 흡사한 새로운 객체 생성 메커니즘을 제시한다.

클래스는 쉽게 말해서 객체를 만들기 위한 설계도이고 인스턴스는 이 설계도를 바탕으로 만들어진 실제 객체라고 볼 수 있다.

클래스와 생성자 함수 모두 프로토타입 기반의 인스턴스를 생성하지만 정확히 동일하게 동작하지는 않는다.
## 클래스  VS 생성자 함수
| 차이점       | 클래스                                                 | 생성자 함수                                                                                                          |
| ------------ | ------------------------------------------------------ | -------------------------------------------------------------------------------------------------------------------- |
| `new` 연산자 | `new` 연산자 없이 호출하면 에러가 발생한다.            | `new` 연산자 없이 호출하면 일반 함수로서 호출된다.                                                                   |
| 상속         | 상속을 지원하는 `extends`와 `super` 키워드를 제공한다. | 지원하지 않는다.                                                                                                     |
| 호이스팅     | 호이스팅이 발생하지 않는 것처럼 동작한다.              | 함수 선언문으로 정의된 생성자 함수는 함수 호이스팅이, 함수 표현식으로 정의한 생성자 함수는 변수 호이스팅이 발생한다. |
| `strict mode`             |  클래스 내의 모든 코드는 `strict mode`가 지정되어 실행되며 해제할 수 없다.                                                      |                                                                                                                성자 함수는 암묵적으로 `strict mode`가 지정되지 않는다.      |

# 클래스 정의
```js
// 클래스 선언문
class Person {}

// 익명 클래스 표현식
const Person = class {};

// 기명 클래스 표현식
const Person = class MyClass {};
```

클래스를 표현식으로 정의할 수 있다는 것은 클래스가 값으로 사용이 가능한 [[Language/JavaScript/함수와  일급 객체#일급 객체로서의 함수\|일급 객체]]라는 뜻이다. 일급 객체는 다음과 같은 특징을 갖는다.

- 무명의 리터럴로 생성할 수 있다.
- 변수나 자료구조에 저장할 수 있다.
- 함수의 매개변수에 전달할 수 있다.
- 함수의 반환값으로 사용할 수 있다.

# 클래스 호이스팅
클래스는 함수로 평가된다. 단, 클래스는 클래스 정의 이전에 참조할 수 없다.
```js
console.log(Person); // ReferenceError 
class Person {}
```

클래스 선언문은 호이스팅이 발생하지 않는 것처럼 보이나 그렇지 않다.
```js
const Person = '';

{
  // 호이스팅이 발생하지 않는다면 ''가 출력되어야 한다.
  console.log(Person): // ReferencError

  class Person {}
}
```
클래스는 `let`, `const` 키워드로 선언한 변수처럼 호이스팅된다. 따라서 클래스는 일시적 사각지대에 빠져 호이스팅이 발생하지 앟는 것처럼 동작한다.

# 인스턴스 생성
클래스는 생성자 함수이며 `new` 연산자와 함께 호출되어 인스턴스를 생성한다.
```js
class Person {}

const me = new Person();

const you = Person(); // TypeError: new 연산자 없이 호출하면 에러 발생
```

표현식으로 정의된 클래스의 경우 식별자를 사용해 인스턴스를 생성해야 한다.
```js
const Person = class MyClass {};

const me = new Person();
const you = new MyClaSS(); // ReferenceError
```

# 메서드
## constructor
`constructor`는 생성자라는 이름으로 인스턴스가 생성될 때 무조건 실행되는 함수다. 즉, 인스턴스를 생성하고 초기화하기 위한 특수한 메서드다.
```js
class Person {
	// constructor는 이름을 변경할 수 없다.
  constructor(name, age) {
    this.name = name;
    this.age = age;
  }
}

```

생성자 함수와 마찬가지로 `constructor` 내부에서 `this`에 추가한 프로퍼티는 인스턴스 프로퍼티가 된다. `constructor` 내부의 `this`는 생성자 함수와 마찬가지로 클래스가 생성한 인스턴스를 가리킨다.

`constructor`는 생성자 함수와 유사하지만 몇 가지 차이가 있다.

1. `constructor`는 클래스 내에 최대 한 개만 존재할 수 있다.
2. `constructor`는 생략할 수 있다. `constructor`를 생략하면 클래스에 빈 `constructor`가 암묵적으로 정의 된다.

인스턴스를 생성할 때 클래스 외부에서 프로퍼티의 초기값을 전달하려면 constructor에 매개변수를 선언하고 인스턴스를 생성할 때 초기값을 전달한다.
```js
class Person {
  constructor(name, age) {
    this.name = name;
    this.age = age;
  }
}

const me = new Person("sanghyeon", 29);
```

`constructor`는 **별도의 반환문을 갖지 않아야 한다.** `new` 연산자와 클래스가 호출되면 암묵적으로 `this`, 즉 인스턴스를 반환하는데 다른 객체를 명시적 반환하면 인스턴스가 반환되지 못하고 `return` 문에 명시한 객체가 반환된다.

```js
class Person {
  constructor(name, age) {
    this.name = name;
    this.age = age;
    return {}; // this 반환이 무시된다.
  }
}

const me = new Person("sanghyeon", 29); // { }
```

## 프로토타입 메서드
클래스 몸체에서 정의한 메서드는 생성자 함수에 의한 방식과는 다르게 클래스의 `prototype` 프로퍼티에 메서드를 추가하지 않아도 기본적으로 프로토타입 메서드가 된다.
```js
class Person {  
  constructor(name, age) {  
    this.name = name;  
    this.age = age;  
  }  
  
  sayHello() {  
    console.log(`hello i'm ${this.name}`);  
  }  
  
  sayAge() {  
    console.log(`i'm ${this.age} old`);  
  }  
}

const person1 = new Person("sanghyeon", "29");
person1.sayHello(); // hello i'm sanghyeon
person1.sayAge(); // i'm 29 old
```

## 정적 메서드
정적 메서드는 인스턴스를 생성하지 않아도 호출할 수 있는 메서드를 말한다.
클래스에서는 메서드에 `static` 키워드를 붙이면 정적 메서드가 된다.
```js
class Calculator {  
  static add(a, b) {  
    return a + b;  
  }  
}  
  
let result = Calculator.add(3, 4);
```

## 정적 메서드 VS 프로토타입 메서드
정적 메서드와 프로토타입 메서드는 다음의 차이점이 있다.

1. 정적 메서드와 프로토타입 메서드는 자신이 속해 있는 프로토타입 체인이 다르다.
2. 정적 메서드는 클래스로 호출하고 프로토타입 메서드는 인스턴스로 호출한다.
3. 정적 메서드는 인스턴스 프로퍼티를 참조할 수 없지만 프로토타입 메서드는 인스턴스 프로퍼티를 참조할 수 있다.

정적 메서드는 애플리케이션 전역에서 사용할 유틸리티 함수를 전역 함수로 정의하지 않고 메서드로 구조화할 때 유용하다.


# 클래스의 인스턴스 생성 과정

1. 인스턴스 생성과 `this` 바인딩
    `new` 연산자와 함께 클래스를 호출하면 `constructor`의 내부 코드가 실행되기 전에 암묵적으로 빈 객체가 생성된다. 그리고 암묵적으로 생성된 빈 객체는 `this`에 바인딩 된다.
2. 인스턴스 초기화
    `constructor`의 내부 코드가 실행되어 `this`에 바인딩되어 있는 인스턴스를 초기화한다. 즉, `this`에 바인딩되어 있는 인스턴스에 프로퍼티를 추가하고 `constructor`가 인수로 전달받은 초기값으로 프로퍼티 값을 초기화한다. 만약 `constructor`가 생략되었다면 이 과정도 생략된다.
3. 인스턴스 반환
    클래스의 모든 처리가 끝나면 인스턴스가 바인딩된 `this`가 반환된다.

# 프로퍼티
## 인스턴스 프로퍼티
인스턴스 프로퍼티는 `constructor` 내부에서 정의해야 한다.
```js
class Person {
  constructor(name) {
    this.name = name; // name 프로퍼티는 public 하다.
  }
}
```

`constructor` 내부의 `this`에는 이미 인스턴스인 빈 객체가 바인딩되어 있다. ES6의 클래스는 `private`, `public`, `protected` 키워드와 같은 접근 제한자를 지원하지 않는다. 따라서 인스턴스 프로퍼티는 언제나 `public`하다.

## getter와 setter
클래스에서는 `getter`와 `setter`를 사용하여 클래스의 프로퍼티에 접근할 수 있다. `getter`는 프로퍼티 값을 반환하는 메소드이며, `setter`는 속성 값을 설정하는 메소드다. 

하나의 속성이 `getter`를 가지면 `setter`도 설정을 해줘야 한다.
```js
class Rectangle {  
  constructor(height, width) {  
    this.width = width;  
    this.height = height;  
  }  
  get width() {  
    return this.width;  
  }
  set width(value) {  
	this.width = value;  
  }  
}

let test = new Rectangle();  
test.width;
```

하지만 위 예제를 실행하면 `RangeError: Maximum call stack size exceeded` 에러가 발생할 것이다.
이는 `this.height`가 `set width(value)`를 호출하는데, 그 안에서 또 자기 자신을 호출하므로 무한루프에 빠지기 때문이다.
따라서 `getter`와 `setter`를 사용할 경우 프로퍼티 속성 앞에 `_`를 붙여줘야 한다.
```js
class Rectangle {  
  constructor(height, width) {  
    this._width = width;  
    this._height = height;  
  }  
  
  get width() {  
    return this._width;  
  }  
  
  set width(value) {  
    this._width = value;  
  }  
}
```

# 상속에 의한 클래스 확장
상속은 기존 클래스를 그대로 이어받아 새롭게 확장하여 정의하는 것이다. 상속에 의한 클래스 확장은 코드 재사용 관점에서 매우 유용하다.

```js
// 동물 전체에 대한 클래스  
class Animal {  
  constructor(name) {  
    this.name = name;  
  }  
  
  speak() {  
    console.log(`${this.name} says!`);  
  }  
}  
  
class Dog extends Animal {  
  // 부모에게서 내려받은 메서드를 재정의할 수 있음  
  // overriding...  speak() {  
    console.log(`${this.name} barks!`);  
  }  
}
```

## extends 키워드
상속을 통해 클래스를 확장하려면 `extends` 키워드를 사용하여 상속받을 클래스를 정의한다.

```js
// 수퍼 클래스
class Base {}

// 서브 클래스
class Dervied extends Base {}
```

상속을 통해 확장된 클래스를 서브 클래스(파생 클래스, 자식 클래스)라 부르고, 서브 클래스에 상속된 클래스를 수퍼 클래스(베이스 클래스, 부모 클래스)라 부른다. `extneds` 키워드의 역할은 수퍼 클래스와 서브 클래스 간의 상속 관계를 설정하는 것이다.

## super 키워드
`super` 키워드는 함수처럼 호출할 수도 있고, `this`와 같이 식별자처럼 참조할 수 있는 특수한 키워드다. `super`는 다음과 같이 동작한다.

- `super`를 호출하면 수퍼 클래스의 `constructor`를 호출한다.
- `super`를 참조하면 수퍼 클래스의 메서드를 호출할 수 있다.

서브 클래스에서 `constructor`를 생략하면 클래스에 다음과 같은 `constructor`가 암묵적으로 정의된다.
```js
constructor(...args) { super(...args); }
```

`super()`는 수퍼 클래스의 `constructor`를 호출하여 인스턴스를 생성한다.

수퍼 클래스와 서브 클래스 모두 `constructor`를 생략하면 빈 객체가 생성된다.

### super 호출
수퍼 클래스의 `constructor` 내부에서 추가한 프로퍼티를 그대로 갖는 인스턴스를 생성한다면 서브클래스의 `constructor`를 생략할 수 있다. `new` 연산자와 함께 서브 클래스를 호출하면서 전달한 인수는 모두 서브클래스에 암묵적으로 정의된 `constructor`의 `super` 호출을 통해 수퍼 클래스의 `constrcutor`로 전달된다.

서브클래스에 추가한 프로퍼티를 갖는 인스턴스를 생성한다면 서브 클래스의 `constructor`를 생략할 수 없다. 이때 수퍼 클래스의 `constructor`에 전달할 필요가 있는 인수는 `super`를 통해 전달한다.

```js
class Base {
  constructor(a, b){
    this.a = a;
    this.b = b;
  }
}

class Derived extends Base {
  constructor(a, b, c) {
    super(a, b);
    this.c = c;
  }
}
const derived = new Derived(1, 2, 3);
```

`super`를 호출할 때 주의할 사항은 다음과 같다.

1. 서브 클래스에서 `constructor`를 생략하지 않는 경우 서브클래스의 `constructor`에서는 반드시 `super`를 호출해야 한다.
    ```js
    class Base {}
    
    class Derived extends Base {
      constructor() {
        // ReferenceError: Must call super constructor in derived class before accessing 'this' or returning from derived constructor
      }
    }
    
    const derived = new Derived();
    ```
    
2. 서브 클래스의 `constructor`에서 `super` 를 호출하기 전에는 `this` 를 참조할 수 없다.
    ```js
    class Base {}
    
    class Derived extends Base {
      constructor() {
        // ReferenceError: Must call super constructor in derived class before accessing 'this' or returning from derived constructor
        this.a = 1;
        super();
      }
    }
    
    const derived = new Derived();
    ```
    
3. `super`는 반드시 서브클래스의 `constructor` 에서만 호출한다.
### super 참조

메서드 내에서 `super`를 참조하면 수퍼 클래스의 메서드를 호출할 수 있다.

1. 서브 클래스의 프로토타입 메서드 내에서 `super.sayHi`는 수퍼 클래스의 프로토타입 메서드 `sayHi`를 가리킨다.
    ```js
    class Base {
      constructor(name) {
        this.name = name;
      }
      sayHi() {
        return `hi ${this.name}`;
      }
    }
    
    class Derived extends Base {
      sayHi() {
        return `${super.sayHi()}. nice to meet you`;
      }
    }
    
    const derived = new Derived('Lee'); // hi Lee. nice to meet you
    console.log(derived.sayHi());
    ```

## 상속 클래스의 인스턴스 생성 과정
```js
class Rectangle {
  constructor(width, height) {
    this.width = width;
    this.height = height;
  }

  getArea() {
    return this.width * this.height;
  }

  toString() {
    return `width = ${this.width}, height = ${this.height}`;
  }
}

class ColorRectangle extends Rectangle {
  constructor(width, height, color) {
    super(width, height); // 1
    this.color = color;
  }

  // 메서드 오버라이딩
  toString() {
    return super.toString() + `, color = ${this.color}`;
  }
}

const colorRectangle = new ColorRectangle(2, 4, 'red');
console.log(colorRectangle.getArea()); // 상속을 통해 getArea 메서드 호출
console.log(colorRectangle.toString()); // 오버라이딩된 toString 메서드 호출
```

## 서브 클래스의 super 호출

서브 클래스는 자신이 직접 인스턴스를 생성하지 않고 수퍼 클래스에게 인스턴스 생성을 위임한다. 이것이 서브클래스의 `constructor`에서 반드시 `super`를 호출해야 하는 이유다.

## 수퍼 클래스의 생성과 this 바인딩

수퍼 클래스의 `constructor` 내부의 코드가 실행되기 이전에 암묵적으로 빈 객체를 생성하고 이 빈 객체가 인스턴스다. 이 인스턴스는 `this`에 바인딩 된다.

## 수퍼 클래스의 인스턴스 초기화
`this`에 바인딩 되어있는 인스턴스를 초기화한다.

## 서브 클래스 constructor로의 복귀와 this 바인딩

서브 클래스는 별도의 인스턴스를 생성하지 않고 `super`가 반환한 인스턴스를 `this`에 바인딩하여 그대로 사용한다. 이처럼 `super`가 호출되지 않으면 인스턴스가 생성되지 않으며 `this` 바인딩도 할 수 없다. 서브 클래스의 `constructor`에서 `super`를 호출하기 전에 `this`를 참조할 수 없는 이유이다.

## 서브 클래스의 인스턴스 초기화
서브 클래스의 인스턴스를 초기화 한다.

## 인스턴스 반환
클래스의 모든 처리가 끝나면 완성된 인스턴스가 바인딩된 `this`가 암묵적으로 반환된다.


---
> **참조**
> - 모던자바스크립트 DeepDive
> - 내일배움캠프 JS 문법 종합반 강의자료