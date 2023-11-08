---
{"tags":["React"],"dg-publish":true,"dg-hide":true,"permalink":"/front-end/react/context-api/","hide":true,"dgPassFrontmatter":true,"noteIcon":""}
---

# Props의 문제점 (Prop Drilling)
리액트에서는 일반적으로 위에서 아래, 즉 부모에서 자식으로 props를 통해 데이터를 전달한다.

그러나 앱이 커지고, 컴포넌트가 복잡해질 수록 전달해야 하는 props의 깊이는 더 깊어지고, 많은 컴포넌트를 거쳐야 데이터가 전달될 수 있다.

예를 들어, 접점이 없는 두 컴포넌트가 `state`를 전달하기 위해서는 두 컴포넌트가 공통으로 가지는 부모 컴포넌트까지 올라가야 한다.

![propdrilling.png](/img/user/FrontEnd/React/propdrilling.png)

즉, 깊이가 깊어질수록 `props`가 어느 컴포넌트에서 왔는지 파악하기 힘들어지고, 관리와 오류 대응이 늦어진다.
# Context API
![propwithcontext.png](/img/user/FrontEnd/React/propwithcontext.png)

이러한 점을 보완하기 위해 등장한 것이 Context API다. `useContext` Hook을 통해 쉽게 전역 데이터를 관리할 수 있다.

많은 컴포넌트에서 동일 `state`를 사용할 경우 **`Context`** 를 사용해서 `props`로 데이터를 전달해주지 않아도 `state`에 접근할 수 있다.

`Context는` 리액트의 `createContext`라는 함수를 이용해 만든다. 인자로는 `Context`의 기본값을 넣으며 어떤 값이든 올 수 있다.

# 사용법

## Context 생성
우선 Context를 생성할 파일을 생성한다. (파일명은 컴포넌트가 아니므로 kebab case로 작성한다.)
```js
// auth-context.js
// 기본 값 설정
const AuthContext = React.createContext({
  isLoggedIn: false,
  onLogout: () => {},
  onLogin: (email, password) => {},
});
```

## useContext
Context를 사용할 컴포넌트에서 `useContext` hook을 이용한다.
```jsx
const Login = () => {
	const ctx = useContext(AuthContext);
	// ...
}
```

`ctx` 변수에서 `props`가 아닌 Context를 통해 외부에서 선언된 값을 읽을 수 있다. 그러나 아직까지는 위에서 지정한 기본값 밖에 읽지 못한다.

## Provider
Context에 값을 동적으로 할당하여 제공하기 위해서는 Provider로 감싸줘야 한다.
```jsx
<AuthContext.Provider
      value={{
        isLoggedIn: isLoggedIn,
        onLogin: loginHandler,
        onLogout: logoutHandler,
      }}
    >
      <Login />
</AuthContext.Provider>
```

> **주의 사항!**
> useContext를 사용할 때, Provider에서 제공한 value가 달라진다면 useContext를 사용하고 있는 모든 컴포넌트가 리렌더링 된다. 따라서 이 부분을 항상 염두하고 사용 하자.