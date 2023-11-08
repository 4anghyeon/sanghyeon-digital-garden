---
{"tags":["React"],"dg-publish":true,"dg-hide":true,"permalink":"/front-end/react/use-effect/","hide":true,"dgPassFrontmatter":true,"noteIcon":""}
---

# useEffect

`useEffect`는 컴포넌트가 렌더링 될 때 side effect를 실행하도록 도와주는 Hook이다. side effect는 컴포넌트가 렌더링 된 후 비동기적으로 처리되어야 하는 작업들을 말한다. 이 Hook 함수를 통해서 함수형 컴포넌트에서 생명주기 메서드를 사용할 수 있게 된다.

# 사용법

```jsx
useEffect(fuction, deps)
```

- `function`에는 실행하고자 하는 코드가 들어간다.
- `deps`에는 **의존성 배열**이 들어간다.

## 의존성 배열
의존성 `useEffect`와 같은 Hook이 불필요하게 반복해서 실행되는 것을 방지하여 성능을 최적화하기 위해서 사용된다. 존성 배열에 포함된 값들이 변경될 때만 Hook이 재실행된다. 이를 통해 불필요한 리렌더링을 막을 수 있다.

```jsx
// useEffect의 두번째 인자가 의존성 배열이 들어가는 곳.
useEffect(()=>{
	// 실행하고 싶은 함수
}, [의존성배열])
```

## 컴포넌트가 Mount 될 때
```jsx
// 컴포넌트가 리렌더링될 때마다 발생함
useEffect(() => {
	console.log("mount");
});


// 컴포넌트가 처음 렌더링될 때만 발생함
useEffect(() => {
	console.log("first");
}, []);
```

## State가 업데이트 될 때
`todo`라는 state가 업데이트 될 경우 실행된다.
```jsx
useEffect(() => {
	console.log("TodoData chagned");
}, todo)
```

## 컴포넌트가 Unmount 될 때

```jsx
useEffect(() => {
	console.log("mount");
	return () => {
		console.log("unmount"); // cleanup
	}
}, []);
```
- `deps` 배열안에 값이 있다면 값이 변화하기 직전에 호출이 된다.

`return`에 들어가는 함수를 clean up 함수라고 한다. 이를 이용해 디바운싱도 구현할 수 있다.

```jsx
useEffect(() => {
  let timeout = setTimeout(() => {
    setFormIsValid(
      enteredEmail.includes('@') && enteredPassword.trim().length > 6
    );
  }, 500)

  return () => {
    clearTimeout(timeout);
  }

}, [enteredPassword, enteredEmail]);
```

## 중첩 속성 전달하기
state가 객체일 경우, state에서 특정 property가 변했을 때만 `useEffect` Hook이 실행되기를 원할 수 있다. 그럴 때는 다음과 같이 한다.

```jsx
useEffect(() => {
    console.log("check")
}, [emailState.isValid]);
```