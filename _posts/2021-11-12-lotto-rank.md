---
title: 로또의 최고 순위와 최저 순위
excerpt: 프로그래머스 코딩테스트 연습 Level 1
date: 2021-11-13 14:10:00 +0800
categories: [코테연습, python3]
tags: [코딩테스트, 프로그래머스(Level1)]
toc: true
toc_sticky: true
---

> 프로그래머스 코딩테스트 연습
> 난이도 Level 1

***

### 문제 요약
* 일부 번호를 알 수 없는 로또의 최저, 최고 순위 구하기
* 알 수 없는 번호는 최대 6개까지이며 0으로 표시

### 나의 풀이

```python
def solution(lottos, win_nums):
    win_dict = {6:1, 5:2, 4:3, 3:4, 2:5, 1:6, 0:6}
    zeroCount = lottos.count(0)
    inters = set(lottos).intersection(set(win_nums))
    inters_len = len(inters)+zeroCount

    answer = [win_dict[inters_len], win_dict[len(inters)]]
    return answer
```
이미 알고 있는 수의 일치번호로 최저 순위를 구하고, 모르는 숫자가 모두 일치할 경우의 최대 순위를 구함

