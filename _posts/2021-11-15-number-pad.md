---
title: 키패드 누르기
excerpt: 키패드를 어떤 손으로 누를까?
date: 2021-11-15 11:15:00 +0800
categories: [코테연습, python3]
tags: [프로그래머스Level1]
toc: true
toc_sticky: true
---

> 프로그래머스 코딩테스트 연습  
> 난이도 Level 1

***

### 문제  
* 순서대로 누를 번호가 담긴 배열 numbers, 왼손잡이인지 오른손잡이인지를 나타내는 문자열 hand가 매개변수로 주어질 때, 각 번호를 누른 엄지손가락이 왼손인 지 오른손인 지를 나타내는 연속된 문자열 형태로 return 하도록 solution 함수를 완성해주세요.  
> 엄지손가락은 상하좌우 4가지 방향으로만 이동할 수 있으며 키패드 이동 한 칸은 거리로 1에 해당합니다.  
> 왼쪽 열의 3개의 숫자 1, 4, 7을 입력할 때는 왼손 엄지손가락을 사용합니다.  
> 오른쪽 열의 3개의 숫자 3, 6, 9를 입력할 때는 오른손 엄지손가락을 사용합니다.  
> 가운데 열의 4개의 숫자 2, 5, 8, 0을 입력할 때는 두 엄지손가락의 현재 키패드의 위치에서 더 가까운 엄지손가락을 사용합니다.  
> 4-1. 만약 두 엄지손가락의 거리가 같다면, 오른손잡이는 오른손 엄지손가락, 왼손잡이는 왼손 엄지손가락을 사용합니다.  

### 나의 풀이
```python
def getDist(tuple1, tuple2):			#거리 구하는 함수
    return (abs(tuple1[0] - tuple2[0])+abs(tuple1[1]-tuple2[1]))/2

def solution(numbers, hand):
    answer=[]
    #숫자 패드에 좌표 설정
    numberPad = {1:(0,0),2:(0,1),3:(0,2),
                 4:(1,0),5:(1,1),6:(1,2),
                 7:(2,0),8:(2,1),9:(2,2),
                 '*':(3,0),0:(3,1),'#':(3,2)}
    hand = hand[0].upper()
    #왼손은 *부터, 오른손은 #부터 시작
    leftHand = (3, 0)
    rightHand = (3, 2)
    for num in numbers:
    #1,4,7은 왼손, 3,6,9는 오른손으로 누르기
        if num in [1,4,7]:
            answer.append("L")
            leftHand = numberPad[num]
        elif num in [3,6,9]:
            answer.append("R")
            rightHand = numberPad[num]
        else:
    #2,5,8,0은 거리 계산해서 정하기
            if getDist(leftHand,numberPad[num]) == getDist(rightHand,numberPad[num]):
                answer.append(hand)
                if hand =='L':
                    leftHand = numberPad[num]
                else:
                    rightHand = numberPad[num]
            elif getDist(leftHand,numberPad[num]) < getDist(rightHand,numberPad[num]):
                answer.append("L")
                leftHand = numberPad[num]
            else:
                answer.append("R")
                rightHand = numberPad[num]
    return ''.join(answer)
```

<br>
### 다른 사람 풀이
```python
def solution(numbers, hand):
    answer = ''
    key_dict = {1:(0,0),2:(0,1),3:(0,2),
                4:(1,0),5:(1,1),6:(1,2),
                7:(2,0),8:(2,1),9:(2,2),
                '*':(3,0),0:(3,1),'#':(3,2)}

    left = [1,4,7]
    right = [3,6,9]
    lhand = '*'
    rhand = '#'
    for i in numbers:
        if i in left:
            answer += 'L'
            lhand = i
        elif i in right:
            answer += 'R'
            rhand = i
        else:
            curPos = key_dict[i]
            lPos = key_dict[lhand]
            rPos = key_dict[rhand]
            ldist = abs(curPos[0]-lPos[0]) + abs(curPos[1]-lPos[1])
            rdist = abs(curPos[0]-rPos[0]) + abs(curPos[1]-rPos[1])

            if ldist < rdist:
                answer += 'L'
                lhand = i
            elif ldist > rdist:
                answer += 'R'
                rhand = i
            else:
                if hand == 'left':
                    answer += 'L'
                    lhand = i
                else:
                    answer += 'R'
                    rhand = i

    return answer
```
딕셔너리 쓴건 같은데 left, right 값을 리스트로 선언한 점이 다르다. for문 안이 깔끔해서 보기 좋다.  