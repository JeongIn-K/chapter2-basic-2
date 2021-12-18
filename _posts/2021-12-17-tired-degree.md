---
title: 피로도
excerpt: https://programmers.co.kr/learn/courses/30/lessons/87946
date: 2021-12-18 09:25:00 +0800
categories: [코딩테스트]
tags: [프로그래머스Level2]
toc: true
toc_sticky: true
---

### 문제
* XX게임에는 피로도 시스템(0 이상의 정수로 표현합니다)이 있으며, 일정 피로도를 사용해서 던전을 탐험할 수 있습니다. 이때, 각 던전마다 탐험을 시작하기 위해 필요한 "최소 필요 피로도"와 던전 탐험을 마쳤을 때 소모되는 "소모 피로도"가 있습니다. "최소 필요 피로도"는 해당 던전을 탐험하기 위해 가지고 있어야 하는 최소한의 피로도를 나타내며, "소모 피로도"는 던전을 탐험한 후 소모되는 피로도를 나타냅니다. 예를 들어 "최소 필요 피로도"가 80, "소모 피로도"가 20인 던전을 탐험하기 위해서는 유저의 현재 남은 피로도는 80 이상 이어야 하며, 던전을 탐험한 후에는 피로도 20이 소모됩니다.
* 이 게임에는 하루에 한 번씩 탐험할 수 있는 던전이 여러개 있는데, 한 유저가 오늘 이 던전들을 최대한 많이 탐험하려 합니다. 유저의 현재 피로도 k와 각 던전별 "최소 필요 피로도", "소모 피로도"가 담긴 2차원 배열 dungeons 가 매개변수로 주어질 때, 유저가 탐험할수 있는 최대 던전 수를 return 하도록 solution 함수를 완성해주세요.

### 나의 풀이

```python
import copy
from itertools import permutations

def solution(k, dungeons):
    answer = []
    ventures = list(permutations(dungeons))
    for dungeons in ventures:
        tired_degree = copy.deepcopy(k)
        count = 0
        for start, end in dungeons:
            if tired_degree>=start and tired_degree>=end:
                count+=1
                tired_degree-=end
        answer.append(count)
    return max(answer)
```

ventures = 던전 탐험하는 순서에 따른 방법을 순열로 모두 구함 <br>
7) 순서에 따라 돌 수 있는 던전 수를 구하기 위해 루프<br>
8) 초기 피로도 복사<br>
9) 던전 돌때마다 카운트할 변수<br>
10~13) 던전을 돌수있는 피로도가 충족되면 카운트 증가시키고 피로도 차감<br>
<br>
* * 어제 푼건데 올리는걸 깜빡해서 잔디에 구멍났다 😥😥 또로록<br>

### 다른 풀이
```python
def solution(k, dungeons):
    answer = 0
    dungeons = sorted(dungeons, key = lambda x : ((x[1]+x[0])/x[0],x[1]))
    for x,y in dungeons:
        print("x :", x, "y : ", y)
        if k >= x:
            k -= y
            answer += 1
    return answer
```

```python
def solution(k, dungeons):
    cnt = 0
    dungeons = sorted(dungeons, key=lambda x: (-(x[0]/x[1]),x[1]))
    for n, m in dungeons:
        if k >= n:
            k -= m
            cnt+=1
    return cnt
```
* 소모 피로도와 최소 피로도의 비율을 구하여 오름차순으로 정렬하여 던전을 도는 방식<br>
* 포문 돌리기전에 항상 식으로 생각하는 버릇을 길러야 겠다.<br>


