---
title: "[99클럽 코테 스터디 27일차 TIL] Minimum Suffix Flips 문제 풀이"
date: 2024-06-18
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
> [Leetcode - Minimum Suffix Flips](https://leetcode.com/problems/minimum-suffix-flips/description/) 문제를 보고 풀이한 내용이다.

You are given a **0-indexed** binary string `target` of length `n`. You have another binary string `s` of length `n` that is initially set to all zeros. You want to make `s` equal to `target`.

In one operation, you can pick an index `i` where `0 <= i < n` and flip all bits in the inclusive range `[i, n - 1]`. Flip means changing `'0' to '1' and '1' to '0'`.

Return the minimum number of operations needed to make `s` equal to `target`.

**Example 1**:

Input: target = "10111"

Output: 3

Explanation: Initially, s = "00000".

Choose index i = 2: "00000" -> "00111"

Choose index i = 0: "00111" -> "11000"

Choose index i = 1: "11000" -> "10111"

We need at least 3 flip operations to form target.

**Example 2**:

Input: target = "101"

Output: 3

Explanation: Initially, s = "000".

Choose index i = 0: "000" -> "111"

Choose index i = 1: "111" -> "100"

Choose index i = 2: "100" -> "101"

We need at least 3 flip operations to form target.

**Example 3**:

Input: target = "00000"

Output: 0

Explanation: We do not need any operations since the initial s already equals target.

# 풀이

`s` 문자열은 `target`과 길이가 똑같은 0으로만 구성된 문자열이다.
문자열 `target`을 다음과 같은 과정을 통해 최소한의 작업으로 `s`와 동일하게 만들수 있다.

1. 인덱스 `i`를 선택하고 `[i, n - 1]` 범위의 모든 비트를 뒤집는다.
2. `s`를 `target`과 동일하게 만들기 위해 필요한 최소 작업 횟수를 구한다.

즉, `target` 문자열의 각 비트를 순회하면서 현재 비트와 `s` 문자열을 비교하고 현재 비트가 `s`와 다르면 전환 횟수를 증가시키고 `target`의 현재 비트를 뒤집어준다. 다음 비교를 위해 `s`도 현재 비트로 업데이트 시켜준다.

예를 들어 `target`이 `01011`인 경우를 살펴보자.

- 초기 상태: `s` = `0`, `count` = 0
- 첫 번째 문자 '0': `s`와 0이 같으므로 변화 없음
- 두 번째 문자 '1': `s`와 1이 다르므로 `count`를 1 증가시키고 `s`를 1로 업데이트
- 세 번째 문자 '0': `s`와 0이 다르므로 `count`를 1 증가시키고 `s`를 0으로 업데이트
- 네 번째 문자 '1': `s`와 1이 다르므로 `count`를 1 증가시키고 `s`를 1로 업데이트
- 다섯 번재 문자 '1': `s`와 1이 같으므로 변화 없음

이렇게 각 비트 변경 지점마다 뒤집기를 수행해 `target` 문자열을 만들어 낼 수 있다.

```java
class Solution {
    public int minFlips(String target) {
        char pre = '0';  // 초기 상태는 '0'으로 설정
        int res = 0;     // 결과 전환 횟수를 저장할 변수

        // 문자열의 각 문자를 순회
        for (char c : target.toCharArray()) {
            // 현재 문자와 이전 문자(pre)가 다르면 전환이 필요함을 의미
            if (c != pre) {
                res++;  // 전환 횟수 증가
            }

            // 이전 문자를 현재 문자로 업데이트
            pre = c;
        }
        
        // 최종 전환 횟수 반환
        return res;
    }
}

```

그때 그떄 최선의 선택을 하는 그리디 알고리즘을 채택해서 `target` 문자열을 최소한의 전환 횟수로 만들 수 있었다.

시간 복잡도는 O(n)으로 충분히 효율적인 성능을 낸다.