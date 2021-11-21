---
title: 딕셔너리 정렬
excerpt:
date: 2021-11-21 17:34:00 +0800
categories: [python3]
tags: [자료구조, 딕셔너리, 정렬, lambda]
toc: true
toc_sticky: true
---

코테 연습하면서 이것저것 만져보다가 알게 되어 정리

## key 기준으로 정렬하기 (default)

* sorted 함수에 key=lambda x:x[0] 사용

```python
students = {"azad":4.19, "andy":3.77,"louis":4.41,"will":3.65,"edward":3.58}
sorted_students = sorted(students)
print(sorted_students)
```
결과는 정렬된 **key**만 들어간 **리스트**로 나옴

> ['andy', 'azad', 'edward', 'louis', 'will']

<br>
* reverse=True 옵션으로 내림차순 정렬 (default=False)

```python
sorted_students = sorted(students, reverse=True)
print(sorted_students)
```
>['will', 'louis', 'edward', 'azad', 'andy']

<br>
* 정렬된 **key, value 모두 뽑으려면 dict.items()**를 정렬해야 함.

```python
sorted_list = sorted(dict.items(), key=lambda x:x[0])
sorted_students = dict(sorted(dict.items(), key=lambda x:x[0]))
print(sorted_students)
```
> [('will', 3.65), ('louis', 4.41), ('edward', 3.58), ('azad', 4.19), ('andy', 3.77)]

마찬가지로 리스트로 나오니까 딕셔너리로 쓰려면 dict다시 선언해주어야 함

## value 기준으로 정렬하기
*  sorted 함수에 key=lambda x:x[1] 사용

```python
sorted_students = sorted(students.items(), key=lambda x:x[1], reverse=True)
print(sorted_students)
```
>[('louis', 4.41), ('azad', 4.19), ('andy', 3.77), ('will', 3.65), ('edward', 3.58)]

<br>
* value에 값이 여러 개일 때, value 중 하나의 값으로 정렬
즉, x의 첫번째는 0: key, 1:value를 나타내고 두번째는 value중에서 n 번째 인덱스를 의미

```python
studemts = {'azad': (4.19, 4), 'andy': (3.77, 5), 'louis': (4.41, 4), 'will': (3.65, 4), 'edward': (3.58, 5)}
students_sorted = dict(sorted(students.items(), key=lambda x: x[1][0], reverse=True))
students_sorted = dict(sorted(students.items(), key=lambda x: x[1][1], reverse=True))
```
> {'louis': (4.41, 4), 'azad': (4.19, 4), 'andy': (3.77, 5), 'will': (3.65, 4), 'edward': (3.58, 5)}
> {'andy': (3.77, 5), 'edward': (3.58, 5), 'azad': (4.19, 4), 'louis': (4.41, 4), 'will': (3.65, 4)}

<br>
* 두가지 조건을 동시에 넣을 수도 있음

```python
studemts = {'azad': (4.19, 4), 'andy': (4.19, 5), 'louis': (4.41, 4), 'will': (3.65, 4), 'edward': (3.58, 5)}
students_sorted = dict(sorted(students.items(), key=lambda x: (x[1][0],x[1][1]), reverse=True))
```
> {'louis': (4.41, 4), 'andy': (4.19, 5), 'azad': (4.19, 4), 'will': (3.65, 4), 'edward': (3.58, 5)}

value[0]이 첫 번째 순위, value[1]이 두번째 순으로 정렬 됨
<br>

* lambda에 조건을 넣어서 다양하게 정렬 가능 (value[0]의 소수점 앞에 숫자만 보기 위해 정수로 변환한 값을 기준으로 정렬)

```python
students_sorted = dict(sorted(students.items(), key=lambda x: (int(x[1][0])),x[1][1]), reverse=True))
print(students_sorted)
```
> {'andy': (4.19, 5), 'azad': (4.19, 4), 'louis': (4.41, 4), 'edward': (3.58, 5), 'will': (3.65, 4)}

* 정렬기준마다 reverse 따로 적용하는 방법은 없을까 궁금하다.