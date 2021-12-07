---
title: 내적
excerpt:
date: 2021-12-08 00:03:00 +0800
categories: [코테연습, python3]
tags: [프로그래머스Level1]
toc: true
toc_sticky: true
---

### 문제
* 길이가 같은 두 1차원 정수 배열 a, b가 매개변수로 주어집니다. a와 b의 내적을 return 하도록 solution 함수를 완성해주세요.<br>

* 이때, a와 b의 내적은 `a[0]*b[0] + a[1]*b[1] + ... + a[n-1]*b[n-1]` 입니다. (n은 a, b의 길이)<br>

### 나의 풀이

```python
def solution(a, b):
    answer = 0
    for i, j in zip(a,b):
        answer += (i*j)
    return answer
```

### 다른 풀이
```python
solution = lambda x, y: sum(a*b for a, b in zip(x, y))
```
* 한 줄 코딩 ㅎㅎ
