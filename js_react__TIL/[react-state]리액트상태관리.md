# [react-state] 리액트 상태관리

강의: 블로그 및 도서
생성일: 2022년 2월 19일 오후 4:35
수정일: 2022년 2월 19일 오후 6:10
중요도: 💜💜💜

***최종 정리***

- 

***오늘 진도***

- 

---

리액트는 단방향 바인딩이기 때문에 부모에서 자식으로 state를 props로 전달할 수 있다. (→ 자식은 부모에게 props를 직접 전달할 수 없다.)

# 상태관리 라이브러리

`redux`, `mobx`, `recoil`

### 상태관리

여러 컴포넌트 간에 데이터를 공유하고 커뮤니케이션하는 단순한 방법을 뜻한다. 읽기와 쓰기가 가능한 앱의 상태를 나타내는 앱의 상태를 나타내는 구체적인 데이터 구조를 생성한다.

- 리액트 16.8버전 이후로 함수형, 클래스형의 구분 없이 모든 컴포넌트는 상태를 가질 수 있게 되었다.

즉, 상태는 사용자의 액션에 따라 변경될 수 있는 컴포넌트의 부분을 나타내는 JS객체이다.

## redux

페이스북이 데이터의 흐름이 단방향으로 진행되는 flux 아키텍처를 발표한 뒤로

- single sourcedof truth

동일한 데이터는 항상 같은 곳에서 가지고 온다. 즉 데이터 공간이 하나라는 것을 의미한다.

- State is read-only

리액트에선 setState(메소드)를 사용해야 했다면 리덕스에선 액션(객체)를 사용해야 상태변경이 가능하다.

- change are made with pure funcitons

변경은 순수 함수로만 가능하다.

리듀서와 연관되는 개념 (Store - Action - Reducer)

### Store

스토어는 **상태가 관리되는 오직 하나의 공간**이다. 컴포넌트와 별개로 앱에서 필요한 상태를 담는 스토어가 있다.

### Action

액션은 앱에서 **스토어에 운반할 데이터**를 뜻함! (JS객체 형식으로 되어있다.)

### Reducer

액션을 스토어에 바로 전달하는 것이 아닌, 리듀서에 전달해야 한다. 리듀서가 주문을 보고 스토어의 상태를 업데이트하는 것이다.

- 액션을 리듀서에 전달하기 위해서는 **dispatch()메소드**를 사용해야 한다.

1. 액션 객체가 dispatch메소드에 전달한다.
2. dispatch(액션)을 통해 Reducer를 호출한다.
3. Reducer는 sotre를 생성한다.

→ ***데이터가 단방향으로 흘러야 하기 때문에 위와 같은 공식을 따른다!***

`mapStateToProps()`, `Redux hook`  ← 구현하는 두가지 방법!

### redux의 장점

- 순수함수를 사용하기 때문에 상태를 예측 가능하게 만든다.
- (복잡한 상태관리와 비교했을 때)유지보수가 간편하다.
- 디버깅에 유리하다.
    - action과 satate log 기록 시 → redux dev tool (크롬 확장 앱)
- 순수함수를 사용하기 때문에 테스트 붙이기 용이하다.