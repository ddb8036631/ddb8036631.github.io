---
title: "[React] useCallback"
date: 2021-1-20 12:30:00 +0900
categories:
  - React
toc: true
classes: wide
---

useCallback 은 **렌더링 성능을 최적화**해야 하는 상황에서 사용합니다. 컴포넌트 안에 정의된 함수는 컴포넌트가 리렌더링될 때마다 새로 만들어지는데, useCallback Hook을 사용하면 이미 만들어진 함수를 새로 만들지 않고 재사용할 수 있습니다. 따라서 컴포넌트의 렌더링이 자주 발생하거나, 렌더링해야 할 컴포넌트의 개수가 많아지면 이 부분을 최적화해주는 것이 좋습니다. useCallback Hook을 통해 `함수를 한 번만 만들거나, 혹은 필요시에만 새로` 만들 수 있습니다.

<br>

```jsx
const onChange = useCallback((e) => {
  setNumber(e.target.value);
}, []); // 컴포넌트가 처음 렌더링될 때만 함수를 생성합니다.

const onInsert = useCallback(() => {
  const nextList = list.concat(parseInt(number));
  setList(nextList);
  setNumber("");
}, [number, list]); // number나 list 상태가 바뀔 때에만 함수를 생성합니다.
```

위 함수 onInsert 는 함수 내부에서 number와 list라는 이름의 상태를 접근합니다. 이처럼 함수 내부에서 상태 값에 의존하는 경우는 반드시 useCallback 두 번째 파라미터에 해당 상태들을 나열해주어야 합니다.
