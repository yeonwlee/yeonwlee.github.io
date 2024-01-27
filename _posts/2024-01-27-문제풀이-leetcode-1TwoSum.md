---
title: 문제풀이 - 1.TwoSum
author: yeonwlee
date: 2024-01-27 12:18:00 +0900
categories: [Algorithm, Problem Solving]
tags: [알고리즘, 리스트, 딕셔너리, 코딩테스트, leetcode]
---

<br>

---

<br>

## 1. 접근

우선 이중 반복문이 떠올랐다. 한 값을 두고, 다음 값들과 더하는 작업을 반복하는 것이다. 하지만 이러면 첫 번째 값 이후의 모든 값들과 모두 비교를 해야한다. 그래서 정렬을 생각했다.

## 2. 실수

### 2.1 정렬해놓고 정렬된 리스트를 기준으로 답 리턴

원래 리스트를 기준으로 답을 리턴해야하는데, 정렬한 구조에서 답을 리턴하는 해괴한 행위를 저질렀다. 이를 깨닫고 정렬을 하되, (값, 원래의 인덱스)라는 튜플 형태로 가공해서 저장하도록 수정했다.

### 2.2 인풋값이 모두 0이 아닌 양수 정수일 것으로 가정

처음 코드에서, 되도록 반복문 도는 경우를 줄이기위해 조건을 추가했었다. 하지만 해당 조건에 걸려서 원하는 답을 내지 못하는 케이스가 발생했다. 왜냐하면 인풋값은 0도, 음의 정수도 있었기 때문.

이는 문제의 조건을 제대로 확인하지 않은 것이다.
문제의 조건을 잘 읽어보자.

## 3. 제출한 답

```python
class Solution:
    def _sort_with_origin_index(self, source_nums: List[int]) -> List[tuple]:
        value_with_index = [(num, index) for index, num in enumerate(source_nums)]
        return sorted(value_with_index)
    def twoSum(self, source_nums: List[int], target: int) -> List[int]:
        sorted_source_nums_with_index = self._sort_with_origin_index(source_nums)
        for first_value_position, first_value in enumerate(sorted_source_nums_with_index[:len(source_nums) - 1]):
            for second_value in sorted_source_nums_with_index[first_value_position + 1:]:
                sum_value = first_value[0] + second_value[0]
                if sum_value == target:
                    return [first_value[1], second_value[1]]
                elif sum_value > target:
                    break
```

> 값, 원래의 인덱스를 튜플로 만든다 O(n)
> 튜플을 오름차순으로 정렬 O(n log n)
>
> for enumerate(정렬된 튜플[:정렬된 튜플의 길이 -1]) O(n^2)
>
> > for 정렬된 튜플[첫 반복문 인덱스 + 1:]
> >
> > > if 값을 더해 타겟 값이 나오면, 리턴
> > >
> > > elif 값이 더 커지면 두번째 반복문을 나간다

의 구조다.

- 정렬과 데이터를 튜플로 만드는 것을 병행할 순 없었을까?

### 시간복잡도

결국 중첩된 반복문의 영향으로 전체 시간 복잡도는 O(n^2)이다.

제출하고보니, 내 코드의 수행은 상당히 느린 편이었다.

![image alt 수행결과1](/assets/img/post/문제풀이-leetcode-1TwoSum/img1.png)

## 4. (훨씬) 더 나은 방법

방법을 찾아보니, 해시 테이블을 이용하는 것이 있었다.
이미 살펴본 값은 딕셔너리에 넣는 것이다.

왜 이 생각을 못했을까.
아마, **'중복된 값은 굳이 저장할 필요가 없다.'** 란 생각을 못해서가 아니었을까?

```python
class Solution:
  def twoSum(self, source_nums: List[int], target: int) -> List[int]:
      seen = {}  # 값을 키로, 인덱스를 값으로 가질 것
      for index, num in enumerate(source_nums):
          value_for_searching = target - num
          if value_for_searching in seen:
              return [seen[value_for_searching], index]  # 두 번째 숫자의 인덱스와 현재 숫자의 인덱스 반환
          seen[num] = index  # 현재 숫자와 인덱스를 해시 테이블에 추가
```

![image alt 수행결과2](/assets/img/post/문제풀이-leetcode-1TwoSum/img2.png)

실행 속도가 확연히 다른 것을 알 수 있었다.
이 경우에는 최악의 경우, 전체를 돌아보는 것 정도밖에 없다. O(n)

코드도 훨씬 간결하고 보기 편하다.
