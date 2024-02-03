---
title: 문제풀이 - 28. Find the Index of the First Occurrence in a String
author: yeonwlee
date: 2024-02-03 15:37:00 +0900
categories: [Algorithm, Problem Solving]
tags: [알고리즘, 문자열, 리스트, 코딩테스트, leetcode]
---

<br>

- [문제](#문제)
- [1. 접근](#1-접근)
- [2. 제출한 답](#2-제출한-답)
  - [시간 복잡도](#시간-복잡도)
- [3. 문제가 간단했던 김에 find 구현해보기](#3-문제가-간단했던-김에-find-구현해보기)

---

<br>

## 문제

![image alt 문제](/assets/img/post/2024-02-03-problemsolving-leetcode-28-find-the-index-of-the-first-occurrence-in-a-string/img0.png)

<https://leetcode.com/problems/find-the-index-of-the-first-occurrence-in-a-string/>

## 1. 접근

내장함수가 잘 구현되어 있어서, find를 사용하면 되겠다고 생각했다.

## 2. 제출한 답

```python
class Solution:
    def strStr(self, haystack: str, needle: str) -> int:
        return haystack.find(needle)
```

### 시간 복잡도

평균적으로 O(n) 이라는 것 같다. 최악으론 O(n\*m)이라고 한다.(n: 원본 문자열 길이, m: 찾으려는 문자열 길이)

이는 정확한 정보는 아니다.

## 3. 문제가 간단했던 김에 find 구현해보기

슬라이싱 등을 이용하지 않고 한땀한땀 비교하듯이 구현해보자면

```python
class Solution:
    def strStr(self, haystack: str, needle: str) -> int:
        return self._find(haystack, needle)

    def _find(self, haystack: str, needle: str) -> int:
        for index in range(len(haystack) - len(needle) + 1): #O(n-m+1)
            needle_index = 0
            haystack_index = index
            start_index_same_char = index
            while haystack[haystack_index] == needle[needle_index]: #O(m)
                needle_index += 1
                haystack_index += 1
                if needle_index == len(needle):
                    return start_index_same_char
        return -1
```

이런 식으로 해봤다. 처음 짠 건 이것보다 더 조악했는데.. 이건 좀 정돈한 버전이다.

이 코드에서의 시간 복잡도는 O((n-m+1)\*m)인 셈이니 대략적으로 O(n\*m)이 될 것 같다.
