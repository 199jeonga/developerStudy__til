# [component] 컴포넌트 생성부터 사용방법까지 작성중

강의: 패스트캠프
생성일: 2022년 2월 1일 오후 9:18
수정일: 2022년 2월 2일 오전 1:57
스킬 & 언어: react

***최종 정리***

- 

***오늘 진도***

- 클래스 컴포넌트와 함수형 컴포넌트 생성 방법
- 

---

# hooks

hooks 이전에는 컴포넌트 내부에 상태가 있다면 class 를 만들어 사용 컴포넌트 내부에 상태가 있다면 라이프 사이클과 관계가 있을 때 class, 없을 때 function을 사용하였다.

**hooks 이후엔 class와 function 을 구분하지 않고 사용하게 되었다.** → 하지만 이 두가지는 엄연히 다른 것으로 개념은 알고 있는 것이 좋음!

# class component

```jsx
import React from 'react';
// 리액트라는 라이브러리를 리액트라는 이름으로 가져오겠다는 뜻, es6문법이다. CDN방식이 아닌 import 방식으로 전역객체로 불러오는 방식!

class ClassComponent extends React.Component{
	render(){
		return (<div>Hello</div>); // jsx는 후에 react.createElement로 변경되기 떄문에 imoprt react를 진행해주어야 한다.
	}
}
```

하단 코드는 `ClassComponent`를 다른 곳에서 사용하면 그것에 대한 결과가 `<div>HELLO!!</div>`로 랜더된다는 의미를 가진다.

***정의*  (class ClassComponent extends React.Component-> render -> return)**

```jsx
<script type="text/babel">

//클래스 클래스이름 / 리액트컴포넌트를사용하겠다는의미-> extends 라는 키워드를 통해 React.Component에게 상속 받음
      class ClassComponent extends React.Component{
//React.Component를 상속받은 후엔 메소드 render를 정의해주어야 한다.
	render(){
		//render는 return을 해주어야 한다. 
		return <div>HELLO!!</div>
	}
}

</script>
```

***사용***

```jsx
ReactDOM.render(
	<classComponent />,
	document.querySelector('#root')
)
```

# function component

***정의 1  - function keyword를 이용해 정의 하는 방법***

```jsx
//인자가 있을 수도 있고, 없을 수도 있으나 리턴은 필수이다.
function FunctionComponent(){
	return <div>HELLO!!!</div>
}
```

***정의 2 - arrow function을 만드는 방법***

```jsx
//화살표 함수는 리턴만 있다면 중괄호와 리턴을 생략할 수 있다.
const FunctionComponent = ()=>{
	return <div>HELLO!!!</div>
}

//생략된 함수식
const FunctionComponent = ()=><div>HELLO!!!</div>
```

***사용***

```jsx
ReactDOM.render(	<FunctionComponent/>,document.querySelector('#root'))
```