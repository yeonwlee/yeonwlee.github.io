---
title: 문제풀이 - 2937. Make Three Strings Equal
author: yeonwlee
date: 2024-02-18 17:37:00 +0900
categories: [Algorithm, Problem Solving]
tags: [알고리즘, 문자열, 코딩테스트, leetcode]
---

<br>

- [문제](#문제)
- [1. 접근](#1-접근)
- [2. 제출한 답](#2-제출한-답)
  - [시간 복잡도](#시간-복잡도)
- [3. 조금 더 나은 코드](#3-조금-더-나은-코드)

---

<br>

## 문제

![image alt 문제](/assets/img/post/2024-02-18-problemsolving-leetcode-2937-make-three-strings-equal/img0.png)

<https://leetcode.com/problems/make-three-strings-equal/>

## 1. 접근

그냥 앞에서부터 문자열이 같은지 비교해서, 같은 문자열이 나온 경우 같지 않은 오른쪽 문자의 개수를 세면 되지 않을까.

첫글자가 다르면 무조건 다른 것.

## 2. 제출한 답

```python
class Solution:
    def findMinimumOperations(self, s1: str, s2: str, s3: str) -> int:
        if s1[0] == s2[0] and s1[0] == s3[0]:
            shortest_str: str = min(s1, s2, s3, key=len) # O(3)
            for index in range(len(shortest_str), 0, -1): # O(n)
                if (s1[:index] == shortest_str[:index] and # O(n), O(n-1),..
                    s2[:index] == shortest_str[:index] and
                    s3[:index] == shortest_str[:index]):
                    same_value_range: int = index
                    return (len(s1) + len(s2) + len(s3)) - (same_value_range * 3)
        return -1
```

### 시간 복잡도

효율이 무척 떨어지는 코드다. 반복해서 슬라이싱을 하며, 각 문자열이 매우 길고 끝까지 다를 경우 시간이 무척 오래 걸릴 것이다. 추가적으로, 어떤게 shortest_str인지도 몰라 모든 문자열을 다 슬라이싱 하고 있다.

시간복잡도는 O(n^2)이다.

## 3. 조금 더 나은 코드

```python
class Solution2:
    def findMinimumOperations(self, s1: str, s2: str, s3: str) -> int:
        shortest_str_length: int = min(len(s1), len(s2), len(s3)) #O(3)
        last_matched_index: int = -1
        for index in range(shortest_str_length): #O(n)
            if s1[index] != s2[index] or s1[index] != s3[index]:
                break
            last_matched_index = index
        if last_matched_index == -1: #좌측에서부터 어떤 문자도 같지 않을 때
            return -1
        # 각 문자열의 우측에서부터 하나씩 글자를 지워나가서 글자를 같게 만들 때의 연산 수 계산
        return (len(s1) + len(s2) + len(s3)) - (last_match_index + 1) * 3
```

그냥 가장 짧은 문자열의 길이를 기준 삼아,
앞에서부터 문자를 각각 비교해 달라질 때는 바로 반복문을 나와버리는 식이다.
시간복잡도는 O(n)이다.

근데 이 경우도 리턴값이 직관적이진 않은 것 같다.
