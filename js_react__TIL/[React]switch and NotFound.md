# [React] Switch and NotFound

강의: 패스트캠프
생성일: 2022년 2월 23일 오후 5:07
수정일: 2022년 2월 23일 오후 5:42
스킬 & 언어: react
중요도: 💜

# switch

- 여러 Route 중 **가장 먼저 맞는 하나만을 보여준다.**
- exact를 뺼 수 있는 로직을 만들 수 있다.
- 일치하는 Path 가 없다면 NotFound 페이지를 생성해 NotFound를 보여줄 수 있다.

스위치 문처럼 동작하게 된다.

***→ 이프문과 같이 일치하는 것이 적은 순으로 작성해야 한다!***

```jsx
import { BrowserRouter, Route, Switch } from "react-router-dom";
import Home from "./pages/Home";
import Profile from "./pages/Profile";
import About from "./pages/About";
import NotFound from "./pages/NotFound";

function App() {
  return (
    <BrowserRouter>
      <Switch>
				{/**/}
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

- 일치하는 것이 적은 순으로 작성한다.
- `/` 루트 경로엔 exact 를 작성해 일치하는 것만 표시되도록 해야 한다. → NotFound를 표시해야 하기 때문!
- NotFound는 path를 작성하지 않는다.