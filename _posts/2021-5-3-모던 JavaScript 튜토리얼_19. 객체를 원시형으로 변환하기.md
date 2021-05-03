---
title: "[모던 JavaScript 튜토리얼] 19. 객체를 원시형으로 변환하기"
date: 2021-5-3 19:00:00 +0900
categories:
  - 모던 JavaScript 튜토리얼
classes: wide
toc: true
---

`obj1 + obj 2` , `obj1 - obj2` , `console.log(obj)` 등의 작업에서 객체는 자동 형 변환이 이뤄진다. 다음은 알아둘만한 규칙이다.

1. 객체는 논리 평가시 무조건 **true** 를 리턴한다.
2. 숫자형으로의 형 변환은 객체끼리 빼는 연산을 할 때나, 수학 관련 함수를 적용할 때 일어난다.
3. 문자형으로의 변환은 대개 console.log(obj) 같이 객체 내용을 출력하려고 할 때 일어난다.

<br>

# ToPrimitive

![http://dl.dropbox.com/s/75lrta1848a5j7j/%EB%AA%A8%EB%8D%98%20JavaScript%20%ED%8A%9C%ED%86%A0%EB%A6%AC%EC%96%BC_19.%20%EA%B0%9D%EC%B2%B4%EB%A5%BC%20%EC%9B%90%EC%8B%9C%ED%98%95%EC%9C%BC%EB%A1%9C%20%EB%B3%80%ED%99%98%ED%95%98%EA%B8%B0-1.png](http://dl.dropbox.com/s/75lrta1848a5j7j/%EB%AA%A8%EB%8D%98%20JavaScript%20%ED%8A%9C%ED%86%A0%EB%A6%AC%EC%96%BC_19.%20%EA%B0%9D%EC%B2%B4%EB%A5%BC%20%EC%9B%90%EC%8B%9C%ED%98%95%EC%9C%BC%EB%A1%9C%20%EB%B3%80%ED%99%98%ED%95%98%EA%B8%B0-1.png)

Abstract Operations(추상 연산) 의 한 종류이다. hint 라는 형 변환 목표가 되는 자료형을 토대로 비객체형 타입으로의 형 변환을 도와준다. 둘 이상의 원시형으로의 변환이 가능하면, preferredType 을 의미하는 부가적인 hint 정보를 통해 타입을 선택한다고 한다.

<br>

# Symbol.toPrimitive

위에서 본 ToPrimitive 아님 주의.

<br>

```jsx
obj[Symbol.toPrimitive] = function(hint) {
  // 반드시 원시값을 반환해야 합니다.
  // hint는 "string", "number", "default" 중 하나가 될 수 있습니다.
};
```

`Symbol.toPrimitive` 는 이전에 본 **시스템 심볼** 중 하나이다. 위와 같이 목표로 하는 자료형(hint)를 명시하는 데 사용된다. hint는 앞에서 살펴본 것과 같이 목표가 되는 형 변환을 도와준다. 예를 들어, hint 가 string 이면 문자열로, number 면 숫자로, default 면 어떤 자료형으로 바꿔야할 지 확신이 안드니 알아서 바꿔달라는 의미 정도로 해석하면 될 것 같다.

<br>

```jsx
function User() {
  this.name = "John";
  this.money = 1000;
  this[Symbol.toPrimitive] = function (hint) {
    console.log(`hint: ${hint}`);
    return hint == "string" ? `{name: "${this.name}"}` : this.money;
  };
}

const user = new User();

console.log(+user);      // hint: number -> 1000
console.log(user + 500); // hint: default -> 1500
```

생성자는 세 개의 프로퍼티를 추가한다. 하나는 문자열을 담고 있고, 다른 하나는 숫자를 담고 있고, 나머지 하나는 키를 toPrimitive 시스템 심볼로 하는 hint 에 따라 형 변환을 도와주는 메소드가 담겨 있다. 이렇게 toPrimitive 를 구현해놓으면 객체의 형 변환을 지정할 수 있다.

<br>

# toString 과 valueOf

`Object.prototype.toString()` 과 `Object.prototype.valueOf()` 로 형 변환을 직접 구현할 수 있다.

**toString** 은 문자열을 반환하는 방법이며, **valueOf** 는 특정 객체의 원시값을 반환하는 데 사용된다.

<br>

```jsx
function Obj(name) {
  this.prefix = "Hello, my name is ";
  this.toString = () => {
    return `${this.prefix} ${name}!`;
  };
}

const obj = new Obj("Kim");

console.log(obj.toString()); // Hello, my name is Kim!
```

생성자 Obj 함수에 `toString 메소드` 를 구현했다. 기본적으로 toString 은 **[object Object]** 를 리턴하지만, 객체가 갖고 있는 **prefix 프로퍼티**에 파라미터로 입력 받은 문자열을 이어 붙여 새로 만들어진 문자열을 리턴하도록 메소드를 오버라이딩했다.

<br>

```jsx
function Obj(n) {
  this.number = n;
  this.valueOf = () => {
    return this.number;
  };
}

const obj = new Obj(5);

console.log(obj1 + 10); // 15
```

`valueOf 메소드` 를 구현하지 않고 `obj1 + 10` 을 계산하게 되면, obj1 이 toString() 을 거쳐 [object Object] 의 문자열을 리턴하고, 문자열 10을 뒤에 덧붙인 **[object Object]10** 이 출력 된다. 하지만 위 코드는 valueOf 로 원시타입 number 로 변환되며, 숫자로 형 변환됩니다.