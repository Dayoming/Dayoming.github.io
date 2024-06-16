---
title: "[99클럽 코테 스터디 25일차 TIL] Group the People Given the Group Size They Belong To 문제 풀이"
date: 2024-06-16
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
> [Leetcode - Group the People Given the Group Size They Belong To](https://leetcode.com/problems/group-the-people-given-the-group-size-they-belong-to/description/) 문제를 보고 풀이한 내용이다.

There are `n` people that are split into some unknown number of groups. Each person is labeled with a **unique ID** from `0` to `n - 1`.

You are given an integer array `groupSizes`, where `groupSizes[i]` is the size of the group that person `i` is in. For example, if `groupSizes[1] = 3`, then person `1` must be in a group of size `3`.

Return a list of groups such that each person i is in a group of size `groupSizes[i]`.

Each person should appear in **exactly one group**, and every person must be in a group. If there are multiple answers, **return any of them**. It is **guaranteed** that there will be **at least one** valid solution for the given input.

 

**Example 1**:

Input: groupSizes = [3,3,3,3,3,1,3]

Output: [[5],[0,1,2],[3,4,6]]

Explanation: 

The first group is [5]. The size is 1, and groupSizes[5] = 1.

The second group is [0,1,2]. The size is 3, and groupSizes[0] = groupSizes[1] = groupSizes[2] = 3.

The third group is [3,4,6]. The size is 3, and groupSizes[3] = groupSizes[4] = groupSizes[6] = 3.

Other possible solutions are [[2,1,6],[5],[0,4,3]] and [[5],[0,6,2],[4,3,1]].

**Example 2**:

Input: groupSizes = [2,1,3,3,3,2]

Output: [[1],[0,5],[2,3,4]]

# 풀이

```java
class Solution {
    public List<List<Integer>> groupThePeople(int[] groupSizes) {
        Set<List<Integer>> group = new HashSet<>(); // 중복을 방지하기 위해 HashSet으로 선언
        List<Integer> groupSizeList = Arrays.stream(groupSizes).boxed().toList();

        for (int size : groupSizes) {
            // 현재 찾는 그룹의 크기 지정
            int target = size;
            
            // 사람 i가 속해 있는 그룹의 크기와 같은 크기를 가진 i들을 찾아 리스트로 반환
            List<Integer> peoples = IntStream.range(0, groupSizeList.size())
                    .filter(i -> groupSizeList.get(i) == target)
                    .boxed()
                    .collect(Collectors.toList());

            // 만약 그룹의 크기가 target인 그룹이 여러 개면
            // ex) target = 3, {0, 1, 2, 3, 4, 6}
            if (peoples.size() > target) {
                List<Integer> people = new ArrayList<>();
                for (Integer num : peoples) {
                    if (people.size() < target) { // people 리스트가 target만큼 차지 않았으면 값을 넣어준다
                        people.add(num);
                    } else {  // people 리스트가 target만큼 차면
                        group.add(new ArrayList<>(people)); // people 리스트를 정답으로 반환할 리스트에 복사해서 넣어준다
                        people.clear(); // people 리스트를 비우고 다음 값을 넣어준다
                        people.add(num);
                    }
                }
                // 마지막 그룹이 비어있지 않으면 추가
                if (!people.isEmpty()) group.add(new ArrayList<>(people));
            } else { // 그룹의 크기가 target인 그룹이 여러 개가 아니면 그대로 정답 리스트에 넣어준다
                group.add(peoples);
            }
        }

        return group.stream().toList(); // HashSet을 List로 변환
    }
}
```

## 더 공부하면 좋을 내용
- Stream 문법
- HashSet 구현