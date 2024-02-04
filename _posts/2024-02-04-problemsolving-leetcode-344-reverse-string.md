---
title: 문제풀이 - 344. Reverse String
author: yeonwlee
date: 2024-02-04 20:12:00 +0900
categories: [Algorithm, Problem Solving]
tags: [알고리즘, 문자열, 리스트, 투포인터, 코딩테스트, leetcode]
---

<br>

- [문제](#문제)
- [1. 접근](#1-접근)
  - [접근1. 내장 함수 사용](#접근1-내장-함수-사용)
  - [접근2. 투 포인터 알고리즘 사용](#접근2-투-포인터-알고리즘-사용)
- [2. 제출한 답(1)](#2-제출한-답1)
  - [시간 복잡도](#시간-복잡도)
- [3. 두 번째 답(2)](#3-두-번째-답2)
  - [시간 복잡도](#시간-복잡도-1)

---

<br>

## 문제

![image alt 문제](/assets/img/post/2024-02-04-problemsolving-leetcode-344-reverse-string/img0.png)

<https://leetcode.com/problems/reverse-string/>

## 1. 접근

### 접근1. 내장 함수 사용

내장 함수가 이미 잘 구현되어 있으니 reverse() 사용. 리턴 없이 있는 그대로의 객체 변경을 요했으니 슬라이싱은 아니다.

### 접근2. 투 포인터 알고리즘 사용

## 2. 제출한 답(1)

```python
class Solution:
    def reverseString(self, strs: List[str]) -> None:
        strs.reverse()
```

### 시간 복잡도

reverse의 시간복잡도는 O(n)이라고 한다.

## 3. 두 번째 답(2)

투 포인터라는 접근 방법이 떠올랐다.

[문제풀이 - 2824. Count Pairs Whose Sum is Less than Target](https://yeonwlee.github.io/posts/problemsolving-leetcode-2824-count-pairs-whose-sum-is-less-than-target/)

```python
class Solution:
    def reverseString(self, strs: List[str]) -> None:
        left, right = 0, len(strs) - 1
        while left < right:
            strs[right], strs[left] = strs[left], strs[right]
            left += 1
            right -= 1
            #left, right = left + 1, right - 1
```

처음에는 자리 바꾸기를 위한 변수를 따로 뒀었다가, 파이썬엔 그것이 필요 없다는 것을 깨달아 수정.

간결하긴 한 것 같다.

### 시간 복잡도

시간복잡도는 O(n)이다.
