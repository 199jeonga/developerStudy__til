# [React] JSX 링크로 라우팅 이동 & JS 링크로 라우팅 이동

강의: 패스트캠프
메모: import { withRouter } from “react-router-dom && Redirect
생성일: 2022년 2월 23일 오후 5:42
수정일: 2022년 2월 23일 오후 10:56
스킬 & 언어: react
중요도: 💜💜

# JSX 링크로 라우팅 이동하기

## React-router-dom → Link

`import { Link } from 'react-router-dom';`

```jsx
import { BrowserRouter, Route, Switch } from "react-router-dom";
import Home from "./pages/Home";
import Profile from "./pages/Profile";
import About from "./pages/About";
import NotFound from "./pages/NotFound";
import Links from "./components/Links"; //Link to 코드가 들어있는 컴포넌트 생성

function App() {
  return (
    <BrowserRouter>
      <Links />
      <Switch>
        <Route path="/profile/:id" component={Profile} />
        <Route path="/profile" component={Profile} />
        <Route path="/about" component={About} />
        <Route path="/" exact component={Home} />
        <Route component={NotFound} />
      </Switch>
    </BrowserRouter>
  );
}

export default App;
```

```jsx
import { Link } from "react-router-dom";

export default function Links() {
  return (
    <ul>
      <li>
        <Link to="/">Home</Link>
      </li>
      <li>
        <Link to="/profile">Profile</Link>
      </li>
      <li>
        <Link to="/profile/1">Profile/1</Link>
      </li>
      <li>
        <Link to="/about">About</Link>
      </li>
      <li>
        <Link to="/about?name=mark">About?name=mark</Link>
      </li>
    </ul>
  );
}
```

## React-router-dom → NavLink

`import { NavLink } from 'react-router-dom';`

- active한 상태에 대한 style 지정이 가능하다.
- Route - path처럼 동작하기 때문에 exact가 있다.

```jsx
import { NavLink } from "react-router-dom";

const activeStyle = { color: "green" };

export default function NavLinks() {
  return (
    <ul>
      <li>
        <NavLink to="/" exact activeStyle={activeStyle}>
          {/*스타일이 정확히 일치할 때만 스타일을 지정시키게 하기 이해 exact 작성*/}
          Home
        </NavLink>
      </li>
      <li>
        <NavLink to="/profile" exact activeStyle={activeStyle}>
          Profile
        </NavLink>
      </li>
      <li>
        <NavLink to="/profile/1" activeStyle={activeStyle}>
          {/*겹칠일이 없기 때문에 exact XX*/}
          Profile/1
        </NavLink>
      </li>
      <li>
        <NavLink
          to="/about"
          activeStyle={activeStyle}
          isActive={(match, location) =>
            match !== null && location.search === ""
          }
        >
          About
        </NavLink>
      </li>
      <li>
        <NavLink
          to="/about?name=mark"
          activeStyle={activeStyle}
          isActive={(match, location) =>
            match !== null && location.search === "?name=mark"
          }
        >
          About?name=mark
        </NavLink>
      </li>
    </ul>
  );
}
```

![Untitled](img/0223Img1.png)

`match` : 값이 동일하지 않으면 null이 나옴

`location` : 좌측 이미지와 같은 isActive에 내장되어 있는 프로퍼티

```jsx
       <NavLink
          to="/about"
          activeStyle={activeStyle}
          isActive={(match, location) =>
            match !== null && location.search === ""
          }
        >
          About
        </NavLink>
      </li>
      <li>
        <NavLink
          to="/about?name=mark"
          activeStyle={activeStyle}
          isActive={(match, location) =>
            match !== null && location.search === "?name=mark"
          }
        >
```

`isActive` : 액티브인지 아닌지를 세세하게 설정할 수 있는 함수, 리턴값이 true, false인지를 확인 후 액티브가 되는지 아닌지를 판단한다.

```jsx
isActive={(match, location) =>
{/*매치과 로케이션의 값은 React-router-dom에서 입력된다.*/}
            match !== null && location.search === ""
          }
```

# js로 라우팅 이동하기

```jsx
// src/pages/Login.jsx

import LoginButton from "../components/LoginButton";

export default function Login(props) {
  return (
    <div>
      <h2>Login 페이지 입니다.</h2>
      <LoginButton {...props} /> {/*props를 연결해주지 않는다면 하위 컴포넌트가 props를 확인할 수 없다!*/}
    </div>
  );
}
```

```jsx
// src/components/LoginButton.jsx

export default function LoginButton(props) {
  console.log(props);
  function login() {
    setTimeout(() => {
      props.history.push("/");
    }, 1000);
  }
  return <button onClick={login}>로그인하기</button>;
}
```

- 바로 하단 컴포넌트이기 때문에 보다 쉽게 (e.g `<LoginButton {...props} />`) 설정을 했지만 한 단계 아래의 컴포넌트가 아닌 여러단계의 하위 컴포넌트라면 오류가 나기 쉽다..!

**!! 해결방법 !!**

`import { withRouter } from “react-router-dom` → hoc

```jsx
// src/pages/Login.jsx

import LoginButton from "../components/LoginButton";

export default function Login() {
  return (
    <div>
      <h2>Login 페이지 입니다.</h2>
      <LoginButton />
    </div>
  );
}
```

```jsx
import { withRouter } from "react-router-dom";

export default withRouter(function LoginButton(props) {
  console.log(props);
  function login() {
    setTimeout(() => {
      props.history.push("/");
    }, 1000);
  }
  return <button onClick={login}>로그인하기</button>;
});
```

- 여러 단계로 연결된 컴포넌트에서 가장 하위 컴포넌트에서 사용한다. `withRouter()`

# Redirect

```jsx
import { Redirect } from "react-router-dom";

// jsx
<Redirect to="/" />;
```

```jsx
const isLogin = true;

function App() {
  return (
    <BrowserRouter>
      <Switch>
        <Route
          path="/login"
          render={() => (isLogin ? <Redirect to="/" /> : <Login />)}
        />
      </Switch>
    </BrowserRouter>
  );
}

export default App;
```

리다이랙트가 랜더되면 `to="/"` 로 연결된다.

→ 다시 요청을 해서 원래 가야하는 곳이 아닌 다른 곳으로 이동되게 하는 기능
