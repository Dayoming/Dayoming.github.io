---
title: "[99클럽 코테 스터디 26일차 TIL] Iterator for Combination 문제 풀이"
date: 2024-06-17
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
> [Leetcode - Iterator for Combination](https://leetcode.com/problems/iterator-for-combination/description/) 문제를 보고 풀이한 내용이다.

Design the `CombinationIterator` class:

- `CombinationIterator(string characters, int combinationLength)` Initializes the object with a string `characters` of sorted distinct lowercase English letters and a number `combinationLength` as arguments.
- `next()` Returns the next combination of length `combinationLength` in lexicographical order.
- `hasNext()` Returns `true` if and only if there exists a next combination.
 
**Example 1**:

Input
["CombinationIterator", "next", "hasNext", "next", "hasNext", "next", "hasNext"]
[["abc", 2], [], [], [], [], [], []]

Output
[null, "ab", true, "ac", true, "bc", false]

Explanation

CombinationIterator itr = new CombinationIterator("abc", 2);

itr.next();    // return "ab"

itr.hasNext(); // return True

itr.next();    // return "ac"

itr.hasNext(); // return True

itr.next();    // return "bc"

itr.hasNext(); // return False
 

**Constraints**:

- `1 <= combinationLength <= characters.length <= 15`
- All the characters of `characters` are unique.
- At most `10^4` calls will be made to `next` and `hasNext`.
- It is guaranteed that all calls of the function `next` are valid.

# 백트래킹을 이용한 풀이

```java
public class CombinationIterator {
    List<String> list;
    int posn = 0;

    public CombinationIterator(String characters, int combinationLength) {
        list = new ArrayList<>();
        // 백트래킹으로 문자열 경우의 수 구하기
        backTrack(combinationLength, 0, characters, new ArrayList<>());

    }

    private void backTrack(int k, int index, String str, List<Character> currList) {
        if (currList.size() == k) {
            list.add(currList.stream().map(Object::toString).collect(Collectors.joining()));
            return;
        }

        for (int i = index; i < str.length(); i++) {
            currList.add(str.charAt(i));
            backTrack(k, i + 1, str, currList);
            currList.remove(currList.size() - 1);
        }
    }

    public String next() {
        return (list.get(posn++));
    }

    public boolean hasNext() {
        return list.size() >= posn + 1;
    }
}
```

## 더 공부해보고 싶은 내용
- 백트래킹
- Collector