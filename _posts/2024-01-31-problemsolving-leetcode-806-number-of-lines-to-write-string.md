---
title: 문제풀이 - 806. Number of Lines To Write String
author: yeonwlee
date: 2024-01-31 12:30:00 +0900
categories: [Algorithm, Problem Solving]
tags: [알고리즘, 리스트, 딕셔너리, 코딩테스트, leetcode]
---

<br>

- [문제](#문제)
- [1. 접근(오답)](#1-접근오답)
- [2. 제출한 답](#2-제출한-답)
  - [시간복잡도](#시간복잡도)
- [3. 오답 풀이](#3-오답-풀이)
  - [왜 문제를 잘못 파악했을까?](#왜-문제를-잘못-파악했을까)
    - [문제를 제대로 안 읽은게 아닐까](#문제를-제대로-안-읽은게-아닐까)
    - [예시를 제대로 보지 않았다](#예시를-제대로-보지-않았다)
- [4. 다시 접근](#4-다시-접근)
  - [시간 복잡도](#시간-복잡도)

---

<br>

## 문제

![image alt 문제](/assets/img/post/2024-01-31-problemsolving-leetcode-806-number-of-lines-to-write-string/img0.png)

<https://leetcode.com/problems/number-of-lines-to-write-string/>

## 1. 접근(오답)

우선, widths 배열에서 쉽게 정보를 찾아오게 하기 위해 딕셔너리를 만들고,
해당 값을 조회할 땐 해당 딕셔너리를 조회하자.

(오답)
앞 줄을 최대 문자 수로 만들기 위해선 정렬이 필요해보인다.
하지만 앞에서부터 값을 꺼내면 요소를 옮기는 데 시간이 들테니, 거꾸로 정렬해서 pop으로 요소를 꺼내자.

## 2. 제출한 답

```python
import string

def number_of_lines(widths, sentence):
    count_of_lines = 1
    width_info = _make_width_info(widths, string.ascii_lowercase)
    sorted_sentence_by_width = sorted(list(sentence), key=lambda x: width_info[x], reverse=True)
    cur_line_width = 0
    while sorted_sentence_by_width:
        cur_char = sorted_sentence_by_width[-1]
        if cur_line_width + width_info[cur_char] <= 100:
            cur_line_width += width_info[cur_char]
        else:
            count_of_lines += 1
            cur_line_width = width_info[cur_char]
        sorted_sentence_by_width.pop()
    return [count_of_lines, cur_line_width]
def _make_width_info(widths, charset_info):
    width_info = {}
    for index, character in enumerate(charset_info):
        width_info[character] = widths[index]
    return width_info

```

> width_info를 만든다 O(m)
> 문자열을 리스트화해, 역방향으로 정렬한다 O(n log n)
>
> 모든 문자열이 다 없어질 때까지 반복해서 돌되 O(n) 마지막 요소부터 확인.
>
> > > 100 픽셀을 넘어가지 않으면 길이를 더하고
> >
> > > 100 픽셀을 넘어가면 라인 수를 추가 카운트 한 뒤, 라인 너비를 초기화한다
>
> > 마지막 요소를 없앤다

### 시간복잡도

시간 복잡도는 O(n log n)이다.

## 3. 오답 풀이

나는 주어진 문자열의 순서와 상관 없이, 앞 줄일수록 더 많은 수의 문자를 채워넣어야하는 것으로 잘못 생각했다.

이렇게 생각했다 하더라도
정렬을 굳이 내림차순으로해서 pop으로 꺼내기보단
그냥 오름차순으로 한 뒤 순회해줬어도 됐을 것이다. 그게 더 직관적이었을 것 같다.

### 왜 문제를 잘못 파악했을까?

#### 문제를 제대로 안 읽은게 아닐까

특정 키워드에 꽂혀서 나도 모르게 문제를 머릿속에서 새로 만든 것일지도 모른다.

#### 예시를 제대로 보지 않았다

예시를 눈여겨 보지 않았던 것 같다.
만약 잘 살펴봤더라면 내가 이해한 바가 잘못됐다는 것을 알아차리지 않았을까.

**문제를 좀 더 꼼꼼하게 읽고,
주어진 예시와 내가 이해한 바가 맞는지 재확인해보는 과정을 거치자.**

## 4. 다시 접근

```python
import string

class Solution:
    def _make_width_info(self, widths, charset_info):
        width_info = {}
        for index, character in enumerate(charset_info):
            width_info[character] = widths[index]
        return width_info
    def numberOfLines(self, widths: List[int], sentence: str) -> List[int]:
        width_info = self._make_width_info(widths, string.ascii_lowercase)
        count_of_lines = 1
        cur_line_width = 0
        for cur_char in sentence:
            if cur_line_width + width_info[cur_char] <= 100:
                cur_line_width += width_info[cur_char]
            else:
                count_of_lines += 1
                cur_line_width = width_info[cur_char]
        return [count_of_lines, cur_line_width]
```

> width_info를 만든다 O(m) -- m은 26개다
>
> 모든 문자열을 순회하면서 조건에 맞는 값을 취한다 O(n)

### 시간 복잡도

시간복잡도는 O(n)이다.
