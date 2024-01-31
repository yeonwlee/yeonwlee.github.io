---
title: 문제풀이 - 383. Ransom Note
author: yeonwlee
date: 2024-01-31 19:55:00 +0900
categories: [Algorithm, Problem Solving]
tags: [알고리즘, 문자열, 딕셔너리, 코딩테스트, leetcode, 추가공부]
---

<br>

- [문제](#문제)
- [1. 접근](#1-접근)
- [2. 제출한 답](#2-제출한-답)
  - [시간복잡도](#시간복잡도)
- [3. 좀 더 나은 코드](#3-좀-더-나은-코드)
  - [시간복잡도](#시간복잡도-1)
- [4. 추가 공부](#4-추가-공부)

---

<br>

## 문제

![image alt 문제](/assets/img/post/2024-01-31-problemsolving-leetcode-383-ransom-note/img0.png)

<https://leetcode.com/problems/ransom-note/>

## 1. 접근

magazine에 있는 문자를 소모해서 ransomnote를 만든다는 개념으로 생각했다.
magazine과 ransomnote에 있는 문자들을 딕셔너리로 만들면서 값으로 수를 세어 넣고
magazine이 ransomnote에 필요한 문자보다 해당 문자를 적게 갖고 있으면 False를 리턴하는 방향을 생각했다.

## 2. 제출한 답

```python
from collections import defaultdict

class Solution:
    def canConstruct(self, ransomnote: str, magazine: str) -> bool:
        magazine_char_count = defaultdict(int)
        ransomnote_char_count = defaultdict(int)
        for char in magazine:
            magazine_char_count[char] += 1
        for char in ransomnote: # 굳이 이걸 만들 필요가 없음
            ransomnote_char_count[char] += 1
        for char in ransomnote_char_count.keys():
            if magazine_char_count[char] < ransomnote_char_count[char]:
                return False
        return True
```

> defaultdict으로 magazine과 ransomnote의 문자를 세어넣을 딕셔너리를 생성
>
> magazine을 순회하며 딕셔너리에 값을 넣는다 O(n)
>
> ransomnote를 순회하며 딕셔너리에 값을 넣는다 O(m)
>
> ransomnote로 만들었던 딕셔너리의 키를 순회하면서 O(m)
>
> > 해당 키로 magazine으로 만든 딕셔너리와 ransomnte 딕셔너리의 값을 비교

### 시간복잡도

시간 복잡도는 O(n+m)이다.

## 3. 좀 더 나은 코드

ransomnote 를 순회하면서 딕셔너리를 만들지 않아도 문제를 풀 수 있다.

```python
from collections import defaultdict

class Solution:
    def canConstruct(self, ransomnote: str, magazine: str) -> bool:
        magazine_char_count: dict[str, int] = defaultdict(int)
        for char in magazine:
            magazine_char_count[char] += 1
        for char in ransomnote:
            if magazine_char_count[char] == 0:
                return False
            magazine_char_count[char] -= 1
        return True
```

### 시간복잡도

시간 복잡도는 마찬가지로 O(n+m)이지만
우선 불필요하게 딕셔너리를 만들지 않아, 반복분이 한 번 준다. 공간도 덜 쓰고.

## 4. 추가 공부

더 나은 코드
