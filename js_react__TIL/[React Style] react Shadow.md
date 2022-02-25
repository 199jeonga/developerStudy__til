# react Shadow

웹컴포넌트 - shadowDom은 어떤 html안에 본래의 html에 영향을 주지 않는 별도의 html 덩어리

→ html에서 shadowDom을 사용하여 스코프가 오염되지 않도록 사전에 차단한 것!

장점

- 컴포넌트 하나하나를 root.div로 묶기 때문에 스코프가 오염될 일이 없다

단점

- 공통적인 스타일도 계속 반복적으로 작성해야 한다.
- 외부와 내부가 차단되어 있다. → 값을 받아 사용해야 할 때 제약이 있다!

```jsx
npx create-react-app react-shadow-example

cd react-shadow-example

npm i react-shadow

code .

npm start
```

---

## react shadow 참고 코드

```jsx
import logo from "./logo.svg";
import root from "react-shadow";

const styles = `...`; {/*길어서 생략했지만 스타일 코드를 작성했다.*/}

function App() {
  return (
    <root.div>{/*최상위를 root.div로 감쌌다. 외부와 연결되지 않은 자체 스타일!*/}
      <div className="App">
        <header className="App-header">
          <img src={logo} className="App-logo" alt="logo" />
          <p>
            Edit <code>src/App.js</code> and save to reload.
          </p>
          <a
            className="App-link"
            href="https://reactjs.org"
            target="_blank"
            rel="noopener noreferrer"
          >
            Learn React
          </a>
        </header>
      </div>
      <style type="text/css">{styles}</style>{/*styles는 css, scss와 같은 스타일 시트 코드가 작성되어야 한다. */}
    </root.div>
  );
}

export default App;
```