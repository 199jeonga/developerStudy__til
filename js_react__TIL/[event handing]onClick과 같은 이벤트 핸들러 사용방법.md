# event handing

html DOM에 클릭과 같은 이벤트가 발생하면 그에 맞는 변경이 일어나야 한다.

- **camelCase**로 작성해야 함
    - `onClick`, `onMouseEnter` **
    - js에서 사용하던 이벤트핸들러를 동일하게 사용할 수 있다. 단, on을 붙이고, camelCase로 작성해야 한다!!
- 이벤트에 연결된 자바스크립트 코드는 **함수**
    - `이벤트={함수}` 와 함수이기 때문에 중괄호 내부에 작성
- **실제 DOM 요소**들에만 사용 가능
    - react component에 사용하면 props로 전달

```jsx
function Component(){
        // return <div><button onClick={}>클릭</button></div>
        return <div><button onClick={()=>{ // onClick 내부엔 함수를 넣어야함!!!
          console.log('clicked');
        }}>클릭</button></div>
      }
      ReactDOM.render(<Component />, document.querySelector("#root"));
			// 컴포넌트는 이름이 아닌 jsx방식으로 작성해야 한다!
```

log가 아닌, 실제 변경이 되는 이벤트를 발생시키고자 할 때에는 props나 state를 사용해야 하는데 이 코드는 function component 를 사용했기 때문에 state를 사용할 수 없다.

```jsx
class Component extends React.Component {
        state = {
          count : 0,  // state의 초기값 설정
        };
        render(){
          return <div>
          <p>{this.state.count}</p> // state의 값이 표시되는 곳
          <button onClick={()=>{  // click이벤트가 발생하는 DOM요소
					// onMouseEnter 이벤트도 가능하다.

          console.log('clicked'); // 이벤트가 발생했을 때 변화하는 코드 1
          this.setState( (state)=>({...state, count : state.count+1})); // 2

        }}>클릭</button>
        </div>
        }
      }
      ReactDOM.render(<Component />, document.querySelector("#root"));
```

- state를 사용하기 위하여 내장함수인 setState를 불러왔다!

`this.setState( (state)=>({...state, count : state.count+1}));`

1. 바로 리턴할 것이기 때문에 `()`를 사용했음
2. 객체를 만들기 위하여  `{}` 로 한번 더 감쌌음 → 깊은 복사 진행 `{...}`
3.  `({...state, count : state.count+1})` 덮어씌어주는 형태로 코드를 작성한 후 return한다.

하지만!! 이렇게 인라인 함수로 적는 것은 가독성이 좋지 못하기 때문에 권장하지 않는다!

```jsx
class Component extends React.Component {
        state = {
          count : 0,
        };
        render(){
          return <div>
          <p>{this.state.count}</p>
          <button onClick={this.click}>클릭</button>
        </div>
        }
        click(){
          console.log('clicked');
          this.setState( (state)=>({...state, count : state.count+1}));
        }
      }
      ReactDOM.render(<Component />, document.querySelector("#root"));
```

이렇게 작성할 수 있는데 문제는 this 가 render 외부로 옮겨졌기 때문에 this를 제대로 찾지 못한다.

**방법1. constructor를 사용한다.**

```jsx
class Component extends React.Component {
        state = {
          count : 0,
        };
        constructor(props){
          super(props);
          this.click = this.click.bind(this);
        }
        render(){
          return <div>
          <p>{this.state.count}</p>
          <button onClick={this.click}>클릭</button>
        </div>
        }
        click(){
          console.log('clicked');
          this.setState( (state)=>({...state, count : state.count+1}));
        }
      }
      ReactDOM.render(<Component />, document.querySelector("#root"));
```

constructor를 사용하여 `[this.click](http://this.click) = this.click.bind(this);` → bind 함수를 사용하여 직접 찾은 this를 `this.click` 에게 할당해준다.

```jsx
class Component extends React.Component {
        state = {
          count : 0,
				}
        render(){
          return <div>
          <p>{this.state.count}</p>
          <button onClick={this.click}>클릭</button>
        </div>
        }
        click=()=>{
          console.log('clicked');
          this.setState( (state)=>({...state, count : state.count+1}));
        }
      }
      ReactDOM.render(<Component />, document.querySelector("#root"));
```

click 함수를 설정할 때, arrow function으로 작성하면 constructor를 작성하지 않고도 작성이 가능!