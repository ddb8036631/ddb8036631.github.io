---
title: "[Java] Pattern, Matcher로 정규식 매칭 문자열 자르기"
date: 2021-1-26 21:59:00 +0900
categories:
  - java
# toc: true
classes: wide
---

정규식을 전문적으로 다루는 클래스 두 가지( `Pattern` , `Matcher` )를 활용해 손쉽게 문자열 패턴 매칭을 수행할 수 있다.

다음 코드를 보며 기본적인 사용법을 알아보자.

```java
Pattern pattern = Pattern.compile("a*b");
Matcher matcher = pattern.matcher("aaaaab");
boolean result = matcher.matches();
```

1. 먼저정규식을 인자로 Pattern 클래스의 static 메소드 `compile(String regex)` 을 호출해 패턴을 만든다.
2. 만들어진 Pattern 객체로 매칭해 볼 문자열을 인자로 전달해 `matcher(String input)` 메소드를 호출해 Matcher 객체를 만든다.
3. 위의 1, 2 작업이 끝나면 Matcher 객체로 `matches()` 메소드를 호출해 매칭 유무를 불리언 값으로 받는다.

<br>

매칭해 볼 문자열 내에 정규식 표현이 여러 번 등장할 수 있다. 예를 들어, [프로그래머스 - 카카오2018. 다트 게임](https://programmers.co.kr/learn/courses/30/lessons/17682) 문제를 정규식 패턴으로 표현하면 다음과 같다.

```java
String regex = "([0-9]*)([SDT])([*#]?)";
```

<br>

[정규식 테스트 사이트](https://regexr.com/)에서 문자열 `1S2D*3T` 는 위에 작성된 정규식 `([0-9]*)([SDT])([*#)?)` 에 세 번 매칭됨을 아래 사진에서 확인할 수 있다.

![http://dl.dropbox.com/s/v3t0f42x3cnxuub/Java-Pattern%2C%20Matcher%EB%A1%9C%20%EC%A0%95%EA%B7%9C%EC%8B%9D%20%EB%A7%A4%EC%B9%AD%20%EB%AC%B8%EC%9E%90%EC%97%B4%20%EC%9E%90%EB%A5%B4%EA%B8%B0-1.png](http://dl.dropbox.com/s/v3t0f42x3cnxuub/Java-Pattern%2C%20Matcher%EB%A1%9C%20%EC%A0%95%EA%B7%9C%EC%8B%9D%20%EB%A7%A4%EC%B9%AD%20%EB%AC%B8%EC%9E%90%EC%97%B4%20%EC%9E%90%EB%A5%B4%EA%B8%B0-1.png)

<br>

**세 번** 매칭되니 matches() 메소드가 true 를 반환할 것이라고 예상하고 아래 코드를 작성해보지만, 그 값을 찍어보면 false 가 나오는 걸 확인할 수 있다. `matches()` 메소드는 **패턴이 전체 문자열과 일치할 경우 true 를 반환**하기 때문이다.

```java
Pattern pattern = Pattern.compile("([0-9]*)([SDT])([*#]?)");
Matcher matcher = pattern.matcher("1S2D*3T");

System.out.println(matcher.matches()); // false

matcher = pattern.matcher("1S");
System.out.println(matcher.matches()); // true

matcher = pattern.matcher("2D*");
System.out.println(matcher.matches()); // true

matcher = pattern.matcher("3T");
System.out.println(matcher.matches()); // true
```

<br>

그럼 위 문제처럼 **전체 문자열 안에 패턴과 일치하는 부분 문자열이 여러 번 등장**하는 경우, 어떻게 매칭 처리를 해야할까?

<br>

Matcher 클래스에는 `find()` 메소드가 있다. 이 메소드는 전체 문자열 내에서 패턴에 일치하는 부분 문자열만을 가지고 매칭 일치 여부를 판단한다. 따라서 find() 메소드를 사용하면, 전체 문자열 `1S2D*3T` 는 **`1S`** , `2D*` , `3T` 총 세 번의 매칭에 성공하게 된다.

![http://dl.dropbox.com/s/2r1xvroukghkz5w/Java-Pattern%2C%20Matcher%EB%A1%9C%20%EC%A0%95%EA%B7%9C%EC%8B%9D%20%EB%A7%A4%EC%B9%AD%20%EB%AC%B8%EC%9E%90%EC%97%B4%20%EC%9E%90%EB%A5%B4%EA%B8%B0-2.png](http://dl.dropbox.com/s/2r1xvroukghkz5w/Java-Pattern%2C%20Matcher%EB%A1%9C%20%EC%A0%95%EA%B7%9C%EC%8B%9D%20%EB%A7%A4%EC%B9%AD%20%EB%AC%B8%EC%9E%90%EC%97%B4%20%EC%9E%90%EB%A5%B4%EA%B8%B0-2.png)

Matcher 클래스에는 `group()` 메소드를 통해 패턴 내 각 그룹을 접근할 수 있다. 정규식 내부에서 중괄호로 감싸진 부분은 하나의 그룹을 뜻한다. 정규식 `([0-9]*)([SDT])([*#]?)` 은 아래와 같이 세 개의 그룹을 갖는다.

1. ([0-9]\*)
2. ([SDT])
3. ([*#]?)

![http://dl.dropbox.com/s/0yy1dcu47mnetob/Java-Pattern%2C%20Matcher%EB%A1%9C%20%EC%A0%95%EA%B7%9C%EC%8B%9D%20%EB%A7%A4%EC%B9%AD%20%EB%AC%B8%EC%9E%90%EC%97%B4%20%EC%9E%90%EB%A5%B4%EA%B8%B0-3.png](http://dl.dropbox.com/s/0yy1dcu47mnetob/Java-Pattern%2C%20Matcher%EB%A1%9C%20%EC%A0%95%EA%B7%9C%EC%8B%9D%20%EB%A7%A4%EC%B9%AD%20%EB%AC%B8%EC%9E%90%EC%97%B4%20%EC%9E%90%EB%A5%B4%EA%B8%B0-3.png)

Matcher 객체를 이용해 find() 메소드를 호출해 패턴에 맞는 각 부분 문자열을 받아온 뒤 `group()` 메소드를 호출하면, 매칭된 부분 문자열을 반환한다.

```java
matcher.find();
System.out.println(matcher.group()); // 1S

matcher.find();
System.out.println(matcher.group()); // 2D*

matcher.find();
System.out.println(matcher.group()); // 3T
```

<br>

또한 인자에 인덱스를 주어 패턴 내 각 그룹에 접근 가능하다.

```java
matcher.find();
System.out.println(matcher.group()); // 2D*
System.out.println(matcher.group(1)); // 2
System.out.println(matcher.group(2)); // D
System.out.println(matcher.group(3)); // *
```

<br>

추가로, 패턴을 생성할 때 `?<name>` 키워드를 사용해 그룹 명을 붙일 수 있다. 이후에는 `matcher.group("name")` 과 같이 인덱스 대신 이름으로 해당 그룹을 접근할 수 있게 된다.

<br>

참고) [[Java] Pattern, Matcher Class 사용법과 메소드 정리](https://girawhale.tistory.com/77?category=915307)
