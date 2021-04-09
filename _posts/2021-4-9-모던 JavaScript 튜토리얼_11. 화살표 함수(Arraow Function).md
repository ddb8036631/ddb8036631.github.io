---
title: "[모던 JavaScript 튜토리얼] 11. 화살표 함수(Arrow Function)"
date: 2021-4-9 21:48:00 +0900
categories:
  - 모던 JavaScript 튜토리얼
classes: wide
toc: true
---

# 화살표 함수

---

화살표 함수는 **ES6** 에서 새로 추가된 문법이다. `⇒` 를 사용해 함수를 간단히 나타내며, 상황에 따라 중괄호와 소괄호도 생략해 사용할 수 있다.

<br>

```jsx
let sum1 = (a, b) => a + b;

let sum2 = function (a, b) {
    return a + b;
};

console.log(sum1(1, 2)); // 3
```

함수 sum1 은 함수 표현식으로 작성된 sum2의 화살표 함수 버전이다. 함수 sum1 은 아래 규칙으로 만들어졌다.

1. **function 예약어를 생략**하고 `⇒` 를 사용함으로써 함수임을 명시한다.
2. `a + b;` 라는 표현식(statement)이 하나이면서, 그 결과가 동시에 **리턴 값**이므로 **return 지시자 및 중괄호를 생략**했다.

<br>

# 본문이 여러 줄인 화살표 함수

---

```jsx
let sum = (a, b) => {
    let result = a + b;
    return result;
};

console.log(sum(1, 2));
```

함수 몸체가 여러 표현식으로 작성되어 있다면 중괄호를 써야 한다. 동시에 리턴이 필요하다면 return 지시자도 써야 된다.