---
title: "[Java] int[] to Integer[]"
date: 2020-12-2 15:35:00 +0900
categories:
  - Java
toc: true
classes: wide
use_math: true
---

정수형 배열을 내림차순으로 정렬 할 때 아래와 같은 에러 메세지를 볼 수 있습니다. 원시타입의 배열을 Wrapper 클래스로 바꿔줘야 정상적으로 작동이 되는데, 이 때 int[]를 Integer[]로 변환해야 합니다.

![int[]_to_Integer[].png](/assets/images/int_to_Integer.png)

아래는 [Stack Overflow - How to convert int[] to Integer[] in Java?](https://stackoverflow.com/questions/880581/how-to-convert-int-to-integer-in-java) 에서 찾은 함수를 이용한 변경 코드입니다.

```java
int[] numbers = {3, 30, 34, 5, 9};

Integer[] convertedArr1 = Arrays.stream(numbers).boxed().toArray(Integer[]::new);
Integer[] convertedArr2 = IntStream.of(numbers).boxed().toArray(Integer[]::new);
```