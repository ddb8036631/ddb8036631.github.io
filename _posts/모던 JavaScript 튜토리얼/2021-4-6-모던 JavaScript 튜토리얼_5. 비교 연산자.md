---
title: "[모던 JavaScript 튜토리얼] 5. 비교 연산자"
date: 2021-4-6 12:59:00 +0900
categories:
  - modernjavascripttutorial
classes: wide
toc: true
---

# 문자열 비교

---

```jsx
console.log("Z" > "A"); // true
console.log("Glow" > "Glee"); // true
console.log("Bee" > "Be"); // true
```

자바스크립트는 문자열을 **사전순**으로 비교한다. 사전순 비교는 **사전 뒤쪽 문자열은 사전 앞쪽 문자열보다 크다**고 판단하는 방식이다.

문자열 비교 시 적용되는 알고리즘은 다음과 같다.

1. 두 문자열의 첫 글자를 비교한다.
2. 첫 번째 문자열의 첫 글자가 다른 문자열의 첫 글자보다 크면(혹은 작으면), 첫 번째 문자열이 두 번째 문자열보다 크다고(혹은 작다고) 결론 내고 비교를 종료합니다.
3. 두 문자열의 첫 글자가 같으면 두 번째 글자를 같은 방식으로 비교합니다.
4. 글자 간 비교가 끝날 때까지 이 과정을 반복합니다.
5. **비교가 종료되었고 문자열의 길이도 같다면** 두 문자열은 **동일**하다고 결론 냅니다. 비교가 종료되었지만 두 문자열의 길이가 다르면 **길이가 긴 문자열이 더 크다**고 결론 냅니다.

<br>

위 규칙을 적용하면, `"Bee" > "Be"` 는 두 번째 인덱스 위치의 글자 e 가 같으므로 다음 글자를 비교하려고 하지만, 두 번째 문자열("Be")이 더 짧아 비교가 끝나버리므로, 길이가 더 긴 첫 번째 문자열("Bee")가 더 크다고 판단한다.

<br>

# 일치 연산자(===)

---

```jsx
console.log(0 == false); // true
console.log("" == false); // true
```

동등 연산자(==) 는 0과 false 를 구분짓지 못한다. false 를 숫자 0으로 변환해 동등 연산자를 실행하기 때문에, 위 예제는 true 가 리턴된다. 빈 문자열도 마찬가지로 0으로 변환되기에 false 와 비교해도 true 가 된다.

<br>

```jsx
console.log(0 === false); // false
```

일치 연산자(===) 는 형 변환 없이 값을 비교할 수 있다. 다른 말로 표현하자면, 일치 연산자는 값은 같아도 타입이 다르면 다르다고 판단한다.

위 예제의 비교 과정을 살펴 보자. 먼저 두 피연자 0 과 false 의 데이터 타입을 비교한다. 각각은 숫자형과 불린형으로 타입이 다르므로 값을 비교해보지도 않고 바로 false 를 리턴하게 된다.

<br>

일치 연산자는 자바스크립트의 느슨한 타입 체크를 보다 엄격하게 하도록 해준다.

<br>

# null이나 undefined 와 비교하기

---

```jsx
console.log(null === undefined); // false
```

일치 연산자를 통해 둘을 비교하면 false 가 리턴된다. null 은 null 라는 독립된 자료형을, undefined 는 undefined 라는 독립된 자료형을 갖기 때문이다.

<br>

```jsx
console.log(null == undefined); // true
```

동등 연산자를 통해 둘을 비교하면 true 가 리턴된다. 이건.. 그냥 JS 에서 특별한 예외 규칙으로 정해 두었다고 한다.

<br>

# null 과 숫자 비교하기

---

```jsx
console.log(null > 0); // false
console.log(null == 0); // false
console.log(null >= 0); // true
```

첫 번째(>)와 세 번째(≥) 비교에서 null 은 숫자 0으로 변환이 된다. 하지만 **동등 연산자**에서는 null 뿐만 아니라 undefined 숫자로 형 변환하지 않는다. 이건 동등 연산자(==) 와 다른 비교 연산자(>, <, ≥, ≤)의 동작 방식이 다르기 때문이다.

동등 연산자는 undefined 와 null 을 비교하는 경우가 아니라면, false 를 리턴해버린다.

<br>

# undefined 와 숫자 비교하기

---

```jsx
console.log(undefined > 0); // false
console.log(undefined < 0); // false
console.log(undefined == 0); // false
```

undefined 는 부등호 비교 연산에서 NaN 으로 변환된다. NaN 을 피연산자로 갖는 비교 연산자는 항상 false 를 리턴한다.

동등 연산자(==)는 위 **null 과 숫자 비교하기에서 봤듯이**, undefined 를 null 과 비교한 게 아니므로 false 를 리턴해버린다.

<br>

**주의:**  
`null` 과 `undefined` 간의 비교는, 그 둘을 동등 연산자(==)를 사용해 비교하는 경우가 아니라면 false 를 리턴한다.
{: .notice--warning}
