---
title: "[React] Element"
date: 2020-12-5 14:19:00 +0900
categories:
  - React
toc: true
classes: wide
use_math: true
---

# React.createElement 메소드

```jsx
<MyButton color="blue" shadowSize={2}>
  Click Me
</MyButton>
```

리액트는 위와 같이 JSX 문법으로 작성된 코드를 `createElement` 메소드를 통해 아래와 같이 컴파일해 변환해줍니다.

```jsx
React.createElement(MyButton, { color: "blue", shadowSize: 2 }, "Click Me");
```

리액트 엘리먼트를 이해하면, 리액트가 내부적으로 어떻게 동작하는 지 쉽게 이해할 수 있습니다. 리액트는 렌더링 성능을 위해 virtual DOM(가상 돔)을 사용합니다. 브라우저에서 돔을 변경하는 것은 비교적 오래 걸리는 일입니다. 따라서, 빠른 렌더링을 위해 돔의 변경을 최소화해주어야 합니다.

리액트는 메모리에 가상 돔을 올려 놓고, 이전과 이후의 가상 돔을 비교합니다. 그러고 나서는 변경된 부분만 실제 돔에 반영을 합니다.

아래는 jsx 문법을 이용해 요소를 하나 만들어 출력해본 결과입니다.

```jsx
const element = (
  <a key="key1" style={{ width: 100 }} href="https://google.com">
    click here
  </a>
);
console.log(element);

export default function App() {
  return <></>;
}
```

![/assets/images/React_element.png](/assets/images/React_element.png)

요소의 type이 a로 a 태그임을 나타내고, 다른 속성들도 props를 통해 확인할 수 있습니다.

다음으로 요소에 태그가 아닌 컴포넌트를 출력해보겠습니다.

```jsx
const element = <Counter value="0"></Counter>;
console.log(element);

export default function App() {
  return <></>;
}
```

![/assets/images/React_element2.png](/assets/images/React_element2.png)

type에 컴포넌트의 함수 명이 적혀있습니다. 이 함수를 이용해서 리액트는 렌더링을 위한 정보를 얻게 됩니다. 리액트 엔진이 컴포넌트 함수를 실행하게 되고, props를 얻을 수 있습니다.

또한 리액트 요소는 불변 객체이기 때문에 내부 값을 변경하려고 하면 에러가 발생합니다.

# 컴포넌트 내부 값 변경

다음은 컴포넌트 내부의 값이 변경될 때 화면에 어떻게 적용되는 지 예제를 통해 확인해보겠습니다.

```jsx
import React, { useState, useEffect } from "react";

export default function App() {
  const [seconds, setSeconds] = useState(0);
  useEffect(() => {
    setTimeout(() => {
      setSeconds((prev) => prev + 1);
    }, 1000);
  });

  return (
    <div>
      <h1>안녕하세요.</h1>
      <h1>지금까지 {seconds} 초가 지났습니다.</h1>
    </div>
  );
}
```

![/assets/images/React_element3.gif](/assets/images/React_element3.gif)

이처럼 개발자 도구의 Elements 탭의 하이라이트되는 부분을 통해 DOM에서의 변경되는 부분을 확인할 수 있습니다.

# key 변경

DOM 엘리먼트의 key 값을 변경시키면, 해당 엘리먼트는 삭제되었다가 다시 추가됩니다.

![/assets/images/React_element4.gif](/assets/images/React_element4.gif)

엘리먼트를 열어서 확인해보려고 해도 곧장 접히는 것을 확인할 수 있는데, 이는 엘리먼트가 제거되었다가 다시 만들어지기 때문입니다.

위와 같은 현상은 컴포넌트의 key 값을 변경시켜도 동일하게 작동합니다.

이렇게 컴포넌트가 삭제되는 것을 `unmount` 라고 하고, 컴포넌트가 추가되는 것을 `mount` 라고 합니다. 컴포넌트의 key 값을 변경하면, 컴포넌트는 unmount 와 mount 를 반복하는 것을 확인할 수 있습니다.
