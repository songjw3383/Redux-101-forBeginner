# Redux-101-forBeginner

## 0212~
### 0.Introduction
* Redux란? 기본적으로 자바스크립트 어플리케이션들의 상태(state)를 관리하는 방법
> Angular,Vue.js,Vanilla JS 등 JS 언어내의 여러곳에서 사용가능
* 요구사항
> VSC, Node.js, Github, Chrome, NPM

### 1. PURE REDUX: COUNTER
* 카운트기능 구현을 Vanila JS 에서 Redux로 구현하는법을 배움.
```
<Vanila JS>
const add = document.getElementById("add");
const minus = document.getElementById("minus");
const number = document.querySelector("span");

let count = 0;

number.innerText = count;

const updateText = () => {
  number.innerText = count;
}

const handleAdd = () => {
  count = count +1;
  updateText();
};

const handleMinus = () => {
  count = count -1;
  updateText();
};

add.addEventListener("click", handleAdd);
minus.addEventListener("click", handleMinus);
```

1. createStore 란?
> store은 data를 넣는 곳.
* State 란?
> application 에서 바뀌는 data를 말한다.
* 데이터가 바뀌는 곳?
> let count = 0;
2. createStore은 reducer이란 함수를 필요로하고 reducer은 data를 수정하기위해 필요로 한다.
> const reducer = () => {};
* reducer가 리턴하는 값은 reducer.getState()로 받을 수 있다.
> 즉, reducer나 modifier이 return 하는 건 application의 data가 되는 것이다.
* 여기선 reducer를 countModifier로 rename해주었고, count(state)를 0 으로 초기값을 주었다.
3. Action : count +, - 기능을 할 수 있게 수정하기위해서 필요
> action은 redux에서 함수를 부를때 쓰는 2번째 인자에 해당된다. 또한 action은 object여야 하고 string이 될 수 없다.
```
const countModifier = (state = 0,action) => {
  return state;
};
```
* Reducer에게 Action을 보내는 방법 ?
> store.dispatch({key : value});

4. Subscribe : store 안에 있는 변화들을 알 수 있게 해준다.
> countStore.subscribe();

#### Conclusion : Redux Counter
```
import { createStore } from "redux";

const add = document.getElementById("add");
const minus = document.getElementById("minus");
const number = document.querySelector("span");

number.innerText = 0;

const ADD = "ADD"
const MINUS = " MINUS"

const countModifier = (count = 0,action) => {
  switch (action.type){
    case ADD :
      return count + 1
    case MINUS : 
      return count - 1
    dfault:
      return count;
  }
};

const countStore = createStore(countModifier);

const onChange = () => {
  number.innerText = countStore.getState();
}

countStore.subscribe(onChange);

const handleAdd = () => {
  countStore.dispatch({ type: ADD });
};

const handleMinus = () => {
  countStore.dispatch({ type: MINUS });
};

add.addEventListener("click", handleAdd);
minus.addEventListener("click", handleMinus);
```
### 2. PURE REDUX: TO DO LIST
1. toDo 기능을 vanilaJS로 구현해보고, Redux로 구현하기위해 셋업을 해준다
> store, action, dispatch 등등..
2. State Mutation 에 대해

* Redux 의 3 Principles
1.) state는 read-only!
> store를 수정할 수 있는 유일한 방법은 action을 보내는 방법뿐.
2.) state를 mutate하지 말아야 한다.
> state를 mutating하는 대신에 새로운 state objects를 리턴 해야한다.
** Mutation? **
- 기존에 state에서 추가시켜 반환하는것이아닌, 새로운 state를 만들고 그 새로운 state를 리턴해야한다!

* addToDo 와 DeleteToDo 함수를 만들고 쪼개기.

* splice() 는 배열을 바꾸기 때문에 사용하지 않을것이다. 여기선 새로운 배열을 만드는 **filter()** 를 사용함.
> filter()는 기존 배열의 element들을 가지고 조건에 맞는 element들로 구성된 새로운 배열을 만드는 기능을 수행한다.
* 여기선 toDo 에서 toDo.id 와 action.id 가 같지 않을때 라는 조건을 적었다.
> state.filter(toDo => toDo.id !== action.id);

#### 정리 :
1) dispatchAddToDo : 오로지 action을 dispatch하기 위한 용도
2) addToDo, deleteToDo : 그저 object를 return 하는 것 뿐 , 그리고 이들이 rturn 되는 것들은 dispatch를 위해 이용됨.
3) state를 mutate하고 있지 않다는 점. 새로운 state를 만들었다. 즉, toDos array를 수정하지 않았다.  -> ADD_TODO
4) DELETE_TODO 에서는 state.filter()를 사용하였다.
5) toDo의 변화에 맞게 리스트를 repainting 하였다.
> repaint한 방법은 새로운 todo가 생기면 리스트의 전체를 비우고 state에 있는 각각의 toDo들을 이용해서 다시 새로운 리스트를 만들었다.
6) dispatchDeleteToDo 는 action creator를 사용해 action을 dispatch

### 3. React Redux
* React를 사용하여 구현하기 위한 setup
> index.js / App.js / index.html
* npm add react-redux react-router-dom
> home.js / detail.js 페이지 구현
* src 폴더안에 store.js 만들기
* subscribe 에 redux-react가 꼭 필요하다!
> 변경사항이 있을때 모든게 다시 reder 되기를 원하기 때문
* 또한 store를 index에 연결하기 위하여 코드를 수정하였다.
```
<index.js>
...
ReactDOM.render(
    <Provider store={store}>
        <App />
    </Provider>,
     document.getElementById("root")
);
```
* Connect()
- components들을 store에 연결시켜주는 기능을 수행
> import { connect } from "react-redux";
- 2개의 인자를 받는다. -> state나 dispatch
> getState는 state를 전달해줬고, dispatch는 store 혹은 reducer에 메세지를 전달해주는 기능을 했었다.
- mapStateToProps() 함수로 Store의 state를 Home에다 가져오게 한다
1. state -> Redux store로 부터 state를 받아온다.
2. ownProps -> component의 props
