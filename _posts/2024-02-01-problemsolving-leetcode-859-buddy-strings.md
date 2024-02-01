---
title: 문제풀이 - 859. Buddy strings
author: yeonwlee
date: 2024-02-01 19:23:00 +0900
categories: [Algorithm, Problem Solving]
tags: [알고리즘, 문자열, 리스트, 코딩테스트, leetcode]
---

<br>

- [문제](#문제)
- [1. 접근](#1-접근)
- [2. 실수](#2-실수)
- [3. 제출한 답](#3-제출한-답)
  - [시간복잡도](#시간복잡도)
- [4. 더 나은 코드](#4-더-나은-코드)
- [5. enumerate와() zip()을 함께 사용하기](#5-enumerate와-zip을-함께-사용하기)
  - [참고. itertools 모듈의 takewhile()](#참고-itertools-모듈의-takewhile)
- [6. collections 모듈의 Counter](#6-collections-모듈의-counter)
  - [기본 사용법](#기본-사용법)
  - [주요 기능](#주요-기능)

---

<br>

## 문제

![image alt 문제](/assets/img/post/2024-02-01-problemsolving-leetcode-859-buddy-strings/img0.png)

<https://leetcode.com/problems/buddy-strings/>

## 1. 접근

- s와 goal의 문자열 길이는 다를 수도 있다. 다르다면 같아질 일도 없긴 하겠다.
- s의 문자열 길이나 goal의 문자열 길이가 1이 될 수도 있을 것이다.

접근1.

1. 두 문자열의 구성요소는 같아야한다.
2. 두 문자열이 완전히 동일하다면, 동일한 문자열 간 이동을 해야한다.
3. 두 문자열이 다르지만
   1. 구성요소는 같은 경우
      1. 두 문자의 위치만 다른 경우 True
      2. 세 문자 이상의 위치가 다른 경우 False
   2. 구성요소도 다른 경우 False

## 2. 실수

두 문자열이 다른 부분이 두 군데일 때, s 문자열의 위치를 변경해서 goal 문자열을 만들 수 있는지를 확인하지 않았다.

## 3. 제출한 답

```python
class Solution2:
    def buddyStrings(self, source_sentence: str, goal_to_make: str) -> bool:
        if len(source_sentence) != len(goal_to_make): #O(1)
            return False
        different_position_and_value: dict[int, int] = {}
        count_of_different_postition = 0
        for index in range(len(source_sentence)): #O(n)
            if source_sentence[index] != goal_to_make[index]:
                if (count_of_different_postition := count_of_different_postition + 1) > 3:
                    return False
                different_position_and_value[count_of_different_postition] = index
        if not different_position_and_value: ## 비어있는 경우: 모두 같다
            if len(source_sentence) == len(set(source_sentence)): # O(n)
                return False
            return True
        if len(different_position_and_value) == 2:
            if source_sentence[different_position_and_value[1]] == goal_to_make[different_position_and_value[2]] and source_sentence[different_position_and_value[2]] == goal_to_make[different_position_and_value[1]]:
                return True
        return False
```

### 시간복잡도

시간복잡도는 O(n)이다.

## 4. 더 나은 코드

```python
class Solution3:
    def buddyStrings(self, source_sentence: str, goal_to_make: str) -> bool:
        if len(source_sentence) != len(goal_to_make):
            return False
        mismatched_indices: list[int] = []
        for index, char in enumerate(source_sentence):
            if source_sentence[index] != goal_to_make[index]:
                if len(mismatched_indices) > 2:
                    return False
                mismatched_indices.append(index)
        if not mismatched_indices: ## 모든 문자열이 같다
            return len(source_sentence) > len(set(source_sentence)) ## source_sentence 문자열 내에 같은 문자열이 있으면 True
        if len(mismatched_indices) == 2:
            first, second = mismatched_indices
            return source_sentence[first] == goal_to_make[second] and source_sentence[second] == goal_to_make[first]
        return False
```

1. 변수명 개선
   different_postion_and_value -> mismatched_indices
   변수명과 실제 데이터 저장이 안 맞았고, 직관적이지 않았다.

2. 데이터 타입 변경
   딕셔너리 -> 리스트
   목표하는 문자열과 다른 인덱스 부분을 저장하는 데이터 타입을 변경했다.
   어차피 많아야 2개의 값만 들어가는 형태였다.

3. range, len -> enumerate
4. mismatched_indices의 값을 언패킹해서 변수에 저장
5. if - return 에서 return으로 변경

## 5. enumerate와() zip()을 함께 사용하기

```python
mismatched_indices = [index for index, (source_char, goal_char) in enumerate(zip(source_sentence, goal_to_make)) if source_char != goal_char]
```

이런 식으로, zip과 enumerate를 함께 쓸 수도 있다.
하지만 도중에 break를 할 순 없다.

### 참고. itertools 모듈의 takewhile()

itertools 모듈의 takewhile()을 사용하면 특정 조건이 참인 동안만 요소를 처리할 수 있다고 한다.
takewhile()은 주어진 조건이 False가 되는 첫 번째 요소에서 반복을 중단한다.

```python
from itertools import takewhile

result = [수행할_연산 for item in takewhile(lambda x: 조건, some_iterable)]

```

## 6. collections 모듈의 Counter

특수한 딕셔너리 서브 클래스라고 한다.

해시 가능한 객체를 요소로 하는 이터러블(iterable)을 카운트하는 데 사용되며, 주로 요소의 등장 횟수를 딕셔너리 형태로 쉽게 계산해준다. 각 요소는 딕셔너리의 키로, 해당 요소의 등장 횟수는 키의 값으로 저장된다.(내가 하나하나 딕셔너리에 저장해가면서 셀 필요가 없다는 것)

### 기본 사용법

```python
from collections import Counter

# 리스트의 요소를 카운트
fruits = ['apple', 'banana', 'cherry', 'apple', 'cherry', 'cherry']
fruit_counter = Counter(fruits)

print(fruit_counter)  # 출력: Counter({'cherry': 3, 'apple': 2, 'banana': 1})

```

### 주요 기능

1. 요소의 수 카운트

2. 빈도수 상위 요소 찾기: Counter.most_common(n)

   가장 빈번하게 등장하는 상위 n개 요소와 그 횟수

3. 수학적 연산

   Counter 객체는 덧셈, 뺄셈, 교집합, 합집합과 같은 수학적 연산을 지원

4. 요소의 총 개수 얻기: sum(counter.values())
5. 단일 요소의 카운트 변경: Counter 객체에서 특정 요소의 카운트를 직접 변경 가능
