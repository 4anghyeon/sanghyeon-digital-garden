---
{"tags":["React"],"dg-publish":true,"dg-hide":true,"permalink":"/front-end/react/state-lifting/","hide":true,"dgPassFrontmatter":true,"noteIcon":""}
---

State Lifting은 말 그대로 state를 위로 끌어올리는 것이다. 리액트에서 데이터 흐름은 부모에서 자식, 상위 컴포넌트에서 하위 컴포넌트로 props를 전달하는 하향식 단방향 흐름 원칙을 가진다.
![Pasted image 20231106113914.png](/img/user/FrontEnd/React/Pasted%20image%2020231106113914.png)

그렇다면 만약 `Child Component 1`에서 변경된 데이터가 `Child Component 2`에 반영되어야 한다면 어떻게 해야할까?

`Child Component 1`과 `Child Component 2`가 직접적으로 연결되어있지 않기 때문에 가장 가까운 공통 부모 컴포넌트를 찾아서 데이터를 전달해야 한다.

<span style='color:#3867d6'>그러나 데이터는 상위에서 하위 컴포넌트로 전달한다고 했는데 하위 컴포넌트에서 어떻게 상위 컴포넌트로 전달할까?</span>

## Handler 전달
다른 상태 관리 라이브러리를 사용하지 않고 하위에서 상위 컴포넌트로 데이터를 전달할 수 있는 방법은 다음과 같다.

부모 컴포넌트에서 데이터를 전달 받는 함수를 만든 뒤 그 함수의 포인터를 하위 컴포넌트의 props로 전달한다. 그리고 하위 컴포넌트에서 전달 받은 함수를 실행하면, 마치 부모 컴포넌트에서 실행한 것과 같은 효과를 가진다.

**부모 컴포넌트**
```jsx
const NewExpense = (props) => {
  const saveExpenseDataHandler = (enteredExpenseData) => {
    const expenseData = {
      ...enteredExpenseData,
      id: Math.random().toString(),
    };

    console.log('부모 컴포넌트에서 실행')
    console.log(expenseData);

    props.onAddExpense(expenseData);
  };

  return (
    <div className="new-expense">
      <ExpenseForm onSaveExpenseData={saveExpenseDataHandler} />
    </div>
  );
};
```

**자식 컴포넌트**
```jsx
const ExpenseForm = (props) => {
	const submitHandler = (ev) => {
    ev.preventDefault();
    setUserInput({
      enteredTitle: '',
      enteredAmount: 0.01,
      enteredDate: '2023-01-01',
    });
    props.onSaveExpenseData(userInput);
  };
	//...
	return <form onSubmit={submitHandler}>
		//...
	</form>
}

```

![state lifting console.png](/img/user/FrontEnd/React/state%20lifting%20console.png)

> 참조
> [Sharing State Between Components – React](https://react.dev/learn/sharing-state-between-components)