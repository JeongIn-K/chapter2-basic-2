---
title: 더 맵게
excerpt: 
date: 2021-12-10 21:21:00 +0800
categories: [코딩테스트]
tags: [프로그래머스Level2, 힙]
toc: true
toc_sticky: true
---

### 문제
* 매운 것을 좋아하는 Leo는 모든 음식의 스코빌 지수를 K 이상으로 만들고 싶습니다. 모든 음식의 스코빌 지수를 K 이상으로 만들기 위해 Leo는 스코빌 지수가 가장 낮은 두 개의 음식을 아래와 같이 특별한 방법으로 섞어 새로운 음식을 만듭니다.
* 섞은 음식의 스코빌 지수 = 가장 맵지 않은 음식의 스코빌 지수 + (두 번째로 맵지 않은 음식의 스코빌 지수 * 2)
* eo는 모든 음식의 스코빌 지수가 K 이상이 될 때까지 반복하여 섞습니다.
* Leo가 가진 음식의 스코빌 지수를 담은 배열 scoville과 원하는 스코빌 지수 K가 주어질 때, 모든 음식의 스코빌 지수를 K 이상으로 만들기 위해 섞어야 하는 최소 횟수를 return 하도록 solution 함수를 작성해주세요.

### 나의 풀이

```python
import heapq
def solution(scoville, K):
    q = []
    answer = 0
    for food in scoville:
        heapq.heappush(q, (food))
    while len(q)>1 and q[0]<K:
        food1 = heapq.heappop(q)
        food2 = heapq.heappop(q)
        new_food = food1 + (food2 * 2)
        heapq.heappush(q, (new_food))
        answer+=1
    if len(q)==1 and min(q)<K:
        return -1
    else:
        return answer
```
첨에 `q[0]` 말고 `min(q)`로 했다가 효율성 0점... 
우선순위 큐로도 해보려고 했는데 효율성이 안넘어간다.

