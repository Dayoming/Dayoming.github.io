---
title: "[99클럽 코테 스터디 17일차 TIL] 동적 계획법으로 Partition Array for Maximum Sum 풀이"
date: 2024-06-08
categories: [TIL]
render_with_liquid: false
tags:
    [
        Java,
        DP,
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
> [Leetcode - Count Sorted Vowel Strings](https://leetcode.com/problems/partition-array-for-maximum-sum/description/) 문제를 보고 풀이한 내용이다.

Given an integer array `arr`, partition the array into (contiguous) subarrays of length at most `k`. After partitioning, each subarray has their values changed to become the maximum value of that subarray.

Return the largest sum of the given array after partitioning. Test cases are generated so that the answer fits in a 32-bit integer.

**Example 1**:

**Input**: arr = [1,15,7,9,2,5,10], k = 3
**Output**: 84
**Explanation**: arr becomes [15,15,15,9,10,10,10]

**Example 2**:

**Input**: arr = [1,4,1,5,7,3,6,1,9,9,3], k = 4
**Output**: 83


**Example 3**:

**Input**: arr = [1], k = 1
**Output**: 1
 

**Constraints**:

- `1 <= arr.length <= 500`
- `0 <= arr[i] <= 109`
- `1 <= k <= arr.length`

# 풀이

주어진 배열을 부분 배열로 나누고, 각 부분 배열의 최댓값을 찾은 후에 그 값을 모두 더한 결과 중 최댓값을 찾는 문제다.

```java
class Solution {
    public int maxSumAfterPartitioning(int[] arr, int k) {
        int n = arr.length;
        int[] dp = new int[n + 1]; // dp 배열 생성 (길이가 n+1인 이유는 인덱스 계산 편의를 위함)
        
        for (int i = 1; i <= n; i++) { // 주어진 배열을 순회하면서 dp 배열을 채워나감
            int max = 0;
            for (int j = 1; j <= k && i - j >= 0; j++) { // 현재 인덱스부터 거슬러 올라가며 부분 배열을 만듦
                max = Math.max(max, arr[i - j]); // 현재 최댓값과 새로운 부분 배열의 최댓값을 비교하여 갱신
                dp[i] = Math.max(dp[i], dp[i - j] + max * j); // 현재 인덱스까지의 최대 합 계산
            }
        }
        
        return dp[n]; // 배열의 마지막 요소에 저장된 값이 최대 합이 됨
    }
}
```

1. 주어진 배열을 순회하면서 각 위치에서의 최대 합을 계산하는 동적 계획법을 사용한다.
2. `dp` 배열을 초기화하고, 주어진 배열을 순회한다.
3. 현재 인덱스에서의 최대 합을 구하기 위해, 현재 위치부터 거슬러 올라가며 가능한 부분 배열을 만든다.
4. 만들어진 부분 배열의 최댓값을 구하고, 이를 현재 위치에서의 최대 합에 반영한다.
5. 최종적으로 `dp` 배열의 마지막 요소에는 주어진 배열을 부분 배열로 나누어 얻을 수 있는 최대 합이 저장된다. 이 값을 반환한다.

이러한 방식으로 주어진 배열을 부분 배열로 나누어 최대 합을 구할 수 있다.

## 시간 복잡도
주어진 배열을 순회하면서 각 위치에서 최대 합을 구하기 위해, 현재 위치부터 거슬러 올라가며 가능한 부분 배열을 만들어야 한다. 이를 위해서는 각 위치에서 최대 `k`만큼의 부분 배열을 고려해야 하므로, 내부 루프의 시간 복잡도는 O(k)이다.

따라서 위 알고리즘의 시간 복잡도는 O(n * k)이다. 여기서 `n`은 배열의 길이이고, `k`는 부분 배열의 최대 길이이다.

그러나 `k`가 `n`과 비슷한 크기라면 시간 복잡도는 O(n^2)에 가까워진다.