---
title: 완주하지 못한 선수
excerpt: Key-Value쌍으로 데이터를 빠르게 찾아보자. hash()
date: 2021-11-14 14:10:00 +0800
categories: [코테연습, python3]
tags: [코딩테스트, 프로그래머스(Level1), 해시]
toc: true
---

> 프로그래머스 코딩테스트 연습
> 난이도 Level 1

***

### 문제 요약
* 마라톤에 참여한 선수들의 이름이 담긴 배열 participant와 완주한 선수들의 이름이 담긴 배열 completion이 주어질 때, 완주하지 못한 선수의 이름을 return 하도록 solution 함수를 작성해주세요.
* 완주하지 못한 선수는 "단 한명"

### 나의 풀이
(시간초과)
```python
import copy
def solution(participant, completion):
    res = list(set(participant)-set(completion))
    if len(res)>0:
        return res[0]
    else:
        res= copy.deepcopy(participant)
        for name in completion:
            res.remove(name)
        return res[0]
```
아 너무 쉽네 하고 풀었는데 시간초과가 걸려서 문제를 보니 해시가 쓰여있는 걸 발견.
```python
def solution(participant, completion):
	dit = {}
    hashValue = 0
    for p in participant:
        dit[hash(p)] = p
        hashValue += hash(p)
    for c in completion:
        hashValue -= hash(c)
    return dit[hashValue]
```
해시값을 모두 더하면서 딕셔너리에 이름을 저장한다.
그리고 완주한 사람의 해시값을 하나씩 빼준 후 남은 해시값으로 완주 못한 사람의 이름을 찾는다.

<br>

***

