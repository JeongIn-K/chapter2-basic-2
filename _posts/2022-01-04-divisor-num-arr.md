---
title: 나누어 떨어지는 숫자 배열
excerpt: https://programmers.co.kr/learn/courses/30/lessons/12910
date: 2021-12-14 15:51:00 +0800
categories: [코딩테스트]
tags: [프로그래머스Level1]
toc: true
toc_sticky: true
---

### 문제
* array의 각 element 중 divisor로 나누어 떨어지는 값을 오름차순으로 정렬한 배열을 반환하는 함수, solution을 작성해주세요.
* divisor로 나누어 떨어지는 element가 하나도 없다면 배열에 -1을 담아 반환하세요.

### 나의 풀이

```python
def solution(arr, divisor):
    answer = sorted([x for x in arr if x%divisor==0])
    return answer if len(answer)!=0 else [-1]
```

### 다른 풀이
```python
def solution(arr, divisor):
    answer = [x for x in sorted(arr) if x%divisor==0]
    return  answer or [-1]
```

비어있는 리스트`[]`는 False로 인식한다는 것을 알게 되었다.