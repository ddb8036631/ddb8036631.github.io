---
title: "[알고리즘] Union-Find"
date: 2020-12-18 16:46:00 +0900
categories:
  - 알고리즘
toc: true
toc_sticky: true
---

# Union-Find란?

Union-Find는 서로소 집합(disjoint-set)을 표현하는 알고리즘입니다. 서로소 집합은 공통 원소가 없는 두 집합을 의미합니다.

아래와 같이 정점과 간선이 주어져 세 개의 그래프를 이룬다고 가정합시다.

![/assets/images/Union-Find1.png](/assets/images/Union-Find1.png)

- 첫 번째 그래프는 `정점 1, 2, 3`으로 그려집니다.
- 두 번째 그래프는 `정점 4, 5`로 그려집니다.
- 세 번째 그래프는 `정점 6, 7, 8`로 그려집니다.

이 때 임의의 두 정점 A와 B가 주어졌을 때, 두 정점이 같은 그래프에 속해있는 지, 아닌지를 판단하는 것이 Union-Find 알고리즘입니다.

# Union-Find 동작 과정

Union-Find는 세 동작을 거칩니다.

1. 초기화(makeSet) : 주어진 원소들이 각각의 집합에 포함되어 있도록 초기화 합니다.
2. 합치기(Union) : 두 원소 a, b가 주어질 때, 이들을 하나로 합칩니다.
3. 찾기(Find) : 원소 a가 어떤 집합에 속해 있는 지 찾습니다.

위의 세 동작들은 1차원 배열을 사용하여 진행됩니다. `합치기` 전에, `찾기` 과정을 거쳐 각 정점의 부모 노드 번호를 파악합니다. 이후 `합치기` 과정에서는 `찾기` 에서 구한 두 부모 노드 번호 중 더 적은 번호로 통일합니다.

아래에서 그림을 통해 Union-Find가 어떻게 동작하는 지 확인해보겠습니다. 마지막 Union이 끝난 뒤의 그래프는 위와 같다고 가정하겠습니다. 따라서 Union은 간선의 개수 만큼인 5회 발생되며, 상세 내용은 아래와 같습니다.

- 정점 1과 2를 Union
- 정점 2와 3을 Union
- 정점 4와 5를 Union
- 정점 6과 8을 Union
- 정점 7과 8을 Union

사용되는 1차원 배열 parent의 요소는 각 정점의 `부모 노드 번호`를 의미합니다.

# 그림으로 확인

## 초기화

가장 먼저 초기화 작업을 거칩니다. 각각의 정점을 자기 자신의 집합에 포함시킵니다.

![/assets/images/Union-Find2.png](/assets/images/Union-Find2.png)

## 정점 1과 2를 Union

먼저 정점 1과 2에 대해 Find 과정을 실행합니다. 정점 1의 부모 노드 번호는 1이고, 정점 2의 부모 노드 번호는 2인 것을 확인할 수 있습니다. 이후, Union 단계에서 더 적은 부모의 노드 번호로 통일합니다. 이 단계에서 정점 2의 부모 노드 번호가 1로 변경됩니다. 이후 단계에서도 같은 작업이 반복됩니다.

![/assets/images/Union-Find3.png](/assets/images/Union-Find3.png)

## 정점 2와 3을 Union

![/assets/images/Union-Find4.png](/assets/images/Union-Find4.png)

## 정점 4와 5를 Union

![/assets/images/Union-Find5.png](/assets/images/Union-Find5.png)

## 정점 6와 8를 Union

![/assets/images/Union-Find6.png](/assets/images/Union-Find6.png)

## 정점 7와 8를 Union

![/assets/images/Union-Find7.png](/assets/images/Union-Find7.png)

## Union이 끝난 후

Union 작업이 모두 끝난 후 parent 배열을 통해 각각의 정점들이 그룹화 되었음을 확인할 수 있습니다.

![/assets/images/Union-Find8.png](/assets/images/Union-Find8.png)

# 코드

```java
static void makeSet(int idx) {
	parent[idx] = idx;
}

static int find(int idx) {
	if (idx == parent[idx])
		return idx;

	return parent[idx] = find(parent[idx]);
}

static void union(int a, int b) {
	int p1 = find(a);
	int p2 = find(b);

	if (p1 > p2) {
		parent[p1] = p2;

	}
	else if(p1 < p2)
		parent[p2] = p1;
}
```

# Union by rank

위에서 알아본 Union-Find 코드는 아래와 같은 완전 비대칭 트리와 같은 구조를 나타낸다면, 최악의 경우 O(N)의 시간 복잡도를 갖습니다.

![/assets/images/Union-Find9.png](/assets/images/Union-Find9.png)

Union by rank는 항상 작은 트리를 큰 트리 루트에 붙이는 방법입니다. 트리의 깊이가 실행 시간에 영향을 주기 때문에, 깊이가 적은 트리를 깊이가 더 깊은 트리의 루트 아래에 추가합니다. 그러면, 두 트리의 깊이가 같을 경우에만 깊이가 증가하게 됩니다. 이 방법을 사용하면, 최악의 경우 O(logn)의 시간 복잡도를 갖습니다.

## 코드

```java
static void makeSet(int idx) {
	parent[idx] = idx;
}

static int find(int idx) {
	if (idx == parent[idx])
		return idx;

	return parent[idx] = find(parent[idx]);
}

static void union(int a, int b) {
	int p1 = find(a);
	int p2 = find(b);

	if (rank[p1] > rank[p2])
		parent[p2] = p1;

	else {
		parent[p1] = p2;

		if(rank[p1] == rank[p2])
			rank[p2]++;
	}
}
```
