# 리액트

강의: 패스트캠프
메모:  Part 10. React - Ch 1.React Getting Started
생성일: 2022년 1월 31일 오후 11:32
수정일: 2022년 2월 1일 오후 9:12
스킬 & 언어: react

***오늘 진도***

- 리액트 키워드(jsx, component, virtualDOM etc.)
- ReactDOM, render etc.

---

# component

**html, css, js를 합쳐서 만든 일종의 태그와 같은 개념**

html에서는 기본 내재되어 있던 요소와 attribute, 속성을 사용했지만 component의 개념을 가지고 있는 리액트에서는 직접 지정한 이름으로 component를 만들고 이에 `name`, `prop={}`를 통해 추가 적인 데이터를 직접 집어넣어줄 수 있다.

- 컴포넌트 트리는 DOM Tree와 비슷한 형태를 가지고 있으나, 내가 직접 만든 목록이란 것의 차이가 있다.

# Virtual DOM - 가상의 돔

가상의 돔이 아닌 실제 돔의 경우에는 바뀐 부분만 정확히 바꿔야 하지만 가상의 돔을 사용하기 때문에 이전 상태와 이후 상태를 비교하여 바뀐 부분만 찾아내 자동으로 바꿀 수 있다.

Virtual DOM

- diff(달라진점)로 변경한다.

# React Clinet side Rendering(CSR), Server Slide Rendering(SSR)

1. html 다운로드
2. js 다운로드
3. js 내부에 있는 리액트 다운
4. 리액트가 실행되면 화면에 표시

- **CSR**

js가 전부 다운로드 된 후, 리액트가 정상 실행되기 전까지 화면이 표시되지 않음.

- **SSR**

JS가 전부 다운로드되지 않아도 화면은 표시된다. 단, 유저가 홈페이지를 사용할 수는 없다.

JS가 전부 다운로드되어 리액트가 정상 실행될 때 유저가 사용 가능

# React 라이브러리

```jsx
import ReactDOM from 'react-dom'; // HJTMLElement 연결하기
import React from 'react';        // 리액트 컴포넌트 만들기
```

---

***react folder 생성을 위한 cli 명령어***

```jsx
mkdir what-is-react
cd what-is-react
npm init -y
npx serve
```

# React 사용하기

기존엔 html로 문서 구조를 잡은 후 css로 스타일을 입히고, JS로 DOM을 작업해주었지만 

- ***CDN을 통한 React 연결***
    
    [https://ko.reactjs.org/docs/cdn-links.html](https://ko.reactjs.org/docs/cdn-links.html)
    
    ```jsx
    <!doctype html>
    <html lang="en">
      <head>
        <meta charset="utf-8" />
        <meta name="viewport" content="width=dwvice-width, initial-scale=1.0" />
        <meta http-equiv="X-UA-Compatible" content="ie=edge" />
        <title>Example</title>
      </head>
      <body>
        <script crossorigin src="https://unpkg.com/react@17/umd/react.development.js"></script>
        <script crossorigin src="https://unpkg.com/react-dom@17/umd/react-dom.development.js"></script>
        <script type="text/javascript">
          console.log(React);
          console.log(ReactDOM);
        </script>
      </body>
    </html>
    ```
    

```jsx
 const root = document.querySelector('#root');
       const btn_plus = document.querySelector('#btn_plus');
       let i=0;
       root.innerHTML = '<p>init : 0</p>';
       btn_plus.addEventListener('click', ()=>{
         root.innerHTML = `<p>init : ${++i}</p>`;
       });
```

```jsx
const component = {
        message:'init', // 행위에 의해 값이 바뀔 수 있다.
        count:0,
        render(){
          return `<p>${this.message} : ${this.count}</p>`; // 표현되는 결과물
        }
      };
      function render(rootElement, component){
        rootElement.innerHTML = component.render();
      }

      render(document.querySelector('#root'), component); // 값이 변경되자마자 랜더를 해줘야 한다.

      document.querySelector('#btn_plus').addEventListener('click',()=>{
        component.message = 'update'; //이벤트가 발생되어 값이 변경된다.
        component.count = component.count+1;

        render(document.querySelector("#root"), component);// 값이 변경되자마자 랜더를 해줘야 한다.
      });
```

***리액트 돔 : 실제 돔에 컴포넌트를 연결하는 라이브러리***

```jsx
const Component = props=>{
// 리액트 엘리먼트로 리턴이 진행되어야 한다. 
	return React.creatElement('p',null, `${props.message}:${props.count}`)
//API, 첫번째 인자는 요소명을, 두번째 인자는 속성, 세번째 인자는 태그 내에 들어갈 내용을 작성한다.
//현재 작성한 컴포넌트는 규정, 정의했다고 볼 수 있다.

}
// ReactDOM.render(/*리액트 컴포넌트*/, document.querySelector('#root');
ReactDOM.render(React.creatElement(Component, {message:'init',count:0},null), document.querySelector('#root');
);

document.querySelector('#btn_plus').addEventListener('click',()=>{
	ReactDOM.render( //ReactDOM.render 후에 ReactDOM.render를 같은 DOM을 대상으로 하기 때문에 같은 부분이 다시 그려진다(업데이트 된다.)-> 가상의 돔
		React.creatElement(
			component,
			{message:"update", count:10},
			null
		),
		document.querySelector("#root")
	)

  render(document.querySelector("#root"), component);
});
});
```