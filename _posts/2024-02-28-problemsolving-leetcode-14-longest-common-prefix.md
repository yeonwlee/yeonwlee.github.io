---
title: 문제풀이 - 14. Longest Common Prefix
author: yeonwlee
date: 2024-02-28 18:23:00 +0900
categories: [Algorithm, Problem Solving]
tags: [알고리즘, 문자열, 코딩테스트, leetcode]
---

<br>

- [문제](#문제)
- [1. 조건](#1-조건)
- [2. 접근](#2-접근)
- [3. 제출한 답(접근1)](#3-제출한-답접근1)
  - [시간복잡도](#시간복잡도)
- [4. 조금 더 개선(접근2)](#4-조금-더-개선접근2)
- [5. 효율성을 좀 더 높여보기](#5-효율성을-좀-더-높여보기)

---

<br>

## 문제

![image alt 문제](/assets/img/post/2024-02-28-problemsolving-leetcode-14-longest-common-prefix/img0.png)

<https://leetcode.com/problems/longest-common-prefix/>

## 1. 조건

- 1 <= strs.length <= 200

- 0 <= strs[i].length <= 200

- strs[i] consists of only lowercase English letters.

## 2. 접근

1. 접근1. zip으로 풀면서, 해당 인자들이 모두 같은지를 비교하면?
   해당 튜플을 set으로 만들어서 개수가 1개인지를 확인하는게 구현이 빠를 것 같긴 함.

2. 접근2. 하지만 1의 경우, set을 만드는 과정이 들어가기 때문에, 비효율적일 수 있음.
   그 경우를 개선해볼 순 있을 것.

## 3. 제출한 답(접근1)

```python
class Solution:
    def longestCommonPrefix(self, strs: List[str]) -> str:
        result: str = []
        for same_index_chars in zip(*strs):
            if len(set(same_index_chars)) == 1:
                result.append(same_index_chars[0])
            else:
                break
        return ''.join(result)

```

### 시간복잡도

n을 문자열의 수, m을 가장 짧은 문자열의 길이라 했을 때
O(mn)으로 볼 수 있다. zip으로 푸는 과정.

## 4. 조금 더 개선(접근2)

```python
    def longestCommonPrefix(self, strs: List[str]) -> str:
        result: str = []
        for same_index_chars in zip(*strs):
            if all(char == same_index_chars[0] for char in same_index_chars):
                result.append(same_index_chars[0])
            else:
                break
        return ''.join(result)
```

시간복잡도는 동일하게 O(mn)이라고 볼 수 있지만, set으로 만드는 과정을 생략해 조금 더
시간적, 공간적 효율성이 낫긴하다.

## 5. 효율성을 좀 더 높여보기

```python
    def longestCommonPrefix(self, strs: List[str]) -> str:
        result: str = []
        for index in range(len(strs[0])):
            for string in strs:
                # 현재 인덱스 i가 현재 문자열의 길이보다 크거나
                # strs[0]의 현재 문자가 다른 문자열의 현재 문자와 다르면
                if index >= len(string) or strs[0][index] != string[index]:
                    return strs[0][:index]
        return strs[0] # 모든 문자열이 첫번째 문자열과 같은 경우
```

zip으로 모든 동일한 인덱스의 요소를 튜플로 만들지 않은 채로,
직접적으로 문자열간 동일 인덱스를 비교하는 방법이다.

시간 복잡도는 마찬가지로 O(mn)이지만
최악의 경우가 아니라면 이 경우가 시간적으로 나을 가능성이 높다.
하지만 직관성과 가독성이 다소 떨어지고 파이썬스러움이 없어보인다.
