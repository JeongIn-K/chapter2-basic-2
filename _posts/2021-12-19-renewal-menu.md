---
title: 메뉴 리뉴얼
excerpt: https://programmers.co.kr/learn/courses/30/lessons/72411
date: 2021-12-18 12:05:00 +0800
categories: [코딩테스트]
tags: [프로그래머스Level2]
toc: true
toc_sticky: true
---

### 문제
* 레스토랑을 운영하던 스카피는 코로나19로 인한 불경기를 극복하고자 메뉴를 새로 구성하려고 고민하고 있습니다.
* 기존에는 단품으로만 제공하던 메뉴를 조합해서 코스요리 형태로 재구성해서 새로운 메뉴를 제공하기로 결정했습니다. 어떤 단품메뉴들을 조합해서 코스요리 메뉴로 구성하면 좋을 지 고민하던 "스카피"는 이전에 각 손님들이 주문할 때 가장 많이 함께 주문한 단품메뉴들을 코스요리 메뉴로 구성하기로 했습니다.
* 단, 코스요리 메뉴는 최소 2가지 이상의 단품메뉴로 구성하려고 합니다. 또한, 최소 2명 이상의 손님으로부터 주문된 단품메뉴 조합에 대해서만 코스요리 메뉴 후보에 포함하기로 했습니다.

### 나의 풀이

```python
from itertools import combinations

def solution(orders, course):
    menus = {}
    answer = []
    for i in course:
        for j in range(len(orders)):
            comb = map(lambda x:''.join(sorted(x)),list(combinations(orders[j], i)))
            for c in comb:
                try:
                    menus[c]+=1
                except KeyError:
                    menus[c]=1

        keys = [x for x in menus.keys() if len(x) == i]
        vals = [menus[x] for x in menus.keys() if len(x) == i]
        try:
            max_val = max(vals)
            for key in keys:
                if menus[key] == max_val and max_val>1:
                    answer.append(key)
        except:
            answer.extend(keys)
    return sorted(answer)
```
* for문을 세개나 써서 시간초과 될까봐 제출 안하려고 했는데 통과가 되긴 했다.
* 밑에 itertools쓴거보다 빠를 때도 있고, 느릴때는 6배정도 차이(1초- > 6초)


### 다른 풀이
```python
import collections
import itertools

def solution(orders, course):
    result = []

    for course_size in course:
        order_combinations = []
        for order in orders:
            order_combinations += itertools.combinations(sorted(order), course_size)

        most_ordered = collections.Counter(order_combinations).most_common()
        result += [ k for k, v in most_ordered if v > 1 and v == most_ordered[0][1] ]

    return [ ''.join(v) for v in sorted(result) ]
```

* Counter를 써서 most_common()으로 많이 나온 순으로 정렬한 뒤 출력하는 것. 기억해둬야겠다.
* Counter를 써볼까 하긴 했는데 나중에 한 번 구현해보기로.. 😥