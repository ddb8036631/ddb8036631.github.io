---
title: "[React] styled-components"
date: 2021-1-21 22:39:00 +0900
categories:
  - react
toc: true
classes: wide
---

# styled-components

'CSS-in-JS' 방식을 이용하면, 컴포넌트를 이루는 자바스크립트 파일 안에 스타일 코드를 작성할 수도 있습니다. 부모 컴포넌트로부터 props를 전달받아 그 값에 따라 스타일을 다르게 적용할 수 있습니다. CSS-in-JS 라이브러리 중 가장 많이 사용되는 styled-components 라이브러리에 대해서 알아보겠습니다.

먼저 yarn 을 통해 styled-components 라이브러리를 설치합니다.

```bash
$ yarn add styled-components
```

<br>

다음은 styled-components 를 활용해 컴포넌트 내부에서 작성된 스타일 코드입니다. ES6의 tagged template literal 문법을 이용해 백틱(`) 내부에 스타일 코드를 작성합니다.

```jsx
import React from "react";
import styled, { css } from "styled-components";

const Box = styled.div`
  background: ${(props) => props.color || "blue"};
  padding: 1rem;
  display: flex;
`;

const Button = styled.button`
  background: white;
  color: black;
  border-radius: 4px;
  padding: 0.5rem;
  display: flex;
  align-items: center;
  justify-content: center;
  box-sizing: border-box;
  font-size: 1rem;
  font-weight: 600;

  &:hover {
    background: rgba(255, 255, 255, 0.9);
  }

    ${(props) =>
      props.inverted &&
      css`
        background: none;
        border: 2px solid white;
        color: white;

        &:hover {
          background: white;
          color: black;
        }
      `};

    & + button {
      margin-left: 1rem;
    }
  }
`;

const StyledComponent = () => {
  return (
    <Box>
      <Button>안녕하세요</Button>
      <Button inverted={true}>테두리만</Button>
    </Box>
  );
};

export default StyledComponent;
```

![/assets/images/React_styled-components1.gif](/assets/images/React_styled-components1.gif)

<br>

# Tagged Template Literals

![/assets/images/React_styled-components2.png](/assets/images/React_styled-components2.png)

<br>

위 코드처럼 템플릿 문자열을 그대로 콘솔 상에 출력해보면, 객체는 toString을 거쳐 [object Object]로, 함수는 자체가 문자열로 출력되는 모습을 확인할 수 있습니다. 일반적으로 함수를 호출할 때 소괄호안에 파라미터를 전달해서 호출하는 기존 방식에 반해, Tagged Template Literal은 아래 코드와 같이 소괄호를 제외하고 백틱으로 파라미터를 전달합니다.

```jsx
tagged`hello ${{foo: 'bar'}} ${() => 'world'}!`;
```

이렇게 전달하게 되면, 객체와 함수는 원본 값을 그대로 추출해낼 수 있게 됩니다.

<br>

# 스타일링된 엘리먼트 만들기

styled-components 를 사용하여 스타일링된 엘리먼트를 만들 때는, 파일 상단에서 styled를 불러오고 `styled.태그명` 형식에 `Tagged Template Literals` 을 사용하여 구현합니다.

```jsx
import styled from 'styled-components';

const MyComponent = styled.div`
  font-size: 2rem;
`;
```

<br>

styled 함수를 이용해 스타일을 적용시키면 리액트 컴포넌트가 생성됩니다. 아래와 같이 일반 컴포넌트처럼 사용할 수 있습니다.

```jsx
<MyComponent>Hello</MyComponent>
```

<br>

또한 태그명이 유동적이거나, 특정 컴포넌트 자체에 스타일링을 해 주고 싶다면 다음과 같이 styled 의 파라미터로 해당 태그명 혹은 컴포넌트 이름을 전달하는 형태로 구현할 수 있습니다.

```jsx
const MyInput = styled('input')`
  background: gray;
`;

const StyledLink = styled(Link)`
  color: blue;
`;
```