---
{"tags":["React"],"dg-publish":true,"dg-hide":true,"sticker":"emoji//1f61d","permalink":"/front-end/react/react-memoization-3/","hide":true,"dgPassFrontmatter":true,"noteIcon":""}
---

리액트에서는 다음과 같은 조건에서 리렌더링이 일어난다.
1. 컴포넌트에서 `state`가 바뀔 때
2. 컴포넌트가 내려받은 `props`가 바뀔 때
3. 부모 컴포넌트가 리렌더링 된 경우, 모든 자식 컴포넌트에서 리렌더링

위의 조건들 때문에 `state`하나가 바뀌면 여기저기서 리렌더링이 일어난다. **리렌더링이 필요하지 않은 컴포넌트에서도..!!**
리렌더링은 비용이 많이 발생하는 작업이기 때문에, 리렌더링 할 필요가 없다면 최대한 하지 않는 것이좋다. 리액트에서 이러한 최적화를 하는 대표적인 방법 3가지가 있다.
- `React.memo`
- `useCallback`
- `useMemo`

![Pasted image 20231108155748.png](/img/user/Pasted%20image%2020231108155748.png)

---
# React.memo
부모 컴포넌트가 렌더링 되면 그 자식 컴포넌트들도 모두 리렌더링 된다고 했다.
자식 컴포넌트 입장에서는 자신이 바뀐 것도 없는데 리렌더링을 해서 성능을 악화시키니 억울하다.
이럴 때 `React.memo`를 사용해서 자식 컴포넌트의 억울함을 풀어줄 수 있다.

리액트는 먼저 가상 DOM에 컴포넌트를 렌더링 한 다음, 이전에 렌더링된 결과와 비교하여 실제 DOM에 적용 여부를 결정한다.

`React.memo()`는 컴포넌트를 래핑하여 사용하고 래핑된 컴포넌트는 렌더링 결과를 메모이징(Memoizing)한다. <span style='color:#eb3b5a'>그리고 다음 렌더링때 컴포넌트로 전달되는 `props`의 값이 같다면 리액트는 메모이징된 내용을 재사용한다. </span>즉, 컴포넌트의 `render()`를 재실행하지 않는다.

## 사용법
```jsx
const DemoComponent = props => {
	return <p>Demo</p>
}

export default React.memo(DemoComponent);
```
`export` 하는 컴포넌트에 `React.memo()` 로 감싸주기만 하면 된다.

## React.memo()의 사용이 적절한 경우
부모 컴포넌트가 자주 리렌더링 되지만 자식 컴포넌트에서는 바뀔일이 없는 경우, 자식 컴포넌트에 `React.memo` 를 사용하면 적절하다.

## 주의사항
`props`의 값을 비교할 때는 얕은 비교를 한다. 따라서 `props`에 참조형 변수나 콜백 함수가 들어간다면 다른 `props`가 들어온 것으로 판단하고 재실행을 한다. 이점에 유의하도록 한다.

```jsx
<DemoComponent show={false} />
<Button onClick={clickHanlder} />
```

위의 예시 코드에서 `DemoComponent`는 재실행되지 않는다. 왜냐하면 false는 기본형(원시값)이기 때문에 새로 생성되어도 같은 값으로 판단 한다.

하지만 `Button` 컴포넌트는 계속해서 재실행된다. 컴포넌트가 실행 될 때마다 새로운 `clickHandler` 함수를 생성하여 `clickHandler`는 이전의 `clickHandler`와 다르다고 인식하여 `React.memo`로 감싸도 컴포넌트가 다시 실행되는 것이다. 즉, Memoizing이 되지 않는다.

그렇다면 함수를 전달하는 경우에는 `React.memo()`를 사용할 수 없을까? 아니다. `useCallback` Hook을 이용하여 함수의 재생성을 방지할 수 있다.

---
# useCallback
컴포넌트가 리렌더링될 때 컴포넌트 안에 있는 함수도 새롭게 다시 만들어진다. 함수의 생성이 많은 리소스를 차지하는 작업은 아니지만, 함수가 필요할 때만 재정의되야 하는건 언제나 중요하다.
따라서 한번 만든 함수를 다시 만들지 않고 필요할 때만 재사용할 필요가 있다. 이런 경우 `useCallback` 함수를 사용한다.

## 사용법
단순히 기존 함수에 다음과 같이 `useCallback(function, [deps])` 으로 감싸주면된다.

```jsx
const delete = useCallback(
    (id) => {
        const newData = data.filter((d) => d.id !== id);
        setTodoData(newData);
    },
    [data] // deps 배열
);
```
주의할 점은 함수 안에서 사용중인 `state`, 혹은 `props`가 있다면 `deps` 배열안에 꼭 포함을 시켜야한다. 만약 포함하지 않는다면, 함수에서 `state`나 `props`의 값을 참조할 때 가장 최신 값을 참조한다고 보장할 수 없기 때문이다.

---
# useMemo
`useMemo`는 연산된 값을 메모이징하여 중복 연산을 피하게 도와주는 Hook이다. 예를 들어, 다음과 같이 무거운 작업을 하는 함수가 있다고 하자.

```jsx
const heavy = (a, b) => {
	// 10초 이상 걸리는 로직
}
```

컴포넌트가 렌더링 될 때마다 이 연산이 일어나게 된다. 하지만 만약 인자로 넘어오는 `a`와 `b`가 같다면 결과도 같을 것이므로 굳이 연산을 하지 않아도 될 것이다.

## 사용법
```jsx
const heavy = useMemo((a,b) => {
 return something..;
}, [a, b]);
```
값을 반환하는 함수를 `useMemo`로 감싸 주면 된다. `deps` 배열에 들어온 `a`와 `b`값이 변할 때만 재연산을 하게 되고, `a`와 `b`가 같다면 기존에 연산후 메모이징 해놓은 값을 반환한다.

이는 함수뿐 아니라 모든 객체 타입의 데이터에도 적용 된다. *(자바스크립트에서는 함수도 객체이지만..)*
예를 들어 다음과 같은 경우를 살펴보자.

```jsx
const me = {
	name: "Ted Chang",
	age: 21,
	isAlive: isAlive ? "생존" : "사망",
};

useEffect(() => {
	console.log("생존여부가 바뀔 때만 호출해주세요!");
}, [me]);
```
여기서 `me`의 어떠한 속성도 바뀌지 않는다고 해보자. 그러나 컴포넌트가 리렌더링 될 때마다 `useEffect`안의 내용이 실행된다. <span style='color:#eb3b5a'>그 이유는 리렌더링 될 때 me에 새로운 객체를 할당하게 되고, 메모리 주소값이 바뀌어 리렌더링 전의 `me`와 후의 `me`가 다른 값이라고 판단하기 때문이다.</span>
이런 경우에도 `useMemo`를 사용해 줄 수 있다.

```jsx
// isAlive 속성이 바뀌면 재실행!
const me = useMemo(() => {
  return {
    name: "Ted Chang",
    age: 21,
    isAlive: isAlive ? "생존" : "사망",
  };
}, [isAlive]);
```


> [!NOTE] **❓ 자바스크립트에서는 함수는 일급 객체로서 값으로 취급되는데..❓ **
> `useCallback`도 함수를 감싸고, `useMemo`도 함수를 감쌀 수 있다. 둘의 차이점은
> `useCallback`은 함수 자체를 저장 하는 것이고, `useMemo`는 함수를 실행한 반환 값을 저장하는 것이다!

---

# 요약
- `React.memo`: 컴포넌트 자체를 메모이징 할 때
- `useCallback`: 함수를 메모이징 할 때
- `useMemo`: 값을 메모이징 할 때