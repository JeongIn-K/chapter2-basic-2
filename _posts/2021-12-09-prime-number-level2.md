---
title: 소수 찾기 level 2
excerpt: 
date: 2021-12-09 23:53:00 +0800
categories: [코딩테스트]
tags: [프로그래머스Level2]
toc: true
toc_sticky: true
---

### 문제
* 한자리 숫자가 적힌 종이 조각이 흩어져있습니다. 흩어진 종이 조각을 붙여 소수를 몇 개 만들 수 있는지 알아내려 합니다.
* 각 종이 조각에 적힌 숫자가 적힌 문자열 numbers가 주어졌을 때, 종이 조각으로 만들 수 있는 소수가 몇 개인지 return 하도록 solution 함수를 완성해주세요.

### 제한사항
* numbers는 길이 1 이상 7 이하인 문자열입니다.
* numbers는 0~9까지 숫자만으로 이루어져 있습니다.
* "013"은 0, 1, 3 숫자가 적힌 종이 조각이 흩어져있다는 의미입니다.

### 나의 풀이

```python
import math
from itertools import permutations

def isPrime(number):
    if number<2:
        return False
    for i in range(2, int(math.sqrt(number))+1):
        if number % i == 0:
            return False
    return True

def solution(numbers):
    n = len(numbers)
    count = 0
    l1 = []
    for i in range(1,len(numbers)+1):
        p = list(permutations(numbers,i))
        for j in p:
            num = ''.join(j)
            l1.append(int(num))
    l1 = set(l1)
    for i in l1:
        if isPrime(i):
            count += 1
    return count
```

permutations 안쓰고 짜고 싶었는데 도저히 생각이 안나서 결국 이걸로 제출 ㅠ.ㅠ<br>
isPrime은 말 그대로 소수인지 아닌지 확인해주는 함수. <br> 
permutations을 이용해서 숫자의 조합을 모두 생성하고 set을 이용해서 중복제거. 중복이 제거된 리스트의 숫자 중 소수일 경우 count+1해주고 리턴
<br>

### 다른 풀이
```python
primeSet = set()


def isPrime(number):
    if number in (0, 1):
        return False
    for i in range(2, number):
        if number % i == 0:
            return False

    return True


def makeCombinations(str1, str2):
    if str1 != "":
        if isPrime(int(str1)):
            primeSet.add(int(str1))

    for i in range(len(str2)):
        makeCombinations(str1 + str2[i], str2[:i] + str2[i + 1:])


def solution(numbers):
    makeCombinations("", numbers)

    answer = len(primeSet)

    return answer
```

permutations 안쓴거 없나 찾다가 재귀함수를 사용한 풀이 발견. 와우 -0- <br>


```python
from itertools import permutations
def solution(n):
    a = set()
    for i in range(len(n)):
        a |= set(map(int, map("".join, permutations(list(n), i + 1))))
    a -= set(range(0, 2))
    for i in range(2, int(max(a) ** 0.5) + 1):
        a -= set(range(i * 2, max(a) + 1, i))
    return len(a)
```

`|` 합집합을 `|=` 이렇게도 쓸수 있다는걸 처음알았다;; (궁금해서 `&=` 해봤는데 안됨..)<br>
소수 찾을 때 `sqrt` 안쓰고  `int(max(a) ** 0.5)`



