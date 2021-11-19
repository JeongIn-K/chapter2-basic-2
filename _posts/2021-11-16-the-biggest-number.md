---
title: 가장 큰 수
excerpt: 숫자를 조합해서 가장 큰 수를 만들어 보자
date: 2021-11-16 10:15:00 +0800
categories: [코테연습, python3]
tags: [코딩테스트, 프로그래머스Level2, 정렬, lambda]
toc: true
toc_sticky: true
---

> 프로그래머스 코딩테스트 연습  
> 난이도 Level 2

***

### 문제
* 0 또는 양의 정수가 담긴 배열 numbers가 매개변수로 주어질 때, 순서를 재배치하여 만들 수 있는 가장 큰 수를 문자열로 바꾸어 return 하도록 solution 함수를 작성해주세요.
* 예를 들어, 주어진 정수가 [6, 10, 2]라면 [6102, 6210, 1062, 1026, 2610, 2106]를 만들 수 있고, 이중 가장 큰 수는 6210입니다.

> 제한 사항  
> numbers의 길이는 1 이상 100,000 이하입니다.  
> numbers의 원소는 0 이상 1,000 이하입니다.  
> 정답이 너무 클 수 있으니 문자열로 바꾸어 return 합니다.  


### 나의 풀이
문자열로 내림차순 정렬해서 이어붙이면 되겠지 뭐 ㅎ ~~level2인데 너무 쉽네 ^^~~  **땡! 틀렸습니다!**
```python
def solution(numbers):
    answer = list(sorted(([str(x) for x in numbers]), reverse=True))
    answer = ''.join(answer)
    return answer
```

테스트 케이스 중에 `[3, 30, 34, 5, 9]` 가 있는데
내 코드로 할 경우 [9, 5, 34, **30**, **3**] 으로 정렬되어서 틀리게 된다.
### 그 다음 풀이
```python
def solution(numbers):
    answer = {}
    #numbers의 length는 1이상 100,000 이하
    #numbers의 원소는 0이상 1,000이하

    for num in numbers:
        if num==1000:
            if 0 not in answer.keys():
                answer[0] = [num]
            else:
                answer[0].append(num)
        elif num%10 ==0:
            if 0 not in answer.keys():
                answer[0] = [num]
            else:
                answer[0].append(num)
        elif num > 99:    #100~1000
            if num//100 not in answer.keys():
                answer[num // 100] = [num]
            else:
                answer[num//100].append(num)
        elif num > 9:
            if num // 10 not in answer.keys():
                answer[num // 10] = [num]
            else:
                answer[num//10].append(num)
        else:
            if num not in answer.keys():
                answer[num] = [num]
            else:
                answer[num].append(num)
    value = []
    for key in sorted(answer.keys(), reverse=True):
        value += sorted(answer[key], reverse=True)
    answer = ''.join([str(x) for x in value])
    return answer
```
나의 눈물나는 노오력...
숫자의 시작을 key, 숫자를 value로 딕셔너리에 모두 저장하고 key도 내림차순, value도 내림차순으로 for문 돌려서 출력한다 라는 아이디어였다. 이렇게 한담에 0으로 끝나는 수를 어떻게 할까.....하면서 3시간 가까이 별에 별 짓을 다 하면서 끙끙 거리다 결국 구글링을 해버렸다. 

이 문제에서 주목할 것은 주어진 제한사항이다. 
> numbers의 length는 1이상 100,000 이하  
> numbers의 원소는 0이상 1,000이하

다른 사람들은 주어지는 원소가 최대 3자리이기 때문에 모든 원소를 3번 반복시켜 비교하는 방법을 사용하였다.
예를 들어 테스트 케이스의 `[6, 10, 2]` 원소들을 string으로 변환하여 3번씩 반복하면 `[666, 101010, 222]` 가 됨을 이용하는 것이다.


```python
def solution2(numbers):
    numbers = [str(x)*3 for x in numbers]

	answer = list(sorted(([str(x) for x in numbers]), reverse=True))
    for i, num in enumerate(answer):
        answer[i] = num[:int(len(num)/3)]
    answer = ''.join(answer)
    return answer
```

### 다른 사람 풀이
```python
def solution(numbers):
    answer = list(sorted(([str(x) for x in numbers]), reverse=True, key=lambda x:x*3))
    answer = ''.join(answer)
    return answer
```
> 다른 사람의 풀이를 보니 sorted 함수에 key값을 넣어 정렬 기준을 만들어 정렬하는 방법을 사용하였다. 그러니까 결국 내 처음 풀이에서`key=lambda x:x*3` 만 추가하면 되는거였다 ^_T 또로록.... 정말 재밌다 파이썬.

+추가
원소가 `[0, 0, 0]` 일 때 string으로 그냥 반환하면 `000`이 되므로 `0`을 반환하도록 바꿔줘야 통과. (11번 케이스가 0만 나오는 듯 하다.)

### 통과한 코드
```python
def solution(numbers):
    answer = list(sorted(([str(x) for x in numbers]), reverse=True, key=lambda x:x*3))
    answer = ''.join(answer)
    if answer[0]=='0':
        return answer[0]
    else:
        return answer
```