---
title: 문제풀이 - 944. Delete Columns to Make Sorted
author: yeonwlee
date: 2024-02-04 19:43:00 +0900
categories: [Algorithm, Problem Solving]
tags: [알고리즘, 문자열, 리스트, 이터레이터, 코딩테스트, leetcode]
---

<br>

- [문제](#문제)
- [1. 접근](#1-접근)
- [2. 제출한 답(1)](#2-제출한-답1)
  - [시간 복잡도](#시간-복잡도)
- [3. 실수했던 부분 - 이터레이터의 특징](#3-실수했던-부분---이터레이터의-특징)
- [4. 이터레이터와 제너레이터](#4-이터레이터와-제너레이터)
- [5. 두 번째 답(2)](#5-두-번째-답2)
  - [시간 복잡도](#시간-복잡도-1)

---

<br>

## 문제

![image alt 문제](/assets/img/post/2024-02-04-problemsolving-leetcode-944-delete-columns-to-make-sorted/img0.png)

<https://leetcode.com/problems/delete-columns-to-make-sorted/>

## 1. 접근

1. zip을 사용해서 row 단위로 묶인 값들을 column 단위로 다시 모은 다음, 정렬한 값과 원래 값을 비교하면 어떨지에 대해 생각했다.

2. 정렬을 반복하기보다, 직접적으로 인덱스의 비교를 해보는 건 어떨지에 대해 생각했다.

1이 2보다 좀 더 파이썬스럽고 구현이 빠를 것 같아 우선 1을 구현,
2의 경우를 추후에 따로 구현해보았다.

## 2. 제출한 답(1)

```python
class Solution:
    def minDeletionSize(self, strs: List[str]) -> int:
        columns = zip(*strs)
        return sum(1 if sorted(col) != list(col) else 0 for col in columns)
```

### 시간 복잡도

\*strs를 언패킹 하는데 걸리는 시간 O(n) (n은 strs의 길이)

zip이 이터레이터를 만드는 시간 O(1)

이터레이터가 튜플을 생성하는 작업 O(nk) (k는 strs의 요소별 길이)
튜플을 리스트로 변환 O(nk)
sorted호출(튜플 정렬 및 리스트 변환) O(nk log k)

여기에서 가장 영향력이 높은 부분은 정렬 부로,
결과적으로 O(nk log k)의 시간복잡도를 가진다.

## 3. 실수했던 부분 - 이터레이터의 특징

zip은 이터레이터를 반환한다.
하지만 이터레이터는 컬렉션의 모든 요소를 메모리에 저장하지 않고, 한 번에 하나씩 요소를 처리한다. 이 말인 즉슨, 한번 사용하면 재사용이 불가능하다는 것이다.

나는 윗부분 코드에서 print 등을 함으로써 해당 이터레이터 객체를 이미 소모한 상태에서 답을 내보려했었다. 어쩐지 값이 안 나오더라.

각 내장 함수의 반환 값을 좀더 유심히 봐둬야겠다.

## 4. 이터레이터와 제너레이터

이터레이터가 좀 더 일반적인 개념, 제너레이터가 이터레이터의 일종이라고 볼 수 있겠다. 즉 모든 제너레이터는 이터레이터이지만, 모든 이터레이터가 제너레이터는 아니다.

제너레이터는 함수 내에서 yield 키워드를 사용하여 값을 하나씩 반환하는 특별한 타입의 이터레이터로, 제너레이터는 함수의 실행 상태를 유지하면서 next() 호출 시마다 함수 실행을 재개하고, 다음 yield 표현까지 실행 후 값을 반환한다. 반면 일반적인 이터레이터는 이러한 실행 상태를 유지하지 않으며, 단순히 컬렉션의 요소를 순회하는 역할만 수행한다고 한다.

제너레이터를 사용자가 이터레이터를 생성하기 위한 한 가지 방법으로 볼 수도 있겠다.

## 5. 두 번째 답(2)

좀 더 전통(?)적인 방식의 접근인 것 같다.

내 생각에는 얘가 더 실행이 빠를 것 같은데, leetcode에선 첫 번째답이 더 빠른 실행을 보이긴 하더라.

```python
class Solution2:
    def minDeletionSize(self, strs: List[str]) -> int:
        num_of_del_cols = 0
        for col_index in range(len(strs[0])):
            for row_index in range(len(strs) - 1):
                if strs[row_index][col_index] > strs[row_index + 1][col_index]:
                    num_of_del_cols += 1
                    break
        return num_of_del_cols
```

### 시간 복잡도

첫 번째 반복문과 두 번째 반복문의 조합으로 O(nk)

장점으로는, 정렬을 하지 않으며 반복문 중에 일찍 중단할 수 있는 가능성이 있다.
단점으로는, 좀 장황하고 파이썬스러운 코드는 아니다.
하지만 strs의 길이가 엄청 커진다면? 고려해볼만하지 않을까.
