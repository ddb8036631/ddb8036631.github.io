---
title: "[모던 JavaScript 튜토리얼] 10. 함수 표현식(Function Expression)"
date: 2021-4-9 21:30:00 +0900
categories:
  - modernjavascripttutorial
classes: wide
toc: true
---

# 함수 표현식

---

```jsx
function sayHi() {
    console.log("Hello");
}
```

이전 포스트 『[모던 JavasScript 튜토리얼 - 9. 함수](http://ddb8036631.github.io/모던 javascript 튜토리얼/모던-JavaScript-튜토리얼_9.-함수)』 에서는 `함수 선언식(Function Declaration)` 방식으로 함수를 생성했다.

<br>

```jsx
const sayHi = function() {
    console.log("Hello");
};
```

위처럼 함수를 생성하는 방법을 `함수 표현식(Function Expression)` 이라고 한다. JS는 **함수를 값으로 취급**하기 때문에, 위와 같이 **함수를 생성한 뒤 변수에 값을 할당**할 수 있는 것이다.

<br>

```jsx
const sayHi = function() {
    console.log("Hello");
};

let func = sayHi;

func();  // Hello
sayHi(); // Hello
```

함수가 값이기 때문에, 만들어진 함수를 다른 변수에 담을 수도 있다.

<br>

# 콜백 함수

---

```jsx
function ask(question, yes, no) {
    if (confirm(question)) yes()
    else no();
}
  
function showOk() {
    alert( "동의하셨습니다." );
}
  
function showCancel() {
    alert( "취소 버튼을 누르셨습니다." );
}
  
ask("동의하십니까?", showOk, showCancel);
```

파라미터로 `showOk` 라는 함수와 `showCancel` 이라는 함수를 전달했다. 사용자의 버튼 클릭 입력에 따라 두 함수 중 하나가 실행된다.

showOk 와 showCancel 함수를 **콜백 함수**라고 한다. 콜백 함수는 called back, 즉 의미 그대로 **나중에 호출**된다. 사용자가 **확인** 버튼을 눌렀다면 **showOk** 함수가 콜백되고, **취소** 버튼을 눌렀다면 **showCancel** 함수가 콜백된다.

위 예제는 내장 함수 `confirm` 과 `alert` 가 포함되어 있기 때문에 브라우저 상에서 실행된다. 

<br>

# 함수 선언식 vs 함수 표현식

---

1. 함수 호출 순서에 따른 차이
2. 스코프의 차이

## 함수 호출 순서에 따른 차이

```jsx
sayHi("John"); // Hello, John

function sayHi(name) {
    console.log( `Hello, ${name}` );
}
```

함수 선언식으로 생성한 함수는 선언 이전에 사용할 수 있다.

<br>

```jsx
sayHi("John"); // error!

let sayHi = function(name) {
    console.log( `Hello, ${name}` );
};
```

반면, 함수 표현식으로 생성한 함수는 함수가 만들어지기 전까지 사용할 수 없다.

자바스크립트 엔진은 함수 선언문이 모두 처리된 이후에 스크립트를 실행시킨다. 반면, 함수 표현식은 실제 실행 흐름이 해당 함수에 도달했을 때 함수를 생성한다. 따라서, 함수 생성 전에 호출을 하면 에러가 발생한다.

<br>

## 스코프의 차이

JS는 **코드블럭 내에서 함수를 선언할 수 있다. 코드블럭 내에서 함수를 함수 선언식으로 생성하면 해당 스코프 밖에서는 접근할 수 없다. 반면, 함수 표현식으로 만든다면, 접근 가능하다.**

아래서 볼 수 있듯이 `함수 선언식` 은 **블럭 내에서만 유효**하다.

```jsx
let age = prompt("나이를 알려주세요.", 18);

// 조건에 따라 함수를 선언함
if (age < 18) {
		welcome(); // 안녕!
    function welcome() {
        alert("안녕!");
    }
} else {
		welcome(); // 안녕하세요!
    function welcome() {
        alert("안녕하세요!");
    }
}

welcome(); // Error: welcome is not defined
```

<br>

반면, `함수 표현식` 은 **전역변수에 담은 뒤 밖에서 호출 가능**하다.

```jsx
let age = prompt("나이를 알려주세요.", 18);

let welcome;

if (age < 18) {
    welcome = function () {
        alert("안녕!");
    };
} else {
    welcome = function () {
        alert("안녕하세요!");
    };
}

welcome(); // 제대로 동작합니다.
```