---
title: 프렌즈4블록
excerpt: https://programmers.co.kr/learn/courses/30/lessons/17679
date: 2021-12-16 18:11:00 +0800
categories: [코딩테스트]
tags: [프로그래머스Level2]
toc: true
toc_sticky: true
---

### 문제

* 블라인드 공채를 통과한 신입 사원 라이언은 신규 게임 개발 업무를 맡게 되었다. 이번에 출시할 게임 제목은 "프렌즈4블록".
* 같은 모양의 카카오프렌즈 블록이 2×2 형태로 4개가 붙어있을 경우 사라지면서 점수를 얻는 게임이다.
* 입력으로 블록의 첫 배치가 주어졌을 때, 지워지는 블록은 모두 몇 개인지 판단하는 프로그램을 제작하라.


### 나의 풀이

```python
def shift_blocks(board,m):
    board = list(map(list, zip(*board)))
    new_board = []
    for i in board:
        temp = ''.join(i)
        temp = temp.replace(' ', '')
        temp = ' '* (m-len(temp)) + temp
        new_board.append([i for i in temp])
    return list(map(list, zip(*new_board)))

def solution(m,n, board):
    same_block = set()
    board = [list(i) for i in board]
    count = 0
    while True:
        for i in range(m-1):
            for j in range(n-1):
                if board[i][j]!=' ' and board[i][j]==board[i][j+1] and board[i][j]==board[i+1][j] and board[i+1][j]==board[i+1][j+1]:
                    same_block.add((i,j))
                    same_block.add((i,j+1))
                    same_block.add((i+1,j))
                    same_block.add((i+1,j+1))
        if len(same_block) == 0:
            break
        else:
            count += len(same_block)
            while len(same_block)>=1:
                (i,j) = same_block.pop()
                board[i][j] = ' '
        board = shift_blocks(board,m)

    return count
```

### 해설
`board = [list(i) for i in board]` <br>

* 문자열로 주어지는 입력을 2차원 리스트로 변환


```python
while True:
        for i in range(m-1):
            for j in range(n-1):
                if board[i][j]!=' ' and board[i][j]==board[i][j+1] and board[i][j]==board[i+1][j] and board[i+1][j]==board[i+1][j+1]:
                    same_block.add((i,j))
                    same_block.add((i,j+1))
                    same_block.add((i+1,j))
                    same_block.add((i+1,j+1))
        if len(same_block) == 0:
            break
```

* 같은 블록으로 구성된 2x2 블록이 없을때까지 루프 돌면서 같은 블록의 인덱스를 same_block에 저장

```python
else:
            count += len(same_block)
            while len(same_block)>=1:
                (i,j) = same_block.pop()
                board[i][j] = ' '
```

* 모든 2x2 블록이 동시에 없어져야 하므로 바로 지우지 않고 set에 좌표값 저장해두었다가 일괄적으로 삭제하는 동시에 삭제한 블록 수 count<br>
* 사라진 블록은 공백으로 대체

```python
def shift_blocks(board,m):
    board = list(map(list, zip(*board)))
    new_board = []
    for i in board:
        temp = ''.join(i)
        temp = temp.replace(' ', '')
        temp = ' '* (m-len(temp)) + temp
        new_board.append([i for i in temp])
    return list(map(list, zip(*new_board)))
```

* 블록이 떨어지는걸 구현하기 위해 행과열을 바꾼 후 각 행의 공백을 없앤 문자열 앞에 원래 길이만큼 공백 채워줌
* 다시 행과열을 바꾼 리스트로 반환

매우 재밌는 문제였다 😀