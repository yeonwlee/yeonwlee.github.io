---
title: 문제풀이 - 1812. Determine Color of a Chessboard Square
author: yeonwlee
date: 2024-02-20 22:36:00 +0900
categories: [Algorithm, Problem Solving]
tags: [알고리즘, 문자열, 코딩테스트, leetcode]
---

<br>

- [문제](#문제)
- [1. 접근](#1-접근)
- [2. 제출한 답](#2-제출한-답)
  - [시간 복잡도](#시간-복잡도)
- [3. 좀 더 직관적으로 만들어보기](#3-좀-더-직관적으로-만들어보기)

---

<br>

## 문제

![image alt 문제](/assets/img/post/2024-02-20-problemsolving-leetcode-1812-determine-color-of-a-chessboard-square/img0.png)

<https://leetcode.com/problems/determine-color-of-a-chessboard-square/>

## 1. 접근

일단 규칙성을 파악했다.
1~8과 a~h의 순서를 홀짝으로 놓고 봤을 때,
홀짝이면 흰색, 아니면 검은색이었다.

어떻게 하면 쉽게 그 부분을 구분할 수 있을지를 생각하다가,
행은 숫자값대로 두되 열 부분을 숫자로 치환해보기로 했다.

홀수와 짝수를 더했을 때만 홀수고
짝짝이나 홀홀을 더했을 때는 짝수인 것을 이용헤 리턴 값을 처리해보면 어떨지 생각했다.

## 2. 제출한 답

```python
class Solution:
    def squareIsWhite(self, coordinates: str) -> bool:
        position_pair: dict[str, int] = {char: index for index, char in enumerate("abcdefgh", 1)}
        return (position_pair[coordinates[0]] + int(coordinates[1])) % 2 == 1
```

### 시간 복잡도

딕셔너리를 만들 때 O(8), 즉 O(1)이다.

판을 n으로 키운다고 하면 하면 O(n)이 되겠다.

## 3. 좀 더 직관적으로 만들어보기

```python
class Solution:
    def squareIsWhite(self, coordinates: str) -> bool:
        row = ord(coordinates[0]) - ord('a') + 1  # 행을 나타내는 문자를 숫자로 변환
        col = int(coordinates[1])
        return (row + col) % 2 == 1
```
