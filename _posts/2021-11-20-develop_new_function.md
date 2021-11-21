---
title: 기능개발
excerpt:
date: 2021-11-20 23:39:00 +0800
categories: [코테연습, python3]
tags: [코딩테스트, 프로그래머스Level2, 스택/큐, lambda]
toc: true
toc_sticky: true
---

### 문제
* 개발 중인 기능은 진도가 100% 일 때 배포가 가능하다
* 진도와 개발 속도가 주어지면 배포 할 때마다 몇 개의 기능이 배포되는지 return
* 이때 뒤에 있는 기능은 앞에 있는 기능이 먼저 배포되어야 배포 할 수 있다.

### 처음 풀이

```python
import math
def solution(progresses, speeds):
    days = []
    for p,s in zip(progresses, speeds):
        day = math.ceil((100-p)/s)		# 기능마다 걸리는 일 수 계산
        days.append(day)
    answer, temp = [], []
    for i, day in enumerate(days):
        if i+1 > len(days)-1:
            answer.append(len(temp))
            break
        if days[i]<days[i+1]:
            answer.append(len(temp))
            temp = []
    return answer
```
* 바로 직후의 기능만 비교해서 엄청 틀림

### 두 번째 풀이
```python
import math
import numpy as np

def solution(progresses, speeds):
    days = []
    for p, s in zip(progresses, speeds):
        day = math.ceil((100 - p) / s)
        days.append(day)

    answer, temps = [], days[0]
    days = np.array(days)
    while len(days) > 0:
        temps = np.array([days[0]] * len(days))
        days = days - temps
        temp = []
        for i, day in enumerate(days):
            if max(days)==0:
                answer.append(len(days))
                days = []
                break
            if day > 0:
                answer.append(len(days[0:i]))
                days=days[i:]
                break
return answer
```

개발하는 기능마다 걸리는 일수를 계산했고, 뒤에서 먼저 끝난 기능개발에 걸린 일수를 앞에껄로 다 통일 시킬까 하다가 (예를 들어 [7 1 1 9 1] 이면 [7 7 7 9 9] 이런식으로) 귀찮아서 빼버리자 하고 제출했는데 넘어간 코드 또로록... 테스트 케이스마다 속도 편차가 심함. 최대 0.35ms, 최소0.03ms 그것이 O(n2)니까..

### 세 번째
```python
import math
def solution(progresses, speeds):
    days = []
    for p, s in zip(progresses, speeds):
        day = math.ceil((100 - p) / s)
        days.append(day)
    answer, count = [], 1
    for i in range(len(days)):
        try:
            if days[i] < days[i+1]:
                answer.append(count)
                count = 1
            else:
                days[i+1] = days[i]
                count += 1
        except:
            answer.append(count)
	return answer
```
* 앞에 기능보다 덜 걸리면 앞에 꺼로 통일. 뒤에 기능이 더 걸리면 앞에것들 모아서 배포


***

### 다른 풀이1 ☆☆☆
```python
def solution(progresses, speeds):
    Q=[]
    for p, s in zip(progresses, speeds):
        if len(Q)==0 or Q[-1][0]<-((p-100)//s):
            Q.append([-((p-100)//s),1])
        else:
            Q[-1][1]+=1
    return [q[1] for q in Q]
```
* <-보고 R인가.? 했는데 `-((p-100)//s)` 였다. ceil함수 안쓰고 올림하려고 사용한 듯 하다.
* line 3) `Q[-1][0]` 이 out of range 나지 않으려나 했는데 len(Q) **or** 이어서 다음 조건문 안보고 넘어가서 에러 안남
* 처음 걸리는 것 보다 개발일수가 작으면 거기에 count 추가 (아.. 이렇게 푸는거였구나)

꺄.. 진짜 이거 보다가 내 코드 보니까 넘 구리고 뭐 했나 싶고 ㅎㅎ 내꺼로 만들어야겠다


### 다른 풀이2
```python
def solution(progresses, speeds):
    answer = []
    time = 0
    count = 0
    while len(progresses)> 0:
        if (progresses[0] + time*speeds[0]) >= 100:
            progresses.pop(0)
            speeds.pop(0)
            count += 1
        else:
            if count > 0:
                answer.append(count)
                count = 0
            time += 1
    answer.append(count)
    return answer
```
* 댓글 보니 pop은 비효율적이라고 한다.


### 다른 풀이3 (lambda)
```python
daysLeft = list(map(lambda x: (ceil((100 - progresses[x]) / speeds[x])), range(len(progresses))))
```
다른건 똑같은데 걸리는 시간 리스트 만드는 걸 한줄로 했다는게 인상깊음
lambda를 저렇게도 쓸 수 있구나

### 배운 것
* -((p-100)//s) 으로 올림계산이 된다는 것<br>
* or 에서는 앞에 조건 걸리면 뒤에 조건 안본다는 사실 잊고 있었는데 다시 생각 남.<br>
* lambda 사용법 추가<br>