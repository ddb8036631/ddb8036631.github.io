---
title: "[알고리즘 풀이] 백준 - 11576. Base Conversion"
date: 2021-4-21 13:00:00 +0900
categories:
  - 알고리즘 풀이
toc: true
classes: wide
---

# 문제 링크

> [백준 - 11576. Base Conversion](https://www.acmicpc.net/problem/11576)

<br>

# 풀이 과정

A 진수를 10진수로 변환한 후, 그 값을 다시 B 진수로 변환하는 문제입니다. **stack**과 **queue** 의 기능을 동시에 사용하기 위해 **Deque** 자료구조를 사용했습니다.

로직은 다음과 같습니다.

1. 먼저 A 진법으로 표현된 숫자의 자리수 개수 m 만큼의 숫자를 입력받아 덱에 저장( `add` )합니다.
2. 숫자의 맨 앞자리부터 꺼내어( `poll` ), 가중치를 곱해 변수 sum 에 누적시킵니다. 이 작업으로 **A 진법으로 표현된 숫자는 10진수로 변환**됩니다.
3. 10진수로 변환된 숫자를 B 로 **나누며** **B 진법으로 변환**합니다. **나머지 값**을 덱에 저장( `push` ) 합니다.
4. 다 변환되었으면, 덱의 top 부터 하나씩 꺼내( `pop` ) 문자열을 이어붙입니다.

<br>

# 코드

```java
import java.io.BufferedReader;
import java.io.IOException;
import java.io.InputStreamReader;
import java.util.*;

public class Main {
    public static void main(String[] args) throws IOException {
        BufferedReader br = new BufferedReader(new InputStreamReader(System.in));
        StringTokenizer st = new StringTokenizer(br.readLine());
        Deque<Integer> deque = new ArrayDeque<>();
        StringBuilder answer = new StringBuilder();
        int A = Integer.parseInt(st.nextToken());
        int B = Integer.parseInt(st.nextToken());
        int m = Integer.parseInt(br.readLine());
        int sum = 0;

        st = new StringTokenizer(br.readLine());

        for (int i = 0; i < m; i++) deque.add(Integer.parseInt(st.nextToken()));

        while (!deque.isEmpty()) {
            int size = deque.size();
            int num = deque.poll();

            sum += (int) Math.pow(A, size - 1) * num;
        }

        while (sum != 0) {
            deque.push(sum % B);
            sum /= B;
        }

        while (!deque.isEmpty()) answer.append(deque.pop() + " ");

        System.out.println(answer);
    }
}
```