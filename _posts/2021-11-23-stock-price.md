---
title: 주식 가격
excerpt:
date: 2021-11-21 22:01:00 +0800
categories: [코테연습, python3]
tags: [프로그래머스Level2]
toc: true
toc_sticky: true
---

### 문제
* 초 단위로 기록된 주식가격이 담긴 배열 prices가 매개변수로 주어질 때, 가격이 떨어지지 않은 기간은 몇 초인지를 return 하도록 solution 함수를 완성하세요.

> 제한사항<br>
> prices의 각 가격은 1 이상 10,000 이하인 자연수입니다.<br>
> prices의 길이는 2 이상 100,000 이하입니다.<br>

### 나의 풀이
```python
def solution(prices):
    time = [0]*len(prices)
    for i in range(0,len(prices)-1):
        for j in range(i+1,len(prices)):
            if prices[i] <= prices[j]:
                time[i] += 1
            else:
                time[i] += 1
                break
    return time
```

* O(n2)하기 싫어서 끙끙 거리다가 그냥 짜봤는데 효율성까지 통과해버림 <br>
* 스택, 큐 문제니까 스택/큐로 다시 한 번 짜보기.. 근데 생각이 안난다<br>
* line 8~10 안쓰고 제출했었는데 테스트 케이스 1만 통과해서 당황.. 주식 가격이 한 번이라도 떨어지면 time 카운팅에서 제외해야 한다.<br>
* 이게 제일 생각하기 쉬운 코드인가보다 .^^ 똑같이 짠 분들 되게 많다
<br>

***

### 다른 풀이
```python
def solution(prices):
    stack = []
    answer = [0] * len(prices)
    for i in range(len(prices)):
        if stack != []:
            while stack != [] and stack[-1][1] > prices[i]:
                past, _ = stack.pop()
                answer[past] = i - past
        stack.append([i, prices[i]])
    for i, s in stack:
        answer[i] = len(prices) - 1 - i
    return answer
```
* 스택으로 구현한 코드 훨씬 빠르다. -0- 멋져<br>
* line 5) if ~ else : 스택에 원소가 없으면 index랑 price 넣기<br>
* line 6~8) 스택의 맨 뒤 값을 비교해서 price[i]가 작으면 (유지 못하면) stack에서 빼고 유지한 초 넣어주기 인덱스가 곧 초이기 때문에.<br>
* 유지되는건 계속 스택에 포함<br>

항상 마지막 값만 비교하기 때문에 빠른 듯. 굳이 계속 더해주지 않아도 (!!)<br>
<br>
### 배운 것
* 스택 활용! 이해했으니 잊어버릴 때 쯤 직접 짜봐야겠다. 복습복습<br>


