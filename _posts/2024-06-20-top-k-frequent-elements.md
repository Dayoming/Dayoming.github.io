---
title: "[99클럽 코테 스터디 28일차 TIL] Java Map 정렬과 Top K Frequent Elements 문제 풀이"
date: 2024-06-20
categories: [TIL]
render_with_liquid: false
tags:
    [
        Java,
        Leetcode,
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
> [Leetcode - Top K Frequent Elements](https://leetcode.com/problems/top-k-frequent-elements/description/) 문제를 보고 풀이한 내용이다.

Given an integer array nums and an integer k, return the k most frequent elements. You may return the answer in any order.

**Example 1**:

Input: nums = [1,1,1,2,2,3], k = 2

Output: [1,2]

**Example 2**:

Input: nums = [1], k = 1

Output: [1]

# 풀이

배열 `nums`가 주어졌을 때, 해당 `num`의 빈도 수가 큰 순서대로 정렬한 후 `k` 개 만큼만 출력하는 문제이다.
내 생각에 `Map`을 만들어서 `{num : num 의 개수}` 형태로 저장하고, `Map`을 각 Key의 value값이 큰 순서대로 정렬한 뒤 `k` 만큼만 `Map`의 key들을 가져오면 될 것 같았다.

## Map은 어떻게 정렬할까?

List나 ArrayList의 경우 [이전에 풀었던 문제처럼](https://dayoming.github.io/posts/%EA%B0%80%EC%9E%A5-%ED%81%B0-%EC%88%98/) Comparator를 이용해 할 수 있는데, Map은 어떻게 정렬을 해야하는지 몰랐다.
그래서 찾아본 결과, Entry를 담는 List를 새로 만들어서 거기에 Map을 담은 후 정렬하면 됐다.
Map은 key=value 쌍으로 이루어져 있기 때문에 원소 대신 Entry라고 불리는 값 묶음을 가지고 있고, 그걸 담아주어야 한다.

```java
Map<Integer, Integer> map = new HashMap<>();
map.put(1, 1);
map.put(2, 2);
map.put(3, 3);

List<Map.Entry<Integer, Integer>> entryList = new LinkedList<>(map.entrySet());
// Lambda를 사용해 value가 큰 순서대로 정렬
entryList.sort((o1, o2) -> o2.getValue() - o1.getValue());
```

![map과 entryList](/assets/img/posts/2024-06-20-1.png)

이렇게 하면 위와 같이 key-value 쌍을 가지고 있는 정렬된 리스트를 얻을 수 있다. 이제 이걸 `k`개 만큼만 `int[]` 배열에 담아주고 반환하면 된다.

## 정답 코드

```java
public class Solution {
    public int[] topKFrequent(int[] nums, int k) {
        Map<Integer, Integer> map = new HashMap<>();

        for (int num : nums) {
            // 만약 map에 해당 키 값이 존재하지 않으면
            if (map.get(num) == null) {
                map.put(num, 1);
            } else { // 해당 키 값이 존재하면 value 1 증가
                map.replace(num, map.get(num) + 1);
            }
        }

        List<Map.Entry<Integer, Integer>> entryList = new LinkedList<>(map.entrySet());
        entryList.sort((o1, o2) -> o2.getValue() - o1.getValue());

        int answer[] = new int[k];
        int idx = 0;

        for (Map.Entry<Integer, Integer> entry : entryList) {
            if (idx == k) break;
            answer[idx] = entry.getKey();
            idx++;
        }

        return answer;
    }
}
```

![result](/assets/img/posts/2024-06-20-2.png)

Map, entryList, answer 등 저장 공간을 여러 개 사용하고 있어서 Memory는 조금 비효율적으로 나왔다. 그러나 시간 복잡도는 O(n)으로 제한 조건에 무리 없이 모든 테스트 케이스를 실행시킬 수 있었다.