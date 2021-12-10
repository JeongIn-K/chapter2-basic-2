---
title: 소수 찾기 level 1
excerpt: 
date: 2021-12-09 23:43:00 +0800
categories: [코딩테스트]
tags: [프로그래머스level1]
toc: true
toc_sticky: true
---

### 문제
* 1부터 입력받은 숫자 n 사이에 있는 소수의 개수를 반환하는 함수, solution을 만들어 보세요.
* 소수는 1과 자기 자신으로만 나누어지는 수를 의미합니다. (1은 소수가 아닙니다.)

### 나의 풀이

```python
def solution(number):
    prime = set([x for x in range(3, number+1, 2)])
    for i in range(3, number+1, 2):
        if i in prime:
            prime -= set([i for i in range(i*2,number+1,i)])
    return len(prime)+1
```
2의 배수는 2로 나누어 떨어지니 짝수는 무조건 소수가 아님 -> 리스트에서 제외<br>
홀수만 리스트로 만들고 3부터 차례대로 배수를 리스트에서 삭제 (에라토스테네스의 체)<br>
레벨 1인데 시간초과때문에 2보다 오래걸림(ㅠ)

