---
title: "[99클럽 코테 스터디 23일차 TIL] 배열 복사와 Subrectangle Queries 문제 풀이"
date: 2024-06-14
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
> [Leetcode - Subrectangle Queries](https://leetcode.com/problems/subrectangle-queries/description/) 문제를 보고 풀이한 내용이다.

Implement the class `SubrectangleQueries` which receives a `rows x cols` rectangle as a matrix of integers in the constructor and supports two methods:

1. `updateSubrectangle(int row1, int col1, int row2, int col2, int newValue)`

Updates all values with `newValue` in the subrectangle whose upper left coordinate is `(row1,col1)` and bottom right coordinate is (row2,col2).
2. `getValue(int row, int col)`

Returns the current value of the coordinate `(row,col)` from the rectangle.
 

**Example 1**:

**Input**
["SubrectangleQueries","getValue","updateSubrectangle","getValue","getValue","updateSubrectangle","getValue","getValue"]
[[[[1,2,1],[4,3,4],[3,2,1],[1,1,1]]],[0,2],[0,0,3,2,5],[0,2],[3,1],[3,0,3,2,10],[3,1],[0,2]]

**Output**

[null,1,null,5,5,null,10,5]

**Explanation**

SubrectangleQueries subrectangleQueries = new SubrectangleQueries([[1,2,1],[4,3,4],[3,2,1],[1,1,1]]);

// The initial rectangle (4x3) looks like:

// 1 2 1

// 4 3 4

// 3 2 1

// 1 1 1

subrectangleQueries.getValue(0, 2); // return 1

subrectangleQueries.updateSubrectangle(0, 0, 3, 2, 5);

// After this update the rectangle looks like:

// 5 5 5

// 5 5 5

// 5 5 5

// 5 5 5 

subrectangleQueries.getValue(0, 2); // return 5

subrectangleQueries.getValue(3, 1); // return 5

subrectangleQueries.updateSubrectangle(3, 0, 3, 2, 10);

// After this update the rectangle looks like:

// 5   5   5

// 5   5   5

// 5   5   5

// 10  10  10 

subrectangleQueries.getValue(3, 1); // return 10

subrectangleQueries.getValue(0, 2); // return 5

# 풀이

문제 풀이 자체는 어렵지 않게 구상할 수 있었는데, (메서드에 주어진 기능을 그대로 구현만 하면 되기 때문에) `SubrectangleQueries` 메서드에서 주어진 `rectangle` 값을 어떻게 배열에 복사하지? 라는 생각에 시간을 많이 썼다.
처음에는 `for`문을 통해 하나하나 대입해주었는데, 생각해보니 `rectangle`이 `int[][]`형 2차원 배열이니 새로운 배열도 `int[][]`로 똑같이 생성해 대입해주면 됐다.

```java
public class SubrectangleQueries {
    private int[][] rectangle;

    // 입력으로 주어진 행렬을 필드로 초기화
    public SubrectangleQueries(int[][] rectangle) {
        this.rectangle = rectangle;
    }

    // 지정된 영역의 값을 newValue로 업데이트
    public void updateSubrectangle(int row1, int col1, int row2, int col2, int newValue) {
        for (int i = row1; i <= row2; i++) {
            for (int j = col1; j <= col2; j++) {
                rectangle[i][j] = newValue;
            }
        }
    }

    public int getValue(int row, int col) {
        return rectangle[row][col];
    }
}
```

헤맸던 시간이 너무 아깝군아..

그래도 `for`문으로 직접 복사하는 방법 외에 배열을 복사하는 방법이 또 뭐가 있을까? 하는 생각이 들어서 공부하게 되었고, 복사 방법이 6가지나 있다는 걸 알게 되었다!

## Java의 배열 복사 방법 6가지

### System.arraycopy

`System.arraycopy` 메서드는 배열을 복사할 때 가장 효율적인 방법 중 하나다. 배열의 특정 범위를 복사할 수 있다.

```java
int[] original = {1, 2, 3, 4, 5};
int[] copy = new int[original.length];
System.arraycopy(original, 0, copy, 0, original.length);
```

### Arrays.copyOf

원본 배열의 길이를 지정하여 새로운 배열을 만든다.

```java
import java.util.Arrays;

int[] original = {1, 2, 3, 4, 5};
int[] copy = Arrays.copyOf(original, original.length);
```

### Arrays.copyOfRange

배열의 특정 범위를 복사할 때 유용하다.

```java
import java.util.Arrays;

int[] original = {1, 2, 3, 4, 5};
int[] copy = Arrays.copyOfRange(original, 1, 4); // [2, 3, 4]
```

### clone

`clone` 메서드는 배열을 복사할 때 간단하게 사용할 수 있다. 하지만 **얕은 복사**를 수행하므로 다차원 배열이나 객체 배열에서는 주의가 필요하다.

❔ 얕은 복사와 깊은 복사
- 얕은 복사는 복사본의 속성이 복사본이 만들어진 원본 객체와 같은 참조(메모리 내의 같은 값을 가리킴)를 공유하는 복사이다. 따라서 원본이다 복사본을 변경하면 다른 객체 또한 변경될 수 있다.
- 깊은 복사는 복사본의 속성이 복사본이 만들어진 원본 객체와 같은 참조(메모리 내의 같은 값을 가리킴)를 공유하지 않는 복사이다. 따라서 원본이나 복사본을 변경할 때, 다른 객체가 변경되지 않는 것을 보장할 수 있다.

```java
int[] original = {1, 2, 3, 4, 5};
int[] copy = original.clone();
```

### 반복문을 사용한 수동 복사

반복문을 사용해 배열을 하나씩 복사할 수 있다. 긴 배열에서는 효율적이지 않을 수 있다.

```java
int[] original = {1, 2, 3, 4, 5};
int[] copy = new int[original.length];
for (int i = 0; i < original.length; i++) {
    copy[i] = original[i];
}
```

### Arrays.setAll

Java 8부터 제공되는 `Arrays.setAll` 메서드를 사용해 배열을 복사할 수 있다. 이 메서드는 배열의 각 요소를 함수형 인터페이스를 통해 설정한다.

```java
import java.util.Arrays;

int[] original = {1, 2, 3, 4, 5};
int[] copy = new int[original.length];
Arrays.setAll(copy, i -> original[i]);
```
