---
title: "[모던 JavaScript 튜토리얼] 3. 형 변환"
date: 2021-4-6 11:40:00 +0900
categories:
  - 모던 JavaScript 튜토리얼
classes: wide
toc: true
---

# 문자형으로의 변환

```jsx
let value = true;
console.log(typeof value); // boolean

value = String(value);
console.log(typeof value); // string
```

String() 함수를 호출해 명시적으로 문자열 형 변환을 변환할 수 있다.

<br>

# 숫자형으로의 변환

```jsx
console.log("100" / "2"); // 50
```

문자열의 나누기가 사용된 표현식에서는 자동으로 숫자형으로 변환된다.

<br>

```jsx
let str = "123";
console.log(typeof str); // string

let num = Number(str);
console.log(typeof num); // number
```

Number() 함수를 이용해 명시적으로 형 변환을 할 수도 있다. 폼(form)을 통해 입력 받을 땐, 문자열로 들어오므로 이렇게 명시적으로 변환해 주어야 한다.

<br>

```jsx
let age = Number("임의의 문자열 123");

console.log(age) // NaN
```

숫자 이외의 글자가 들어가 있는 물자열의 경우, 형 변환 결과는 NaN 이 된다.


<br>

숫자형으로의 변환은 다음 규칙을 따른다.

| 전달받은 값| 형 변환 후 |
| :-------- | :-------- |
| undefined | NaN |
| null | 0 |
| true and false | 1 과 0 |
| string | 문자열의 처음과 끝 공백이 제거된다. <br/> 공백 제거 후 남아있는 문자열이 없다면 0, 그렇지 않다면 문자열에서 숫자를 읽는다. <br/> 변환에 실패하면 Nan 이 반환된다. |

**주의:**  
`null` 과 `undefined` 는 숫자형으로 변환 시 그 결과가 다름을 주의하자. null 은 0 이 되고, undefined 는 NaN 이 된다.
{: .notice--warning}

<br>

# 불린형으로의 변환

불린형으로 변환 시 적용되는 규칙은 다음과 같다.

- 숫자 `0` , 빈 문자열, `null` , `undefined` , `NaN` 과 같이 직관적으로도 **비어있다고 느껴지는 값**들은 `false`
- 그 외 값들은 `true` 가 리턴된다.

참고로 "0" 을 포함한 비어있지 않은 문자열은 언제나 true 가 리턴된다.
