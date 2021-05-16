---
title: "[모던 JavaScript 튜토리얼] 12. 객체"
date: 2021-4-9 22:49:00 +0900
categories:
  - modernjavascripttutorial
classes: wide
toc: true
---

# 객체 생성 방법
---

```jsx
let user = new Object(); // '객체 생성자' 문법
let user = {};  // '객체 리터럴' 문법
```

객체는 위 코드처럼 객체 생성자를 사용해 만들 수도 있으며, 객체 리터럴을 사용해 만들 수도 있다.

<br>

# 프로퍼티
---

객체 내부에는 `키: 값` 쌍으로 구성된 프로퍼티가 위치한다. `키` 는 **문자형**, `값` 은 **모든 자료형**이 가능하다.

## 프로퍼티 삭제

```jsx
delete user.age;
```

delete 연산자를 통해 객체 내부의 프로퍼티를 지울 수 있다.

<br>

## 프로퍼티의 키(이름)

```jsx
let user = {
  name: "John",
  age: 30,
  "likes birds": true  // 복수의 단어는 따옴표로 묶어야 합니다.
};
```

여러 단어를 조합해 키를 만드려면, 따옴표가 필요하다.

<br>

```jsx
console.log(user["likes birds"]); // true
```

위의 likes birds 프로퍼티의 값을 참조하려면, 위와 같이 `대괄호 표기법` 을 사용해야 한다.

<br>

## 대괄호 표기법

```jsx
let key = "likes birds";

console.log(user[key]); // true
```

대괄호 표기법 안에 직접 문자열을 작성하지 않고 변수를 줄 수도 있다.

<br>

# 계산된 프로퍼티
---
```jsx
let fruit = prompt("어떤 과일을 구매하시겠습니까?", "apple");

let bag = {
  [fruit]: 5, // 변수 fruit에서 프로퍼티 이름을 동적으로 받아 옵니다.
};

alert( bag.apple ); // fruit에 "apple"이 할당되었다면, 5가 출력됩니다.
```

`계산된 프로퍼티` 는 객체 리터럴 안의 프로퍼티가 **대괄호로 묶여 있는 프로퍼티**를 말한다. 위 코드처럼, 동적으로 프로퍼티 키를 받아올 수 있다.

<br>

# 단축 프로퍼티
---
```jsx
function makeUser1(name, age) {
  return {
    name: name,
    age: age,
  };
}

function makeUser2(name, age) {
  return {
    name,
    age,
  };
}

let user = makeUser2("John", 30);
/*
user = {
    name: "John",
    age: 30,
}
*/
```

위 코드의 함수 makeUser1 과 makeUser2 는 같은 동작을 수행한다.프로퍼티 키와 값이 변수의 이름과 같다면, 생략할 수 있다.

<br>

# for...in 반복문
---
```jsx
for(let key in user) {
    console.log(key);
    console.log(user[key]);
}
```

`for...in` 으로 객체를 순회하며 키를 받아올 수 있다.

<br>

# 객체 정렬 방식
---
```jsx
let items = {
    "1": "컴퓨터",
    "5": "노트북",
    "stock": 10,
    "2": "마우스",
    "manager": "kim",
}

for(let key in items) {
    console.log(key + ", " + items[key]);
}
/*
1, 컴퓨터
2, 마우스
5, 노트북
stock, 10
manager, kim
*/
```

객체는 기본적으로 키의 숫자를 오름차순으로, 키가 문자열이라면 객체에 추가한 순서대로 정렬된다.