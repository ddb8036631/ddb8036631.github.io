---
title: "[모던 JavaScript 튜토리얼] 2. 자료형"
date: 2021-4-6 02:43:00 +0900
categories:
  - 모던 JavaScript 튜토리얼
classes: wide
toc: true
use_math: true
---

자바스크립트에는 8가지의 기본 자료형이 있다.

- 숫자형 : 정수, 부동 소수점 등 숫자를 나타낼 때 사용한다. (범위 : $-2^{53}$ ~ $2^{53}$ )
- bigint : 길이 제약 없이 정수를 나타낼 수 있다.
- 문자형 : 빈 문자열이나 글자로 이뤄진 문자열을 나타낼 때 사용한다. JS에는 **단일 문자**를 나타내는 별도의 자료형이 존재하지 않음.
- 불린형 : true, false 나타낼 때 사용
- null : null 값만을 위한 독립된 자료형. null 은 알 수 없는 값을 나타냄.
- undefined : undefined 값만을 위한 독립된 자료형. undefined 는 할당되지 않은 값을 나타냄.
- 객체형 : 복잡한 데이터 구조를 표현할 때 사용
- 심볼형 : 객체의 고유 식별자를 만들 때 사용

<br>

```jsx
let message = "hello";
message = 123;
```

위 코드처럼 자바스크립트의 변수는 **자료형 관계없이 그 어떠한 데이터**일 수 있다. 따라서 변수는 어떤 순간에 문자열일 수도 있고, 다른 때엔 숫자일 수도 있다. 이처럼 **변수에 저장되는 값의 타입이 언제든지 바뀔 수 있는 언어**를 **동적 타입 언어**라고 부른다.

<br>

# 숫자형

```jsx
let n = 123;
n = 12.345;
```

숫자형은 **정수** 및 **부동소수점** 숫자를 나타낸다. 숫자형에는 일반적인 숫자 외에 `Infinity` , `-Infinity` , `NaN` 과 같은 특수 숫자 값이 포함된다.

<br>

```jsx
console.log(1 / 0); // Infinity
console.log(Infinity); // Infinity
```

0으로 나누면 `Infinity` 를 얻을 수 있다. 혹은, Infinity 를 직접 참조할 수도 있다.

<br>

```jsx
console.log("숫자가 아님" / 2); // NaN
```

`Nan` 은 숫자가 아닌 다른 타입과의 수 계산을 실행했을 때 반환된다. 자바스크립트에서는 말이 안되는 수학 연산을 하더라도, 에러를 내며 프로그램이 종료되는 대신에 NaN 을 리턴한다.

<br>

# BigInt

자바스크립트에서는 $2^{53}$ 보다 큰 값 혹은 $-2^{53}$ 보다 작은 값은 **숫자형**을 통해 나타낼 수 없다. BigInt 는 이 범위를 벗어나는 수를 표현할 때 사용되며, 아래와 같이숫자 끝에 `n` 을 붙어 표현한다.

```jsx
const bigInt = 1234567890123456789012345678901234567890n;
```

<br>

# 문자형

자바스크립트에서는 문자열을 아래 세 가지 따옴표로 묶는다.

1. 큰 따옴표(")
2. 작은 따옴표(')
3. 백틱(`)

<br>

```jsx
let name = "Kim";

console.log(`Hello, ${name}`); // Hello, Kim
console.log(`result is ${1 + 2}`); // result is 3
```

백틱은 ES6 문법의 `템플릿 문자열` 로, 위 코드처럼 **$** 와 함께 사용해 변수의 값이나 표현식의 값을 표기할 수 있다.

<br>

# 불린형

true 혹은 false 를 저장할 때 사용한다.

```jsx
let isGreater = 4 > 1;

console.log(isGreater); // true
```

불린 값은 위 코드처럼 비교 결과를 저장할 때에도 사용된다.

<br>

# null 값

null 값은 오로지 null 값만을 포함하는 별도의 자료형을 만든다.

자바스크립트의 null 은 다른 언어의 null 과 다른 성격을 띤다. null 을 다른 언어에는 **존재하지 않는 객체에 대한 참조**나 **널 포인터**를 나타낼 때 사용하는 반면, 자바스크립트에서는 다음 세 가지를 표현할 때 사용한다.

1. 존재하지 않는(nothing) 값
2. 비어 있는(empty) 값
3. 알 수 없는(unknown) 값

<br>

`let age = null;` 은 프로그래머에게 나이(age) 를 알 수 없거나, 그 값이 비어있음을 코드 상으로 보여준다.

<br>

# undefined 값

undefined 도 null 처럼 자신만의 자료형을 형성한다. `undefined` 는 값이 할당되지 않은 상태를 나타낼 때 사용한다.

<br>

```jsx
let age;

console.log(age); // undefined
```

위 코드처럼 변수는 선언했지만, 값을 할당하지 않았다면 해당 변수에 undefined 가 자동으로 할당된다.

<br>

```jsx
let age = 100;
age = undefined;
console.log(age); // undefined
```

위처럼 undefined 를 직접 할당할 수도 있지만, 이런 식으로 사용하진 말자. 변수가 **비어 있거나**, **알 수 없는 상태**라는 걸 나타내려면 null 을 사용하자. undefined 예약어는 만들어진 의미 대로, 변수 선언은 했지만 할당되지 않은 상태를 표현하는 데에 사용하자.

<br>

# 객체형

객체형을 제외한 다른 자료형은 단 한 가지의 타입만 표현할 수 있기에 원시 자료형이라고 부른다. 이와 반대로, 객체는 데이터 컬렉션이나 복잡한 개체를 표현할 때 사용한다.

<br>

# 심볼형

객체의 고유한 식별자를 만들 때 사용한다.

<br>

# typeof 연산자

typeof 연산자는 인수의 자료형을 반환한다.

typeof 연산자는 다음과 같은 두 가지 형태의 문법을 지원한다.

1. typeof x
2. typeof(x)

<br>

```jsx
console.log(typeof undefined);    // undefined
console.log(typeof 0);            // number
console.log(typeof 10n);          // bigint
console.log(typeof true);         // boolean
console.log(typeof "foo");        // string
console.log(typeof Symbol("id")); // symbol
console.log(typeof Math);         // object
console.log(typeof null);         // object

const func = () => {};
console.log(typeof func);         // function
```

- Math 는 내장 객체이므로 object 를 반환한다.
- null 은 객체는 아니지만 object 를 반환한다. 이는, **버전 하위 호환성을 유지하기 위해 오류를 수정하지 않고 남겨둔 것이다.**
- func 는 함수이다. 함수형은 따로 없다. 함수는 객체형에 속한다. 하지만, 이 규칙은 오래전에 만들어졌기 때문에 하위 호환성 유지를 위해 남겨둔 것이다.
