---
title: 문제풀이 - 2586. Count the Number of Vowel Strings in Range
author: yeonwlee
date: 2024-02-02 11:56:00 +0900
categories: [Algorithm, Problem Solving]
tags: [알고리즘, 문자열, 리스트, 코딩테스트, leetcode]
---

<br>

- [문제](#문제)
- [1. 접근](#1-접근)
- [2. 제출한 답](#2-제출한-답)
  - [시간 복잡도](#시간-복잡도)
- [3. 조건에 따라 고려해 볼 만한 부분](#3-조건에-따라-고려해-볼-만한-부분)
- [4. enumerate에 대한 짤막 공부](#4-enumerate에-대한-짤막-공부)

---

<br>

## 문제

![image alt 문제](/assets/img/post/2024-02-02-problemsolving-leetcode-2586-count-the-number-of-vowel-strings-in-range/img0.png)

<https://leetcode.com/problems/count-the-number-of-vowel-strings-in-range/>

## 1. 접근

우선 유효한 범위는 words[left:right + 1]
해당 범위를 슬라이싱 한 후 words[i]이가 모음으로 시작하고 끝나는지를 확인.

빠른 조회를 위해 set()에 모음들을 저장해도 괜찮을 것 같다.

## 2. 제출한 답

```python
class Solution:
  def vowelStrings(self, words: list[str], left: int, right: int) -> int:
      vowels = {'a', 'e', 'i', 'o', 'u'}
      return len([word for word in words[left:right + 1] if word[0] in vowels and word[-1] in vowels])
```

### 시간 복잡도

슬라이싱 O(n) 슬라이싱 되는 범위만큼의 순회 및 복사

리스트 컴프리헨션 내의 순회 및 조회 O(n)

시간 복잡도는 O(n)이다

## 3. 조건에 따라 고려해 볼 만한 부분

슬라이싱을 하는 데도 해당 범위 만큼의 시간과, 메모리가 소모된다는 것이다.

그렇다면 만약 words.length가 굉장히 커지면서 left, right 범위 역시도 커진다면 슬라이싱이 비효율적이 될 수 있다.

range로 직접 index에 접근하는 것을 고려해볼 수 있을 것 같다.

```python
class Solution2:
    def vowelStrings(self, words: list[str], left: int, right: int) -> int:
        vowels = {'a', 'e', 'i', 'o', 'u'}
        count = 0
        for index in range(left, right + 1):
            word = words[index]
            if word[0] in vowels and word[-1] in vowels:
                count += 1
        return count
```

## 4. enumerate에 대한 짤막 공부

처음에는 enumerate에 시작 인덱스를 지정할 수 있는 걸로 기억해서, range 대신 enumerate를 사용해보려 했다.

하지만 곧 뭔가 이상하다는 것을 깨달았는데, 바로 그 시작 인덱스를 지정한다는 것이 해당 인덱스부터 순회를 한다는 개념이 아니란 것이었다.

시작 인덱스의 지정은 enumerate가 뱉어낼 index의 시작 부를 해당 수부터 한다는 의미다.

enumerate는 모든 요소를 순회한다.
