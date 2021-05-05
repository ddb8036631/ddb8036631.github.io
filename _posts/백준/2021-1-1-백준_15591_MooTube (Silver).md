---
title: "[알고리즘 풀이] 백준 - 15591. MooTube (Silver)"
date: 2021-1-1 15:54:00 +0900
categories:
  - boj
toc: true
classes: wide
---

# 문제 링크

> [백준 - 15591. MooTube (Silver)](https://www.acmicpc.net/problem/15591)

# 풀이 과정

설명 중 아래의 문구가 제대로 이해되지 않았던 문제입니다.

> 존은 임의의 두 쌍 사이의 동영상의 USADO를 그 경로의 모든 연결들의 USADO 중 최솟값으로 하기로 했다.

> 존은 N-1개의 동영상 쌍을 골라서 어떤 동영상에서 다른 동영상으로 가는 경로가 반드시 하나 존재하도록 했다.

임의의 두 정점 사이에는 `적어도 하나의 경로가 존재` 함을 알 수 있습니다. 이를 토대로, 정점 V를 기준으로 bfs 탐색을 하며, 진행하는 경로에 존재하는 유사도가 K 이상인 정점들의 개수를 세어 출력하면 됩니다.

![/assets/images/MooTube1.png](/assets/images/MooTube1.png)

위 그래프를 예로 들어보겠습니다.

먼저 정점 1과 정점 4 사이의 유사도를 구해보겠습니다. 정점 1과 정점 4 사이의 유사도는 다음과 같이 경로 상 존재하는 유사도들 중 가장 작은 값으로 설정됩니다.

1. 정점 1 - 정점 2의 유사도 3
2. 정점 2 - 정점 3의 유사도 5
3. 정점 3 - 정점 4의 유사도 1

따라서, 3번 간선에 해당하는 유사도 1이 정점 1과 정점 4 사이의 유사도가 됩니다.

이렇듯 경로 상 존재하는 모든 유사도를 확인해 봐야 하므로, 그래프 탐색 알고리즘이 사용되야 한다는 것을 알 수 있습니다. 정점 1에서 bfs 탐색을 시작해보면, 그 경로는 1 → 2 → 3 → 4 가 될 것이며, 경로를 탐색하면서 최소 유사도 K 보다 작은 경로는 탐색하지 않는 조건을 적용시키면, 가능한 경로는 1 → 2 → 3 이 될 것입니다. 만약 위 그래프에서 정점 4에 연결된 또 다른 정점들이 더 있다고 가정해도, 그 둘 사이의 유사도가 1보다 크다면 정점 1과의 유사도는 1이 될 것입니다.

![/assets/images/MooTube2.png](/assets/images/MooTube2.png)

매 단계에서 갈 수 있는 정점으로의 유사도가 K 보다 작은지 확인하고, 작다면 해당 정점을 큐에 넣지 않습니다. 해당 정점을 기준으로 더이상 진행해볼 필요가 없기 때문입니다. 진행 경로에 문제 조건의 최소 유사도 K 보다 작은 유사도 U가 존재할 시, 그 정점까지의 유사도는 U가 될 것이기 때문입니다. 

![/assets/images/MooTube3.png](/assets/images/MooTube3.png)

# 코드

```java
package 그래프;

import java.util.*;

public class BOJ_15591_MooTube {
    public static void main(String[] args) {
        Scanner sc = new Scanner(System.in);

        int N = sc.nextInt();
        int Q = sc.nextInt();

        List<int[]>[] adj = new ArrayList[N + 1];
        for (int i = 1; i <= N; i++)
            adj[i] = new ArrayList<>();

        for (int i = 0; i < N - 1; i++) {
            int a = sc.nextInt();
            int b = sc.nextInt();
            int c = sc.nextInt();

            adj[a].add(new int[]{b, c});
            adj[b].add(new int[]{a, c});
        }

        for (int i = 0; i < Q; i++) {
            boolean[] visit = new boolean[N + 1];

            int k = sc.nextInt();
            int v = sc.nextInt();

            Queue<Integer> queue = new LinkedList<>();
            visit[v] = true;
            queue.add(v);

            int cnt = 0;
            while (!queue.isEmpty()) {
                int now = queue.poll();

                for (int j = 0; j < adj[now].size(); j++) {
                    int[] next = adj[now].get(j);

                    if (next[1] < k || visit[next[0]]) continue;

                    cnt++;
                    visit[next[0]] = true;
                    queue.add(next[0]);
                }
            }

            System.out.println(cnt);
        }
    }
}
```