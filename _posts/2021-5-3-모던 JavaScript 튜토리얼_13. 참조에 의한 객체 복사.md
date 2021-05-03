---
title: "[모던 JavaScript 튜토리얼] 13. 참조에 의한 객체 복사"
date: 2021-5-3 13:00:00 +0900
categories:
  - 모던 JavaScript 튜토리얼
classes: wide
toc: true
---

객체와 원시 타입의 근본적 차이 중 하나는, 원시값은 '값 그대로' 저장 및 할당되고 복사되는 반면, **객체는 참조에 의해 저장되고 복사**된다는 점이다.

<br>

```jsx
let message = "Hello!";
let phrase = message;
```

<center><img src="http://dl.dropbox.com/s/7j5g6w7mui50ele/%EB%AA%A8%EB%8D%98%20JavaScript%20%ED%8A%9C%ED%86%A0%EB%A6%AC%EC%96%BC_13.%20%EC%B0%B8%EC%A1%B0%EC%97%90%20%EC%9D%98%ED%95%9C%20%EA%B0%9D%EC%B2%B4%20%EB%B3%B5%EC%82%AC-1.png"></center>

모던 자바스크립트 문서에서는 위 코드를 실행하게 되면, 위 그림과 같이 원시 타입의 문자열 `"Hello"` 가 두 변수에 각각 저장된다고 설명하고 있다. 하지만 이 설명은 조금 부족한 감이 있어 아래에서 살펴보려고 한다.

자바스크립트는 데이터 할당을 크게 `변수 영역` 과 `데이터 영역` , 두 가지 영역으로 나누어 작업한다. 이는 **데이터 변환을 자유롭게 하고** 동시에 **메모리를 효율적으로 사용**하기 위한 방법이다. 위 그림을 보면, 변수 **message** 와 **phrase** 에 직접적으로 데이터 "Hello" 가 저장되는 것이 아니라, **데이터 영역**에 "Hello" 가 저장된 영역의 주소값을 가리키는 것을 알 수 있다.

<br>

# 변수 선언에 따른 메모리의 변화

자바스크립트는 **숫자형** 데이터에 **8바이트**의 메모리 공간을 확보하고, 이에 반해 **문자열**은 따로 **정해진 규칙이 없다**. 그럼, 먼저 데이터 변환을 자유롭게 하는 자바스크립트의 메모리 관리는 어떻게 이루어지는지 살펴보겠다.

<br>

![http://dl.dropbox.com/s/lo4irp7bbqbnld2/%EB%AA%A8%EB%8D%98%20JavaScript%20%ED%8A%9C%ED%86%A0%EB%A6%AC%EC%96%BC_13.%20%EC%B0%B8%EC%A1%B0%EC%97%90%20%EC%9D%98%ED%95%9C%20%EA%B0%9D%EC%B2%B4%20%EB%B3%B5%EC%82%AC-2.png](http://dl.dropbox.com/s/lo4irp7bbqbnld2/%EB%AA%A8%EB%8D%98%20JavaScript%20%ED%8A%9C%ED%86%A0%EB%A6%AC%EC%96%BC_13.%20%EC%B0%B8%EC%A1%B0%EC%97%90%20%EC%9D%98%ED%95%9C%20%EA%B0%9D%EC%B2%B4%20%EB%B3%B5%EC%82%AC-2.png)

초기에 **변수 a** 에 숫자 **100**을 담았다( `let a = 100;` ). 이후에, 이 변수에 문자열 "abc" 를 담게되면( `a = "abc";` ), 자바스크립트는 같은 데이터 공간을 문자열 "abc" 로 변경하는 것이 아니라, 다른 영역에 값을 할당하고, 그 주소를 바꾸는 작업을 거친다.

위 그림에서, 선언과 동시에 할당하는 명령어를 거쳐 @5003 에 100 이 저장되어 있고, 새롭게 문자열을 재할당하게 되면, @5003 을 "abc" 로 채우는 것이 아니라, 비어있는 임의의 주소 @5004를 선택한 뒤 해당 데이터 영역에 문자열을 저장하게 된다. 그리고 변수 a 가 참조하고 있는 값의 주소를 @5004로 변경함으로써 할당 과정을 마친다.

<br>

![http://dl.dropbox.com/s/361v52t5eihc9e2/%EB%AA%A8%EB%8D%98%20JavaScript%20%ED%8A%9C%ED%86%A0%EB%A6%AC%EC%96%BC_13.%20%EC%B0%B8%EC%A1%B0%EC%97%90%20%EC%9D%98%ED%95%9C%20%EA%B0%9D%EC%B2%B4%20%EB%B3%B5%EC%82%AC-3.png](http://dl.dropbox.com/s/361v52t5eihc9e2/%EB%AA%A8%EB%8D%98%20JavaScript%20%ED%8A%9C%ED%86%A0%EB%A6%AC%EC%96%BC_13.%20%EC%B0%B8%EC%A1%B0%EC%97%90%20%EC%9D%98%ED%95%9C%20%EA%B0%9D%EC%B2%B4%20%EB%B3%B5%EC%82%AC-3.png)

문자열은 길이가 정해져 있지 않으므로 가변적인 성격을 띈다. 도중에 변수에 담긴 문자열을 길이가 더 긴 문자열로 변경하게 되면, 이미 확보되어 있던 공간을 늘리는 작업이 필요하다. 하지만, 위와 같이 참조하고 있는 주소값을 끊고, 새로 할당된 주소로 연결지으면 위와 같은 불필요한 작업을 하지 않게 된다. 따라서, **메모리를 효율적**으로 사용할 수 있게 된다.

<br>

![http://dl.dropbox.com/s/nuq3sclloec98sf/%EB%AA%A8%EB%8D%98%20JavaScript%20%ED%8A%9C%ED%86%A0%EB%A6%AC%EC%96%BC_13.%20%EC%B0%B8%EC%A1%B0%EC%97%90%20%EC%9D%98%ED%95%9C%20%EA%B0%9D%EC%B2%B4%20%EB%B3%B5%EC%82%AC-4.png](http://dl.dropbox.com/s/nuq3sclloec98sf/%EB%AA%A8%EB%8D%98%20JavaScript%20%ED%8A%9C%ED%86%A0%EB%A6%AC%EC%96%BC_13.%20%EC%B0%B8%EC%A1%B0%EC%97%90%20%EC%9D%98%ED%95%9C%20%EA%B0%9D%EC%B2%B4%20%EB%B3%B5%EC%82%AC-4.png)

변수 영역과 데이터 영역이 따로 존재하는 이점은 위 예제에서도 살펴볼 수 있다. 앞서 맨 위에서 본 것과 같이, 변수 **message** 와 **phrase** 는 "Hello" 라는 같은 문자열을 담고 있다. 데이터 영역을 따로 갖기에 "Hello"라는 중복된 문자열을 한 번만 저장하고, 그 주소를 가리킴으로써 **같은 데이터를 중복으로 저장하지 않는다.** 만약, **message** 가 새로운 값을 갖게 된다면, 그 값이 **데이터 영역에 존재하지 않으면 새로 할당**하고, **존재한다면 그 주소를 가리키게** 될 것이다.

<br>

```jsx
let obj = {
	age: 28,
	name: "Kim",
};
```

객체의 경우를 살펴보겠다.

<br>

![http://dl.dropbox.com/s/t1031v2odmk88e9/%EB%AA%A8%EB%8D%98%20JavaScript%20%ED%8A%9C%ED%86%A0%EB%A6%AC%EC%96%BC_13.%20%EC%B0%B8%EC%A1%B0%EC%97%90%20%EC%9D%98%ED%95%9C%20%EA%B0%9D%EC%B2%B4%20%EB%B3%B5%EC%82%AC-5.png](http://dl.dropbox.com/s/t1031v2odmk88e9/%EB%AA%A8%EB%8D%98%20JavaScript%20%ED%8A%9C%ED%86%A0%EB%A6%AC%EC%96%BC_13.%20%EC%B0%B8%EC%A1%B0%EC%97%90%20%EC%9D%98%ED%95%9C%20%EA%B0%9D%EC%B2%B4%20%EB%B3%B5%EC%82%AC-5.png)

객체의 경우는 깊이를 하나 더 들어가고, 나머지 작업은 위에서 본 것과 같다. 객체에는 여러 프로퍼티가 존재하므로, 이들을 한 번에 가리킬 수가 없다. 따라서, 위 그림과 같이 객체에 대한 독립적인 영역을 보장받은 뒤, 해당 영역들이 각 프로퍼티의 실제 값을 참조하고 있다.

<br>

# 객체의 복사

객체의 복사 방법은 얕은 복사와 깊은 복사가 있다.

**얕은 복사**는 원본과 복사된 객체가 같은 참조를 가리키는 것을 말한다. 바로 아래 단계의 값만을 복사하는 방법이라고 생각하면 된다.

반면, **깊은 복사**는 객체 내부에 객체가 존재할 지라도, 모두 원본과의 참조를 끊어 완전히 독립된 객체로 설정되는 것을 말한다. 이는 내부적의 모든 값들을 하나하나 찾아 직접 복사하는 방법이다.

<br>

## 얕은 복사 방법

### 1. Object.assign

```jsx
let obj = {
    a: 1,
    b: {
        c: null,
        d: [1, 2],
    },
};

let obj2 = {};
Object.assign(obj2, obj);
// let obj2 = Object.assign({}, obj);

obj2.a = 3;
obj2.b.c = 4;
obj.b.d[1] = 3;

console.log(obj);  // { a: 1, b: { c: 4, d: [1, 3] } }
console.log(obj2); // { a: 3, b: { c: 4, d: [1, 3] } }
```

`Object.assign` 의 첫 번째 파라미터는 **target 객체**를 지정하고, 나머지 파라미터로 **sources 객체**를 전달한다. 이 방법으로 객체 내부의 모든 프로퍼티를 옮겨 적을 수 있다. Object.assign 은 대상 객체를 반환하므로, 위 주석과 같이 작성할 수도 있다.

<br>

```jsx
let privacy = {
    age: 28,
    name: "Kim",
};

let phone = {
    category: "Samsung",
    phoneNum: "010-xxxx-xxxx",
    manufacturingYear: "2021",
};

let academic = {
    university: "in Seoul",
    attending: "graduated",
};

let totalInfo = {};
Object.assign(totalInfo, privacy, phone, academic);

// let totalInfo = Object.assign({}, privacy, phone, academic);

console.log(totalInfo);
/*
{
    age: 28,
    name: "Kim",
    category: "Samsung",
    phoneNum: "010-xxxx-xxxx",
    manufacturingYear: "2021",
    university: "in Seoul",
    attending: "graduated"
}
*/
```

위와 같은 식으로 여러 객체의 프로퍼티들을 모아 하나의 대상 객체에 담을 수 있다.

<br>

### 2. spread operator

```jsx
let obj = {
    a: 1,
    b: {
        c: null,
        d: [1, 2],
    },
};

let obj2 = { ...obj };

obj2.a = 3;
obj2.b.c = 4;
obj.b.d[1] = 3;

console.log(obj);  // { a: 1, b: { c: 4, d: [1, 3] } }
console.log(obj2); // { a: 3, b: { c: 4, d: [1, 3] } }
```

ES6의 전개 연산자 문법을 통해 얕은 복사를 구현할 수도 있다.

<br>

## 깊은 복사 방법

### 1. 재귀 함수

```jsx
let copyObj = function(target) {
    let result = {};
	  if(typeof target == "object" && target != null) {
		    for(var prop in target) {
			      result[prop] = copyObj(target[prop]);
		    }
	  } else {
        result = target;
    }

		return result;
};

let obj = {
		a: 1,
		b: {
				c: null,
				d: [1, 2],
		},
};

let obj2 = copyObj(obj);

obj2.a = 3;
obj2.b.c = 4;
obj.b.d[1] = 3;

console.log(obj);  // { a: 1, b: { c: null, d: [1, 3] } }
console.log(obj2); // { a: 3, b: { c: 4,    d: {"0": 1, "1": 2} } }
```

객체 내부의 모든 프로퍼티에 대해 재귀 함수를 호출한다. 위 코드에서 모든 객체를 **키-값 쌍**으로 설정하기에, obj2.b.d 는 **배열이 아닌 객체**로 변경된 점을 확인할 수 있다.

<br>

### 2. JSON.stringify

```jsx
let copyObj = function(target) {
		return JSON.parse(JSON.stringify(target));
};

let obj = {
		a: 1,
		b: {
				c: null,
				d: [1, 2],
		},
};

let obj2 = copyObj(obj);

obj2.a = 3;
obj2.b.c = 4;
obj.b.d[1] = 3;

console.log(obj);  // { a: 1, b: { c: null, d: [1, 3] } }
console.log(obj2); // { a: 3, b: { c: 4,    d: [1, 2] } }
```

`JSON.stringify` 는 **자바스크립트 객체를 JSON 문자열로** 변환한다. 반대로, `JSON.parse` 는 **JSON 문자열을 자바스크립트 객체로** 변환한다.

JSON 문자열로 변환했다가 다시 자바스크립트 객체로 변환하는 과정에서 객체에 대한 참조가 끊어진 것이다.

<br>

### 3. lodash.cloneDeep

```jsx
let _ = require("lodash");

let obj = {
    a: 1,
    b: {
        c: null,
        d: [1, 2],
    },
};

let obj2 = _.cloneDeep(obj);

obj2.a = 3;
obj2.b.c = 4;
obj.b.d[1] = 3;

console.log(obj);  // { a: 1, b: { c: null, d: [1, 3] } }
console.log(obj2); // { a: 3, b: { c: 4,    d: [1, 2] } }
```

lodash 라이브러리의 cloneDeep 메소드를 사용해서 깊은 복사를 구현할 수 있다. 모듈을 설치하거나, CDN 을 첨부한 후에 사용해야 한다.