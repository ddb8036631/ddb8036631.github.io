---
title: "[모던 JavaScript 튜토리얼] 16. 'new' 연산자와 생성자 함수"
date: 2021-5-3 16:00:00 +0900
categories:
  - 모던 JavaScript 튜토리얼
classes: wide
toc: true
---

앞서 살펴본 객체 리터럴 방식({ })은 일회성 객체를 생성하는 방식이다. 객체는 일반적으로 하나의 규칙적인 틀이 있고, 이를 찍어내는 데에 사용한다. **new 연산자**와 **생성자 함수**를 같이 사용하면 다수의 객체를 찍어낼 수 있다.

<br>

# new 연산자의 작동 방식

```jsx
function User(name) {
    this.name = name;
    this.isAdmin = false;
}

let user = new User("Cpt Jack");

console.log(user.name);    // Cpt Jack
console.log(user.isAdmin); // false
```

new 연산자를 통해 함수를 실행하면 다음과 같은 과정을 거친다고 한다.

1. 먼저, 빈 객체를 만들어 이를 this 에 바인딩한다.
2. 함수 본문을 실행해 this 에 프로퍼티를 추가한다.
3. 완성된 this 를 리턴한다.

<br>

```jsx
function User(name) {
  // this = {};  (빈 객체가 암시적으로 만들어짐)

  // 새로운 프로퍼티를 this에 추가함
  this.name = name;
  this.isAdmin = false;

  // return this;  (this가 암시적으로 반환됨)
}
```

위에서 본 과정을 적용하면, 위 코드 주석과 같은 일들이 벌어진다. 빈 객체가 this 에 바인딩 되며, 이 객체에 프로퍼티가 추가되며, 리턴되는 과정이 암묵적으로 진행되는 것이다.

<br>

```jsx
let user = {
  name: "Jack",
  isAdmin: false
};
```

위 같이 객체 리터럴로 선언하는 방법 대신, 이제 new 연산자와 생성자 함수를 통해 일반화할 수 있게 됐다.

**📕참고:**  
모든 함수는 **생성자 함수**가 될 수 있다. new 를 붙여 생성자 함수를 호출하면, 위의 동작이 동일하게 진행된다.
{: .notice--warning}

<br>

```jsx
let user = new function() {
  this.name = "John";
  this.isAdmin = false;
};
```

위와 같이 익명 생성자 함수를 통해 객체를 생성할 수도 있다. 이렇게 코드를 작성하면 **생성자의 재사용을 막고**, **코드를 캡슐화**하는 이점이 생긴다.

<br>

# new.target

`new.target` 프로퍼티를 사용하면 함수가 new 와 함께 호출됐는지(constructor mode), 아닌지(regular mode)를 확인할 수 있다.

<br>

```jsx
function User() {
    console.log(new.target);
}

User();     // undefined
new User(); // [Function: User]
```

new 없이 호출하면 undefined 를, new를 붙여 호출하면 함수 그 자체가 출력된다.

<br>

```jsx
function User(name) {
    if (!new.target) {
        return new User(name);
    }

    this.name = name;
}

let john = User("John");
console.log(john.name); // John
```

이 프로퍼티를 사용해 new 없이 호출하면 위 코드처럼 new 키워드를 붙여주는 작업을 수행할 수 있다. `let john = User("John");` 의 명령이 실행되면, `new.target` 프로퍼티는 undefined 를 리턴할거고, 따라서 User 함수 호출을 new 를 붙여 한 번 더 호출해 새로 생성된 객체가 성공적으로 반환된다.

<br>

# 생성자와 return문

생성자 함수는 this가 암묵적으로 생성되고 이걸 리턴하기 때문에 보통 리턴문이 없다. 그런데도 명시적으로 리턴문을 작성하면 다음 규칙에 따라 리턴값이 결정된다.

- 객체를 리턴하면, this 대신 그 객체가 리턴된다.
- 원시값을 리턴하면, 리턴문은 무시된다.

<br>

```jsx
function User(name) {
    this.name = "John";

    return { name: "Godzilla" };
}

console.log(new User().name); // Godzilla
```

생성자 함수 내부에서 따로 객체를 명시적으로 리턴하는 모습을 볼 수 있다. 이러면 name 프로퍼티 값은 John 이 아니라 Godzilla 가 된다.

<br>

```jsx
function User(name) {
    this.name = "John";

    return;
}

console.log(new User().name); // John
```

위 코드도 마찬가지로 리턴을 한다. 근데 표현식이 없다. 이런 경우는 **undefined**를 리턴한다. undefined 는 원시값이므로, 위 규칙에 따라 리턴문은 무시된다. 따라서, John 이 출력된다.

<br>

# 생성자 내 메서드

```jsx
function User(name) {
    this.name = name;
    this.sayHi = function () {
        console.log("My name is " + this.name);
    };
}

let john = new User("John");
john.sayHi(); // My name is John

/*
john = {
   name: "John",
   sayHi: function() { ... }
}
*/
```

생성자 함수는 객체를 만들어주는 틀이므로, 당연히 프로퍼티로 메서드를 지정해줄 수도 있다. 생성자 함수는 이처럼 객체 내부를 자유롭게 구성할 수 있도록 도와준다.