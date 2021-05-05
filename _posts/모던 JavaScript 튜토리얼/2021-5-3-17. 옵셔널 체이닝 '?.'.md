---
title: "[모던 JavaScript 튜토리얼] 17. 옵셔널 체이닝 '?.'"
date: 2021-5-3 17:00:00 +0900
categories:
  - modernjavascripttutorial
classes: wide
toc: true
---

**옵셔널 체이닝** `?.` 을 사용하면 프로퍼티가 없는 중첩 객체를 에러 없이 안전하게 접근할 수 있다.

<br>

```jsx
let user = {}; // 주소 정보가 없는 사용자

console.log(user.address.street); // TypeError: Cannot read property 'street' of undefined
```

코드를 작성하다 보면, 위와 같이 undefined 인 객체의 프로퍼티를 참조하려고 하는 상황이 발생한다. 이는 에러를 유발하게 되고, 에러 원인을 찾는 시간이 소요된다.

위에 주석에 달아논 것처럼, 여러 사용자들 중 주소를 입력하지 않은 상황이 충분히 발생할 수 있고,이게 **잘못**된 에러가 아닐 수 있다.

<br>

```jsx
let user = {}; // 주소 정보가 없는 사용자

console.log( user && user.address && user.address.street ); // undefined, 에러가 발생하지 않습니다.
```

그럼 우리는 위와 같은 특수한 예외 상황을 가리기 위해 코드를 작성해야 한다. 이전에는 `&&` 연산자를 통해 이런 **undefined** 를 참조하는 일을 방지했다. 앞에서부터 연쇄되는 접근( user, user.address, user.address.street)을 다 확인하기 위해 코드가 불필요하게 길어지는 것을 볼 수 있다.

<br>

# 옵셔널 체이닝의 등장

`?.` 은 ?. 앞 평가 대상이 **undefined** 나 **null** 이면 뒤의 모든 평가를 멈추고 **undefined** 를 리턴한다.

<br>

```jsx
let user = null;

console.log(user?.address);        // undefined
console.log(user?.address.street); // undefined
```

위 코드에서 user 는 null 로 할당되어 있다. `user?.address.street` 는 user 가 null 이므로 뒤의 평가는 무시한채 undefined 를 리턴한다. 이와 같이, 뒤의 평가가 얼마가 있던 싸그리 무시되는 평가 방법을 `단락 평가` 라고 부른다.

<br>

```jsx
let user = {};

console.log(user?.address);        // undefined
console.log(user?.address.street); // TypeError: Cannot read property 'street' of undefined
```

반면, 위의 코드에서 user 는 null 이 아닌 **빈 객체**로 할당이 되어 있다. `user?.address.street` 는 user 가 null 이 아니므로 address 프로퍼티까지 접근을 하고, address 라는 이름의 프로퍼티가 없기 때문에 undefined 를 리턴한다. 곧이어, **undefined 를 가지고 street 을 접근**하려고 하니 위와 같이 에러가 발생하게 된다.

<br>

# ?.() 와 ?.[]

```jsx
let user1 = {
    admin() {
        console.log("관리자 계정입니다.");
    },
};

let user2 = {};

user1.admin?.(); // 관리자 계정입니다.
user2.admin?.();
```

`?.()` 은 존재가 확실치 않은 함수를 호출할 때 사용된다.

<br>

```jsx
let user1 = {
    firstName: "Violet",
};

let user2 = null; // user2는 권한이 없는 사용자라고 가정해봅시다.

let key = "firstName";

console.log(user1?.[key]);                           // Violet
console.log(user2?.[key]);                           // undefined
console.log(user1?.[key]?.something?.not?.existing); // undefined
```

`?.[]` 은 존재가 확실치 않은 프로퍼티를 접근할 때 사용된다. 위 예시의 마지막 콘솔 출력 명령은 `user1[key].something` 프로퍼티가 존재하지 않으므로, undefined 를 리턴해 단락 평가를 마친다.