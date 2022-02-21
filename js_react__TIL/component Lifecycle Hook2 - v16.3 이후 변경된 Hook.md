# component Lifecycle Hook2 - v16.3 이후 변경된 Hook

1. **constructor**
2. ~~componentWillMount~~ → **getDerivedStateFromProps**
3. **render**
4. **componentDidMount**

### update, props, state가 변경되었을 때

1. ~~componentWillReceiveProps~~ → **getDerivedStateFromProps**
2. **shouldComponentUpdate**
3. **render**
4. ~~componentWillUpdate (변경 전엔 render 전에 있던 HOOk이었으나 변경된 후에는 render된 직후에 위치함)~~ → **getSnapshotBeforeUpdate** 
5. <DOM에 적용>
6. **componentDidUpdate**

**componentWillUnmout**

---

```jsx
static getDerivedStateFromProps(nextProps, prevState){
          console.log(nextProps, prevState);
          return null;
        }
```

getDerivedStateFromProps 는 return 을 작성해야 한다.

- 만약 리턴해야 할 게 없다면 undefined 가 아닌 null; 을 작성해야 한다!!

```jsx
static getDerivedStateFromProps(nextProps, prevState){
          console.log(nextProps, prevState);
          return {
            age:390,
          };
        }
```

새로운 state (nextProps)가 들어올 때 state가 `return{age:390,};` 로 변경된다.

시간의 흐름에 따라 변화하는 props에 state가 의존하는 (드문) 경우

```jsx
let i = 0;
      class App extends React,Component{
        state ={list:[]};
        render(){
          return(
            <div id="list" style={{height:100, overflow:"scroll"}}>
            {this.state.list.map((i)=>{
              return <div>{i}</div>; 
            })}
            </div>
          )
        }
    }

  componentDidMount(){
    setInterval(()=>{
      this.setState((state)=>({
          list:[...state.list, i++],
        }));
      },1000);
    }
    getSnapshotBeforeUpdate(prevProps,PrevState){
      if(prevStat.list.lengyd === this.state.list.length) return null;
      const list = document.querySelector('#list'); //리액트에서 권장하는 방법은 아니다 .
      return list.scrollHeight - list.scrollTop;
    }
    componentDidUpdate(prevProps, prevState, snapshot){
      console.log(snapshot);
      if(snapshot === null) return ;
      const list = document.querySelector('#list');
      list.scrollTop = list.scrollHeight - snapshot;
    }
  }
```

state가 변화될 때마다 화면에 내용을 추가하고, 추가된 내용이 바로 보이게 하기 위하여 스크롤의 위치를 추가된 내용에 맞춰 늘어난 세로사이즈와 동일하게 한다.

## component 에러 캐치 → componentDidCatch

componentDidCatch가 없을 때 오류가 발생했을 때는 어디에서 오류가 발생했는지에 대한 확인이 어려웠다.(화면이 표시되지 않는 등) 

error Boundaries (직접 만들어서 사용할 수 있지만 에러 바운더리라는 라이브러리를 사용한다.)→ componentDidCatch는 자기 자신에게 문제가 있을 때는 확인할 수 없다. 그렇기 떄문에 에러 바운더리를 가장 큰 부모로 두고 내부에 작성한다.

```jsx
class App extends React.component{
        state ={
          hasError : false // 기본
        };
        render(){
          if(this.state.hasError){ // 조건이 true가 된다면 실행된다.
            return <div>예상치 못한 에러가 발생했습니다.</div>
          }
          return <webService />; // 조건이 flase일 때 실행되는 리턴문 -> 정상적인 코드
        }
        componentDidCatch(error, info){
          this.setState({hasError:true}); 
        }
      }
```

componentDidCatch에 의해 랜더가 다시 실행되면서 if 내부에 작성한 리턴문이 표시된다.