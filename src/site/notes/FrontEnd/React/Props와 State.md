---
{"tags":["React"],"dg-publish":true,"dg-hide":true,"permalink":"/front-end/react/props-state/","hide":true,"dgPassFrontmatter":true,"noteIcon":""}
---

# Props
리액트의 기본적인 데이터 전달 방식은 위에서 아래로, 상위 컴포넌트에서 하위 컴포넌트로 전달하는 **하향식 단방향 흐름 원칙**을 가진다.

부모 컴포넌트는 자식 컴포넌트에게 attribute를 지정하여 값을 객체 형태로 전달한다.  이때 전달되는 객체가 바로 `props`다.
```jsx
// src/App.js

import React from "react";

function Parent() {
	const name = '홍부인';
  return <Child parentName={name} />; // 💡"props로 name을 전달했다."
}

function Child(props) {
  console.log(props.parentName); // 이것이 바로 props
  return <div>연결 성공</div>;
}

export default Parent;
```

`props`는 읽기 전용이며 부모에서 자식으로 전달할 때만 사용한다.
`props`는 읽기 전용이기 때문에 `props`의 값을 변경하는 일이 생겨서는 안된다!

## Props Drilling
다음과 같은 구조의 코드를 작성한다고 해보자.
```jsx
export default function App() {
  return (
    <div className="App">
      <FirstComponent content="Who needs me?" />
    </div>
  );
}

function FirstComponent({ content }) {
  return (
    <div>
      <h3>I am the first component</h3>;
      <SecondComponent content={content} />|
    </div>
  );
}

function SecondComponent({ content }) {
  return (
    <div>
      <h3>I am the second component</h3>;
      <ThirdComponent content={content} />
    </div>
  );
}

function ThirdComponent({ content }) {
  return (
    <div>
      <h3>I am the third component</h3>;
      <ComponentNeedingProps content={content} />
    </div>
  );
}

function ComponentNeedingProps({ content }) {
  return <h3>{content}</h3>;
}
```


![Pasted image 20231031210127.png|400](/img/user/FrontEnd/React/Pasted%20image%2020231031210127.png)

리액트는 부모에서 자식으로, 하향식으로만 데이터를 전달할 수 있기 때문에 실제로 `props`가 필요한 `ComponentNeedingProps` 컴포넌트로 데이터를 전달하기 위해 불필요한 컴포넌트들을 지나쳐서 와야 한다.

이렇게 `props`가 아래로 드릴처럼 뚫고 내려온다고 해서 `props drilling`이라 한다.
이러한 중첩이 많아지게 되면 다음과 같은 문제점이 발생한다.
- 코드의 가독성이 안좋아진다.
- 코드의 유지보수가 힘들어진다.
- `state` 변경시 무관한 컴포넌트도 리렌더링 된다.

이를 해결하기 위해 Context, Redux 등 다양한 상태 관리 기법을 활용한다.


# State
`state`는 컴포넌트 내부에서 바뀔 수 있는 값을 의미한다. 함수형 컴포넌트에서 사용하기 위해서는 `useState`라는 Hook을 사용한다.

컴포넌트의 어떠한 정보가 변하는 것을 감지하기 위해서는 현재 상태를 기억하고 있어야 한다. 리액트에서는 이러한 컴포넌트가 가지고 있는 특정한 메모리를 `state`라고 한다.

```jsx
import React, { useState } from 'react';

function Component() {
  const [name, setName] = useState("이상현"); // 이것이 state!
  return <Child name={name} />;
}

// .. 중략 
```

`useState` Hook의 반환값은 배열로 첫 번째 요소는 현재 상태값, 두 번째 요소는 상태를 업데이트 함수를 반환한다. 두 반환값을 자바스크립트의 구조 분해 할당을 이용하여 가져올 수 있다.
```js
const [name, setName] = useState("이상현");
```

## 컴포넌트 리렌더링
```jsx
function ExpenseItem(props) {
  let {date, title, amount} = props.data;

  const clickHandler = () => {
    title = 'updated'; // nothing change
  };

  return (
    <Card className="expense-item">
			<div className="expense-item__description">
        <h2>{title}</h2>
      </div>
      <button onClick={clickHandler}>Change Title</button>
    </Card>
  );
}
```
위의 코드에서 `title` 변수를 아무리 바꿔도 화면에서는 바뀌지 않는다. 리액트에서는 상태에 변화가 없다면 더 이상 렌더링 하지 않는다. 로컬 변수를 바꾸는 것은 리액트에게 상태 변화를 알리지 못한다.

`useState` Hook을 사용하면 리액트에게 상태의 변화를 알릴 수 있다.
```jsx
function ExpenseItem(props) {
  let {date, amount} = props.data;

  const [title, setTitle] = useState(props.data.title);

  const clickHandler = () => {
    setTitle('updated'); // Title아 변해라!!!
  };

  return (
    <Card className="expense-item">
      <ExpenseDate date={date} />
      <div className="expense-item__description">
        <h2>{title}</h2>
      </div>
      <button onClick={clickHandler}>Change Title</button>
    </Card>
  );
}
```

위에서 봤듯이 리액트에게 상태의 변화를 알리기 위해서는 `set~` 함수를 이용하여 상태를 업데이트 해야한다. `set~` 함수는 단순히 변수에 새로운 값을 할당하는게 아니다.

`set~` 을 호출하면 리액트에게 해당 `useState`를 호출한 **독립적인** 컴포넌트에게 변화가 필요하다고 알리고, 다시 그리게 한다.

위에서 말한 **독립적인** 컴포넌트라는 것은 같은 컴포넌트를 여러 개 생성하면 각 컴포넌트는 독립적으로 각각의 `state`가 생기게된다. 그 중 하나의 컴포넌트의 `state`가 변화되면 모든 컴포넌트가 변하는 것이 아닌 **그 `state`를 소유한 인스턴스 컴포넌트만 변화를 감지하게 된다.**


---
> **참조**
> - https://react.dev/learn/sharing-state-between-components
> - 내일배움캠프 리액트 입문 학습 자료