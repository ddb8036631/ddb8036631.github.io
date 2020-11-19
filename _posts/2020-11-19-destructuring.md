---
title: "비구조화"
date: 2020-11-19 23:57:43 +0900
categories:
  - Javascript
toc: true
toc_sticky: true
---

# 비구조화

## 배열 비구조화

배열 비구조화는 배열의 여러 속성값을 변수로 쉽게 할당할 수 있는 문법입니다.

다음은 배열 비구조화를 통해 배열의 각 원소들을 변수에 담는 코드입니다.

```jsx
const arr = [1, 2];
const [a, b] = arr;
console.log(a); // 1
console.log(b); // 2
```

배열 비구조화 시 다음과 같이 기본값을 정의할 수 있습니다.

```jsx
const arr = [1];
const [a = 10, b = 20] arr;
console.log(a); // 1
console.log(b); // 20
```

배열 비구조화를 이용하면, 임시 변수를 사용하지 않고 두 변수를 손쉽게 swap 할 수 있습니다.

```jsx
let a = 1;
let b = 2;

[a, b] = [b, a];

console.log(a); // 2
console.log(b); // 1
```

쉼표를 통해 일부 요소를 건너뛸 수 있습니다.

```jsx
const arr = [1, 2, 3, 4, 5];
const [a, , , , c] = arr;
console.log(a); // 1
console.log(c); // 5
```

쉼표 개수만큼을 제외한 나머지를 새로운 배열로 만들 수도 있습니다. 마지막에 ...와 함께 변수명을 입력하면, 나머지 모든 속성값이 새로운 배열로 만들어집니다. 나머지 원소가 존재하지 않으면, 빈 배열이 만들어집니다.

```jsx
const arr = [1, 2, 3];
const [first, ...rest1] = arr;
console.log(rest1); // [2, 3]

const [a, b, c, ...rest2] = arr;
console.log(rest2); // []
```

## 객체 비구조화

객체 비구조화는 객체의 여러 속성값을 변수로 쉽게 할당할 수 있는 문법입니다. 객체 비구조화에서 순서는 무의미합니다. 아래 샘플 코드에서 주석되어진 부분으로 사용해도 결과는 같습니다.

```jsx
const obj = { age: 27, name: "Kim" };

const { age, name } = obj;
// const { name, age } = obj;

console.log(age); // 27
console.log(name); // Kim
```

객체 비구조화에서는 속성명에 별칭을 사용할 수 있습니다. 하지만 속성명에 별칭을 주게 되면, 기존 속성명으로 더이상 참조가 불가능합니다.

```jsx
const obj = { age: 27, name: "Kim" };
const { age: alias, name } = obj;

console.log(alias); // 27
console.log(age); // 참조 에러
```

객체 비구조화에서도 기본값을 정의할 수 있습니다.

```jsx
const obj = { age: undefined, name: null, grade: "A" };
const { age = 0, name = "noName", grade = "F" } = obj;

console.log(age); // 0
console.log(name); // null
console.log(grade); // A
```

obj의 age 속성은 undefined로, 할당은 되었지만 초기화가 되지 않은 상태라 값이 없습니다. name 속성은 null 값으로 초기화가 되어 있고, grade 속성은 문자 A로 초기화 되어있습니다.

따라서 age는 디폴트 0 값으로 설정되고, name과 grade는 기존에 갖고 있는 값이 출력됩니다.

객체 비구조화에서도 나머지 속성들을 별도의 객체로 생성할 수 있습니다.

```jsx
const obj = { age: 27, name: "Kim", grade: "A" };
const { age, ...rest } = obj;
console.log(rest); // { name: 'Kim', grade: 'A' }
```

for 문에서 객체를 원소로 갖는 배열을 순회할 때, 객체 비구조화를 사용하면 편리합니다.

```jsx
const people = [
  { age: 27, name: "Kim" },
  { age: 20, name: "Lee" },
];

for (const { age, name } of people) {
  console.log(age);
  console.log(name);
}
```

## 객체와 배열이 중첩된 경우

```jsx
const obj = { name: "Kim", mother: { name: "Lee" } };

const {
  name,
  mother: { name: motherName },
} = obj;

console.log(name); // Kim
console.log(motherName); // Lee
console.log(mother); // 참조 에러
```

위의 코드의 비구조화의 결과로 motherName 이라는 이름의 변수만 생성됩니다.
