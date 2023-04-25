---
layout: single
title: "Redux Toolkit"
categories: React
tag: [Redux, Toolkit]
toc: true
toc_label: "목차"
toc_sticky: true
---

# Redux Toolkit 이란?

기존 Redux 구조를 좀 더 쉽게 해주는 도구들의 모음  
새로운 것이 아니고, 구조나 패러다임이 모두 Redux와 동일하다.

React에서 렌더링을 발생시키기 위해서  
불변성을 유지시켜야 하기 때문에  
push 등 불변성을 유지시키지 않는 메서드의 사용을 할 수 없으나,
Toolkit에 포함된 **<span style='color: #00ADB5'>immer</span>**
기능으로 사용이 가능해진다.

**<span style='color: #00ADB5'>Redux Devtools</span>** : 아주 강력한 React 개발툴  
React로 페이지를 구성하면서 state가 다양하고 많이 변경되는데  
크롬 확장프로그램 **Redux DevTools**을 설치하면  
Redux로 구성된 페이지에서 활성화 되며,  
실행되고 있는 부분, state의 변화, test 등 유용한 기능을 제공한다.

```javascript
yarn add @reduxjs/toolkit
```

위 코드로 설치하는 것으로 시작한다.

store를 만드는 부분, module을 만드는 부분을  
Toolkit을 사용해 아주 간소하게 작성할 수 있다.  
아래 예시 코드로 보자.

## Store 생성

기존 스토어 생성 방법이 아래 코드 였다면

```javascript
// 기본 Redux cofigStore.js

const rootReducer = combineReducers({
  counter,
});
const store = createStore(rootReducer);

export default store;
```

Toolkit을 사용하면

```javascript
// Toolkit configStore.js

const store = configureStore({
  // 객체가 들어감
  reducer: {
    // reducer가 여려개일 수도 있기에 객체로 한 번더 묶어줌
    counter,
  },
  //
});
export default store;
```

## module 생성

module 또한 상당 부분 간소화 되는데,  
기존 action value, action creator, reducer를 전부 따로 만들어줘야 했다면,  
toolkit을 사용해 입력하는 코드를 많이 줄일 수 있다.

```javascript
// 기본 Redux action value
const PLUS_ONE = "counter/PLUS_ONE";
const MINUS_ONE = "counter/MINUS_ONE";

// 기본 Redux action creator
export const plusOne = () => {
  // action 객체를 return함
  return {
    type: PLUS_ONE,
  };
};

export const minusOne = () => {
  return {
    type: MINUS_ONE,
  };
};

const initialState = {
  number: 0,
};

// 기본 Redux Reducer
Reducer;
const counter = (state = initialState, action) => {
  // console.log(state)
  switch (action.type) {
    case PLUS_ONE:
      // action 객체를 던져줄 때 액션의 type을 +1으로 해서 던져주겠다
      return {
        number: state.number + 1,
      };
    case MINUS_ONE:
      return {
        number: state.number - 1,
      };
    default:
      return state;
  }
};
```

위 코드는 Toolkit 사용전 기본 Redux 기본 양식으로, 상당히 길다.

```javascript
// Redux Toolkit을 사용한 코드
import { createSlice } from "@reduxjs/toolkit";

const initialState = {
  number: 0,
};

const counterSlice = createSlice({
  name: "conter",
  initialState,
  reducers: {
    // 여러개가 들어 갈 수 있음으로 객체로 묶음
    plusNumber: (state, action) => {
      state.number = state.number + action.payload;
    },
    minusNumber: (state, action) => {
      state.number = state.number - action.payload;
    },
  },
});
// action creator와 reducer가 전부 들어있음

export default counterSlice.reducer;
export const { plusNumber, minusNumber } = counterSlice.actions; // action = reducers
```

Toolkit을 사용한 코드가 훨씬 간소화 된 것을 볼 수 있다.  
'reducers' 안에 있는 plusNumber, minusNumber은 reducer인 동시에 action cerator가 되므로  
기존 휴먼에러 방지와 코드를 일괄관리가 편하도록 설정해  
reducer의 case에 위치했던 action value 또한 필요없어졌다.

Redux의 데이터 흐름을 파악하고 있다면 Toolkit을 사용해 코드를 간소화 하며,  
좀 더 현업에 가깝게 코드를 만들어 갈 수 있을 것 같다.
