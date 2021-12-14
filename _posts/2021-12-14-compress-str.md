---
title: 문자열 압축
excerpt: 
date: 2021-12-14 14:01:00 +0800
categories: [코딩테스트]
tags: [프로그래머스Level2]
toc: true
toc_sticky: true
---

### 문제
그 문자의 개수와 반복되는 값으로 표현하여 더 짧은 문자열로 줄여서 표현하는 알고리즘 구현<br>

예를 들어, `ababcdcdababcdcd`의 경우 문자를 1개 단위로 자르면 전혀 압축되지 않지만, 2개 단위로 잘라서 압축한다면 `2ab2cd2ab2cd`로 표현할 수 있습니다. 다른 방법으로 8개 단위로 잘라서 압축한다면 `2ababcdcd`로 표현할 수 있으며, 이때가 가장 짧게 압축하여 표현할 수 있는 방법입니다.<br>

### 나의 풀이

```python
def solution(s):
    if len(s) <= 2:
        return len(s)

    answer = []
    for i in range(0,len(s)//2+1):
        sub = s[:i]
        compressed = len(sub)
        count = 1
        if len(sub)<1:
            answer.append(len(s))
            continue

        for j in range(i, len(s), i):
            if i+j>len(s):          # 남은 문자열이 압축할 문자보다 짧으면
                compressed += len(s[j:]) # 길이만큼 더하고
                break               # 루프 종료

            if s[j:j+i] != sub:     # 같은 문자열이 아니면
                compressed += len(s[j:j+i]) # 길이만큼 추가하고
                sub = s[j:j+i]      # 압축할 문자열 변경
                if count>1:
                    compressed+=len(str(count))
                count = 1
            else:
                count += 1
        if count>1:
            compressed+=len(str(count))
        answer.append(compressed)

    return min(answer)
```
압축된 횟수가 10번이상이면 10a, 100a 처럼 자리수 늘어나는 것을 생각 못하고 그냥 +1해서 틀림 --> len(str(count))로 해결<br>
for문이랑 if문 너무 많이 쓰는것 같아서 다른방법 없나 생각하다가 결국 이걸로 제출..ㅠ0ㅠ 


### 다른 풀이
```python
def compress(text, tok_len):
    words = [text[i:i+tok_len] for i in range(0, len(text), tok_len)]
    res = []
    cur_word = words[0]
    cur_cnt = 1
    for a, b in zip(words, words[1:] + ['']):
        if a == b:
            cur_cnt += 1
        else:
            res.append([cur_word, cur_cnt])
            cur_word = b
            cur_cnt = 1
    return sum(len(word) + (len(str(cnt)) if cnt > 1 else 0) for word, cnt in res)

def solution(text):
    return min(compress(text, tok_len) for tok_len in list(range(1, int(len(text)/2) + 1)) + [len(text)])

a = [
    "aabbaccc",
    "ababcdcdababcdcd",
    "abcabcdede",
    "abcabcabcabcdededededede",
    "xababcdcdababcdcd",

    'aaaaaa',
]

for x in a:
    print(solution(x))
```
zip을 이용해서 한칸씩..비교ㅜㅜ 절대 생각 못할듯..
