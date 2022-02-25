# styled Component 1 & 2

- 스코프 오염이 없다

---

```jsx
npx create-react-app styled-components-example

cd styled-components-example

npm i styled-components

code .

npm start
```

## styled Component 선언

```jsx
import styled from 'styled-components';

const StyledButton = styled.button``; //js문법으로 `` 내부에 style 문서를 작성할 수 있다.

export default StyledButton;
```

```jsx
import styled from 'styled-components';

const StyledButton = styled.button`
  background: transparent;
  border-radius: 3px;
  border: 2px solid palevioletred;
  color: palevioletred;
  margin: 0 1em;
  padding: 0.25em 1em;
`; // 작성을 하면 className이 자동으로 생성되어 스타일이 적용된다.
// 문자열이기 떄문에 오타가 났을 때 안내가 되지 않아 판단하기 어렵다. -> 플러그인 사용하면 좋음

export default StyledButton;
```

## styled Component 사용

```jsx
import logo from './logo.svg';
import './App.css';
import StyledButton from './components/StyledButton';

function App() {
  return (
    <div className="App">
      <header className="App-header">
        <img src={logo} className="App-logo" alt="logo" />
        <p>
          <StyledButton>버튼</StyledButton>
        </p>
      </header>
    </div>
  );
}

export default App;
```

---

## stylesd Component에 props 전달

```jsx
import styled, { css } from 'styled-components';

const StyledButton = styled.button`
  background: transparent;
  border-radius: 3px;
  border: 2px solid palevioletred;
  color: palevioletred;
  margin: 0 1em;
  padding: 0.25em 1em;

  ${props =>
    props.primary &&
    css`
      background: palevioletred;
      color: white;
    `};
`;

export default StyledButton;
```

```jsx
import StyledButton from './components/StyledButton';

function App() {
  return (
    <div className="App">
      <p>
        <StyledButton>버튼</StyledButton>
        <StyledButton **primary**>Primary 버튼</StyledButton>
      </p>
    </div>
  );
}

export default App;
```

## 이미 있는 컴포넌트가 아닌, 새로 컴포넌트를 만들 때 + 이미 있는 컴포넌트를 활용해 새로 만들 때!

```jsx
import styled, { css } from 'styled-components';

const StyledButton = styled.button`
  background: transparent;
  border-radius: 3px;
  border: 2px solid palevioletred;
  color: palevioletred;
  margin: 0 1em;
  padding: 0.25em 1em;
`;

// 새로 만들기 때문에 styled(); 실행을 해준다.
// styled를 실행하고 StyledButton을 인자로 넣어준다! 
const PrimaryStyledButton = styled(StyledButton)`
  background: palevioletred;
  color: white;
`;
// `` 내부에 따로 적용시킬 코드를 작성

export default PrimaryStyledButton;
```

## 같은 스타일을 적용시키지만 다른 요소로 변경하고자 할 때, as

```jsx
import StyledButton from './components/StyledButton';

function App() {
  return (
    <div className="App">
      <p>
        <StyledButton as="a" href="/"> // 해당 스타일을 같이 적용하는데 다른 태그를 사용하고자 할 때 (button이 아닌 a로 바꾸려고 함!) 사용하는 as attr
          a 태그 버튼
        </StyledButton>
        <StyledButton>버튼</StyledButton>
      </p>
    </div>
  );
}

export default App;
```

```jsx
import styled from 'styled-components';

const StyledButton = styled.button`
  background: transparent;
  border-radius: 3px;
  border: 2px solid palevioletred;
  color: palevioletred;
  margin: 0 1em;
  padding: 0.25em 1em;
  font-size: 1em;
  display: inline-block;

  text-decoration: none;
`;

export default StyledButton;
```

++ 여기서 as를 컴포넌트로 만들어 불러와 사용하고자 할 때에는 아래와 같이 작성해야 한다.

```jsx
import StyledButton from './components/StyledButton';

const UppercaseButton = props => (
  <button {...props} children={props.children.toUpperCase()} />
);
// prop =>{<button />}만작성했다면 props를 전달받기만 했을 뿐 실제 전달해주지는 않았기 때문에 해당 styled가 제대로 동작하지 않는다. prop => {<button {...prop} />} 로 전달 해당 태그에 제대로 전달해주어야 동작함 !

function App() {
  return (
    <div className="App">
      <p>
        <StyledButton as={UppercaseButton}>button</StyledButton>
        <StyledButton>button</StyledButton>
      </p>
    </div>
  );
}

export default App;
```

---

as보다 일반적인 방식

```jsx
import styled from 'styled-components';

function MyButton({ className, children }) {
  return <button className={className}>MyButton {children}</button>;
}

const StyledButton = styled(MyButton)`
  background: transparent;
  border-radius: 3px;
  border: 2px solid palevioletred;
  color: palevioletred;
  margin: 0 1em;
  padding: 0.25em 1em;
  font-size: 1em;
`;

export default StyledButton;
```

```jsx
<StyledButton>button</StyledButton>
```

---

## Global scope style

```jsx
import StyledButton from './components/StyledButton';
import { createGlobalStyle } from 'styled-components';

const GlobalStyle = createGlobalStyle` //어디선가는 랜더가 되어야 한다!! (글로벌한 영역)
  button {
    color: palevioletred;
  }
`;

function App() {
  return (
    <div className="App">
      <p>
        <GlobalStyle />
        <StyledButton>버튼</StyledButton>
        <button>버튼</button>
      </p>
    </div>
  );
}

export default App;
```

---

## 속성, 스타일 props로 전달

```jsx
import styled from 'styled-components';

const StyledA = styled.a.attrs(props => ({
  href: props.href || 'https://www.fastcampus.co.kr',
  color: props.color || 'palevioletred',
  target: '_BLANK',
}))`
  color: ${props => props.color};
`;

export default StyledA;
```

```jsx
import StyledA from './components/StyledA';

function App() {
  return (
    <div className="App">
      <p>
        <StyledA>링크</StyledA>
        <StyledA color="red">링크</StyledA>
      </p>
    </div>
  );
}

export default App;
```