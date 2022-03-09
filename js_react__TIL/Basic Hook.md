# Basic Hook 1&2

---

class에서만 lifeCycle, State를 사용할 수 있었는데 Hook 이후로 function에서도 사용이 가능해졌다. → 가장 큰 의의는 재사용이 가능하다는 것!!

# Basic Hooks

```jsx
npx create-react-app react-hooks-example
```

## useState

클래스를 이용한 State Code

```jsx
import React from 'react';

export default class Example1 extends React.Component {
  state = {
    count: 0,
  };

  render() {
    const { count } = this.state;
    return (
      <div>
        <p>You clicked {count} times</p>
        <button onClick={this.click}>Click me</button>
      </div>
    );
  }

  click = () => {
    this.setState({ count: this.state.count + 1 });
  };
}
```

**함수를 이용한 State code**

```jsx
import React, { useState } from 'react';

const Example2 = () => {
  const [count, setCount] = useState(0);
  // [값&변결될값, 함수] = useState(초기값);

  function click() { //class와 달리 this.click이 아닌 click!!
    setCount(count + 1);
  }

  return (
    <div>
      <p>You clicked {count} times</p>
      <button onClick={click}>Click me</button> //click을 누르면 function click이 호출된다. 
    </div>
  );
};

export default Example2;
```

*const [count, setCount] = useState(0);*

*const [스테이트 값, 스테이트 변경 함수] = useState(스테이트 초기값);*

**state를 객체로 사용한다.**

```jsx
import React, { useState } from 'react';

const Example3 = () => {
  const [state, setState] = useState({ count: 0 }); //클래스의 state = {count: 0,};

  function click() {
    setState({ count: state.count + 1 });
  }

  return (
    <div>
      <p>You clicked {state.count} times</p>
      <button onClick={click}>Click me</button>
    </div>
  );
};

export default Example3;
```

기존의 state값에 의존적으로 변경하고 싶을 땐 객체가 아닌 함수를 사용한다. `setState(state⇒{return {count:state.count+1} }`

→ 추 후에는 setState만 사용하는 것이 아닌 setState가 어떤 것을 의존하여 사용하는지 (디팬더씨..??) 가 중요해진다. 리턴해서 사용하는 함수와 같은 형태의 setState는 useState에 의존하지 않고 단독으로 있어도 함수에 인자로 들어오기 때문에 함수로 사용하는 것을 기억하고 있는 것이 좋다!!!

function - hooks가 나온 이유

- *컴포넌트 사이에서 상태와 관련된 로직을 재사용하기 어렵습니다*
    - *컨테이너 방식 말고, 상태와 관련된 로직*
- *복잡한 컴포넌트들은 이해하기 어렵습니다.*
- *Class 는 사람과 기계를 혼동시킵니다.*
    - *컴파일 단계에서 코드를 최적화하기 어렵게 만든다.*
- *this.state 는 로직에서 레퍼런스를 공유하기 때문에 문제가 발생할 수 있다.*

## useEffect

라이프사이클훅을 대체할 수 있다. (*componentDidMount, componentDidUpdate, componentWillUnmount*)

```jsx
import React from 'react';

export default class Example4 extends React.Component {
  state = { count: 0 };
  componentDidMount() {
    console.log('componentDidMount', this.state.count);
  }

  componentDidUpdate() {
    console.log('componentDidUpdate', this.state.count);
  }

  render() {
    const { count } = this.state;
    return (
      <div>
        <p>You clicked {count} times</p>
        <button onClick={this.click}>Click me</button>
      </div>
    );
  }

  click = () => {
    this.setState({ count: this.state.count + 1 });
  };
}
```