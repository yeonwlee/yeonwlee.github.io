---
title: 문제풀이 - 520. Detect Capital
author: yeonwlee
date: 2024-02-06 12:53:00 +0900
categories: [Algorithm, Problem Solving]
tags: [알고리즘, 문자열, 코딩테스트, leetcode]
---

<br>

- [문제](#문제)
- [1. 접근](#1-접근)
- [2. 제출한 답](#2-제출한-답)
  - [시간 복잡도](#시간-복잡도)
- [3. 조금 더 나은 코드](#3-조금-더-나은-코드)
- [4. 조금 더 가독성을 높이면서 모듈화해본다면?](#4-조금-더-가독성을-높이면서-모듈화해본다면)

---

<br>

## 문제

![image alt 문제](/assets/img/post/2024-02-06-problemsolving-leetcode-520-detect-capital/img0.png)

<https://leetcode.com/problems/detect-capital/>

## 1. 접근

True인 경우는 모두 3가지 케이스이다.
모두 대문자거나, 모두 소문자거나, 첫글자만 대문자거나.

그렇다면 대문자의 수를 센 다음에 그 수를 가지고 접근해보면 어떨까?

## 2. 제출한 답

```python
class Solution:
    def detectCapitalUse(self, word: str) -> bool:
        capital_chars = [char for char in word if char.isupper()]
        if word[0].isupper():
            return len(capital_chars) == 1 or len(capital_chars) == len(word)
        return not capital_chars
```

### 시간 복잡도

시간복잡도는 O(n)이다. 모든 문자를 순회하는 것.

## 3. 조금 더 나은 코드

```python
class Solution2:
    def detectCapitalUse(self, word: str) -> bool:
        num_of_capital_chars = sum(1 for char in word if char.isupper())
        if word[0].isupper():
            return num_of_capital_chars == 1 or num_of_capital_chars == len(word)
        return not num_of_capital_chars
```

어차피 대문자의 수만을 셀 것이라면 굳이 리스트 컴프리헨션으로 대문자들을 저장해놓을 필요가 없다.

## 4. 조금 더 가독성을 높이면서 모듈화해본다면?

```python
class Solution3:
    def detectCapitalUse(self, word: str) -> bool:
        num_of_capital_letters = sum(1 for char in word if char.isupper())
        return (self._has_one_capital_letter_at_first(word, num_of_capital_letters) or
                self._has_only_capital_letters(word, num_of_capital_letters) or
               not num_of_capital_letters)

    def _is_first_letter_capital(self, word: str):
        """사전 조건: word는 비어있지 않다."""
        return word[0].isupper()

    def _has_one_capital_letter_at_first(self, word: str, num_of_capital_letters: int):
        return self._is_first_letter_capital(word) and num_of_capital_letters == 1

    def _has_only_capital_letters(self, word: str, num_of_capital_letters: int):
        return self._is_first_letter_capital(word) and num_of_capital_letters == len(word)
```

코드의 길이는 더 길다. 그리고 미세하게 더 하는 작업이 많다(추가 조건 비교).

하지만 아무것도 모르는 사람이 코드를 처음 봤을 때 해당 함수가 뭘 하고 있는지 좀 더 잘 읽힐 것 같기도 하다.
