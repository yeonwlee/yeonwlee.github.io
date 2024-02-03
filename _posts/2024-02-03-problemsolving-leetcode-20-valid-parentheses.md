---
title: 문제풀이 - 20. Valid Parentheses
author: yeonwlee
date: 2024-02-03 15:45:00 +0900
categories: [Algorithm, Problem Solving]
tags: [알고리즘, 문자열, 리스트, 스택, 코딩테스트, leetcode, 추가공부]
---

<br>

- [문제](#문제)
- [1. 접근](#1-접근)
- [2. 제출한 답(중첩 괄호 가능성 배제)](#2-제출한-답중첩-괄호-가능성-배제)
- [3. 중첩 괄호 조건에 맞게 제출한 답](#3-중첩-괄호-조건에-맞게-제출한-답)
  - [재접근](#재접근)
- [4. 추가 공부](#4-추가-공부)

---

<br>

## 문제

![image alt 문제](/assets/img/post/2024-02-03-problemsolving-leetcode-20-valid-parentheses/img0.png)

<https://leetcode.com/problems/valid-parentheses/>

## 1. 접근

- 주어지는 문자열은 모두 괄호로만 이뤄져 있어 그 외의 문자열이 포함되어있는지 여부는 확인이 불필요하다.

- 하나의 문자열에 같은 괄호(여닫기 포함)가 여러 개 나올 수 있다.

- **괄호 안에 괄호를 포함하는 개념이 있을 수 있다.** 그런 경우에도 괄호의 대-중-소의 의미는 포함하지 않을 것으로 보인다(예제2)

  --> 하지만 문제에서 제시한 2.Open brackets must be closed in the correct order가 처음엔 조금 모호하게 느껴져서, 이 가능성을 우선 배제하고 단순하게 먼저 풀어봤다.

- 확실하게 틀린 케이스로는,

  1. 문자열의 길이가 홀수다
  2. 닫는 괄호가 먼저 나왔다(열기-닫기-열기 순이 될 순 있을 것)

     가 있을 것이다.

## 2. 제출한 답(중첩 괄호 가능성 배제)

```python
class Solution:
    def isValid(self, parentheses: str) -> bool:
        brackets = {'(': ')', '{': '}', '[':']'}
        if len(parentheses) % 2 == 1:
            return False
        for index in range(0, len(parentheses) - 1, 2):
            if parentheses[index] not in brackets:
                return False
            if parentheses[index + 1] != brackets[parentheses[index]]:
                return False
        return True
```

그리고, 문제에선 중첩 괄호가 허용하는 것이 맞았다.

일단 해당 코드의 시간 복잡도는 O(n)이다(상수 계수 1/2 무시)

## 3. 중첩 괄호 조건에 맞게 제출한 답

### 재접근

처음에는 어떻게 접근해야하나 싶었다. 그러다가,

- 하나 하나의 요소를 리스트에 넣되, 그 괄호의 짝이되는 닫는 괄호가 나오면 해당 요소를 꺼내면 어떨까? 싶었다.

  - 꺼낼 요소가 없거나 꺼냈는데 닫는 괄호와 짝이 안 맞으면 False일 거고.

  - 마지막에 리스트가 비어있으면 True일 거고

이렇게 생각해보다보니 '어, 이게 스택을 이용하는 건가' 싶더라.

그래서 변수명도 stack으로 지어봤다.

```python
class Solution:
    def isValid(self, parentheses: str) -> bool:
        close_brackets = {')': '(', '}': '{', ']':'['}
        if len(parentheses) % 2 == 1:
            return False
        stack = []
        for index, char in enumerate(parentheses):
            if char in close_brackets:
                if not stack or stack.pop() != close_brackets[char]:
                    return False
            else:
                stack.append(char)
        return not stack ## 모두 비어있으면 True

```

## 4. 추가 공부

더 빠르게 동작하도록 변경할 순 없을까?

스택
