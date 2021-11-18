---
title: 숫자 문자열과 영단어
excerpt: 문자열과 replace()
date: 2021-11-13 14:10:00 +0800
categories:  [코테연습, python3]
tags: [코딩테스트, 프로그래머스Level1]
toc: true
toc_sticky: true
---

> 프로그래머스 코딩테스트 연습
> 난이도 Level 1

***

### 문제
* 네오와 프로도가 숫자놀이를 하고 있습니다. 네오가 프로도에게 숫자를 건넬 때 일부 자릿수를 영단어로 바꾼 카드를 건네주면 프로도는 원래 숫자를 찾는 게임입니다.
> - 1478 → "one4seveneight"
> - 234567 → "23four5six7"
> - 10203 → "1zerotwozero3"
* 이렇게 숫자의 일부 자릿수가 영단어로 바뀌어졌거나, 혹은 바뀌지 않고 그대로인 문자열 `s`가 매개변수로 주어집니다. `s`가 의미하는 원래 숫자를 return 하도록 solution 함수를 완성해주세요.

### 나의 풀이

```python
def solution(s):
    numDict = {'one': '1', 'two': '2', 'three': '3', 'four': '4','five': '5',
                'six': '6', 'seven': '7', 'eight': '8', 'nine': '9','zero':'0'}
    answer = ''
    temp = ''
    for i in s:
        if i in numDict.values():
            answer = answer+i
            s = s[1:]
        else:
            temp = temp+s[0]
            s = s[1:]
        if temp in numDict.keys():
            answer = answer + numDict[temp]
            temp = ''
    return answer
```
풀고 나니 생각난 replace() 함수

```python
def solution(s):
    numDict = {'one': '1', 'two': '2', 'three': '3', 'four': '4','five': '5',
                'six': '6', 'seven': '7', 'eight': '8', 'nine': '9','zero':'0'}

    for key, value in numDict.items():
        answer= s.replace(key, value)

    return answer
```
그렇게 잘 쓰던 replace 함수가 생각이 안나다니.. 문제 많이 풀어봐야겠다.

