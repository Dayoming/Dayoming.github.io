---
title: "[99클럽 코테 스터디 21일차 TIL] BFS로 가장 먼 노드 문제 풀이"
date: 2024-06-12
categories: [TIL]
render_with_liquid: false
tags:
    [
        Java,
        BFS,
        프로그래머스,
        알고리즘,
        99클럽,
        코딩테스트 준비,
        개발자 취업,
        항해99,
        TIL
    ]
---

![99클럽 썸네일](/assets/img/posts/99클럽_thumbnail.png)

# 문제
> [프로그래머스 - 가장 먼 노드](https://school.programmers.co.kr/learn/courses/30/lessons/49189) 문제를 보고 풀이한 내용이다.

`n`개의 노드가 있는 그래프가 있습니다. 각 노드는 1부터 `n`까지 번호가 적혀있습니다. 1번 노드에서 가장 멀리 떨어진 노드의 갯수를 구하려고 합니다. 가장 멀리 떨어진 노드란 최단경로로 이동했을 때 간선의 개수가 가장 많은 노드들을 의미합니다.

노드의 개수 `n`, 간선에 대한 정보가 담긴 2차원 배열 `vertex`가 매개변수로 주어질 때, 1번 노드로부터 가장 멀리 떨어진 노드가 몇 개인지를 `return` 하도록 `solution` 함수를 작성해주세요.

**제한사항**
- 노드의 개수 `n`은 2 이상 20,000 이하입니다.
- 간선은 양방향이며 총 1개 이상 50,000개 이하의 간선이 있습니다.
- `vertex` 배열 각 행 [a, b]는 a번 노드와 b번 노드 사이에 간선이 있다는 의미입니다.

# 풀이

문제에서 '양방향 그래프'와 '최단경로' 가 보였으므로 해당 그래프는 BFS를 사용하면 효율적으로 탐색할 수 있을 것 같았다. 따라서 주어진 배열에 따라 그래프를 만들고, BFS를 사용해 최단 거리를 구한 후 그 중 가장 먼 거리를 가지고 있는 노드를 세어 문제를 해결할 수 있었다.

```java
public int solution(int n, int[][] vertex) {
        List<List<Integer>> graph = new ArrayList<>();
        for (int i = 0; i < n + 1; i++) {
            graph.add(new ArrayList<>());
        }

        // 그래프 생성
        for (int[] edge : vertex) {
            int from = edge[0];
            int to = edge[1];
            graph.get(from).add(to);
            graph.get(to).add(from);
        }

        // BFS를 사용하여 최단 거리를 구합니다.
        int[] distance = new int[n + 1];
        Arrays.fill(distance, -1);
        Queue<Integer> queue = new LinkedList<>();

        queue.add(1);
        distance[1] = 0;

        while (!queue.isEmpty()) {
            int current = queue.poll();
            for (int neighbor : graph.get(current)) {
                if (distance[neighbor] == -1) {
                    queue.add(neighbor);
                    distance[neighbor] = distance[current] + 1;
                }
            }
        }

        // 최장 거리 찾기
        int maxDistance = 0;
        for (int i = 1; i <= n; i++) {
            if (distance[i] > maxDistance) {
                maxDistance = distance[i];
            }
        }

        // 최장 거리를 가진 노드의 개수를 셉니다.
        int count = 0;
        for (int i = 1; i <= n; i++) {
            if (distance[i] == maxDistance) {
                count++;
            }
        }

        return count;
    }
```