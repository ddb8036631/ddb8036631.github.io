---
title: "[JavaScript] Hoisting & TDZ"
date: 2020-11-19 23:57:43 +0900
categories:
  - javascript
toc: true
classes: wide
---

# 호이스팅

## 호이스팅의 개념

hoist는 끌어올리다 라는 뜻을 가집니다.

호이스팅은 Javascript 인터프리터가 코드를 해석할 때 변수 및 함수의 선언 처리와 실제 코드 실행을 나누어서 처리하기 때문에 발생하는 현상입니다.

함수 안에 있는 변수들의 선언을 함수의 최상단으로 끌어올려 선언하는 것을 의미합니다. 전역 범위에서 선언되었을 경우에는, 전역 범위의 최상단에서 선언하게 됩니다.

```jsx
const print = () => {
  console.log("before : ", a); // undefined
  var a = 1;
  console.log("after : ", a); // 1
};

print();
```

위의 코드를 보면, 순차적인 방식에 의해 생각한다면, before 위치에서는 변수 a에 대해 미리 알지 못하므로 에러가 나야되지만, 호이스팅을 통해 변수 a를 함수 내 최상단으로 호이스팅 해 선언 되고 undefined 로 초기화 되었지만, 값은 할당되지 않은 상태로 인식하게 됩니다. 위의 코드는 호이스팅의 결과로 아래의 코드로 해석됩니다.

```jsx
const print = () => {
  var a = undefined;
  console.log("before : ", a); // undefined
  a = 1;
  console.log("after : ", a); // 1
};

print();
```

따라서 before에서는 undefined(변수는 존재하나, `어떠한 값으로도 할당되지 않아 자료형이 정해지지 않은 상태`)로 출력됩니다.

## 호이스팅의 대상

위의 예제에서의 var를 let 혹은 const로 변경해서 실행시켜보면 다음과 같이 우리가 예상했던 ReferenceError를 볼 수 있습니다.

![http://dl.dropbox.com/s/y5alq7iedrgt00j/JavaScript-Hoisting%20%26%20TDZ-1.png](http://dl.dropbox.com/s/y5alq7iedrgt00j/JavaScript-Hoisting%20%26%20TDZ-1.png)

이를 통해 let, const 변수는 호이스팅이 발생하지 않는다고 오해할 수 있습니다. 호이스팅에 관한 부분은 아래 **변수의 생성 단계**에서 다룹니다.

### 변수의 생성 단계

변수는 선언 단계, 초기화 단계, 할당 단계를 거쳐 생성됩니다.

1. 선언단계 : 변수를 실행 컨텍스트의 변수 객체에 등록합니다. 이 단계에서 변수를 참조할 경우 참조 에러(Reference Error)가 발생합니다.
2. 초기화 단계 : 실행 컨텍스트에 등록된 변수 객체에 대한 메모리를 할당합니다. 이 단계에서 변수는 undefined 로 초기화 됩니다.
3. 할당 단계 : undefined 로 초기화 된 변수에 값을 할당합니다.

위의 변수의 생성 단계를 기준으로 var 키워드로 선언된 변수는 선언과 초기화가 한 번에 이루어집니다. 따라서 변수 객체에 변수를 등록하고, 변수를 메모리에 할당하여 undefined 로 초기화 하는 과정이 한 번에 이루어지므로, 변수 선언 이전에 변수에 접근해도 에러가 발생하지 않습니다.

반면, let 과 const 키워드로 선언된 변수는 선언과 초기화 단계가 분리되어 진행됩니다. 따라서 let 과 const 키워드의 초기화 단계는 변수 선언문에 도달했을 때 이루어지고, 초기화 이전에 변수에 접근할 경우, 참조 에러가 발생하게 됩니다.

# TDZ(Temporal Dead Zone)

TDZ는 변수의 선언과 동시에 초기화하는 작업 이전에 해당 변수를 접근하는 것을 막습니다.

let과 const 변수는 TDZ의 영향을 받습니다.

let, const 변수는 선언과 초기화를 동시에 하지 않기 때문에 호이스팅해 선언만 한 상태이고, 초기화가 이루어지지 않은 상태에서 해당 변수를 접근하려고 하면 참조 에러가 발생하게 되는 것입니다.
