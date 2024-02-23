---
title: 문제풀이 - 1880. Check if Word Equals Summation of Two Words
author: yeonwlee
date: 2024-02-23 21:02:00 +0900
categories: [Algorithm, Problem Solving]
tags: [알고리즘, 문자열, 코딩테스트, leetcode, ascii]
---

<br>

- [문제](#문제)
- [1. 조건](#1-조건)
- [2. 접근](#2-접근)
- [3. 제출한 답](#3-제출한-답)
  - [시간복잡도](#시간복잡도)
- [4. 문자를 숫자로 변경하는 과정 수정해보기](#4-문자를-숫자로-변경하는-과정-수정해보기)

---

<br>

## 문제

![image alt 문제](/assets/img/post/2024-02-23-problemsolving-leetcode-1880-check-if-word-equals-summation-of-two-words/img0.png)

<https://leetcode.com/problems/check-if-word-equals-summation-of-two-words/>

## 1. 조건

- 1 <= firstWord.length, secondWord.length, targetWord.length <= 8

- firstWord, secondWord, and targetWord consist of lowercase English letters from 'a' to 'j' inclusive.

## 2. 접근

- ascii 코드를 이용해서 숫자로 치환할 수 있을 것

- 전체를 모두 치환한 다음에 비교하기보다, 뒷자리 수부터 셈을 해나가면서 값이 맞는지 확인해가면 어떨지 ---근데 이렇게 구현하는게 시간이 좀 더 걸릴 테니 쉽게 가자.

## 3. 제출한 답

```python
class Solution:
    def isSumEqual(self, firstWord: str, secondWord: str, targetWord: str) -> bool:
        return (self._change_char_to_integer(firstWord) +
                self._change_char_to_integer(secondWord) ==
                self._change_char_to_integer(targetWord))

    def _change_char_to_integer(self, word: str) -> int:
        result: str = ''
        for char in word:
            result += str(ord(char) - ord('a'))
        return int(result)

```

### 시간복잡도

각각 단어의 길이를 n, m, o라고 할 때
O(n+m+o) 라고 볼 수 있다. 이는 O(n)이다.

## 4. 문자를 숫자로 변경하는 과정 수정해보기

```python
 def _change_char_to_integer(self, word: str) -> int:
    result = 0
    for char in word:
        result = result * 10 + (ord(char) - ord('a'))
    return result

```

기존에는 문자열 연산으로 했으나,
이렇게 계산하면 str->int의 과정을 거치지 않고
바로 int로 만들 수 있다.

앞의 자리에서부터 계산하므로, 뒤 문자로 갈 수록 10을 곱해주는 방식.
