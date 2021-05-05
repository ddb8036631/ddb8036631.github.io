---
title: "[모던 JavaScript 튜토리얼] 18. 심볼형"
date: 2021-5-3 18:00:00 +0900
categories:
  - modernjavascripttutorial
classes: wide
toc: true
---

**심볼**은 ES6에서 새롭게 추가된 7번째 타입으로, 변경 불가능한 원시 타입 값이다. 심볼은 주로 이름의 충돌 위험이 없는 **유일한 객체의 프로퍼티 키**를 만들기 위해 사용한다.

자바스크립트의 객체 프로퍼티의 키는 문자형과 심볼형만 허용된다.

<br>

# 심볼

```jsx
let id1 = Symbol();
let id2 = Symbol("id");
```

심볼은 앞서 말한 것처럼 유일한 식별자를 만들고 싶을 때 사용한다. Symbol() 을 사용해 심볼값을 만들 수 있다. 파라미터로 심볼을 설명하는 심볼 이름을 달 수 있다.

<br>

```jsx
let symbol1 = Symbol("id");
let symbol2 = Symbol("label");

console.log(symbol1.toString()); // Symbol(id)
console.log(symbol2.toString()); // Symbol(label)
```

심볼값은 다른 자료형으로 자동 형 변환이 되지 않는다. 따라서, 위와 같이 `Object.prototype.toString()` 를 통해 **문자열**로 형 변환해줄 수 있다.

<br>

```jsx
let symbol = Symbol("id");

console.log(symbol.description); // id
```

혹은 설명(심볼 이름)만을 출력하기 위해 description 프로퍼티를 참조할 수 있다.

<br>

```jsx
let id1 = Symbol("id");
let id2 = Symbol("id");

console.log(id1 == id2); // false
```

심볼은 **유일성이 보장**되는 자료형이기 때문에, 동일한 설명(심볼 이름)을 주고 심볼을 만들어도 각 심볼값은 다르다.

<br>

# 히든 프로퍼티

**히든 프로퍼티**는 **외부 코드에서 접근이 불가능**하며, **값도 덮어쓸 수 없는 프로퍼티**다. 내가 이 문장을 해석한 바는 다음과 같다..

- 히든 프로퍼티 : console.log 로 객체를 찍으면 해당 프로퍼티는 **보인다**. 숨겨져서 보이지 않는게 아니다. 여기서 말하는 hidden 은 외부로부터 감춰져 있다는 정도로 받아들이면 될 것 같다.
- 외부 코드에서 접근이 불가능 : 새로운 파일에서 프로퍼티의 추가가 진행되므로, 당연히 외부 코드에서는 접근할 수 없다. 작성된 객체를 export 하면 사용할 수 있을지도?
- 값도 덮어쓸 수 없다 : 접근을 못하니 새로 만들어진 프로퍼티를 변경할 수 없다.

<br>

![http://dl.dropbox.com/s/y8w1swt3gdc4siv/%EB%AA%A8%EB%8D%98%20JavaScript%20%ED%8A%9C%ED%86%A0%EB%A6%AC%EC%96%BC_18.%20%EC%8B%AC%EB%B3%BC%ED%98%95-1.png](http://dl.dropbox.com/s/y8w1swt3gdc4siv/%EB%AA%A8%EB%8D%98%20JavaScript%20%ED%8A%9C%ED%86%A0%EB%A6%AC%EC%96%BC_18.%20%EC%8B%AC%EB%B3%BC%ED%98%95-1.png)

위 사진을 보면, B.js 파일에서 작성된 객체를 A.js 파일이 가져다 사용하는 모습을 볼 수 있다. 이처럼 써드파티 코드 내부의 객체를 가져와 가공할 일이 생긴다.

해당 객체 내부에 어떤 프로퍼티가 있을 지 모르므로, 함부로 프로퍼티를 생성하거나 접근해서 변경하는 등의 행동은 기존 것과의 충돌을 유발할 수 있다. 위 예시 코드는, John 이었던 이름을 Kim 으로 바꿔버리게 된다.

위와 같은 상황에 이미 name 이라는 프로퍼티 키가 존재하지만, 새롭게 name 이라는 명칭을 통해 프로퍼티를 추가하고 값을 저장하는 등의 작성을 하고 싶을 때 **심볼**을 사용할 수 있다.

<br>

```jsx
const user = require("./B");
console.log(user); // { name: "John" }

let name = Symbol("name");
user[name] = "Kim";

console.log(user);       // { name: "John", [Symbol(name)]: "Kim"}
console.log(user.name);  // John
console.log(user[name]); // Kim
```

위처럼 name 이라는 심볼을 하나 만든 뒤, **대괄호 표기법**을 통해 프로퍼티를 부여하면, 기존에 작성된 프로퍼티를 건들지 않으면서 새로운 프로퍼티를 추가할 수 있다. 여기서 유의할 점은, **대괄호 표기법**으로 접근해야 한다는 것이다. .(dot) 으로 접근할 때 같은 이름의 키가 존재하면, **문자열**로 접근하지 심볼로 접근하지 않는다. 따라서, 심볼을 기존에 존재하는 프로퍼티 키와 동일하게 만들어 놓고, .(dot)으로 접근해 조작하면 의도한 바와 다르게 기존 프로퍼티가 바뀌는 모습을 볼 수 있다.

<br>

# 심볼은 키를 뱉는 for...in 구문에서 배제된다.

```jsx
let id = Symbol("id");
let user = {
  name: "John",
  age: 30,
  [id]: 123,
};

for (let key in user) console.log(key); // name과 age만 출력되고, 심볼은 출력되지 않습니다.

console.log(user[id]);          // 123
console.log(Object.keys(user)); // [ "name", "age" ]
```

키가 심볼이면 `for...in` 반복문에서 배제된다. 또한 객체 내부의 모든 프로퍼티들의 키를 배열에 담아 출력하는 Object.keys() 에서도 심볼인 키는 배제된다.

<br>

```jsx
let id = Symbol("id");
let user = {
  [id]: 123,
};

let clone = Object.assign({}, user);

console.log(clone[id]); // 123
```

`Object.assign` 을 통해 source 객체의 프로퍼티들로 target 객체를 새롭게 생성할 땐, 심볼을 키로 갖는 프로퍼티도 배제하지 않고 모두 포함시킨다. 이는 원래 설계가 이렇게 됐다고 한다.

<br>

# 전역 심볼

```jsx
let id = Symbol.for("id");
let idAgain = Symbol.for("id");

console.log(id === idAgain); // true
```

`Symbol.for(심볼이름)` 를 통해 **전역 심볼 레지스트리** 내부에 생성된 심볼을 해당 심볼을 반환하고, 없다면 만들고 반환한다. 

<br>

```jsx
let sym = Symbol.for("name");
let sym2 = Symbol.for("id");

console.log(Symbol.keyFor(sym));  // name
console.log(Symbol.keyFor(sym2)); // id

console.log(sym.description);     // name
console.log(sym2.description);    // id

console.log(sym.toString());      // Symbol(name)
console.log(sym2.toString());     // Symbol(id)
```

`Symbol.keyFor(심볼)` 을 통해 **전역 심볼의 이름**을 찾을 수 있다. 혹은, 모든 심볼은 `description 프로퍼티` 가 있으므로, 이걸로 이름을 찾을 수 있다. 혹은, 메소드 명이나 프로퍼티 명이 기억나지 않으면, `toString()` 을 통해서도 찾을 수 있겠다..

<br>

# 시스템 심볼

시스템 심볼은 자바스크립트 내부에서 사용되는 심볼이다. 시스템 심볼을 활용하면 객체를 미세하게 다룰 수 있다고 한다. 아래는 알아둘만한 시스템 심볼이다.

- Symbol.hasInstance
- Symbol.isConcatSpreadable
- Symbol.iterator
- Symbol.toPrimitive

`Symbol.toPrimitive` 시스템 심볼을 통해 객체를 원시 값으로 변환할 수 있다고 한다. 다음 장에서 알아보자고 한다.