# [react 실전] 리액트 실전활용

생성일: 2022년 3월 1일 오후 3:04
수정일: 2022년 3월 1일 오후 9:10

***최종 정리***

- 

***오늘 진도***

- 

***참고***

- 

---

# higher-order Component, HOC

컴포넌트를 인자로 받아 새로운 컴포넌트를 리턴하는 함수

```jsx
HOC = function(컴포넌트) { return 새로운 컴포넌트; }
```

- 리액트에만 국한된 내용이 아님
- 조합된 패턴
- 재사용이 가능한

전에 사용했던 withRoute()도 HOC이다. (with~~는 hoc일 확률이 높다.) withRoute내부에 Props를 작성해 새로운 컴포넌트를 생성했다.

`컴포지션` : 인자로 받은 컴포넌트를 jsx어딘가에 놓는 행위

주의할점

- hoc를 render 메소드 내부에서 사용하지 않는다. (랜더될 때마다 컴포넌트가 생성된다.)
- 인자로 들어간 스태틱 메소드는 복사되지 않는다.

# *Controlled Component & Uncontrolled Component*

```jsx
npx create-react-app controlled-uncontrolled-example
```

상태를 가지고 있는 엘리먼트

- input
- select
- textarea

- **controlled Component**

엘리먼트를 가지고 있는 컴포넌트가 관리

컴포넌트가 가지고있는 (상태를 가지고 있는) 엘리먼트를 컴포넌트의 상태로 관리

- **Uncontrolled Conpinent**

엘리먼트의 상태를 관리하지 않고 엘리먼트의 참조만 컴포넌트가 소유

## controlled Component

실제 ui가 변경될 때는 uncontrolled Component보다 controlled Component를 사용하는 것이 제어하기 쉬움!

```jsx
import React from 'react';

export default class Controlled extends React.Component {
  state = { value: '' };

  render() {
		const {value} = this.state;
    return (
      <div>
        <input value={value} onChange={this.change}/>
				<button onClick={this.click}>전송</button>
      </div>
    );
		change = e => {
    this.setState({ value: e.target.value });
  };
		click=()=>{
			console.log(this.state.value);
		}
  }
}
```

## uncontrolled Component

```jsx
import React from 'react';

export default class Uncontrolled extends React.Component {
  _input = React.createRef();

  render() {
    return (
      <div>
        <input ret={this._input} />
				<button onClick={this.click}>전송</button>
      </div>
    );
		click=()=>{
			//인풋 엘리먼트의 현재 상태값을 꺼내 전송한다.
			document.querySelector("#my-input");
			// console.log(input.value); //실제 DOM이기 때문에 꺼낼 수 있다.
			//하지만 리액트는 가상DOM을 사용하는 것을 지향하기 때문에 ㅇ이 방법을 권장하지 않음.
			
		}
  }
}
```

### 실제 DOM이 아닌 가상의 DOM(createRef)을 사용해서 값을 저장, 가져오는 방법

```jsx
import React from 'react';

const Uncontrolled = () => {
  const inputRef = React.createRef(); //랜더가 되는 시점에 inputRef에 값을 넣어놓고 필요할 때 꺼내서 사용한다. 일종의 저장ㅊ장치 !!

  function click() {
    console.log('최종 결과', inputRef.current.value); 
		{/*최초엔 null*/}
  }
  
  return (
    <div>
      <input ref={inputRef} /> {/*createRef가 어떤 요소인지 선언*/}
      <button onClick={click}>전송</button>
    </div>
  );
	componenytDidMount(){ //한번 이상 랜더된 것
		console.log(this.inputRef);
	}
	click = ()=>{
		console.log(this.inputRef.current);
	}
};

export default Uncontrolled;
```