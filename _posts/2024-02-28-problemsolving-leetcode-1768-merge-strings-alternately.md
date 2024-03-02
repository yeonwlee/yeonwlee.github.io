---
title: 문제풀이 - 1768. Merge Strings Alternately
author: yeonwlee
date: 2024-02-28 19:01:00 +0900
categories: [Algorithm, Problem Solving]
tags: [알고리즘, 문자열, 코딩테스트, leetcode]
---

<br>

- [문제](#문제)
- [1. 조건](#1-조건)
- [2. 접근](#2-접근)
- [3. 제출한 답](#3-제출한-답)
  - [시간복잡도](#시간복잡도)
- [4. 조금 더 파이썬스럽게](#4-조금-더-파이썬스럽게)

---

<br>

## 문제

![image alt 문제](/assets/img/post/2024-02-28-problemsolving-leetcode-1768-merge-strings-alternately/img0.png)

<https://leetcode.com/problems/merge-strings-alternately/>

## 1. 조건

- 1 <= word1.length, word2.length <= 100

- word1 and word2 consist of lowercase English letters.

## 2. 접근

zip을 사용해서 풀어주면서 병합하되, 남는 길이의 값은 뒷쪽에 덧붙여주면 어떨지에 대해 생각했다.

## 3. 제출한 답

```python
class Solution:
    def mergeAlternately(self, word1: str, word2: str) -> str:
        min_length: int = min(len(word1), len(word2))
        merged_word: list[str] = []
        for char_set in zip(word1, word2):
            merged_word.extend(char_set)
        if len(word1) == min_length:
            return ''.join(merged_word) + word2[min_length:]
        return ''.join(merged_word) + word1[min_length:]

```

### 시간복잡도

시간 복잡도는 word1과 word2 중 더 긴 문자열을 n이라고 했을 때 O(n)이다.

## 4. 조금 더 파이썬스럽게

itertools에 zip_longest라는 것이 있다.
기본적인 zip이 가장 짧은 이터러블의 길이까지만 순회하는 반면,
해당 메소드는 가장 긴 이터러블의 길이를 기준으로 순회한다.

대신, fillvalue를 사용해 더 짧은 이터러블이 끝났을 때 해당 값으로 채워 반환한다.(기본값은 None)

```python
from itertools import zip_longest

class Solution:
    def mergeAlternately(self, word1: str, word2: str) -> str:
        merged_word: list[str] = [
            char
            for pair in zip_longest(word1, word2, fillvalue='')
            for char in pair
        ]
        return ''.join(merged_word)

```
