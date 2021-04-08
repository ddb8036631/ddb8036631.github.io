---
title: "[모던 JavaScript 튜토리얼] 7. null 병합 연산자(??)"
date: 2021-4-8 21:57:00 +0900
categories:
  - 모던 JavaScript 튜토리얼
classes: wide
toc: true
---

null 병합 연산자 `??` 를 사용하면 여러 피연산자 중 **값이 확정되어 있는 변수**를 찾을 수 있다.

`a ?? b` 는 다음 규칙에 따라 값을 리턴한다.

- a 가 **null** 도 아니고 **undefined** 도 아니면 a
- 그 외 경우는 b

```jsx
let a = null;
let b = 1;

let x = a !== null && a !== undefined ? a : b;
let y = a ?? b;

console.log(x == y); // true
```

null 병합 연산자를 사용하지 않는 보통 방식은 위 코드에서 변수 x를 구하는 것과 같다. 하지만, ?? 연산자를 통해 변수 y 를 구하는 것처럼 코드를 단축시킬 수 있다.

<br>

# || 과 ?? 의 차이

`||` 과 `??`의 차이는 다음과 같다.

- `||` 는 첫 번째 truthy 값을 반환한다. (truthy 값이란, 불리으로 형 변환했을 때 true 인 피연산자의 값 자체를 말함)
- `??` 첫 번째 정의된(defined) 값을 반환한다.

<br>

```jsx
let height = 0;

console.log(height || 100); // 100
console.log(height && 100); // 0
```

height \|\| 100 은 height 가 0으로 falsy 하기에 100을 리턴한다.

height && 100 은 height 가 정의되어(defined) 있기 때문에 0을 리턴한다.

<br>

```jsx
const func = () => null;

let defaultValue = 0;
let result = func() ?? defaultValue;

console.log(result); // 0
```

어떤 함수를 호출해 그 결과가 `null` 이나 `undefined` 인 경우를 걸러낼 때 사용하면 좋을 것 같다.
