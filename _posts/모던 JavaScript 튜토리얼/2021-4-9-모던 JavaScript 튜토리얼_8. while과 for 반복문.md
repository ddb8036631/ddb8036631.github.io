---
title: "[모던 JavaScript 튜토리얼] 8. while과 for 반복문"
date: 2021-4-9 18:51:00 +0900
categories:
  - modernjavascripttutorial
classes: wide
toc: true
---

# while 반복문

```jsx
let i = -3;

while (i) {
  console.log(i); // -3, -2, -1
  i++;
}
```

C/C++ 처럼 **숫자값만으로** 조건문을 표현할 수 있다. 0 은 false, 그 외 숫자는 true를 나타낸다.

<br>

# for 반복문

```jsx
let cnt = 1;
let arr = Array.from(Array(3), () => Array(3).fill(0));

outer: for (let i = 0; i < 3; i++) {
    for (let j = 0; j < 3; j++) {
        if (i == 1) break outer;
        
        arr[i][j] = cnt++;
    }
}

for (let i = 0; i < 3; i++) {
    console.log(arr[i]);
}
// 1, 2, 3
// 0, 0, 0
// 0, 0, 0
```

반복문 앞에 label 을 설정해 특정 반복문을 빠져나올 수 있다.