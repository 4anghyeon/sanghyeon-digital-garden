---
{"tags":["React"],"dg-publish":true,"dg-hide":true,"permalink":"/front-end/react/redux/","hide":true,"dgPassFrontmatter":true,"noteIcon":""}
---


# Redux
Redux란, 자바스크립트 애플리케이션의 전역 상태를 효율적으로 관리 할 수 있게 도와주는 라이브러리이다.
복잡한 상태 관리가 이루어지는 SPA에서 특히 유용하게 사용된다. Redux는 리액트가 뿐만 아니라 순수 Javascript에서도 사용 할 수 있다.

## 리액트 상태 관리에 대한 설명
예를 들어 다음과 같은 구조의 컴포넌트들이 있다고 보자.
![Pasted image 20231109094219.png](/img/user/Pasted%20image%2020231109094219.png)

RootComponent에서 F까지 값을 전달해주기 위해서는 `ROOT → B → E → F` 와 같은 경로를 따라야한다.

리덕스를 쓰게 된다면 [[FrontEnd/React/Props와 State#Props Drilling\|Props Drilling]] 없이 `state`의 변화를 리덕스 스토어를 이용하여 감지 하게 된다. 즉, 전역 `state`를 리덕스 스토어를 통해 관리할 수 있게 된다.

![Pasted image 20231109094528.png](/img/user/Pasted%20image%2020231109094528.png)
`ROOT → 리덕스 스토어 → F`
즉, 리덕스는 하나의 전역 상태를 관리하는 중앙화 저장소와, 특정한 패턴을 사용하여 전역 state를 쉽게 관리할 수 있도록 도와주는 상태 관리 라이브러리이다.

리덕스를 사용한 data의 flow를 그림으로 나타내면 다음과 같다.
![https://redux.js.org/assets/images/ReduxDataFlowDiagram-49fa8c3968371d9ef6f2a1486bd40a26.gif](https://redux.js.org/assets/images/ReduxDataFlowDiagram-49fa8c3968371d9ef6f2a1486bd40a26.gif)


# 리덕스 사용법
리덕스를 사용하기 위해 우선 다음과 같이 프로젝트 구조를 만든다.
> 📂 src
> 	📂 redux
> 		📂 config
> 			📄 `configStore.js`
> 		📂 modules
> 			📄 `counter.js`
> 			📄 `users.js`
> 			...

- `configStore.js`: `root reducer`와 `store`를 만드는 리덕스 설정 파일이다.
- `modules 하위 파일`: 각 기능별 `reducer`, `action`, `action creator` 를 만든다.

## Store
Redux의 state를 저장하는 단 하나의 공간을 store라고 한다. store는 ~~`createStore`~~, `configureStore` 함수에 생성한 root reducer를 인자로 넘겨 생성한다.

```js
import { configureStore } from '@reduxjs/toolkit'
// import { createStore } from "redux";

export default const store = configureStore({ reducer: counterReducer })

// const legacyStore = createStore(counterReducer);
```

생성한 store를 적용하기 위해서는 root 컴포넌트에서 `Provider`로 감싸주고, store 속성에 생성한 store를 담아준다.
```jsx
const root = ReactDOM.createRoot(document.getElementById('root'));  
root.render(  
  <Provider store={store}>  
    <App />  
  </Provider>  
);
```

> [!NOTE] 하나의 어플리케이션, 하나의 스토어
>     하나의 애플리케이션 에서는 단 한개의 스토어를 만들어서 사용한다. 여러개의 스토어를 사용하는게 가능하기는 하나, 권장 되지는 않는다.

## Action
action은 `type`과 `payload` 속성을 포함한 자바스크립트 객체이다.

```js
const PLUS_ONE = "counter/PLUS_ONE"

const addTodoAction = {
  type: PLUS_ONE,
  payload: 'Buy milk'
}
```
action 객체는 `type` 속성을 <span style='color:#eb3b5a'>필수</span>적으로 가지고 있어야 하고, 그 외의 값들은 개발자가 마음대로 넣어 줄 수 있다.

-  `type`: action을 설명하는 이름이다. 휴먼에러를 방지하기 위해 상수로 선언하자.
- `payload`: action에 전달되는 정보(데이터)를 의미한다.

## Action Creator
```js
const PLUS_N = "counter/PLUS_N"

export const plusN = (payload) => {  
  return {  
    type: PLUS_N,  
    payload: payload  
  }  
}
```

action creator는 handler라고도 불리며, action 객체를 리턴하는 함수를 말한다. 단순히 파라미터를 받아와서 액션 객체 형태로 만들어 준다.

## Reducer
리듀서는 `state`와 `action`을 인수로 받고, 하나의 `state`로 반환하는 함수이다.

> "Reducer" functions get their name because they're similar to the kind of callback function you pass to the [Array.reduce()](https://developer.mozilla.org/en-US/docs/Web/JavaScript/Reference/Global_Objects/Array/reduce) method.

- 리듀서는 불변성을 지키기 위해 기존 `state`를 변경하는 것이 아닌 새로운 `state`를 만들어 반환해야 한다. 즉, 기존 `state`를 복사한 후 값을 교체하여 반환하거나, 그냥 기존 `state`를 그대로 반환하던가 해야 한다.

- 리듀서는 순수 함수여야 한다.
    - 동일한 인풋이라면 언제나 동일한 아웃풋이 있어야 한다. 그런데 일부 로직 중에서는 실행 할 때마다 다른 결과값이 나타날 수 도 있다. 랜덤 숫자를 생성 한다던가, 네트워크에 요청을 한다던가... 그러한 작업은 결코 순수하지 않은 작업이므로, 리듀서 함수의 바깥에서 처리를 해줘야 한다.

```js
// reducer: 'state에 변화를 일으키는' 함수  
// input: state와 action  
const counter = (state = initialState, action) => {  
  switch (action.type) {  
    case PLUS_ONE:  
      return {...state, number: state.number + 1};  
    case MINUS_ONE:  
      return {...state, number: state.number - 1};  
    case PLUS_N:  
      return {...state, number: state.number + action.payload}; 
    case MINUS_N:  
      return {...state, number: state.number - action.payload}; 
    default:  
      return state;  
  }  
}
```


## Selectors
selector는 store에 저장된 state중 특정한 value를 가져올 때 사용하는 함수이다. `useSelector`라는 Hook을 이용하여 데이터를 가져올 수 있다.

```jsx
const counter = useSelector((state) => state.counter);
```

selector를 사용한 컴포넌트는 자동으로 store의 변화를 구독하게되며, 값이 변할 경우 컴포넌트가 리렌더링 된다.

## Dispatch
`dispatch`는 `state`를 업데이트할 수 있는 유일한 방법이다. `useDispatch` Hook을 이용하여 액션을 발생시킨다!

```jsx
const [number, setNumber] = useState(0);

// store에 접근하여 counter 값을 읽어오자  
const counter = useSelector(state => {  
  return state.counter;  
});

const dispatch = useDispatch();  
  
const handlePlus = () => {  
  dispatch(plusN(number))  
}  
  
const handleMinus = () => {  
  dispatch(minusN(number))  
}  
  
return (  
  <div className="App">  
    <button onClick={handlePlus}>+</button>  
    <button onClick={handleMinus}>-</button>  
  </div>  
);
```

