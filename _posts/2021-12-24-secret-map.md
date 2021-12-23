---
title: 비밀지도
excerpt: https://programmers.co.kr/learn/courses/30/lessons/17681
date: 2021-12-24 08:38:00 +0800
categories: [코딩테스트]
tags: [프로그래머스Level1, 진법]
toc: true
toc_sticky: true
---

### 문제
* 지도는 한 변의 길이가 n인 정사각형 배열 형태로, 각 칸은 "공백"(" ") 또는 "벽"("#") 두 종류로 이루어져 있다.
* 전체 지도는 두 장의 지도를 겹쳐서 얻을 수 있다. 각각 "지도 1"과 "지도 2"라고 하자. 지도 1 또는 지도 2 중 어느 하나라도 벽인 부분은 전체 지도에서도 벽이다. 지도 1과 지도 2에서 모두 공백인 부분은 전체 지도에서도 공백이다.
* "지도 1"과 "지도 2"는 각각 정수 배열로 암호화되어 있다.
* 암호화된 배열은 지도의 각 가로줄에서 벽 부분을 1, 공백 부분을 0으로 부호화했을 때 얻어지는 이진수에 해당하는 값의 배열이다.


### 나의 풀이

```python
def solution(n, arr1, arr2):
    answer = []
    for n1, n2 in zip(arr1,arr2):
        map1 = '0'* (n-len(bin(n1)[2:])) + bin(n1)[2:]
        map2 = '0'* (n-len(bin(n2)[2:])) + bin(n2)[2:]
        temp = ''
        for m1, m2 in zip(map1,map2):
            if m1=='1' or m2=='1':
                temp+='#'
            else:
                temp+=' '
        answer.append(temp)
    return answer
```

### 다른 풀이

```python
def solution(n, arr1, arr2):
    answer = []
    for i,j in zip(arr1,arr2):
        a12 = str(bin(i|j)[2:])
        a12=a12.rjust(n,'0')
        a12=a12.replace('1','#')
        a12=a12.replace('0',' ')
        answer.append(a12)
    return answer
```

### bin() 10진법을 2진법으로 표현<br>
* 문자열로 표현된 숫자를 2진법으로 변환해줌<br>
* bin(i|j) 두개의 이진수를 구한뒤 or 연산 결과를 의미<br>
* bin(i|j) 는 i랑 j를 이진변환 하고 같은 자리에 모두 0이 오면 0, 하나라도 1이 있으면 1로 출력<br>
* `str(bin(i|j)[2:])` == `bin(i|j)[2:]`<br>


### rjust, zfill 자릿수 맞춰주는 메서드<br>
* rjust는 문자열 그대로 인식하기 때문에 음수의 경우 `0000-1'` 처럼 채워지고, zfill은 `-00001`과 같이 채워진다.<br>
```python
m1 = '1'
m1.rjust(5,'0')
m1.zfill(5)
```
