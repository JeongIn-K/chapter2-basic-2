---
title: 다리를 지나는 트럭
excerpt:
date: 2021-11-21 22:01:00 +0800
categories: [코테연습, python3]
tags: [프로그래머스Level2, 스택/큐]
toc: true
toc_sticky: true
---

### 문제
* 트럭 여러 대가 강을 가로지르는 일차선 다리를 정해진 순으로 건너려 합니다. 모든 트럭이 다리를 건너려면 최소 몇 초가 걸리는지 알아내야 합니다. 다리에는 트럭이 최대 bridge_length대 올라갈 수 있으며, 다리는 weight 이하까지의 무게를 견딜 수 있습니다. 단, 다리에 완전히 오르지 않은 트럭의 무게는 무시합니다.
* solution 함수의 매개변수로 다리에 올라갈 수 있는 트럭 수 bridge_length, 다리가 견딜 수 있는 무게 weight, 트럭 별 무게 truck_weights가 주어집니다. 이때 모든 트럭이 다리를 건너려면 최소 몇 초가 걸리는지 return 하도록 solution 함수를 완성하세요.

### 첫 번째 풀이
> 테스트 1만 넘어감


```python
def solution(bridge_length, weight, truck_weights):
    time = 0
    q = deque()
    i = 0
    truck_weights = deque(truck_weights)
    while len(truck_weights)>0:
        if len(q) < bridge_length and sum(q)[1]+truck_weights[i]<=weight:
            q.append(truck_weights[i])
            truck_weights.popleft()
            time+=1
        else:
            q.popleft()
            time+=1
    return time+1
```
> 문제에 1초에 1 length 만큼 지나갈수 있다는 설명이 빠져있어서 한참 헤맸다<br>
> 테스트 케이스 2,3 답이 너무 이상해서 질문하기 들어가보니 ㅠㅠ 우쒸 내 시간..<br>


### 두 번째 풀이

```python
def solution(bridge_length, weight, truck_weights):
    time = 0
    bridge = deque()
    for _ in range(bridge_length):
         bridge.append(0)

    while len(truck_weights) > 0 or sum(bridge):
        bridge.popleft()
        time += 1
        if truck_weights:
            if sum(bridge) + truck_weights[0] <= weight:
                bridge.append(truck_weights[0])
                truck_weights.pop(0)
            else:
                bridge.append(0)
    return time
```
> 띠용 이렇게 푸는게 아닌가..;;<br>
> 실행 결과<br>
> 테스트 1 〉	통과 (28.25ms, 10.2MB)<br>
> 테스트 2 〉	통과 (2023.54ms, 10.3MB)<br>
> 테스트 3 〉	통과 (0.06ms, 10.2MB)<br>
> 테스트 4 〉	통과 (383.99ms, 10.2MB)<br>
> 테스트 5 〉	실패 (시간 초과)<br>
> 테스트 6 〉	통과 (2322.42ms, 10.3MB)<br>
> 테스트 7 〉	통과 (7.34ms, 10.3MB)<br>
> 테스트 8 〉	통과 (0.46ms, 10.4MB)<br>
> 테스트 9 〉	통과 (5.46ms, 10.2MB)<br>
> 테스트 10 〉	통과 (0.28ms, 10.3MB)<br>
> 테스트 11 〉	통과 (0.01ms, 10.3MB)<br>
> 테스트 12 〉	통과 (0.45ms, 10.3MB)<br>
> 테스트 13 〉	통과 (1.93ms, 10.3MB)<br>
> 테스트 14 〉	통과 (0.12ms, 10.2MB)<br>

* line 3~5) 1초에 1 length 만큼 지나가도록 하기 위해 0으로 채운 bridge 만듦.<br>
* line 7) `len(truck_weight)>0` 대기중인 트럭이 있거나 `sum(bridge)` 다리에 트럭이 있으면  루프 반복
* line 8~9) 제일 앞의 큐 제거하고 시간 증가
* line 10~15) 현재 다리에 있는 트럭들의 무게랑 포함할 트럭무게 합이 제한하중 이하면 포함 아니면 대기`bridge.append(0)`
* ......pop()때문인가 싶어서 popleft()했는데 그대로 시간 초과. 이렇게 푸는게 아닌가보다..<br>

<br>
### 클래스 사용한 풀이
```python
from collections import deque

class Bridge(object):

    def __init__(self, bridge_length, weight):
        self.bridge_length = bridge_length
        self.max_weight = weight
        self.current_weight = 0
        self.bridge = deque([0]*bridge_length)

    def enqueue(self,truck):
        if self.current_weight+truck <= self.max_weight:
            self.bridge.append(truck)
            self.current_weight += truck
            return False
        else:
            self.bridge.append(0)
            return True

    def deque(self):
        truck = self.bridge.popleft()
        self.current_weight -= truck

    def _print(self):
        print(f"Q: {self.bridge}")
        print(f"current_weight/max_weight: {self.current_weight}/{self.max_weight}")

def solution(bridge_length, weight, truck_weights):
    time = 0
    bridge = Bridge(bridge_length,weight)
    truck_weights = deque(truck_weights)
    while truck_weights or sum(bridge.bridge):
        bridge.deque()
        time+=1
        if truck_weights:
            waitTruck = bridge.enqueue(truck_weights[0])
            if not waitTruck:
                truck_weights.popleft()
        #bridge._print()
    return time
```


* 첨부터 클래스로 만들려고 할때는 넘 어려웠는데 짰던 코드 보면서 하니까 금방 만들었다 뿌듯..<br>
* 클래스로 만드니 속도가 엄청 빨라졌다;; 별 차이 없을 줄 알았는데.. 웬만하면 클래스로 짜려고 노력해야겠다<br>


> 테스트 1 〉	통과 (7.41ms, 10.3MB)<br>
> 테스트 2 〉	통과 (210.05ms, 10.4MB)<br>
> 테스트 3 〉	통과 (0.04ms, 10.3MB)<br>
> 테스트 4 〉	통과 (24.24ms, 10.4MB)<br>
> 테스트 5 〉	통과 (207.72ms, 10.4MB)<br>
> 테스트 6 〉	통과 (71.23ms, 10.3MB)<br>
> 테스트 7 〉	통과 (1.98ms, 10.2MB)<br>
> 테스트 8 〉	통과 (0.18ms, 10.3MB)<br>
> 테스트 9 〉	통과 (4.61ms, 10.4MB)<br>
> 테스트 10 〉	통과 (0.21ms, 10.3MB)<br>
> 테스트 11 〉	통과 (0.01ms, 10.3MB)<br>
> 테스트 12 〉	통과 (0.34ms, 10.4MB)<br>
> 테스트 13 〉	통과 (1.35ms, 10.2MB)<br>
> 테스트 14 〉	통과 (0.07ms, 10.3MB)<br>
***

### 배운 것
* ~~시간 줄이는 건 내일 다시 해봐야지.. ㅠ~~ <br>
* deque, queue 공부<br>
* ~~class로 만들어보기~~ 완성! 굿굿<br>