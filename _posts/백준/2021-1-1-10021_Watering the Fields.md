---
title: "[백준] 10021. Watering the Fields"
date: 2021-1-1 18:00:00 +0900
categories:
  - boj
toc: true
classes: wide
---

# 문제 링크

> [[백준] 10021. Watering the Fields](https://www.acmicpc.net/problem/10021)

<br>

# 풀이 과정

최소 스패닝 트리를 구현하는 문제입니다. 추가적으로는 `최소 비용 C 이상인 간선만 선택` 해서 트리를 구성해야 됩니다.

기본 다익스트라의 폼을 사용해 구현했고 시간 초과를 받았습니다. 간선 리스트를 구성할 때, 비용이 C 미만인 간선들은 애초에 넣질 말아야 했습니다. 이 조건을 걸어주고나서 통과했습니다.

<br>

# 코드

```java
import java.io.BufferedReader;
import java.io.IOException;
import java.io.InputStreamReader;
import java.util.*;

public class Main {
    static int N, C;
    static int[] x, y;
    static ArrayList<Edge> edges;
    static int[] parent;
    static int[] rank;

    public static void main(String[] args) throws IOException {
        BufferedReader br = new BufferedReader(new InputStreamReader(System.in));
        StringTokenizer st = new StringTokenizer(br.readLine());

        N = Integer.parseInt(st.nextToken());
        C = Integer.parseInt(st.nextToken());
        x = new int[N];
        y = new int[N];
        edges = new ArrayList<>();
        parent = new int[N];
        for (int i = 0; i < N; i++)
            makeSet(i);
        rank = new int[N];

        for (int i = 0; i < N; i++) {
            st = new StringTokenizer(br.readLine());
            x[i] = Integer.parseInt(st.nextToken());
            y[i] = Integer.parseInt(st.nextToken());
        }

        for (int i = 0; i < N - 1; i++) {
            for (int j = i + 1; j < N; j++) {
                int cost = (int) (Math.pow(x[i] - x[j], 2) + Math.pow(y[i] - y[j], 2));

                if (cost >= C)
                    edges.add(new Edge(i, j, cost));
            }
        }

        Collections.sort(edges, new Comparator<Edge>() {
            @Override
            public int compare(Edge o1, Edge o2) {
                return o1.cost - o2.cost;
            }
        });

        int cnt = 0;
        int answer = 0;
        for (Edge edge : edges) {
            if (find(edge.v1) == find(edge.v2)) continue;

            union(edge.v1, edge.v2);
            answer += edge.cost;
            cnt++;
        }

        System.out.println(cnt == N - 1 ? answer : -1);
    }

    static class Edge {
        int v1, v2, cost;

        public Edge(int v1, int v2, int cost) {
            this.v1 = v1;
            this.v2 = v2;
            this.cost = cost;
        }
    }

    static void makeSet(int idx) {
        parent[idx] = idx;
    }

    static int find(int idx) {
        if (idx == parent[idx]) return idx;
        return parent[idx] = find(parent[idx]);
    }

    static void union(int a, int b) {
        int pa = find(a);
        int pb = find(b);

        if (rank[pa] > rank[pb]) {
            parent[pb] = pa;
        } else {
            parent[pa] = pb;
            if (rank[pa] == rank[pb])
                rank[pa]++;
        }
    }
}
```