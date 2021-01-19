---
title: "[Javascript] map(), reduce()"
date: 2021-1-18 22:33:00 +0900
categories:
  - Javascript
toc: true
classes: wide
---

# map()

```jsx
array.map((currentValue, index, array) => { return someValue });
```

배열을 순회하며 callback 함수 내부에서 currentValue 를 조작하고, 이로 인해 새로운 요소인 someValue 가 만들어집니다. map() 함수는 이처럼 각 요소를 조작해 만들어진 someValue 들로 구성된 **새로운 배열**을 생성한 후에 이를 반환합니다.

예를 들어, 아래와 같은 기존 배열 각 요소를 제곱하여 새로운 배열을 만드는 작업을 map() 함수를 이용해 구현할 수 있습니다.

```jsx
const array = [1, 2, 3, 4, 5];

console.log(array.map((value, index) => return value * value)); // [1, 4, 9, 16, 25]
```

<br>

# reduce()

```jsx
const reducer = (accumulator, currentValue) => accumulator + currentValue;
array.reduce(reducer);
```

배열을 순회하며 주어진 reducer callback 함수를 실행하고, 하나의 결과값을 반환합니다. 누산기와 각 배열 요소에 대해 반복적인 작업을 수행할 때 사용됩니다.

예를 들어, 아래와 같이 배열의 각 요소를 순회하며 모든 요소들의 값들을 더하는 누산기를 reduce() 함수를 이용해 구현할 수 있습니다.

```jsx
const array = [1, 2, 3, 4, 5];

console.log(array.reduce((a, b) => a + b)); // 15
```