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
- [6. 정규표현식에 대해(Regular Expression, Regex)](#6-정규표현식에-대해regular-expression-regex)
  - [장단점](#장단점)
  - [내부 작동 방식](#내부-작동-방식)
    - [NFA 엔진:](#nfa-엔진)
    - [DFA 엔진:](#dfa-엔진)

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

## 6. 정규표현식에 대해(Regular Expression, Regex)

정규표현식은 특정 규칙을 이용해 문자열에서 값의 검증, 검색 및 추출, 대체 및 수정 등을 간편히 할 수 있는 도구다.

### 장단점

코드를 단순화할 수 있지만 복잡한 정규표현식은 가독성을 떨어드릴 수 있다.
또 복잡한 정규표현식은-특히 큰 데이터 처리시- 처리 속도가 느려질 수 있다고 한다.

### 내부 작동 방식

정규 표현식 엔진은 주로 두 가지 유형으로 분류된다고 한다.

DFA(Deterministic Finite Automaton)와 NFA(Non-deterministic Finite Automaton).

#### NFA 엔진:

대부분의 정규 표현식 엔진은 NFA를 사용하는데,
NFA 엔진은 주어진 패턴을 만족하는 모든 가능성을 탐색하며, 이 과정에서 백트래킹(backtracking)을 수행할 수 있다고 한다.(이미 탐색한 경로를 되돌아가 다른 가능성을 탐색하는 과정) 이로 인해, NFA 엔진은 특정 상황에서 성능이 저하될 수 있다.

파이썬의 re 모듈은 해당 엔진을 사용한다.

#### DFA 엔진:

DFA 엔진은 각 시점에서 하나의 가능성만을 추적한다. 백트래킹을 수행하지 않으므로 일반적으로 더 빠른 성능을 보이지만, NFA에 비해 표현할 수 있는 패턴의 유연성이 다소 떨어진다고 한다.

해당 엔진을 사용하는 경우는 google의 RE2가 있다.
RE2는 자동화 이론에 기반을 두고 있으며, 선형 시간 복잡도를 유지하고 고정 크기의 스택을 사용한다고 한다.대규모 데이터를 처리하는 데 적합하며, 백트래킹으로 인한 성능 저하 문제를 해결하기 위해 개발되었다고.
