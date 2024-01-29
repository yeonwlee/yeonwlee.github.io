---
title: 문제풀이 - 1464. Maximum Product of Two Elements in an Array
author: yeonwlee
date: 2024-01-29 22:45:00 +0900
categories: [Algorithm, Problem Solving]
tags: [알고리즘, 리스트, 정렬, Timsort, 코딩테스트, leetcode, 추가공부]
---

<br>

- [문제](#문제)
- [1. 접근](#1-접근)
- [2. 제출한 답](#2-제출한-답)
  - [시간복잡도](#시간복잡도)
- [3. 파이썬의 리스트 정렬 알고리즘에 대해](#3-파이썬의-리스트-정렬-알고리즘에-대해)
- [4. 더 나은 방법](#4-더-나은-방법)
- [5. 추가공부](#5-추가공부)

---

<br>

## 문제

![image alt 문제](/assets/img/post/문제풀이-leetcode-2011-maximum-product-of-two-elements-in-an-array/img0.png)

<https://leetcode.com/problems/maximum-product-of-two-elements-in-an-array/>

## 1. 접근

서로 다른 두 인덱스를 각각 -1 한 값을 곱한 것. 결국 두 수는 max와 그 다음 max란 의미이다.
그래서 정렬해서 첫 번째, 두 번째 요소를 가져오는 건 어떨지에 대해 생각했다.

## 2. 제출한 답

```python
class Solution:
    def maxProduct(self, nums: List[int]) -> int:
        nums.sort(reverse=True)
        return (nums[0]-1) * (nums[1]-1)
```

> 내림차순으로 정렬한다. O(n log n)
> 셈해서 리턴.

### 시간복잡도

시간 복잡도는 O(n log n)이다.

## 3. 파이썬의 리스트 정렬 알고리즘에 대해

파이썬의 리스트 정렬에 사용되는 정렬 알고리즘은 Tim Peters가 개발한 Timsort 알고리즘이라고 한다.
병합 정렬(Merge Sort)과 삽입 정렬(Insertion Sort)의 장점을 결합한 알고리즘이며, 데이터가 종종 어느정도는 정렬된 패턴을 가지고 있다는 점에 착안해서 개발됐다. 평균적으로 O(n log n)의 시간 복잡도를 가진다.

## 4. 더 나은 방법

수행 시간이 우수한 편은 아니어서 최대 값, 그 다음으로 큰 값을 찾는 코드로 변경해보았다.

```python
class Solution:
    def maxProduct(self, nums: List[int]) -> int:
        max_value = next_max_value = 0 #조건으로 봤을 때 0보다 작은 수는 없음
        for number in nums:
            if number > max_value:
                next_max_value = max_value
                max_value = number
            elif number > next_max_value:
                next_max_value = number
        return (max_value-1) * (next_max_value-1)

```

코드는 기존 것보다 꽤 길지만 로직 자체는 역시나 단순하다.
시간복잡도는 O(n)으로 조금 더 빠른 편이다.

## 5. 추가공부

더 나은 풀이 방법

병합 정렬

삽입 정렬
