---
title: "[Javascript] 객체와 배열 생성, 수정"
date: 2020-11-19 23:57:43 +0900
categories:
  - Javascript
toc: true
classes: wide
use_math: true
---

# 단축 속성명

단축 속성명(shorthand property names)은 객체 리터럴 코드를 간편하게 작성할 목적으로 만들어진 문법이다.

```jsx
const name = "Kim";
const obj = {
  age: 27,
  name,
  getName() {
    return this.name;
  },
};
```

새로 만들려고 하는 객체의 속성값이 이미 변수로 존재한다면, 해당 변수의 이름을 통해 새롭게 속성을 추가할 수 있습니다. 속성명은 변수의 이름(위의 예제에서는 name)과 같아집니다.

함수를 통해 객체를 리턴하고 싶다면, 아래와 같이 중괄호 안에 변수의 파라미터의 이름을 속성명으로 갖는 객체를 생성할 수 있습니다.

```jsx
function makePerson(age, name) {
  return { age, name };
}
```

로그 출력시에도 단축 속성명을 활용해 간단하게 작성할 수 있습니다.

```jsx
const name = "Kim";
const age = 27;

// console.log('name = ', name, ', age = ', age);
console.log({ name, age });
```

# 계산된 속성명

계산된 속성명은 객체의 속성명을 동적으로 결정하기 위해 나온 문법입니다.

아래의 코드는 각각 계산된 속성명을 사용하지 않은 코드와 사용한 코드를 나타냅니다.

```jsx
function makeObject(key, value) {
  const obj = {};
  obj[key] = value;

  return obj;
}

function makeObject(key, value) {
  return { [key]: value };
}
```

백틱(`)을 활용한 계산된 속성명을 적용한 예시입니다.

```jsx
function func(str) {
  let objArr = [];
  for (let i = 0; i < 5; i++) {
    let key = `${str}${i}`;
    objArr.push({ [key]: i });
  }

  return objArr;
}

console.log('func("test") : ', func("test"));

// func("test") : [
//     { test0: 0 },
//     { test1: 1 },
//     { test2: 2 },
//     { test3: 3 },
//     { test4: 4 }
// ]
```

# 전개 연산자

전개 연산자(spread operator)는 배열이나 객체의 모든 속성을 풀어놓을 때 사용하는 문법입니다. 다음과 같이 매개변수가 많은 함수를 호출할 때 유용합니다.

```jsx
// Math.max(1, 3, 5, 7);
const numbers = [1, 3, 5, 7];
Math.max(...numbers);
```

전개 연산자는 위 코드와 같이 매개변수가 동적으로 변할 때에도 전달할 수 있습니다.

전개 연산자는 배열이나 객체를 복사할 때도 유용하게 사용됩니다.

```jsx
const arr1 = [1, 2, 3];
const arr2 = [...arr1];

const obj1 = { age: 27, name: "Kim" };
const obj2 = { ...obj1 };

arr2.push(4);
obj2.age = 80;

console.log("arr1: ", arr1); // arr1: [1, 2, 3]
console.log("arr2: ", arr2); // arr2: [1, 2, 3, 4]
console.log("obj1: ", obj1); // obj1: { age: 27, name: 'Kim' }
console.log("obj2: ", obj2); // obj2: { age: 80, name: 'Kim' }
```

위 코드의 콘솔에 출력되는 결과에서 볼 수 있듯이, 전개 연산자를 통해 생성된 배열이나 객체는 원본 배열이나 객체와는 달리 새로운 주소에 메모리가 할당됩니다.

_하지만_, 다차원의 배열이나 객체를 복사할 땐 `얕은 복사`의 효과가 나는 것을 확인할 수 있습니다.

```jsx
const origin = [1, 2, [3, 4, 5], 6];
const copy = [...origin];

console.log("전개 연산자 사용 직후");
console.log("origin: ", origin);
console.log("copy: ", copy);
console.log();

origin[0] = 100;
console.log(
  "원본 배열의 깊이 1에 해당하는 값을 변경한 후 >>> origin[0] = 100;"
);
console.log("origin: ", origin);
console.log("copy: ", copy);
console.log();

origin[2][0] = 300;
console.log(
  "원본 배열의 깊이 2에 해당하는 값을 변경한 후 >>> origin[2][0] = 300;"
);
console.log("origin: ", origin);
console.log("copy: ", copy);
```

![https://s3-us-west-2.amazonaws.com/secure.notion-static.com/43fc8970-2049-4136-aad3-863a229ae5cf/_2020-11-19__9.10.12.png](/assets/images/arr_obj.png)
