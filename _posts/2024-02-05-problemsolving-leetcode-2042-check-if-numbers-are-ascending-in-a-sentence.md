---
title: 문제풀이 - 2042. Check if Numbers Are Ascending in a Sentence
author: yeonwlee
date: 2024-02-05 18:33:00 +0900
categories: [Algorithm, Problem Solving]
tags: [알고리즘, 문자열, 코딩테스트, leetcode, 추가공부]
---

<br>

- [문제](#문제)
- [1. 접근](#1-접근)
- [2. 제출한 답](#2-제출한-답)
  - [시간 복잡도](#시간-복잡도)
- [3. 추가공부](#3-추가공부)

---

<br>

## 문제

![image alt 문제](/assets/img/post/2024-02-05-problemsolving-leetcode-2042-check-if-numbers-are-ascending-in-a-sentence/img0.png)

<https://leetcode.com/problems/check-if-numbers-are-ascending-in-a-sentence/>

## 1. 접근

주어진 문자열을 공백으로 split 후, 해당 문자들에서 숫자에 해당하는 값만 추려내서 int화 -> 잘 정렬되어 있는지 확인

## 2. 제출한 답

```python
class Solution:
    def areNumbersAscending(self, sentence: str) -> bool:
        numbers = [int(word) for word in sentence.split() if word.isnumeric()]
        previous_num = float('-inf') #조건으로 보아, 0으로 초기화해도 됨
        for num in numbers:
            if num <= previous_num:
                return False
            previous_num = num
        return True
```

### 시간 복잡도

- split O(n)

문장의 길이를 n이라 할 때, 문자열을 모두 확인해야하므로.

- split 제외, 리스트 컴프리헨션의 시간복잡도는 O(m \* w)

int, isnumeric 각 단어의 최대 길이를 w라고 할 때 각각 O(w).

split으로 생성된 단어의 수를 m이라 할 때.

- 변환된 숫자의 수가 k라 할 때 O(k)

전체 시간 복잡도는
O(n) + O(m \* w) + O(k)

모두 n에 비례해서 나타나므로 단순화자면 O(n)

## 3. 추가공부

isnumeric

int

split
