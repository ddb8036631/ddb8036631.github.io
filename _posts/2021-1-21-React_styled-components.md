---
title: "[React] styled-components"
date: 2021-1-21 22:39:00 +0900
categories:
  - React
toc: true
classes: wide
---

'CSS-in-JS' 방식을 이용하면, 컴포넌트를 이루는 자바스크립트 파일 안에 스타일 코드를 작성할 수도 있습니다. 부모 컴포넌트로부터 props를 전달받아 그 값에 따라 스타일을 다르게 적용할 수 있습니다.

CSS-in-JS 라이브러리 중 가장 많이 사용되는 styled-components 라이브러리에 대해서 알아보겠습니다.

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
