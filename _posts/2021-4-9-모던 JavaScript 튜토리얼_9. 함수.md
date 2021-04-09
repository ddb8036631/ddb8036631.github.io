---
title: "[모던 JavaScript 튜토리얼] 9. 함수"
date: 2021-4-9 20:51:00 +0900
categories:
  - 모던 JavaScript 튜토리얼
classes: wide
toc: true
---

# 파라미터의 default value 설정하기

```jsx
function showMessage(from, text) {
  console.log(from + ": " + text);
}

showMessage("Ann"); // Ann: undefined
```

위 예시처럼 매개 변수에 값을 전달하지 않으면 그 값은 `undefined` 가 된다.

<br>

```jsx
function showMessage(from, text = "no text given") {
  console.log(from + ": " + text);
}

showMessage("Ann"); // Ann: no text given
```

값을 전달하지 않아도 그 값이 undefined 가 되지 않게 하려면, 위와 같이 디폴트 값을 설정해주면 된다. `=` 를 붙인 뒤, 설정하고자 하는 기본 값을 적어주면 된다.

<br>

```jsx
function showMessage(from, text = anotherFunction()) {
  console.log(from + ": " + text);
}

function anotherFunction() {
  return "default text";
}

showMessage("Ann"); // Ann: default text
showMessage("Ann", "Hello"); // Ann: Hello
```

디폴트 값을 지정하는 부분에서 **함수를 호출할 수도 있다.** 즉, 파라미터를 전달 안하면, 알아서 설정해 둔 함수를 호출하고, **그 결과를 기본 값으로 설정**하게 된다.

<br>

```jsx
function showMessage(text) {
  text = text || "empty text";
  console.log(text);
}

showMessage(); // empty text
showMessage("Hello"); // Hello
```

위와 같이 `||` 연산자를 통해 디폴트 값을 설정할 수도 있다. 앞서 살펴본 것처럼, **\|\|** 연산자의 **첫 번째 truthy 값을 리턴**하는 속성을 적용한 것이다. 첫 번째 피연산자인 **undefined** 는 **falsy**한 값이므로, 두 번째 피연산자인 **"empty text"**가 리턴되고, 이 값이 변수 text에 대입된다.

<br>

```jsx
function showCount(count) {
  console.log(count ?? "unknown");
}

showCount(0); // 0
showCount(null); // unknwon
showCount(); // unknown
```

null 병합 연산자 `??` 를 활용해도 된다. **\|\|** 연산자는 **0**을 falsy로 처리하기 때문에, **\|\|** 을 사용했다면 뒤에 적힌 **"unknown"**이 출력됐을 것이다. `??` 연산자를 사용한다면, 0을 체크하는 별다른 조건문을 작성하지 않고도 위처럼 0을 걸러낼 수도 있다.

<br>

# 리턴값

```jsx
function doNothing1() {}

function doNothing2() {
  return;
}

console.log(doNothing1()); // undefined
console.log(doNothing2()); // undefined
```

return 문이 없거나, return 지시자만 있는 함수는 `undefined` 를 리턴한다.
