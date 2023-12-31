---
{"tags":["JavaScript"],"dg-hide":true,"dg-publish":true,"permalink":"/language/java-script/lexcial-environment/","hide":true,"dgPassFrontmatter":true,"noteIcon":""}
---

렉시컬 환경(Lexcial Environment)은 식별자와 식별자에 바인딩된 값, 그리고 상위 스코프에 대한 참조를 기록하는 자료구조로 [[Language/JavaScript/실행 컨텍스트\|실행 컨텍스트]]를 구성하는 컴포넌트다.

실행 컨텍스트에서, 컨텍스트 내의 변수등을 참조하였는데 이것을 관리해주는 것이 렉시컬 환경이다. **렉시컬 환경은 스코프와 식별자를 관리한다**. 렉시컬 환경은 키와 값을 갖는 객체 형태의 스코프를 생성하여 식별자를 관리한다.
<mark style='background:#f7b731'>즉, 렉시컬 환경은 스코프를 구분하여 식별자를 등록하고 관리하는 저장소 역할을 하는 렉시컬 스코프의 실체다.</mark>

실행 컨텍스트는 `LexicalEnvironment` 컴포넌트와 `VariableEnvironment` 컴포넌트로 구성된다. 생성 초기에는 두 컴포넌트는 하나의 동일한 렉시컬 환경을 참조한다. 둘의 차이점은 VE는 스냅샷을 유지하고, LE는 스냅샷을 유지 하지않고 실시간으로 변경사항을 반영 한다는 점에서 다르다.

**결국, 실행 컨텍스트를 생성할 때, VE에 정보를 먼저 담은 다음, 이를 그대로 복사해서 LE를 만들고 이후에는 주로 LE를 활용한다.**

`LexicalEnvironment`와 `VariableEnvironment` 구성요소는 같으며, 다음과 같은 프로퍼티를 포함한다.
1. 환경 레코드 (Environment Record(=record))
	- 스코프에 포함된 식별자를 등록하고 식별자에 바인딩된 값을 관리하는 저장소다. 소스코드의 타입에 따라 관리하는 내용에 차이가 있다.

2. 외부 렉시컬 환경 (Outer Environment Reference)
	- 상위 스코프를 가리킨다. 이때 상위 스코프란 외부 렉시컬 환경, 즉 해당 실행 컨텍스트를 생성한 소스코드를 포함하는 상위 코드의 렉시컬 환경을 말한다. 외부 렉시컬 환경에 대한 참조를 통해 단방향 [[Computer Science/Data Structure/연결 리스트 (Linked List)\|Linked List]] 스코프 체인을 구현한다.

# 실행 컨텍스트의 생성과 식별자 검색 과정
예제 코드와 [[Language/JavaScript/실행 컨텍스트\|실행 컨텍스트]]가 생성될 때 어떤식으로 렉시컬 환경이 만들어지는지 살펴보자.

```js
var x = 1;
const y = 2;

function foo (a) {
	var x = 3;
	const y = 4;
	
	function bar(b) {
		const z = 5;
		console.log(a + b+ + x + y + z);
	}
	bar(10);
}

foo(20); // 42
```

## 전역 객체 생성
전역 객체는 전역 코드가 평가되기 이전에 생성된다. 전역 객체에는 빌트인 전역 프로퍼티와 전역 함수, 표준 빌트인 객체가 추가되며 동작 환경에 따라 클라이언트 사이드 Web API(DOM, Canvas, XMLHttpRequest, fetch 등..) 또는 특정 환경을 위한 호스트 객체를 포함한다.

## 전역 코드 평가
### 전역 실행 컨텍스트 생성
먼저 전역 실행 컨텍스트를 생성하고, 실행 컨텍스트 스택에 푸시한다.

### 전역 렉시컬 환경 생성
전역 LE를 생성하고 전역 실행 컨텍스트에 바인딩한다. 
![Pasted image 20231016112128.png](/img/user/Language/JavaScript/Pasted%20image%2020231016112128.png)

#### 전역 환경 레코드 생성
전역 환경 레코드에는 전역 변수를 관리하는 전역 스코프, 전역 객체의 빌트인 전역 프로퍼티와 빌트인 전역 함수, 표준 빌트인 객체를 제공한다.

ES6 이전에는 모든 전역 변수가 전역 객체의 프로퍼티가 됐기 때문에 전역 객체가 전역 환경 레코드의 역할을 수행했다. 하지만 `let` 과 `const` 의 등장 후 두 키워드로 선언한 전역 변수는 전역 객체의 프로퍼티가 되지 않고 개념적인 블록 내에 존재하게 된다.

이처럼 기존의 `var` 키워드로 선언한 전역 변수와 `let` , `const` 키워드로 선언한 전역 변수를 구분하여 관리하기 위해 **전역 환경 레코드는 객체 환경 레코드(Object Environment Record)와 선언적 환경 레코드(Declarative Environment Record)로 구성되어 있다.**

1. 객체 환경 레코드 생성
전역 환경 레코드를 구성하는 컴포넌트인 객체 환경 레코드는 `BindingObject` 라고 부르는 객체와 연결된다. 이 `BindingObject` 가 [[Language/JavaScript/렉시컬 환경 (Lexcial Environment)#전역 객체 생성\|전역 객체 생성]]에서 생성된 전역 객체다.

전역 코드 평가 과정에서 `var` 키워드로 선언한 전역 변수와 함수 선언문으로 정의된 전역 함수가 `BindingObject` 를 통해 전역 객체의 프로퍼티와 메서드가 된다.

그리고 이때 등록된 식별자를 환경 레코드에서 검색하면 전역 객체의 프로퍼티를 검색해 반환한다.

바로 이것이 `var` 키워드로 선언된 전역 변수와 함수 선언문으로 정의된 전역 함수가 전역 객체를 가리키는 식별자(`window`) 없이 전역 객체의 프로퍼티를 참조할 수 있는 메커니즘이다. (`window.alert` → `alert`)

![Pasted image 20231016112742.png](/img/user/Language/JavaScript/Pasted%20image%2020231016112742.png)

`var` 키워드로 선언한 변수는 코드 평가 단계에서 초기화까지 진행됐기 때문에 실행 단계에서 변수 선언문 이전에도 참조할 수 있다. <mark style='background:#f7b731'>이것이 변수 호이스팅이 발생하는 원인이다.</mark>

함수 선언문으로 정의한 함수가 평가되면 함수 이름을 키로 `BindingObject` 를 통해 함수 객체를 즉시 할당한다. 이것이 변수 호이스팅과 함수 호이스팅의 차이다. 함수 선언문으로 정의한 함수는 런타임 환경에서 함수 선언문에 도달하기 전에 호출할 수 있다.

2. 선언적 환경 레코드 생성
`let`, `const` 키워드로 선언한 전역변수는 선언적 환경 레코드(Declarative Environment Record)에 등록되고 관리된다.
따라서 변수 `y` 는 `const` 키워드로 선언한 변수이므로 전역 객체의 프로퍼티가 되지 않기 때문에 window.y 로 참조할 수 없다. 또한 `const` 키워드로 선언한 변수는 “선언 단계" 와 “초기화 단계" 가 분리되어 진행한다. 따라서 실행 흐름이 변수 선언문에 도달하기 전까지 일시적 사각지대에 빠지게 된다.

![Pasted image 20231016113204.png](/img/user/Language/JavaScript/Pasted%20image%2020231016113204.png)


#### this 바인딩
`GlobalEnvironmentRecord` 의 GlobalThisValue 내부 슬롯에 `this`가 바인딩된다. 일반적으로 전역 코드에서 this 는 전역 객체를 가리키므로 GlobalThisValue 내부 슬롯에는 전역 객체가 바인딩 된다.
![Pasted image 20231016114516.png](/img/user/Language/JavaScript/Pasted%20image%2020231016114516.png)

#### 외부 렉시컬 환경에 대한 참조 결정
`OuterLexicalEnvironmentReference` 는 현재 평가 중인 소스코드(지금은 전역 코드)를 포함하는 외부 소스코드의 렉시컬 환경, 즉 상위 스코프를 가리킨다. 이를 통해 단방향 Linked List인 스코프 체인을 구현한다.

전역 코드를 포함하는 소스코드는 없으므로 전역 렉시컬 환경의 `OuterLexicalEnvironmentReference` 에 `null` 이 할당된다.
![Pasted image 20231016114625.png](/img/user/Language/JavaScript/Pasted%20image%2020231016114625.png)


## 전역 코드 실행
전역 코드 평가가 끝나고, 이제 코드가 순차적으로 실행된다. 전역 변수 `x`, `y`가 할당되고 `foo`가 호출된다.

변수 할당문 또는 함수 호출문을 실행하려면 변수 또는 함수가 선언된 식별자인지 확인해야 한다. 식별자는 스코프가 다르면 같은 이름을 가질 수 있다. <mark style='background:#f7b731'>즉, 동일한 이름의 식별자가 다른 스코프에 여러 개 존재할 수도 있고, 이를 식별할 필요가 있다.</mark> 이를 **식별자 결정**이라 한다.

**식별자 결정을 위해 식별자를 검색할 때는 실행 중인 실행 컨텍스트에서 식별자를 검색하기 시작한다.** 선언된 식별자는 실행 컨텍스트의 렉시컬 환경의 환경 레코드에 등록되어 있다.

현재 실행 중인 실행 컨텍스트는 전역 실행 컨텍스트이므로 전역 렉시컬 환경에서부터 식별자를 검색한다. 만약 실행 중인 실행 컨텍스트의 렉시컬 환경에서 식별자를 못찾으면 `OuterLexicalEnvironmentReference` 가 가리키는 렉시컬 환경, 상위 스코프로 이동하여 식별자를 검색한다.

<mark style='background:#f7b731'>이것이 바로 스코프 체인의 동작 원리다.</mark>

전역 렉시컬 환경은 스코프 체인의 종점이고, 전역 렉시컬 환경에서 검색할 수 없는 식별자는 Reference Error를 발생시킨다.

## foo 함수 코드 평가
`foo` 함수가 호출되면 전역 코드의 실행을 일시 중단하고 `foo` 함수 내부로 코드의 제어권이 이동한다. 그리고 `foo` 함수 코드를 평가하기 시작한다. 다음과 같은 순서로 진행된다.

### foo 함수 실행 컨텍스트 생성
먼저 `foo` 함수 실행 컨텍스트를 생성한다. 생성된 함수 실행 컨텍스트는 함수 렉시컬 환경이 완성된 다음 실행 컨텍스트 스택에 푸시된다.

![Pasted image 20231016114713.png](/img/user/Language/JavaScript/Pasted%20image%2020231016114713.png)


### foo 함수 렉시컬 환경 생성
`foo` 함수 렉시컬 환경을 생성하고 `foo` 함수 실행 컨텍스트에 바인딩한다.

![Pasted image 20231016114827.png](/img/user/Language/JavaScript/Pasted%20image%2020231016114827.png)

#### 함수 환경 레코드 생성
함수 환경 레코드는 매개변수, `arguments` 객체, 함수 내부에서 선언한 지역 변수와 중첩 함수(예제에선 `bar`)를 등록하고 관리한다.
![Pasted image 20231016115146.png](/img/user/Language/JavaScript/Pasted%20image%2020231016115146.png)

#### this 바인딩
`foo` 함수 환경 레코드의 내부 슬롯에 `this`가 바인딩 된다. `foo` 함수는 일반 함수로 호출되었으므로 `this`는 전역 객체를 가리킨다.
![Pasted image 20231016115315.png](/img/user/Language/JavaScript/Pasted%20image%2020231016115315.png)

#### 외부 렉시컬 환경에 대한 참조 결정
외부 렉시컬 환경에 대한 참조에 `foo` 함수 정의가 평가된 시점에 실행 중인 실행 컨텍스트(예제에선 전역 실행 컨텍스트)의 렉시컬 환경의 참조가 할당된다.

![Pasted image 20231016115458.png](/img/user/Language/JavaScript/Pasted%20image%2020231016115458.png)

<mark style='background:#f7b731'>자바스크립트는 렉시컬 스코프(정적 스코프)를 따르기 때문에 함수를 어디서 호출했는지가 아니라 어디에 정의했는지에 따라 상위 스코프를 결정한다.</mark>

[[Language/JavaScript/클로저 (Closure)#렉시컬 스코프\|렉시컬 스코프]]는 클로저를 이해할 수 있는 중요한 단서다.

## foo 함수 코드 실행
`foo` 함수 코드 평가가 끝나고 소스코드가 순차적으로 실행되기 시작한다. 매개변수 `a` 에 인수가 할당되고, 지역 변수에 `x`, `y`에 값이 할당된다. 그리고 `bar` 함수가 호출된다.

이때 **식별자 결정을 위해 실행 중인 실행 컨텍스트(foo 실행 컨텍스트)의 렉시컬 환경에서 식별자를 검색하기 시작한다.**
현재 실행중인 실행 컨텍스트의 렉시컬 환경에서 식별자를 검색할 수 없으면`OuterLexicalEnvironmentReference` 가 가리키는 렉시컬 환경, 상위 스코프로 이동하여 식별자를 검색한다.

![Pasted image 20231016115705.png](/img/user/Language/JavaScript/Pasted%20image%2020231016115705.png)

## bar 함수 코드 평가
`bar` 함수가 호출되면 `bar` 함수 내부로 코드의 제어권이 이동한다. 그리고 `bar` 함수 코드를 평가하기 시작한다.

과정은 `foo` 함수 코드 평가와 같다.
![Pasted image 20231016120129.png](/img/user/Language/JavaScript/Pasted%20image%2020231016120129.png)

## bar 함수 코드 실행
`bar` 함수의 소스코드가 실행되며 매개변수와 지역변수에 값이 할당된다.
그리고 `console.log(a + b + x + y + z);` 가 실행된다.

### console 식별자 검색
먼저 `console` 식별자를 스코프 체인에서 검색한다. 현재 실행중인 실행 컨텍스트(`bar` 함수 실행 컨텍스트)의 렉시컬 환경에 `console` 이 없으므로 상위 스코프로 타고 타고 올라가서 전역 렉시컬 환경에서 `console` 식별자를 검색한다. `console` 식별자는 객체 환경 레코드의 BindingObject 를 통해 전역 객체에서 찾을 수 있다.

### log 메서드 검색
`console` 객체에서 `log` 메서드를 검색한다. 이때는 `console` 객체의 프로토타입 체인을 통해 메서드를 검색한다.

### 표현식 a + b + x + y + z의 평가
각 식별자를 스코프체인을 통해 `bar` 렉시컬 환경에서부터 foo 렉시컬 환경까지 검색하여 참조한다.

## bar 함수 코드 실행 종료
`console.log` 메서드가 호출되고 종료하면 `bar` 함수에서 더 이상 실행할 코드가 없으므로 `bar` 함수의 실행이 종료된다. 이때 실행 컨텍스트에서 `bar` 함수 실행 컨텍스트가 제거되고, `foo` 실행 컨텍스트가 다시 실행 중인 실행 컨텍스트가 된다.

![Pasted image 20231016120530.png](/img/user/Language/JavaScript/Pasted%20image%2020231016120530.png)

실행 컨텍스트 스택에서 `bar` 함수 실행 컨텍스트가 제거되었다고 해서 `bar` <mark style='background:#f7b731'>함수 렉시컬 환경까지 즉시 소멸하는 것은 아니다. 렉시컬 환경은 독립적인 객체이고 객체를 포함한 모든 값은 누군가에 의해 참조되지 않을 때 가비지 컬렉터에 의해 메모리가 해제된다.</mark>

## 나머지 코드 실행 종료
`foo`함수와 전역 함수 순서대로 실행이 종료되고, 실행 컨텍스트 스택에서 제거된다.


---
> **참조**
> - 모던자바스크립트 DeepDive
> - 내일배움캠프 JS 문법 종합반 강의자료