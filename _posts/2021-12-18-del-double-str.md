---
title: 짝지어 제거하기
excerpt: https://programmers.co.kr/learn/courses/30/lessons/12973
date: 2021-12-18 12:05:00 +0800
categories: [코딩테스트]
tags: [프로그래머스Level2]
toc: true
toc_sticky: true
---

### 문제
* 짝지어 제거하기는, 알파벳 소문자로 이루어진 문자열을 가지고 시작합니다. 먼저 문자열에서 같은 알파벳이 2개 붙어 있는 짝을 찾습니다. 그다음, 그 둘을 제거한 뒤, 앞뒤로 문자열을 이어 붙입니다. 이 과정을 반복해서 문자열을 모두 제거한다면 짝지어 제거하기가 종료됩니다. 문자열 S가 주어졌을 때, 짝지어 제거하기를 성공적으로 수행할 수 있는지 반환하는 함수를 완성해 주세요. 성공적으로 수행할 수 있으면 1을, 아닐 경우 0을 리턴해주면 됩니다.

### 나의 풀이

```python
def solution(s):
    answer = []
    for i in s:
        if len(answer)<1:
            answer.append(i)
            continue
        if i==answer[-1]:
            answer.pop()
        else:
            answer.append(i)

    if len(answer)>=1:
        return 0
    else:
        return 1
```

* answer가 비어있으면 원소 하나를 넣고 다음 루프로 continue
* 비어있지 않으면 이전 원소와 현재 원소를 비교해서 같으면 삭제 다르면 answer에 추가
* 최종 길이가 0이면 모든 원소가 제거되었으므로 1, 1이상이면 0을 리턴

