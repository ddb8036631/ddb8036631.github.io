---
title: "[Javascript] RegExp, String으로 정규식 매칭 문자열 자르기"
date: 2021-1-27 14:55:00 +0900
categories:
  - javascript
toc: true
classes: wide
---

# Java로...

이전 [[Pattern, Matcher로 정규식 매칭 문자열 자르기]](http://ddb8036631.github.io/java/Java_Pattern,-Matcher로-정규식-매칭-문자열-자르기) 포스팅에서 Java로 패턴 매칭을 진행해봤다. Javascript도 정규식을 이용해 문자열 패턴 매칭을 진행할 수 있다.

<br>

# Javascript로!

# RegExp

Javascript 는 정규식을 사용하기 위한 객체 `RegExp` 를 제공한다. 정규식 객체는 아래 코드에서 볼 수 있듯이, **리터럴**로 생성할 수도, **생성자**로 생성할 수도 있다.

```jsx
// 리터럴 표기법
let regex = /ab+c/i;

// 생성자 함수
let regex = new RegExp(/ab+c/, "i");
let regex = new RegExp("ab+c", "i");
```

## 1. 리터럴 표기로 객체 생성

리터럴로 표기할 때에는 정규식 객체임을 알리기 위해 두 빗금(/) 사이에 패턴을 입력한다. 이 때, 패턴을 따옴표로 묶지 않아야 한다.

## 2. 생성자 함수로 객체 생성

생성자 함수는 다음과 같다.

`RegExp(pattern[, flags])`

첫 번째 인자로 패턴을 받으며, 패턴은 **RegExp 객체 형식**과 **문자열 형식** 둘 다 허용이 된다. 다시 말해서, 패턴을 빗금 안에 담을 수도, 따옴표 안에 담을 수도 있다.

선택적으로 두 번째 인자 플래그를 받으며, 다음 여섯 가지의 플래그를 부여할 수 있다.

1. g(global) : 전역 검색
2. i(case insensitive) : 대소문자 구분 없는 검색
3. m(multiline) : 다중 행 검색
4. s(single line) : .에 개행 문자도 매칭
5. u(unicode) : 유니코드. 패턴을 유니코드 포인트의 나열로 취급.
6. y(sticky) : "sticky" 검색을 수행. 문자열의 현재 위치부터 검색을 수행한다.

<br>

# 패턴 매칭 예시

[프로그래머스 - 카카오2018. 다트 게임](https://programmers.co.kr/learn/courses/30/lessons/17682) 문제를 정규식 패턴으로 표현하면 다음과 같다.

```jsx
// 리터럴 표기법
let regex = /(?<SCORE>[0-9]+)(?<SDT>[SDT])(?<OPTION>[*#]?)/g;

// 생성자 함수
let regex = new RegExp("(?<SCORE>[0-9]+)(?<SDT>[SDT])(?<OPTION>[*#]?)", "g");
let regex = new RegExp(/(?<SCORE>[0-9]+)(?<SDT>[SDT])(?<OPTION>[*#]?)/, "g");
```

<br>

입력으로 주어진 문자열을 String 클래스의 `matchAll()` 메소드로 정규식 패턴과 매칭 판별을 할 수 있다. matchAll() 메소드는 호출한 문자열 내부에서 정규식 패턴에 매칭되는 모든 부분 문자열에 대한 매칭 결과를 배열의 형태로 반환한다.

아래는 입력 문자열 `1S2D*3T` 를 정규식 `/(?<SCORE>[0-9]+)(?<SDT>[SDT])(?<OPTION>[*#]?)/g` 에 매칭시켜 본 코드와 출력 결과이다.

```jsx
let regex = new RegExp(/(?<SCORE>[0-9]+)(?<SDT>[SDT])(?<OPTION>[*#]?)/, "g");
let str = "1S2D*3T";

let result = [...str.matchAll(regex)];

for (res of result) {
  console.log(res);
}
```

![/assets/images/JS_RegExp, String으로 정규식 매칭 문자열 자르기1.png](/assets/images/JS_RegExp, String으로 정규식 매칭 문자열 자르기1.png)

<br>

matchAll() 메소드는 다음의 내용을 포함하는 **배열**을 반환한다.

- 매칭된 문자열의 전체
- 매칭된 문자열의 전체 중 정규식 패턴의 각 그룹에 해당하는 문자열
- 매칭이 발생한 문자열의 시작 인덱스
- matchAll() 메소드를 호출한 문자열(input)
- 매칭된 문자열 내의 그룹핑된 그룹들

<br>

반환 받은 배열의 `groups` 프로퍼티에 접근해, 패턴 내 모든 그룹에 접근할 수 있다.

```jsx
for (res of result) {
  console.log(res.groups);
}

// 출력 결과
// [Object: null prototype] { SCORE: '1', SDT: 'S', OPTION: '' }
// [Object: null prototype] { SCORE: '2', SDT: 'D', OPTION: '*' }
// [Object: null prototype] { SCORE: '3', SDT: 'T', OPTION: '' }
```

<br>

또한 정규식에서 각 그룹에 준 이름으로 그룹 개별에 대한 값을 접근할 수 있다.

```jsx
for (res of result) {
  console.log(
    "SCORE : " +
      res.groups.SCORE +
      ", SDT : " +
      res.groups.SDT +
      ", OPTION : " +
      res.groups.OPTION
  );
}

// 출력 결과
// SCORE : 1, SDT : S, OPTION :
// SCORE : 2, SDT : D, OPTION : *
// SCORE : 3, SDT : T, OPTION :
```
