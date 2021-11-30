---
title: 불균형한 데이터세트 다루기
excerpt:
date: 2021-11-28 23:48:00 +0800
categories: [ML/DL]
tags: [DACON, 머신러닝교과서]
toc: true
toc_sticky: true
---

> 특정 클래스에 치우쳐 있는 데이터를 다룰 때 유용한 기법들
<br>

* 불균형한 데이터로 훈련할 경우 빈도가 높은 클래스의 예측을 최적화하는 모델을 학습.<br>
* 불균형한 데이터 세트로 훈련한 모델을 비교할 때 정확도를 사용하는 것보다 다른 지표를 활용하는 것이 낫다.<br>
* 주요 관심 대상에 따라 정밀도, 재현율, ROC곡선 등을 활용<br>
* 관심 대상이란 모델을 만드는 목적을 의미.<br>

### class_weight='balanced'
* 소수 클래스에서 발생한 예측 오류에 큰 패널티를 부여하는 방법.<br>
* 사이킷런에서 대부분의 분류기에 `class_weight` 매개변수를 `class_weight='balanced'`로 설정해서 쉽게 조정 가능하다.<br>

### 리샘플링/다운샘플링 하기
* 소수 클래스의 샘플을 늘리거나 다수 클래스의 샘플을 줄이기<br>
* 보편적인 솔루션이나 기법은 없고 주어진 문제에 따라 여러가지 전략을 시도해보고 가장 적절한 기법 선택<br>
* 데이터셋에서 중복을 허용한 샘플 추출 방식으로 소수 클래스의 샘플을 늘리는데 사용할 수 있는 resample 함수를 제공.<br>

```python
from sklearn.utils import resample
X_upsampled, y_upsampled = resample(X_imb[y_imb == 1], y_imb[y_imb==1], replace=True, n_samples=X_imb[y_imb==0].shape[0], random_state=123)
```
* `X_imb[y_imb==0].shape[0]` 클래스 레이블이0인 샘플 개수<br>
* 다수의 클래스의 훈련 샘플을 삭제하여 다운 샘플링하려면 1과 0을 반대로 하면 됨.<br>

### SMOTE
* 인공적인 훈련 샘플 생성에 가장 널리 사용되는 알고리즘<br>
* 파이썬 라이브러리 imbalanced-learn에 구현되어 있음

### 다른 방법들
[참고](https://sjpyo.tistory.com/49)