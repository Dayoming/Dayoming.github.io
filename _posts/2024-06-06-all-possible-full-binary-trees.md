---
title: "[99클럽 코테 스터디 15일차 TIL] 동적 계획법으로 All Possible Full Binary Trees 풀이"
date: 2024-06-06
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
> [Leetcode - All Possible Full Binary Trees](https://leetcode.com/problems/all-possible-full-binary-trees) 문제를 보고 풀이한 내용이다.

Given an integer `n`, return a list of all possible **full binary trees** with n nodes. Each node of each tree in the answer must have `Node.val == 0`.

Each element of the answer is the root node of one possible tree. You may return the final list of trees in **any order**.

A **full binary tree** is a binary tree where each node has exactly `0` or `2` children.

**Example 1**:
![example 1](/assets/img/posts/2024-06-06-11.png)

**Input**: n = 7

**Output**: [[0,0,0,null,null,0,0,null,null,0,0],[0,0,0,null,null,0,0,0,0],[0,0,0,0,0,0,0],[0,0,0,0,0,null,null,null,null,0,0],[0,0,0,0,0,null,null,0,0]]


**Example 2**:

Input: n = 3

Output: [[0,0,0]]
 

**Constraints:** `1 <= n <= 20`

# 풀이
DFS 풀이 방법 - 트리 그림 그려서 설명
중복이 많아지는 문제가 발생 - 중복되는 부분을 기억함으로써 해결

## 정답 코드

```java
public class Solution {
    // tree memorize list
    private Map<Integer, List<TreeNode>> memo = new HashMap<>();

    public List<TreeNode> allPossibleFBT(int n) {
        if (n % 2 == 0) {
            return new ArrayList<>();  // n이 짝수이면 빈 리스트 반환
        }
        if (n == 1) {
            List<TreeNode> baseCase = new ArrayList<>();
            baseCase.add(new TreeNode(0));
            return baseCase;
        }
        if (memo.containsKey(n)) {
            return memo.get(n);  // 이미 계산된 경우 캐시된 결과 반환
        }

        List<TreeNode> result = new ArrayList<>();

        for (int leftNodes = 1; leftNodes < n; leftNodes += 2) {
            int rightNodes = n - 1 - leftNodes;
            List<TreeNode> leftTrees = allPossibleFBT(leftNodes);
            List<TreeNode> rightTrees = allPossibleFBT(rightNodes);

            for (TreeNode left : leftTrees) {
                for (TreeNode right : rightTrees) {
                    TreeNode root = new TreeNode(0);
                    root.left = left;
                    root.right = right;
                    result.add(root);
                }
            }
        }

        memo.put(n, result);  // 결과를 캐싱
        return result;
    }
}
```