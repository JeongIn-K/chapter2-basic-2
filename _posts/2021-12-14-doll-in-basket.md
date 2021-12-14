---
title: 크레인 인형뽑기 게임
excerpt:
date: 2021-12-14 15:51:00 +0800
categories: [코딩테스트]
tags: [프로그래머스Level1]
toc: true
toc_sticky: true
---

### 문제

https://programmers.co.kr/learn/courses/30/lessons/64061

### 나의 풀이

```python
def solution(board,moves):
    answer= 0
    board = list(map(list, zip(*board)))
    q = []
    for i in moves:
        j = 0
        while board[i-1][j]==0 and j<len(board)-1:
            j += 1      # 인형 있는곳까지 들어가기

        if board[i-1][j]!=0:
            try:
                if q[-1]!=board[i - 1][j]: #바구니에 있는 인형 비교해서
                    q.append(board[i - 1][j])   # 다른인형이면 넣고
                elif q[-1]==board[i - 1][j]:
                        q.pop()     # 같은 인형이면 사라짐
                        answer+=1
            except:
                q.append(board[i - 1][j])

            board[i - 1][j] = 0
    return answer*2
```

### 다른 풀이
```python
def solution(board, moves):
    cols = list(map(lambda x: list(filter(lambda y: y > 0, x)), zip(*board)))
    a, s = 0, [0]

    for m in moves:
        if len(cols[m - 1]) > 0:
            if (d := cols[m - 1].pop(0)) == (l := s.pop()):
                a += 2
            else:
                s.extend([l, d])

    return a
```

while을 없앨 방법이 없을까 했는데 이 방법처럼 첨부터 0을 없애고 시작하면 됐었다..
:= 연산자는 파이썬 3.8이상 사용가능하다는데 무슨 연산자인지 모르겠다... 검색해도 안나옴 ㅠㅠ