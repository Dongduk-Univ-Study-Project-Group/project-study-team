## 리액트는 무엇인가?

- UI를 위한 자바스크립트 기능 모음집
- 자바 스크립트 라이브러리
- 프레임 워크 vs 라이브러리 : 흐름에 대한 제어가 누구한테 있는지. 프레임 워크는 개발자가 흐름 제어 X 라이브러리는 O
- SPA를 쉽고 빠르게 만들 수 있게 하는 도구

## 리액트의 장점과 단점

### 장점

- 빠른 업데이트 & 랜더링 속도 : Virtual DOMDOM: 사이트의 정보가 모두 담겨 있는 객체라고 보면 됨
- Component-Based : 레고 블록을 조합하듯재사용성 : 다른 모듈의 의존성을 낮추고 호환성 문제를 낮추는 방향으로 개발을 해야 한다. 유지 보수가 용이하다.

### 단점

- 방대한 학습량
- 계속 바뀜
- 높은 상태관리 복잡도

## 직접 리액트 연동하기

- index.html, style.css 작성

```
<html><head><title>비야 그만와</title><link rel="stylesheet" href="style.css"></head><body><h1>비야 그만와</h1><!--DOM Container (Root DOM Node)-->
		<div id="root"></div><!-- 리액트 가져오기 -->
		<script src="https://unpkg.com/react@17/umd/react.development.js" crossorigin></script><script src="https://unpkg.com/react-dom@17/umd/react-dom.development.js" crossorigin></script>

		<!-- 리액트 컴포넌트 가져오기 -->
		<script src="MyButton.js"></script></body></html>
```

```
function MyButton(props) {
	const [isClicked, setIsClicked] = React.useState(false);

	return React.createElement(
		'button',
		{ onClick: () => setIsClicked(true) },
		isClicked ? 'Clicked' : 'Click here!'
	)
}

const domContainer = document.querySelector('#root');
ReactDOM.render(React.createElement(MyButton), domContainer);
```

## create-react-app

```
$ npx create-react-app <project-name>
```

으로 react app 생성이후 `cd <project-name>` `npm start` 로 실행
