---
title: "[모던 JavaScript 튜토리얼] 4. 연산자"
date: 2021-4-6 12:17:00 +0900
categories:
  - modernjavascripttutorial
classes: wide
toc: true
---

# 거듭제곱 연산자

```jsx
console.log(2 ** 2); // 4
console.log(2 ** 3); // 8
console.log(2 ** 4); // 16
```

`a ** b` 연산은 a 를 b 번 곱한 값이 반환한다.

<br>

```jsx
console.log(4 ** (1/2)); // 2
console.log(8 ** (1/3)); // 2
```

뿐만 아니라, 거듭제곱 연산자는 정수가 아닌 수에서도 동작한다. 이를 사용해 **제곱근**을 구할 수도 있다.

<br>

# 단항 연산자 + 를 이용해 숫자형으로의 변환

```jsx
console.log(+true); // 1
console.log(+"");   // 0
```

`+` 는 이항 연산자 뿐 아니라 단항 연산자로도 사용될 수 있다. 피연산자가 **숫자가 아닌 경우**, + 를 **앞에** 붙이면(뒤에 붙이면 이항 연산자로 인식) 숫자형으로의 변환이 일어난다. **Number()** 함수를 통해 변환하는 것과 같은 효과를 낸다.

<br>

```jsx
let apples = "2";
let oranges = "3";

console.log(+apples + +oranges); // 5
```

Number() 함수를 이용한 형 변환보다 코드 길이는 짧다. 근데 좀 그렇다.

<br>

# 할당 연산자

할당 연산자 우선순위는 아~~주 낮다. 그래서 왠만한 연산자를 다 거친 결과 값을 변수에 할당하는 것이다.

<br>

```jsx
let a = 1;
let b = 2;

let c = 3 - (a = b + 1);

console.log(a); // 3
console.log(c); // 0
```

할당 연산자는 다른 연산자들과 같이 **값을 반환**한다. 예를 들어, `x = value` 라는 식에서 value 가 x에 쓰여지는 것에 그치지 않고, 반환도 된다!

<br>

```java
// java
int a = 1;
int b = 2;
int c = 3 - (a = b + 1);

System.out.println(a); // 3
System.out.println(c); // 0
```

근데 이건 자바에서도 그렇더라. 몰랐다.

<br>

# 쉼표 연산자

```jsx
let a = (1 + 2, 3 + 4);

console.log(a); // 7
```

`쉼표 연산자` 는 여러 표현식을 한 줄에 작성할 수 있게 해준다. 쉼표로 구분된 표현식들이 각각 수행되지만, 마지막 표현식의 결과만 반환된다. 따라서 위 코드에서는 변수 a 에 3 + 4의 결과인 7이 할당된다.

<br>

```jsx
for (a = 1, b = 3, c = a * b; a < 10; a++) {
	...
}
```

위처럼 for 문의 초기식에서 쉼표 연산자를 종종 쓴다.