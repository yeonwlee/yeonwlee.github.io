---
title: 문제풀이 - 1047. Remove All Adjacent Duplicates In String
author: yeonwlee
date: 2024-02-04 23:51:00 +0900
categories: [Algorithm, Problem Solving]
tags: [알고리즘, 문자열, 리스트, 스택, 코딩테스트, leetcode]
---

<br>

- [문제](#문제)
- [1. 접근](#1-접근)
- [2. 제출한 답](#2-제출한-답)
  - [시간 복잡도](#시간-복잡도)
- [3. 만약에, 모든 연속된 문자열을 제거하려고 한다면?](#3-만약에-모든-연속된-문자열을-제거하려고-한다면)

---

<br>

## 문제

![image alt 문제](/assets/img/post/2024-02-04-problemsolving-leetcode-1047-remove-all-adjacent-duplicates-in-string/img0.png)

<https://leetcode.com/problems/remove-all-adjacent-duplicates-in-string/>

## 1. 접근

스택을 사용해보자

## 2. 제출한 답

```python
class Solution:
    def removeDuplicates(self, source: str) -> str:
        stack = []
        for index, char in enumerate(source):
            if stack and stack[-1] == char:
                stack.pop()
            else:
                stack.append(char)
        return ''.join(stack)
```

### 시간 복잡도

시간 복잡도는 O(n) 이다.

## 3. 만약에, 모든 연속된 문자열을 제거하려고 한다면?

```python
class Solution2:
    def removeDuplicates(self, source: str) -> str:
        stack = []
        is_dupled = False
        index = 0
        while index < len(source):
            if stack and stack[-1] == source[index]:
                is_dupled = True
            else:
                if is_dupled:
                    stack.pop()
                    is_dupled = False
                    continue
                stack.append(source[index])
            index += 1
        if is_dupled:
            stack.pop()
        return ''.join(stack)
```

테스트한 케이스들에서는 잘 동작하던데, 예외인 경우가 있을까?
