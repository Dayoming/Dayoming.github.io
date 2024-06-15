---
title: "[99클럽 코테 스터디 24일차 TIL] Find The Original Array of Prefix Xor 문제 풀이"
date: 2024-06-15
categories: [TIL]
render_with_liquid: false
tags:
    [
        Java,
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
> [Leetcode - Find The Original Array of Prefix Xor](https://leetcode.com/problems/find-the-original-array-of-prefix-xor/description/) 문제를 보고 풀이한 내용이다.

You are given an **integer** array `pref` of size `n`. Find and return the array `arr` of size `n` that satisfies:

`pref[i] = arr[0] ^ arr[1] ^ ... ^ arr[i]`.
Note that `^` denotes the **bitwise-xor** operation.

It can be proven that the answer is unique.

**Example 1**:

Input: pref = [5,2,0,3,1]

Output: [5,7,2,3,2]

Explanation: From the array [5,7,2,3,2] we have the following:

- pref[0] = 5.

- pref[1] = 5 ^ 7 = 2.

- pref[2] = 5 ^ 7 ^ 2 = 0.

- pref[3] = 5 ^ 7 ^ 2 ^ 3 = 3.

- pref[4] = 5 ^ 7 ^ 2 ^ 3 ^ 2 = 1.

**Example 2**:

Input: pref = [13]

Output: [13]

Explanation: We have pref[0] = arr[0] = 13.
 
**Constraints**:
- 1 <= pref.length <= 105
- 0 <= pref[i] <= 106

# 풀이

배열 선호도 `pref[i] = arr[0] ^ arr[1] ^ ... arr[i]` 와 같고, 이를 만족하는 `n` 크기의 `arr`을 반환해야 한다.

예를 들어 `pref = [5, 2, 0, 3, 1]`일 때 계산은 다음과 같다.

```
pref[0] = arr[0] = 5

pref[1] = arr[0] ^ arr[1] = 5 ^ arr[1] = 2
arr[1]의 값을 구하는 방법은 arr[0] ^ 2이다.

pref[2] = arr[0] ^ arr[1] ^ arr[2] = 5 ^ 7 ^ arr[2] = 0
arr[2]의 값을 구하는 방법은 arr[0] ^ arr[1] ^ 0이다.

pref[3] = arr[0] ^ arr[1] ^ arr[2] ^ arr[3] = 5 ^ 7 ^ 2 ^ arr[3] = 3
arr[3]의 값을 구하는 방법은 arr[0] ^ arr[1] ^ arr[2] ^ 3이다.

pref[4] = arr[0] ^ arr[1] ^ arr[2] ^ arr[3] ^ arr[4] = 5 ^ 7 ^ 2 ^ 3 ^ arr[4] = 1
arr[4]의 값을 구하는 방법은 arr[0] ^ arr[1] ^ arr[2] ^ arr[3] ^ 1이다.
```

따라서 처음에 생각한 방법은 다음과 같다.

```java
class Solution {
    public int[] findArray(int[] pref) {
        int[] arr = new int[pref.length];
        arr[0] = pref[0];

        if (pref.length == 1) {
            return arr;
        }

        for (int i = 1; i < pref.length; i++) {
            int temp = arr[0];
            for (int j = 1; j < i; j++) { // arr[0] ^ arr[1] ^ ... arr[i - 1] 구하기
                temp = temp ^ arr[j];
            }
            arr[i] = temp ^ pref[i]; // arr[i - 1] ^ pref[i]
        }

        return arr;
    }
}
```

계산 방법을 그대로 적용해서 매 계산마다 `arr[0]`부터 현재 인덱스 - 1까지 XOR해주고 `pref[현재 인덱스]` 와 XOR 해주었다. 예제에 대한 결과는 잘 나왔지만, leetcode에 코드를 올려보니 time limit exceeded가 발생했다.

![leetcode time limit exceeded](/assets/img/posts/2024-06-15-1.png)

아무래도 `i`가 증가할 때마다 `j`가 처음부터 끝까지 다시 돌아야해서 시간 복잡도가 O(N^2)에 가까우므로 수가 커지면 부담이 될 수밖에 없다. 

다른 방법을 생각해보자. 아까 생각했던 계산 방식을 조금만 바꿔주면 된다.

```
arr[0] = pref[0] = 5
arr[1] = arr[0] ^ pref[1] = pref[0] ^ pref[1] = 7
arr[2] = arr[0] ^ arr[1] ^ pref[2] = pref[1] ^ pref[2] = 2
arr[3] = arr[0] ^ arr[1] ^ arr[2] ^ pref[3] = pref[2] ^ pref[3] = 3
arr[4] = arr[0] ^ arr[1] ^ arr[2] ^ arr[3] ^ pref[4] = pref[3] ^ pref[4] = 2
```

이제 위 로직을 코드로 구현해보자.

```java
class Solution {
    public int[] findArray(int[] pref) {
        int[] arr = new int[pref.length];
        arr[0] = pref[0];

        if (pref.length == 1) {
            return arr;
        }

        for (int i = 1; i < pref.length; i++) {
            arr[i] = pref[i - 1] ^ pref[i];
        }

        return arr;
    }
}
```

그럼 이제 O(N^2)의 시간 복잡도에서 O(N)의 시간 복잡도로 문제를 해결할 수 있게 된다!