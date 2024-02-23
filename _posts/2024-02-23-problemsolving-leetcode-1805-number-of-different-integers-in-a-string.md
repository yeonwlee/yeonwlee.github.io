---
title: 문제풀이 - 1805. Number of Different Integers in a String
author: yeonwlee
date: 2024-02-23 12:12:00 +0900
categories: [Algorithm, Problem Solving]
tags: [알고리즘, 문자열, 코딩테스트, leetcode, 정규표현식]
---

<br>

- [문제](#문제)
- [1. 접근](#1-접근)
- [2. 제출한 답(오답)](#2-제출한-답오답)
  - [어쩌다가 틀렸는가?](#어쩌다가-틀렸는가)
- [3. 고친 답](#3-고친-답)
- [4. 시간복잡도](#4-시간복잡도)
- [5. 좀 더 직관적으로 만들어보기](#5-좀-더-직관적으로-만들어보기)
- [6. 추가공부](#6-추가공부)

---

<br>

## 문제

![image alt 문제](/assets/img/post/2024-02-23-problemsolving-leetcode-1805-number-of-different-integers-in-a-string/img0.png)

<https://leetcode.com/problems/number-of-different-integers-in-a-string/>

## 1. 접근

1. 숫자가 아닌 것을 공백으로 변환.

근데 리턴할 것이 아니라서 굳이 변환을 안 해도 될 것 같긴 한데, 요구사항이 '공백'으로 변환하는 것이라고 했으니 고려해야하나?

2. 공통된 것은 한 번만 셈 -> set을 활용하자
3. 앞자리에 0이 오는 것은 수로 안 취급해도 될 것 같음--> 실수: 0이 하나인 경우엔 숫자임

## 2. 제출한 답(오답)

```python
class Solution:
    def numDifferentIntegers(self, word: str) -> int:
        is_numeric_before: bool = False
        cur_value_of_num: str = ''
        result: set[int] = set()
        for index, char in enumerate(word):
            if char.isdecimal():
                if not is_numeric_before and char == '0': # 문제의 부분
                    continue
                is_numeric_before = True
                cur_value_of_num += char
            else:
                self._add_num_to_result(cur_value_of_num, result)
                is_numeric_before = False
                cur_value_of_num = ''
        self._add_num_to_result(cur_value_of_num, result)
        return len(result)

    def _add_num_to_result(self, cur_value_of_num: str, result: set[int]) -> None:
        if cur_value_of_num:
            result.add(int(cur_value_of_num))
```

### 어쩌다가 틀렸는가?

0이 한 글자로 끝나면, 그것은 숫자다. 이 경우를 고려하지 못했다.

조건을 추가하다가
고려하지 못한 케이스를 만나 틀리는 경우였다.

## 3. 고친 답

```python
class Solution2:
    def numDifferentIntegers(self, word: str) -> int:
        cur_value_of_num: str = ''
        result: set[int] = set()
        for index, char in enumerate(word):
            if char.isdecimal():
                cur_value_of_num += char
            else:
                self._add_num_to_result(cur_value_of_num, result)
                cur_value_of_num = ''
        self._add_num_to_result(cur_value_of_num, result)
        return len(result)

    def _add_num_to_result(self, cur_value_of_num: str, result: set[int]) -> None:
        if cur_value_of_num:
            result.add(int(cur_value_of_num))
```

is_numeric_before가 필요 없어져서 제거했다.

## 4. 시간복잡도

시간복잡도는 전체를 한 번 도는 데 소모되는 O(n)이다.

## 5. 좀 더 직관적으로 만들어보기

정규표현식을 써볼 수 있다.
코드도 간결하고, 보다 직관적이다.

```python
import re

class Solution3:
    def numDifferentIntegers(self, word: str) -> int:
        # 숫자가 아닌 모든 문자를 공백으로 대체
        only_numeric = re.sub(r'\D', ' ', word)
        numbers = set(int(num) for num in only_numeric.split())
        return len(numbers)
```

## 6. 추가공부

정규표현식
