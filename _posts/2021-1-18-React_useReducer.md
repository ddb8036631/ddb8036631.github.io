---
title: "[React] useReducer"
date: 2021-1-18 22:50:00 +0900
categories:
  - React
toc: true
classes: wide
---

`useReducer` 는useState보다 더 다양한 컴포넌트 상황에 따라 다양한 상태를 다른 값으로 업데이트해 주고 싶을 때 사용하는 Hook 입니다.

리듀서는 현재 상태, 업데이트를 위해 필요한 정보를 담은 액션(action) 값을 전달받아, 새로운 상태를 반환하는 함수입니다.

<br>

다음은 버튼을 눌러 카운트의 값을 변화시키는 예제입니다.

```jsx
import React, { useReducer } from "react";

function reducer(state, action) {
  switch (action.type) {
    case "INCREMENT":
      return { value: state.value + 1 };
    case "DECREMENT":
      return { value: state.value - 1 };
    default:
      return state;
  }
}

const Counter = () => {
  const [state, dispatch] = useReducer(reducer, { value: 0 });

  return (
    <div>
      <p>
        현재 카운터 값은 <b>{state.value}</b> 입니다.
      </p>
      <button onClick={() => dispatch({ type: "INCREMENT" })}>+1</button>
      <button onClick={() => dispatch({ type: "DECREMENT" })}>+1</button>
    </div>
  );
};

export default Counter;
```

useState 를 사용했을 때와의 차이점은 **state 변경 함수 로직이 컴포넌트에서 분리**되었다는 것입니다. 이처럼 useReducer 를 사용했을 때의 장점은 **컴포넌트 업데이트 로직을 컴포넌트 바깥으로 빼낼 수 있다는 점**입니다.

<br>

```jsx
const [state, dispatch] = useReducer(reducer, { value: 0 });
```

useReducer는 첫 번째 파라미터로 `리듀서 함수` 를, 두 번째 파라미터로는 `초기 state` 를 전달받습니다. 이 Hook은 배열을 반환하는데, 첫 번째 요소에는 state가, 두 번째 요소에는 action을 발생시키는 함수가 위치합니다.

<br>

```jsx
function reducer(state, action) {
  switch (action.type) {
    case "INCREMENT":
      return { value: state.value + 1 };
    case "DECREMENT":
      return { value: state.value - 1 };
    default:
      return state;
  }
}
```

위 함수는 리듀서 함수입니다. 리듀서 함수는 `현재 state` 와 `action 객체` 를 파라미터로 받아, action 별로 다른 작업을 state 에 수행하고, 그 결과물인 새로운 state 를 반환해주는 함수입니다.

위 코드에서 action 객체의 type 이라는 프로퍼티의 값이 INCREMENT 라면, 객체로 정의되어 있는 state 의 value 프로퍼티를 참조해 그 값을 1 증가시키고, 새로운 객체를 생성해 반환합니다. type 프로퍼티의 값이 DECREMENT 라면, 1을 감소해 새로운 state 객체를 생성 후 반환하게 됩니다.

<br>

# 여러 state를 reducer로 관리하기

아래와 같이 여러 state를 useReducer의 리듀서 함수를 활용해 한 번에 관리할 수도 있습니다.

```jsx
import React, { useReducer } from "react";

function reducer(state, action) {
  return {
    ...state,
    [action.name]: action.value,
  };
}

const Counter = () => {
  const [state, dispatch] = useReducer(reducer, { name: "", nickname: "" });
  const { name, nickname } = state;

  const onChange = (e) => {
    dispatch(e.target);
  };

  return (
    <div>
      <div>
        <input name="name" value={name} onChange={onChange} />
        <input name="nickname" value={nickname} onChange={onChange} />
      </div>
      <div>
        <div>
          <b>이름:</b> {name}
        </div>
        <div>
          <b>닉네임:</b> {nickname}
        </div>
      </div>
    </div>
  );
};

export default Counter;
```

<br>

이는 아래 코드 클래스형 컴포넌트의 `setState`  를 사용한 것과 유사한 방식으로 작업을 처리할 수 있습니다.

```jsx
state = {
	name,
	nickname
}

this.setState({
	[e.target.name]: e.target.value
});
```

<br>

한 번에 여러 state 를 처리할 수 있는 특성은 **input이 여러 개 있을 때, input의 name 별로 값을 조작**하는 데에 활용할 수 있습니다.