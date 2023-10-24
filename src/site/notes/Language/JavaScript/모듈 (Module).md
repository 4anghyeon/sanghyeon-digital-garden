---
{"tags":["JavaScript"],"dg-publish":true,"dg-hide":true,"permalink":"/language/java-script/module/","hide":true,"dgPassFrontmatter":true,"noteIcon":""}
---


# 모듈 (Module) 이란
모듈이란 재사용 가능한 코드 조각을 말하고, 일반적으로 기능을 기준으로 파일 단위로 분리한다.

자신만의 파일 스코프를 갖는 모듈의 변수, 함수, 객체등은 기본적으로 비공개 상태다. 즉, 모듈은 개별적인 존재로 분리되어 존재한다.

하지만 모듈은 재사용이 가능해야 의미가 있다. 따라서 모듈은 공개가 필요한 자산에 한정하여 명시적으로 선택적 공개가 가능하다. 이를 `export`라 한다.

사용자는 모듈이 `export`한 일부 또는 전체를 선택해 자신의 스코프 내로 불러들여 재사용할 수 있다. 이를 `import`라 한다.

**<span style='color:#eb3b5a'>
이처럼 모듈은 분리되어 개별적으로 존재하다 필요에 따라 다른 모듈에 의해 재사용된다. 모듈은 기능별로 분리되어 개별적인 파일로 작성된다. 따라서 코드의 단위를 명확히 분리하여 구성할 수있고, 재사용성이 좋아 개발 효율성과 유지보수성을 높일 수 있다.</span>**

# 자바스크립트와 모듈
초기 자바스크립트는 모듈 기능을 지원하지 않았고, 자바스크립트 파일을 여러 개의 파일로 분리하여 작성하여도 결국 하나의 파일처럼 동작했다.

자바스크립트를 브라우저 환경에 국한하지 않고 범용적으로 사용하려는 움직임이 생기면서 모듈 시스템은 반드시 해결해야할 과제가 되었고, 이런 상황에서 제안된 것이 CommonJS와 AMD다.

# CommonJS
CommonJS는 자바스크립트를 브라우저 밖에서도 쓸 수 있도록 하기위해 조직한 자발적 그룹이다.

- 서버 사이드 (Node.js 등)에서 주로 사용된다.
- `require` 문법을 사용한다.
- 동기적으로 작동한다. 따라서 브라우저에서 사용하기 부적합하다.

**내보낼 때**
```js
module.exports.foo = function () {
	// ...
};
```

**불러올 때**
```js
var foo = require('./foo.js').foo;
```

# AMD (Asynchronous Module Definition)
AMD는 비동기 상황에서도 JavaScript 모듈을 쓰기 위해 CommonJS에서 함께 논의하다 합의점을 이루지 못하고 독립한 그룹이다.

- `define`, `require` 문법을 사용한다.
- 비동기적으로 작동한다. 따라서 클라이언트 사이드에서 주로 사용된다.

**내보낼 때**
```js
const foo = () => {
	// ...
}

export default { foo };
```

**불러올 때**
```js
import foo from "foo";
```

# ESM(ECMAScript Module)
ES6에서 클라이언트 사이드 자바스크립트에서도 동작하는 모듈 기능을 추가했다.

사용법은 `script` 태그 안에 `type="module"` 속성을 추가하면 자바스크립트 파일이 모듈로서 동작한다.

확장자는 일반적인 자바스크립트 파일이 아닌 ESM임을 명확히 하기 위해 `.mjs` 를 사용할 것을 권장한다.

## 모듈 스코프
ESM은 파일 자체의 독자적인 모듈 스코프를 제공한다. 따라서 모듈 내의 `var` 키워드로 선언한 변수는 더는 전역 변수가 아니며 `window` 객체의 프로퍼티도 아니다.

<span style='color:#eb3b5a'>스코프가 다르기에 모듈 내에서 선언한 식별자는 모듈 외부에서 참조할 수 없다.</span>

## 내보내기
모듈 내부에서 선언한 식별자를 외부에 공개하여 다른 모듈이 재사용할 수 있게 하려면 `export` 키워드를 사용한다.

`export` 키워드는 선언문 앞에 사용한다. 변수, 함수, 클래스 등 모든 식별자를 `export`할 수 있다.

```js
export const pi = Math.PI;

export function square(x) {
	return x * x;
}

export class Person {
	constructor(name) {
		this.name = name;
	}
}
```

export할 대상을 하나의 객체로 구성하여 한 번에 export할 수도 있다.
```js
export { pi, square, Person };
```

모듈에서 하나의 값만 `export`한다면 `default` 키워드를 사용할 수 있다.
`default` 키워드를 사용한 경우 `var`, `let`, `const` 키워드는 사용할 수 없다.
```js
export default x => x * x;
```

## 불러오기
다른 모듈에서 공개한 식별자를 자신의 모듈 스코프 내부로 로드하려면 `import` 키워드를 사용한다. <span style='color:#eb3b5a'>ESM의 경우 파일 확장자를 생략할 수 없다.</span>

```js
import { pi, square, Person} from './lib.mjs';
```

> **AMD방식과 헷갈리지 말것!**
> AMD: `import from "foo"`
> ESM: `import from "foo.js"`


모듈이 `export`한 식별자 이름을 일일이 지정하지 않고 하나의 이름으로 import할 수도 있다. as 뒤에 지정한 객체에 프로퍼티로 할당된다.

```js
import * as lib from './lib.mjs';

console.log(lib.pi);
console.log(lib.square(10));
console.log(new lib.Person('Lee'));
```

모듈이 `export`한 식별자 이름을 변경하여 `import`할 수도 있다.
```js
import {pi as PI, square as sq, Person as P} from './lib.mjs';

console.log(PI);
console.log(sq(100));
console.log(new P('Lee'));
```

`default` 키워드와 함께 `export`한 모듈은 {} 없이 임의의 이름으로 `import`한다.

---
> **참조**
> - 모던자바스크립트 DeepDive