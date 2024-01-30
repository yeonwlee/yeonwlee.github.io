---
title: 문제풀이 - 2824. Count Pairs Whose Sum is Less than Target
author: yeonwlee
date: 2024-01-29 23:38:00 +0900
categories: [Algorithm, Problem Solving]
tags: [알고리즘, 리스트, 정렬, 코딩테스트, leetcode]
---

<br>

- [문제](#문제)
- [1. 접근](#1-접근)
- [2. 제출한 답](#2-제출한-답)
  - [시간복잡도](#시간복잡도)
- [3. (경우에 따라)더 나은 방법](#3-경우에-따라더-나은-방법)
  - [투 포인터(Two Pointers) 알고리즘](#투-포인터two-pointers-알고리즘)
  - [시간 복잡도](#시간-복잡도)
- [4. 파이썬에서 len은 시간복잡도가 어떻게 될까?](#4-파이썬에서-len은-시간복잡도가-어떻게-될까)
  - [왜 O(1)이지?](#왜-o1이지)

---

<br>

## 문제

![image alt 문제](/assets/img/post/problemsolving-leetcode-2824-pairs-whose-sum-is-less-than-target/img0.png)

<https://leetcode.com/problems/count-pairs-whose-sum-is-less-than-target/>

## 1. 접근

nums[i] + nums[j]가 target 보다 적은 값을 찾는 것이니, 정렬을 생각했다. 오름차순으로 정렬해서, 이중 반복문으로 확인하되 합이 target 값을 넘어가면 두 번째 반복문을 break 하면 어떨까.

## 2. 제출한 답

```python
class Solution:
    def countPairs(self, nums: List[int], target: int) -> int:
        nums.sort()
        count = 0
        for index, first_number in enumerate(nums):
            for second_number in nums[index + 1:]:
                if first_number + second_number >= target:
                    break
                count += 1
        return count

```

> nums를 오름차순으로 정렬한다 O(n log n)
>
> nums를 돌면서 O(n)
>
> > nums[현재 인덱스 + 1:]을 돈다 O(n^2)
> >
> > > 두 수의 합이 target 이상이 되면 break

### 시간복잡도

시간 복잡도는 O(n^2)이다. 물론 이렇게까지 될 일은 적을 것 같지만 이론적으로는 그렇다. 언뜻봐도 좀 아쉽다.

## 3. (경우에 따라)더 나은 방법

### 투 포인터(Two Pointers) 알고리즘

주로 정렬된 배열이나 리스트에서 두 개의 요소의 합, 차이, 비교 등을 평가할 때 사용되는 알고리즘 기법이라 한다. 배열의 양 끝 또는 특정 위치에서 시작해 두 개의 포인터를 이용해 배열을 순회하는 것이다. 두개의 포인터는 조건에 따라 서로 멀어지거나 가까워지면서 값을 순회한다.

주 사용 사례는 정렬도니 배열에서 두 수의 합을 찾거나, 정렬된 배열에서 세 수의 합 찾기, 정렬된 배열에서 최소 차이 찾기, 슬라이딩 윈도우-시퀀스에서 주어진 조건을 만족하는 연속된 부분 시퀀스를 찾는 문제 유형- 등이 있다.

```python
class Solution:
    def countPairs(self, nums: List[int], target: int) -> int:
        nums.sort()
        count = 0
        left_pointer, right_pointer = 0, len(nums) - 1
        while left_pointer < right_pointer:
            if nums[left_pointer] + nums[right_pointer] < target:
                count += (right_pointer - left_pointer)
                left_pointer += 1
            else:
                right_pointer -= 1
        return count
```

> nums를 오름차순으로 정렬한다 O(n log n)
> 왼쪽 포인터와 오른쪽 포인터를 지정한다.
> 왼쪽 포인터와 오른쪽 포인터가 교차할 때까지 반복O(n)
>
> > 만약 두 포인터를 인덱스로 갖는 값의 합이 target보다 작을 경우, 두 포인터 사이의 요소의 수를 합하고 왼쪽 포인터를 오른쪽으로 한 칸 옮긴다
>
> > 아닌 경우에는 오른쪽 포인터를 왼쪽으로 한 칸 옮긴다

### 시간 복잡도

시간복잡도는 O(n log n)이다.

## 4. 파이썬에서 len은 시간복잡도가 어떻게 될까?

나는 이것이 혹시 O(n) 일까 싶었다. 요소를 하나하나 돌면서 센다면 그렇게 될 것이기 때문에. 하지만 파이썬에서는 대부분의 내장 데이터 타입(리스트, 문자열, 딕셔너리, 튜플 등)에 대해 O(1)의 시간 복잡도로 동작한다고 한다.

### 왜 O(1)이지?

len함수가, 객체의 길이를 저장하는 내부 속성을 바로 조회하기 때문이다.
