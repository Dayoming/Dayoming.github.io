---
title: "[99클럽 코테 스터디 3일차 TIL] Leetcode - Smallest Number In Infinite Set"
date: 2024-05-25
categories: [TIL]
render_with_liquid: false
tags:
    [
        Java,
        Leetcode,
        힙,
        우선순위 큐,
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
You have a set which contains all positive integers `[1, 2, 3, 4, 5, ...]`.

Implement the SmallestInfiniteSet class:

- `SmallestInfiniteSet()` **Initializes** the SmallestInfiniteSet object to contain all positive integers.
- `int popSmallest()` **Removes** and returns the smallest integer contained in the infinite set.
- `void addBack(int num)` **Adds** a positive integer num back into the infinite set, if it is not already in the infinite set.

**Constraints:**
- 1 <= `num` <= 1000
- At most 1000 calls will be made in total to popSmallest and addBack.

`SmallestInfiniteSet()` 생성자는 1부터 1000까지 숫자를 가지고 있는 SmallestInfiniteSet 객체를 만들어야 한다.

`popSmallest()` 메서드는 가장 작은 값을 infinite set에서 반환하고 삭제한다.

`addBack(int num)` 메서드는 `num`이 infinite set에 존재하지 않으면 `num`값을 infinite set에 추가한다.

# 풀이
어제 풀었던 [프로그래머스 - 더 맵게](https://dayoming.github.io/posts/%EB%8D%94-%EB%A7%B5%EA%B2%8C/)문제와 비슷한 문제였다.

이 문제도 **가장 작은 값**을 필요로 하고, 들어오는 값에 따라 **객체를 계속해서 갱신**해주어야 하기 때문에 **우선순위 큐**를 사용하면 쉽게 구현할 수 있겠다는 생각이 들었다.

```java
public class SmallestInfiniteSet {
    private PriorityQueue priorityQueue;

    public SmallestInfiniteSet() {
        this.priorityQueue = new PriorityQueue();
        for (int i = 1; i <= 1000; i++) {
            priorityQueue.add(i);
        }
    }

    public int popSmallest() {
        return (int) priorityQueue.poll();
    }

    public void addBack(int num) {
        if (!priorityQueue.contains(num))
            priorityQueue.add(num);
    }
}
```

```java
public SmallestInfiniteSet() {
        this.priorityQueue = new PriorityQueue();
        for (int i = 1; i <= 1000; i++) {
            priorityQueue.add(i);
        }
    }
```

`priorityQueue` 객체를 생성하고, 1부터 1000까지 값을 넣어준다.

```java
public int popSmallest() {
        return (int) priorityQueue.poll();
    }
```

`priorityQueue`에서 가장 작은 값을 반환한 뒤 삭제한다.

```java
public void addBack(int num) {
    if (!priorityQueue.contains(num))
            priorityQueue.add(num);
}
```

만약 `priorityQueue` 안에 `num`이 존재하지 않을 경우 `num`을 `priorityQueue`안에 넣어준다.