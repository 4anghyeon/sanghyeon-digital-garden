---
{"tags":["React"],"dg-publish":true,"dg-hide":true,"permalink":"/front-end/react/styled-components/","hide":true,"dgPassFrontmatter":true,"noteIcon":""}
---


# CSS In JS

스타일링을 위해 CSS를 작성하는 것에는 많은 방법이 있다. 잘 아는 방식인 css 파일에 작성할 수도 있고, html에 인라인으로 작성할 수도 있다. 

**CSS In JS는 <span style='color: #2196F3'>CSS를</span> 자바스크립트 파일, 즉 <span style='color: #2196F3'>리액트에서는 컴포넌트안에 작성하는 것이다.</span>**


# styled-components

styled-components 는 CSS In JS를 구현하는 대표 라이브러리이다.
![](https://velog.velcdn.com/images/sanghyeon/post/53460ac1-b994-47af-990b-50edd7ab2483/image.png)

styled-components를 사용할 경우 다음과 같은 이점이 있다.

1. 실제로 렌더링에 필요한 CSS만 코드에 삽입하여 사용자의 코드 로드 부하를 줄일 수 있다.
2. 유니크한 클래스 이름을 생성하여 클래스 이름이 중복되거나, 오타가 나는 경우를 없애준다.
3. 여기저기 얽혀 있는 CSS 코드의 삭제를 쉽게 해준다.
4. 쉬운 동적 스타일링이 가능하다.
5. 간편한 유지 관리가 가능하다.

## 사용 준비
### 패키지 설치
```bash
yarn add styled-components
```

### 플러그인 설치 (Optional)
![](https://velog.velcdn.com/images/sanghyeon/post/008d5b1e-9706-4791-8df6-61bbcb317bfd/image.png)VSCode를 사용할 경우 다음 플러그인을 사용하면 좀 더 편하게 스타일 코드를 작성할 수 있다.


## 사용
styled-components는 자바스크립트의 템플릿 리터럴을 활용하여 작성한다.
```jsx
// html style
const Button = styled.button`
  font: inherit;
  padding: 0.5rem 1.5rem;
  border: 1px solid #8b005d;
  color: white;
  background: #8b005d;
  box-shadow: 0 0 4px rgba(0, 0, 0, 0.26);
  cursor: pointer;

  &:focus {
    outline: none;
  }

  &:hover,
  &:active {
    background: #ac0e77;
    border-color: #ac0e77;
    box-shadow: 0 0 8px rgba(0, 0, 0, 0.26);
  }
`

// 기존 컴포넌트 style
const NewButton = styled(Button)`
	color: blue
`
```

styled-components 에서는 기본적으로 선택자를 사용할 필요가 없으며, 자신의 하위 항목을 가리킬 때는 자기 자신을 가리키는 `&` 예약어를 사용한다.

어떻게 작성하던 결국에는 리액트에서 사용 가능한 새로운 컴포넌트로 반환이 된다.


## props 사용
styled-components 에서도 `props`를 사용할 수 있다.
일반 컴포넌트에서 사용하던 것처럼 `props`라는 인자에서 내려주는 값을 받아온다.

```jsx
const Button = styled.button`
  font: inherit;
  padding: 0.5rem 1.5rem;
  border: 1px solid #8b005d;
  color: white;
  background: ${props => props.invalid ? "red" : "#8b005d"};
  box-shadow: 0 0 4px rgba(0, 0, 0, 0.26);
  cursor: pointer;

  &:focus {
    outline: none;
  }

  &:hover,
  &:active {
    background: #ac0e77;
    border-color: #ac0e77;
    box-shadow: 0 0 8px rgba(0, 0, 0, 0.26);
  }
`

return (
	<Button invalid={!isValid} />
)
```