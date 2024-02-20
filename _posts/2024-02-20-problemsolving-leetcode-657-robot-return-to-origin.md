---
title: 문제풀이 - 657. Robot Return to Origin
author: yeonwlee
date: 2024-02-20 21:43:00 +0900
categories: [Algorithm, Problem Solving]
tags: [알고리즘, 문자열, 코딩테스트, leetcode]
---

<br>

- [문제](#문제)
- [1. 접근](#1-접근)
- [2. 제출한 답](#2-제출한-답)
  - [시간 복잡도](#시간-복잡도)
- [3. 로직이 좀 더 눈에 보이게](#3-로직이-좀-더-눈에-보이게)

---

<br>

## 문제

![image alt 문제](/assets/img/post/2024-02-20-problemsolving-leetcode-657-robot-return-to-origin/img0.png)

<https://leetcode.com/problems/robot-return-to-origin/>

## 1. 접근

RLUD 수를 세고, RL 상쇄시키고, UD를 상쇄.

문자열의 길이가 홀수면 모두 상쇄될 수가 없음 -> False

## 2. 제출한 답

```python
from collections import Counter

class Solution:
    def judgeCircle(self, moves: str) -> bool:
        if len(moves) % 2 == 0:
            result: Counter = Counter(moves)
            return result['R'] == result['L'] and result['U'] == result['D']
        return False
```

### 시간 복잡도

Counter 객체를 만들 때의 O(n)이다.

## 3. 로직이 좀 더 눈에 보이게

```python
from collections import Counter

class Solution:
    def judgeCircle(self, moves: str) -> bool:
        if len(moves) % 2 == 0:
            result: Counter = Counter(moves)
            return result['R'] - result['L'] == 0 and result['U'] - result['D'] == 0
        return False

```
