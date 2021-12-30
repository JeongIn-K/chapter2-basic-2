---
title: 다단계 칫솔 판매
excerpt: https://programmers.co.kr/learn/courses/30/lessons/77486
date: 2021-12-30 15:19:00 +0800
categories: [코딩테스트]
tags: [프로그래머스Level2]
toc: true
toc_sticky: true
---

### 문제
* 모든 판매원은 칫솔의 판매에 의하여 발생하는 이익에서 10% 를 계산하여 자신을 조직에 참여시킨 추천인에게 배분하고 나머지는 자신이 가집니다. 모든 판매원은 자신이 칫솔 판매에서 발생한 이익 뿐만 아니라, 자신이 조직에 추천하여 가입시킨 판매원에게서 발생하는 이익의 10% 까지 자신에 이익이 됩니다. 자신에게 발생하는 이익 또한 마찬가지의 규칙으로 자신의 추천인에게 분배됩니다. 단, 10% 를 계산할 때에는 원 단위에서 절사하며, 10%를 계산한 금액이 1 원 미만인 경우에는 이득을 분배하지 않고 자신이 모두 가집니다.<br>
* 각 판매원의 이름을 담은 배열 enroll, 각 판매원을 다단계 조직에 참여시킨 다른 판매원의 이름을 담은 배열 referral, 판매량 집계 데이터의 판매원 이름을 나열한 배열 seller, 판매량 집계 데이터의 판매 수량을 나열한 배열 amount가 매개변수로 주어질 때, 각 판매원이 득한 이익금을 나열한 배열을 return 하도록 solution 함수를 완성해주세요. 판매원에게 배분된 이익금의 총합을 계산하여(정수형으로), 입력으로 주어진 enroll에 이름이 포함된 순서에 따라 나열하면 됩니다.<br>


### 나의 풀이

```python
def solution(enroll, referral, seller, amount):
    answer = {x:0 for x in enroll}
    refer = {}
    for name, ref in zip(enroll, referral):
        refer[name] = ref

    for s, a in zip(seller, amount):
        profit = a * 100
        while True:
            refPerson = refer[s]
            p = profit - profit//10 if profit//10 >= 1 else profit
            answer[s] += p

            if refPerson == '-' or profit//10 == 0:
                break
            profit = profit//10
            s = refPerson

    return list(answer.values())
```

* 원단위로 절삭: 정수 나눗셈 `//`을 이용하여 1원 이상일 경우만 빼줌.<br>
* 추천인이 있을 경우 profit을 10%로 다시 넘겨주고 추천인으로 변경<br>
* 다음 추천인이 없을때까지 반복<br>
