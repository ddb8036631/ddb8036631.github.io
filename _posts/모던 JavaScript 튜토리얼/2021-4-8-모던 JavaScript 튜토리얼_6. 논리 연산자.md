---
title: "[모던 JavaScript 튜토리얼] 6. 논리 연산자"
date: 2021-4-8 17:30:00 +0900
categories:
  - modernjavascripttutorial
classes: wide
toc: true
---

# OR 연산자와 피연산자가 여러 개인 경우

아래 규칙을 따른다.

- 피연산자를 불린 값으로 형 변환한 값이 true 이면 변환 전 값을 리턴한다.
- 마지막 피연산자까지 false 라면, 마지막 피연산자가 리턴된다.

```jsx
console.log(0 || 1 || 2);        // 1
console.log(0 || false || null); // null
```

<br>

# AND 연산자와 피연산자가 여러 개인 경우

아래 규칙을 따른다.

- 피연산자를 불린 값으로 형 변환한 값이 false 이면 변환 전 값을 리턴한다.
- 마지막 피연산자까지 true 라면, 마지막 피연산자가 리턴된다.

```jsx
console.log(1 && 0 && 2);    // 0
console.log(1 && "0" && -1); // -1
```

**참고:**  
`alert( alert(1) && alert(2) );` 의 결과는?  
얼럿 창엔 **1**과 **undefined** 가 차례로 출력된다. alert 함수는 undefined 를 리턴하고, undefined는 **falsy** 한 값이기에 alert(2) 는 호출조차 되지 않는다.
{: .notice--warning}

<br>

# NOT 연산자 두 번

NOT 연산자는 피연산자를 불린형으로 변환한다.

```jsx
console.log(!0);                  // false
console.log(!null);               // true
console.log(!true);               // false
console.log(!"non-empty string"); // false
```

<br>

NOT 연산자를 두 번 사용해 불린 형으로 변환할 수 있다. 이는 Boolean() 함수를 통한 형 변환과 기능적으로 같다.

```jsx
console.log(!!"non-empty string");          // true
console.log(Boolean(!!"non-empty string")); // true
```
