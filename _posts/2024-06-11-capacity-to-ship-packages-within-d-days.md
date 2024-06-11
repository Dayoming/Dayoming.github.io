---
title: "[99클럽 코테 스터디 20일차 TIL] 이분 탐색으로 Capacity To Ship Packages Within D Days 문제 풀이"
date: 2024-06-11
categories: [TIL]
render_with_liquid: false
tags:
    [
        Java,
        이분탐색,
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
> [Leetcode - Capacity To Ship Packages Within D Days](https://leetcode.com/problems/capacity-to-ship-packages-within-d-days/description/) 문제를 보고 풀이한 내용이다.

A conveyor belt has packages that must be shipped from one port to another within `days` days.

The ith package on the conveyor belt has a weight of `weights[i]`. Each day, we load the ship with packages on the conveyor belt (in the order given by `weights`). We may not load more weight than the maximum weight capacity of the ship.

Return the least weight capacity of the ship that will result in all the packages on the conveyor belt being shipped within `days` days.


**Example 1**:

Input: weights = [1,2,3,4,5,6,7,8,9,10], days = 5
Output: 15
Explanation: A ship capacity of 15 is the minimum to ship all the packages in 5 days like this:
1st day: 1, 2, 3, 4, 5
2nd day: 6, 7
3rd day: 8
4th day: 9
5th day: 10

Note that the cargo must be shipped in the order given, so using a ship of capacity 14 and splitting the packages into parts like (2, 3, 4, 5), (1, 6, 7), (8), (9), (10) is not allowed.

**Example 2**:

Input: weights = [3,2,2,4,1,4], days = 3
Output: 6
Explanation: A ship capacity of 6 is the minimum to ship all the packages in 3 days like this:
1st day: 3, 2
2nd day: 2, 4
3rd day: 1, 4

**Example 3**:

Input: weights = [1,2,3,1,1], days = 4
Output: 3
Explanation:
1st day: 1
2nd day: 2
3rd day: 3
4th day: 1, 1
 

**Constraints**:

- `1 <= days <= weights.length <= 5 * 10^4`
- `1 <= weights[i] <= 500`

# 풀이

이번 문제도 [어제 풀었던 문제](https://dayoming.github.io/posts/entry-screening/)와 마찬가지로 `weights`의 제한 조건을 살펴봤다.

`weights`의 최대 크기가 5 * 10^4, 즉 50,000이다. 수 자체가 크지는 않은데, 이 문제를 완전탐색으로 풀 경우 가능한 모든 배송 방법을 시도해 가장 적절한 용량을 찾아야 한다.

모든 가능한 용량에 대해 반복문을 시행하고, 각 용량에 대해 주어진 날짜 안에 모든 패키지를 배송할 수 있는지 확인해야 하므로 가능한 용량의 범위는 1부터 패키지의 총 무게까지이다. 모든 가능한 배송 방법을 시도하기 위해 이 범위를 반복해야 한다.

패키지의 수가 `N`이고 주어진 날짜가 `D`라면 완전탐색으로 문제를 해결하는 데 필요한 시간 복잡도는 대략 `O(N^D)`이다. 이렇게 지수적으로 증가하는 복잡도는 좋지 않다.

패키지의 총 무게에 대해 이진 탐색을 하면 `O(NlogN)`의 시간 복잡도로 더 효율적이게 문제를 해결할 수 있다.

![문제 풀이 1](/assets/img/posts/2024-06-11-1.png)

Example 1을 바탕으로 이진 탐색하는 과정의 일부를 그려보았다.

`left`의 초기값은 **배열의 최대값**으로 초기화되어야 한다. 문제에서 각 패키지는 순서대로 배송되어야 하기 때문에, 가장 큰 패키지의 무게는 최소 용량보다 작거나 같아야 하기 때문이다. 이 조건을 만족하지 않으면 해당 패키지를 혼자 배송하는데 필요한 날이 최소 용량보다 커진다.

`right`의 초기값은 **배열의 총 합**으로 초기화되어야 한다. 모든 패키지를 하루 안에 다 보낼 수 있는 경우가 최대 용량으로 설정할 수 있는 가장 큰 값이기 때문이다.

`mid`값은 매 탐색마다 `(left + right) / 2` 값으로 초기화된다. 여기서 더 작은 값 범위를 탐색해야 할지, 큰 값 범위를 탐색해야 할지 판별해야 하기 때문이다.

`left`가 `right`보다 작거나 같은 동안 계속해서 위 그림과 같은 반복을 수행한다. 현재 `mid` 값을 최대 적재 가능 중량이라고 할 때, 현재 패키지들을 몇 일 만에 배송할 수 있는지 살펴보고 `days`의 값보다 작으면(너무 빨리 배송되면) 실을 수 있는 최대 무게를 줄인다. 즉 왼쪽 범위를 살펴본다. `days`의 값보다 크면(너무 늦게 배송되면) 실을 수 있는 최대 무게를 늘린다. 즉 오른쪽 범위를 살펴본다.


## 정답 코드

```java

public class Solution {
    public int shipWithinDays(int[] weights, int days) {
        // 최소 무게는 배열의 최대 값
        int left = Arrays.stream(weights).max().getAsInt();
        // 최대 최대 무게
        int right = Arrays.stream(weights).sum();
        int answer = 0;

        while (left <= right) {
            int mid = (left + right) / 2;
            int sum = 0;
            int count = 0;
            // 현재 최대 무게로 가능한 배송 일자 구하기
            for (int weight : weights) {
                sum += weight;
                if (sum > mid) {
                    sum = weight;
                    count += 1;
                } else if (sum == mid) {
                    sum = 0;
                    count += 1;
                }
            }

            if (sum != 0) count += 1;

            // 배송해야할 기간보다 짧거나 같은 경우
            if (count <= days) {
                // 현재 최대 무게를 answer에 저장
                answer = mid;
                // 왼쪽 범위 탐색
                right = mid - 1;
            } else { // 배송해야할 기간보다 긴 경우
                // 오른쪽 범위 탐색
                left = mid + 1;
            }
        }

        return answer;
    }
}

```