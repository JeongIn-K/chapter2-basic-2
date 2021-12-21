---
title: 행렬 테두리 회전하기
excerpt: https://programmers.co.kr/learn/courses/30/lessons/77485
date: 2021-12-21 10:35:00 +0800
categories: [코딩테스트]
tags: [프로그래머스Level2]
toc: true
toc_sticky: true
---

### 문제
* rows x columns 크기인 행렬이 있습니다. 행렬에는 1부터 rows x columns까지의 숫자가 한 줄씩 순서대로 적혀있습니다. 이 행렬에서 직사각형 모양의 범위를 여러 번 선택해, 테두리 부분에 있는 숫자들을 시계방향으로 회전시키려 합니다. 각 회전은 (x1, y1, x2, y2)인 정수 4개로 표현하며, 그 의미는 다음과 같습니다.
* x1 행 y1 열부터 x2 행 y2 열까지의 영역에 해당하는 직사각형에서 테두리에 있는 숫자들을 한 칸씩 시계방향으로 회전합니다.
* 행렬의 세로 길이(행 개수) rows, 가로 길이(열 개수) columns, 그리고 회전들의 목록 queries가 주어질 때, 각 회전들을 배열에 적용한 뒤, 그 회전에 의해 위치가 바뀐 숫자들 중 가장 작은 숫자들을 순서대로 배열에 담아 return 하도록 solution 함수를 완성해주세요.

### 나의 풀이

```python
import numpy as np

def solution(rows, columns, queries):
    answer = []
    board = np.array([x for x in range(1,columns*rows+1)]).reshape(rows,columns)
    for query in queries:
        rotated = []

        y1, x1, y2, x2 = query
        # 움직일 숫자 넣기
        top = board[(y1 - 1), (x1 - 1):x2]
        rotated.extend(top)

        r_top = top[-1]
        board[(y1 - 1), x1:x2] = top[:-1]
        right = board[y1:y2, (x2 - 1)]
        rotated.extend(right)
        r_bottom = right[-1]
        board[y1+1:y2, (x2-1)] = right[:-1]
        board[y1,x2-1] = r_top

        bottom = board[(y2-1), (x1-1):x2]
        rotated.extend(bottom)
        l_bottom = bottom[0]
        board[(y2-1), (x1-1):(x2-1)] = bottom[1:]
        board[(y2-1),(x2-2)] = r_bottom

        left = board[y1:y2, (x1-1)]
        rotated.extend(left)
        board[y1-1:y2-1, (x1-1)] = left
        board[y2-2,(x1-1)] = l_bottom

        answer.append(min(set(rotated)))
    answer = list(map(int,answer))

    return answer
```

* board : numpy 배열 선언
* 쿼리를 통해 해당 좌표 x1,y1,x2,y2 저장
* top-right-bottom-left 순서로 위치를 변경할 숫자들을 rotated 배열에 넣고 위치를 바꾸어준다.
* top의 경우 y1에 있는 x1~x2에 해당하는 숫자들 중 가장 우측의 숫자를 r_top으로 빼주고 한칸씩 오른쪽으로 밀어준다.
* right의 경우 x2에 있는 y1~y2에 해당하는 숫자들 중 가장 아래쪽의 숫자를 r_bottom으로 빼주고 한칸씩 아래쪽으로 밀어준 뒤 r_top을 y1+1(인덱스는 y1) 에 넣어준다.
* 움직인 숫자들 중 가장 작은 숫자를 answer에 넣어주고 반복.
* `answer = list(map(int,answer))` : json에 np.int64 타입이 없어서 에러 발생. 모든 원소를 int타입으로 변환시켜주었더니 해결.
* 리스트로도 비슷한 방식으로 행, 열 바꾸면서 숫자를 로테이션하면 될 것 같다.



### 다른 풀이

```python
def solution(rows, columns, queries):
    answer = []

    board = [[i+(j)*columns for i in range(1,columns+1)] for j in range(rows)]
    # print(board)

    for a,b,c,d in queries:
        stack = []
        r1, c1, r2, c2 = a-1, b-1, c-1, d-1


        for i in range(c1, c2+1):

            stack.append(board[r1][i])
            if len(stack) == 1:
                continue
            else:
                board[r1][i] = stack[-2]


        for j in range(r1+1, r2+1):
            stack.append(board[j][i])
            board[j][i] = stack[-2]

        for k in range(c2-1, c1-1, -1):
            stack.append(board[j][k])
            board[j][k] = stack[-2]

        for l in range(r2-1, r1-1, -1):
            stack.append(board[l][k])
            board[l][k] = stack[-2]

        answer.append(min(stack))


    return answer
```

* 스택에 2개까지만 넣고 다음 루프때 이전에 넣었던 값을 넣는 방식
* 처음에 구현하고 싶었던 방식이었는데 ㅠㅠ numpy보다 훨씬 빠르다.