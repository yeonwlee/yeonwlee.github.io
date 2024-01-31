---
title: 문제풀이 - 771. Jewels and Stones
author: yeonwlee
date: 2024-01-31 23:12:00 +0900
categories: [Algorithm, Problem Solving]
tags: [알고리즘, 문자열, 리스트, 딕셔너리, 코딩테스트, leetcode, 추가공부]
---

<br>

- [문제](#문제)
- [1. 접근](#1-접근)
- [2. 제출한 답](#2-제출한-답)
  - [시간복잡도](#시간복잡도)
- [3. 실수한 부분 및 개선 방안](#3-실수한-부분-및-개선-방안)
- [4. 제너레이터(generator)](#4-제너레이터generator)
- [5. 추가 공부](#5-추가-공부)

---

<br>

## 문제

![image alt 문제](/assets/img/post/2024-01-31-problemsolving-leetcode-771-jewels-and-stones/img0.png)

<https://leetcode.com/problems/jewels-and-stones/>

## 1. 접근

stones를 딕셔너리로 만들면서 요소별로 개수를 센다.
jewel이 해당 딕셔너리에 있으면, 해댕 요소의 개수를 sum.

## 2. 제출한 답

```python
from collections import defaultdict

class Solution:
    def numJewelsInStones(self, jewels: str, stones: str) -> int:
        stone_info: dict[str, int] = defaultdict(int)
        mapped_jewels = set(jewels)
        for stone in stones:
            stone_info[stone] += 1
        return sum([stone_info[jewel] for jewel in mapped_jewels if jewel in stone_info])
```

> 딕셔너리 생성
> jewels를 set으로 변환 O(n)
> stone_info에 각 문자별 숫자 세기O(m)
> jewels을 순회하면서O(n), 해당 값이 stone_info에 있는 경우인 값들을 합산

### 시간복잡도

시간 복잡도는 O(n+m+n)-> 결과적으로 O(n+m)이다.

## 3. 실수한 부분 및 개선 방안

1. 나는 문제를 읽고 jewels에 중복값이 없음을 알면서도 set으로 변환했다. 이유는 조회(in)에서 O(1)의 시간복잡도를 갖기 위해서였는데.. 이 코드에서는 존재 여부를 조회하는 in이 아니라 for ~ in, 즉 순회를 하고 있다. 기계적으로 생각을 넘기지 말자.
2. 합산할 때 리스트 컴프리헨션을 쓰기 보다 제너레이터를 활용하면 메모리를 절약할 수 있다.

```python
class Solution:
    def numJewelsInStones(self, jewels: str, stones: str) -> int:
        stone_info: dict[str, int] = defaultdict(int)
        for stone in stones:
            stone_info[stone] += 1
        return sum(stone_info[jewel] for jewel in jewels if jewel in stone_info)
```

## 4. 제너레이터(generator)

파이썬의 제너레이터는 반복자(iterator)를 생성하는 도구로,
큰 데이터 집합을 처리할 때나 계산 비용이 큰 연산을 할 때 유용하다고 한다.
그 이유는 전체 데이터를 메모리에 한 번에 로드하지 않고 필요할 때마다 데이터를 하나씩 생성해서 반환하기 때문.
바꿔말하자면, 제너레이터는 값이 필요할 때에서야 실행한다. (마지막으로 yield된 값을 기억하고, 다음 값이 요청될 때 실행을 재개)

## 5. 추가 공부

제너레이터의 활용
