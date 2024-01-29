---
title: 문제풀이 - 2418. Sort the People
author: yeonwlee
date: 2024-01-29 23:38:00 +0900
categories: [Algorithm, Problem Solving]
tags: [알고리즘, 리스트, 정렬, 코딩테스트, leetcode]
---

<br>

- [문제](#문제)
- [1. 접근](#1-접근)
- [2. 제출한 답](#2-제출한-답)
  - [시간복잡도](#시간복잡도)

---

<br>

## 문제

![image alt 문제](/assets/img/post/문제풀이-leetcode-2418-sort-the-people/img0.png)

<https://leetcode.com/problems/sort-the-people/>

## 1. 접근

zip 함수를 이용해 각기 다른 정렬의 하나의 값으로 묶는 것에 대해 생각했다. 정답으로 바라는 것이, 키의 내림차순이므로 정렬은 필요할 것으로 보였다.

## 2. 제출한 답

```python
class Solution:
    def sortPeople(self, names: List[str], heights: List[int]) -> List[str]:
        return [info[0] for info in sorted(zip(names, heights), key=lambda x: x[1], reverse=True)]

```

> zip으로 값을 묶어 O(n)
>
> height 내림차순으로 정렬 O(n log n)
>
> 순회하며 이름만 리스트화. O(n)

### 시간복잡도

시간 복잡도는 O(n log n)이다.
