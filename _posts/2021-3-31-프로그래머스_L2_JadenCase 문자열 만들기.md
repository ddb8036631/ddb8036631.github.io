---
title: "[알고리즘 풀이] 프로그래머스 - Level2. JadenCase 문자열 만들기"
date: 2021-3-31 15:42:00 +0900
categories:
  - programmers
toc: true
classes: wide
---

# 문제 링크

> [프로그래머스 - Level2. JadenCase 문자열 만들기](https://programmers.co.kr/learn/courses/30/lessons/12951)

<br>

# 풀이 과정

```jsx
return s
    .split(" ")
    .map((word) => (word == "" ? "" : word[0].toUpperCase() + word.slice(1).toLowerCase()))
    .join(" ");
```

1. 먼저 split 메소드를 통해 입력된 문자열을 공백 문자 기준으로 각 요소로 나누어 배열로 만듭니다.
2. 배열을 순회하며 각 word 에 대해 첫 글자를 대문자로, 나머지 글자들은 소문자로 변형시킵니다.
3. 배열 각 요소 사이사이에 공백 문자를 합쳐 만들어진 문자열을 리턴합니다.

<br>

![/assets/images/프로그래머스_L2_JadenCase 문자열 만들기-1.png](/assets/images/프로그래머스_L2_JadenCase 문자열 만들기-1.png)

알파벳 사이에 공백이 하나라면 위와 같이 나뉘어질 것입니다. 그러면 word 는 알파벳으로 시작하는 문자가 됩니다.

<br>

![/assets/images/프로그래머스_L2_JadenCase 문자열 만들기-2.png](/assets/images/프로그래머스_L2_JadenCase 문자열 만들기-2.png)

하지만, 공백 문자가 연달아 등장할 수 있습니다. 문자열에 공백이 연달아 여러 개 사용된 문자열이면, 공백 기준으로 나뉘어진 word 는 위처럼 빈 문자열("")일 수 있습니다. 이러면 0번 째 인덱스 참조가 불가능 하므로, 참조 에러가 발생하게 됩니다. 따라서, 잘린 문자열이 빈 문자열일 경우에 대한 예외 처리를 해주어야 했습니다.

<br>

# 코드

```jsx
function solution(s) {
    return s
        .split(" ")
        .map((word) => (word == "" ? "" : word[0].toUpperCase() + word.slice(1).toLowerCase()))
        .join(" ");
}

console.log(solution("3people unFollowed me"));
// console.log(solution("for the last week"));
// console.log(solution("A B C D E"));
// console.log(solution("         "));
```