---
title: "[React] React.memo"
date: 2020-12-31 14:41:00 +0900
categories:
  - React
# toc: true
classes: wide
use_math: true
---

React.memo 함수는 해당 컴포넌트의 props가 변경될 때에만 새로 렌더링 되도록 할 때 사용하는 함수입니다.

```jsx
// App.js

import React, { useState } from "react";
import Title from "./Title";

function App() {
  const [count, setCount] = useState(0);
  const [count2, setCount2] = useState(0);

  const onClick = () => {
    setCount(count + 1);
  };
  const onClick2 = () => {
    setCount2(count2 + 1);
  };

  return (
    <div>
      <Title title={`현재 카운트 : ${count}`} />
      <button onClick={onClick}>증가</button>
      <button onClick={onClick2}>증가2</button>
    </div>
  );
}

export default App;
```

```jsx
// Title.js

import React from "react";

const Title = ({ title }) => {
  console.log("Title rendered");
  return <p>{title}</p>;
};

export default React.memo(Title);
```

아래 사진처럼 `증가2 버튼` 을 눌러도 해당 state를 Title 컴포넌트의 prop으로 전달하지 않기 때문에 렌더링이 되지 않는 것을 확인할 수 있습니다.

![/assets/images/React.memo.gif](/assets/images/React.memo.gif)