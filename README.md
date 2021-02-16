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
- 2개의 인자를 받는다. -> state나 dispatch (밑에 mapStateToProps 와 mapDispatchToProps 가 해당, 혹시나 첫번째 인자가 불필요할 경우 null 이라 써줘도 됨)
> getState는 state를 전달해줬고, dispatch는 store 혹은 reducer에 메세지를 전달해주는 기능을 했었다.

* mapStateToProps() 함수로 Store의 state를 Home에다 가져오게 한다
1. state -> Redux store로 부터 state를 받아온다.
2. ownProps -> component의 props

* mapDispatchToProps
- mapDispatchToProps는 connect의 두번째 인자에 해당.(mapStateToProps 가 첫번째 인자에해당)
- 인자로는 dispatch(=store.dispatch()), ownProps 를 갖는다.
- addToDo에 대한 함수를 만들고 props로 지정 (text를 required한다)

* ToDo.js
- mapStateToProps 에서 받아온 toDo props 를 Home.js에서 렌더링하였다.
- delete기능 : ToDo 를 지우기 위해선 id가 필요 
```
function mapDispatchToProps(dispatch, ownProps){

}

export default connect(null,mapDispatchToProps)(ToDo);
```
> 여기서 두번째인자인 ownProps에 우리가 원하는 내용과 ID 가 포함되어있다.
- onBtnClick 을 만들어 ownProps의 id를 가지고 delete 기능을 구현하였다.
```
onBtnClick: () => dispatch(actionCreators.deleteToDo(ownProps.id))
```

* Detail.js
- 기본적으로 ToDo에 링크를 걸어 Detail 페이지로 이동하게 한다.
- mapStateToProps() 의 ownProps로 부터 id 값을 얻어온다
- 그리고 state.find() 를 사용하여 toDo의 id 와 param의 id가 같은 것을 찾게 한다.
> .find()는 testing function에 만족하는 첫번째 요소를 반환한다.

### 4. REDUX TOOLKIT
- Redux 문제점? -> 반복되는 많은 코드양 -> Redux toolkit으로 해결
> 설치 : yarn add @reduxjs/toolkit

#### How to use Redux toolkit
1. createAction
> import { createAction } from "@reduxjs/toolkit";
- 여기선 addToDo 와 deleteToDo 를 교체하였다.
- 또한 action은 이제 함수인 **createAction** 으로 만들어지므로, action은 type과 **payload(=text)** 를 갖는다. ( delete 에서의 id 또한 payload로 교체됨)
> toolkit에선 text대신 payload를 갖는다
```
<before>
const addToDo = (text) => {
    return {
        type:ADD,
        text
    };
}

const deleteToDo = id => {
    return {
        type: DELETE,
        id : parseInt(id)
    }
}

<after>
const addToDo = createAction("ADD");
```

2. createReducer
- 첫번째 인자는 [] 이다.
> toDo를 push 해주기 위해서 빈 배열로 초기상태를 시켜주었다.

- createReducer은 state를 mutate 하기 쉽게 만들어준다. (전에는 새로운 state를 만들어서 mutate 해주었다.)
** 2 options**
1.) 새로운 state를 리턴하는 방법 (전에 쓰던 방법)
2.) state를 mutate하는 방법 ( 새로운 방법 )
* 기본적으로 state를 mutate를 하게되면 반환할 필요가 없어지고, state를 mutate하지 않으면 새로운 state를 반환해야한다.

- 여기서 .push는 state를 mutate 하기 때문에 return 하지않고, .filter은 state를 mutate 하지 않기 때문에 새로운 array를 리턴하게된다.
```
<before>
const reducer = (state =  [], action ) => {
    switch (action.type) {
        case addToDo.type:
            return [{ text: action.payload, id: Date.now()}, ...state];
        case deleteToDo.type:
            return state.filter(toDo => toDo.id !== action.payload);
        default:
            return state;
    }
    
<after>
const reducer = createReducer([],{
    [addToDo] : (state, action) => {
        state.push({ text: action.payload, id: Date.now() })
    },
    [deleteToDo] : (state, action) =>
        state.filter(toDo => toDo.id !== action.payload)
})
```

3. configureStore
- configureStore() 은 함수이고, 미들웨어와 함께 store을 생성하게 된다. ( defaults 가 추가 됨)
- Redux Developer Tools를 함께 사용할 수 있으며, state의 상태를 보다 쉽게 확인할 수 있다.
```
const store = configureStore({reducer});
```

4. createSlice
- reducer와 actions 도 생성해줌에 따라 코드를 현저히 줄여줄수 있다.
```
import { configureStore, createSlice } from "@reduxjs/toolkit";

const toDos = createSlice({
    name:'toDosReducer',
    initialState: [],
    reducers: {
        add: (state, action) => {
            state.push({ text: action.payload, id: Date.now() });
        },
        remove : (state, action) => state.filter(toDo => toDo.id !== action.payload)
    }
})

export const { add, remove } = toDos.actions;

export default configureStore({ reducer: toDos.reducer });
```
