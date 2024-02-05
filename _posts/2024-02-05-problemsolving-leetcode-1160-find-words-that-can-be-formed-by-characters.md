---
title: 문제풀이 - 1160. Find Words That Can Be Formed by Characters
author: yeonwlee
date: 2024-02-05 11:55:00 +0900
categories: [Algorithm, Problem Solving]
tags: [알고리즘, 문자열, 카운터, 코딩테스트, leetcode, 추가공부]
---

<br>

- [문제](#문제)
- [1. 접근](#1-접근)
  - [접근1. 카운터 활용- 원본 카운터를 매번 복사](#접근1-카운터-활용--원본-카운터를-매번-복사)
- [2. 제출한 답](#2-제출한-답)
  - [시간 복잡도](#시간-복잡도)
- [3. 개선](#3-개선)
  - [시간복잡도](#시간복잡도)
- [4. 추가공부](#4-추가공부)

---

<br>

## 문제

![image alt 문제](/assets/img/post/2024-02-05-problemsolving-leetcode-1160-find-words-that-can-be-formed-by-characters/img0.png)

<https://leetcode.com/problems/find-words-that-can-be-formed-by-characters/>

## 1. 접근

### 접근1. 카운터 활용- 원본 카운터를 매번 복사

파이썬의 카운터를 이용해 원본 문자별 수를 세보고자 했다.
직접 원본 카운터를 수정하면 다음 단어들에 대해 비교를 할 수 없으니, 각 단어마다 복사한 카운터 객체들을 사용하면 어떨지에 대해 생각했다.

우선은 이렇게 먼저 구현하고 개선안을 생각해보기로 했다.

## 2. 제출한 답

```python
from collections import Counter
import copy

class Solution:
    def countCharacters(self, words: List[str], chars: str) -> int:
        char_counter = Counter(chars) # O(n) n은 chars의 길이
        num_of_can_be_formed = 0
        for word in words: #O(m) m은 words의 길이
            if self._is_all_in_chars(word, copy.deepcopy(char_counter)): # deepcopy O(u) u는 고유 문자의 수
            # _is_all_in_chars의 순회 O(w) w는 word의 길이
                num_of_can_be_formed += len(word)
        return num_of_can_be_formed

    def _is_all_in_chars(self, word, char_counter):
        for char in word:
            if char in char_counter and char_counter[char] != 0:
                char_counter[char] -= 1
            else:
                return False
        return True
```

### 시간 복잡도

시간 복잡도는 O(n) + O(m) \* O(u + w) 로 볼 수 있을 것 같다.

매번 원본 chars 카운터 객체를 복사를 한다는 부분에서 무척 별로라고 느꼈다.

## 3. 개선

```python
from collections import Counter

class Solution2:
    def countCharacters(self, words: List[str], chars: str) -> int:
        source_char_counter = Counter(chars)
        num_of_can_be_formed = 0
        for word in words:
            if self._is_all_in_chars(word, source_char_counter):
                num_of_can_be_formed += len(word)
        return num_of_can_be_formed

    def _is_all_in_chars(self, word, source_char_counter):
        word_char_counter = Counter(word)
        if any(count for word_char, count in word_char_counter.items() if word_char not in source_char_counter or source_char_counter[word_char] < count):
            return False
        return True
```

이번에는 \_is_all_in_chars에서 word의 카운터 객체를 만들어 비교하는 방식을 사용했다. 카운터의 연산을 이용해 더 단순화해보자면,

```python
from collections import Counter

class Solution3:
    def countCharacters(self, words: List[str], chars: str) -> int:
        source_char_counter = Counter(chars) #O(n)
        num_of_can_be_formed = 0
        for word in words: #O(m)
            if self._is_all_in_chars(word, source_char_counter): #O(w)
                num_of_can_be_formed += len(word)
        return num_of_can_be_formed

    def _is_all_in_chars(self, word, source_char_counter):
        word_char_counter = Counter(word) #O(w)
        return source_char_counter > word_char_counter #O(w)
```

로 해볼 수 있을 것 같다.

### 시간복잡도

마지막 모드의 시간복잡도는 O(n + mw) 이다.

## 4. 추가공부

더 나은 코드.
자원을 덜 사용하도록 만들 순 없을까?
