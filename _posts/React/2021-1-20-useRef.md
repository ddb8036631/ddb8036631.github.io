---
title: "[React] useRef"
date: 2021-1-20 12:43:00 +0900
categories:
  - react
toc: true
classes: wide
---

useRef Hook은 함수형 컴포넌트에서 ref를 쉽게 사용할 수 있도록 해 줍니다.useRef를 사용하여 ref를 설정하면, useRef를 통해 만든 객체 안의 current 값이 실제 엘리먼트를 가리킵니다.

```jsx
import React, { useRef, useCallback } from 'react';

const Average = () => {
  const inputEl = useRef(null);

  ...

  const onInsert = useCallback(() => {
    ...
    inputEl.current.focus();
    ...
  });


  return (
    <div>
      ...
      <input value={number} onChange={onChange} ref={inputEl} />
      ...
    <div>
  );
}
```

<br>

useRef는 엘리먼트를 접근하는 용도 말고도 컴포넌트 안의 지역 변수로도 사용할 수 있습니다. useRef를 통해 생성한 객체는 heap 영역에 저장되어 컴포넌트가 리렌더링 되어도 값이 유지된다는 특징이 있습니다. 또한 값이 바뀌어도 리렌더링되지 않습니다. 아래는 useRef를 지역 변수로 이용한 예시 코드입니다.

```jsx
import React, { useState, useEffect, useRef } from "react";

const RefSample = () => {
  const [text, setText] = useState("");
  const id = useRef(1);

  const printId = () => {
    console.log(id.current);
  };

  const onClick = () => {
    id.current++;
  };

  useEffect(() => {
    printId();
  });

  const onChange = (e) => {
    setText(e.target.value);
  };

  return (
    <div>
      <div>
        <input value={text} onChange={onChange}></input>
      </div>
      <b>현재 id : </b> {id.current}
      <button onClick={onClick}>증가</button>
    </div>
  );
};

export default RefSample;
```

<br>
id는 ref 객체이므로 값이 변경되어도 리렌더링이 발생하지 않다가, 상태 값인 text가 변경될 시 리렌더링이 발생하는 모습을 확인할 수 있습니다.

![/assets/images/React_useRef.gif](/assets/images/React_useRef.gif)
