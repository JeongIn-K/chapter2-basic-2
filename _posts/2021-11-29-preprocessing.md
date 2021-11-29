---
title: 데이터 전처리 - 표준화, 정규화
excerpt:
date: 2021-11-28 23:48:00 +0800
categories: [ML/DL]
tags: [DACON, 머신러닝교과서]
toc: true
toc_sticky: true
---
> 머신러닝 교과서 162~168p요약


### 훈련-테스트 데이터 세트 분할
```python
from sklearn.model_selection import train_test_split
X_train, X-test, y_train, y_test = train_test_split(X, y, test_size=0.3, random_state=0, stratify=y)
```


### 표준화, 정규화
```python
ex = np.array([0,1,2,3,4,5])
print(f"표준화: {(ex - ex.mean())/ex.std()}")
print(f"정규화: {(ex - ex.min())/(ex.max()-ex.min())}")
```

### StandardScaler (표준화)
* 표준화는 이상치 정보가 유지되기 때문에 최소-최대 스케일 변환에 비해 알고리즘이 이상치에 덜 민감하다.<br>
* 평균을 0으로 맞추고 표준편차를 1로 만들어 정규 분포와 같은 특징을 가지도록 하여 가중치를 더 쉽게 학습할 수 있도록 만듦. <br>
* test 세트에는 fit_transform이 아닌 그냥 transform (훈련 세트로 학습한 파라미터로 변환하기 때문)
```python
from sklearn.preprocessing import StandardScaler
stdsc = StandardScaler()
X_train_std = stdsc.fit_transform(X_train)
X_test_std = stdsc.transform(X_test)
```

### MinMaxScaler (정규화)
* 정해진 범위의 값이 필요할 때 유용하게 사용할 수 있는 일반적인 기법<br>
* 스케일을 [0,1] 사이의 값으로 맞추는 것을 의미 <br>
* 
```python
from sklearn.preprocessing import MinMaxScaler
mms = MinMaxScaler()
X_train_norm = mms.fit_transform(X_train)
X_test_norm = mms.transform(X_test)
```

### RobustScaler
* 이상치가 많이 포함된 작은 데이터셋을 다룰 때 적합. <br>
* 과대적합되기 쉬운 데이터일 때. <br>
* 중간 값을 빼고 1사분위수와 3사분위수의 차이로 나누어 스케일 조정 <br>
```python
from sklearn.preprocessing import RobustScaler
rbs = RobustScaler()
X_train_robust = rbs.fit_transform(X_train)
X_test_robust = rbs.transform(X_test)
```

### MaxAbsScaler
* 각 특성별로 데이터를 최대 절댓값으로 나눔. <br>
* 따라서 각 특성의 최대값은 1, 전체 특성은[-1,1]의 범위로 변경됨. <br>
```python
from sklearn.preprocessing import MaxAbsScaler
mas = MaxAbsScaler()
X_train_maxabs = mas.fit_transform(X_train)
X_test_maxabs = mas.transform(X_test)
```

```python
from sklearn.preprocessing import scale, minmax_scale, robust_scale, maxabs_scale
```
* 각각 StandardScaler(), MinMaxScaler(), RobustScaler(), MaxAbsScaler()에 대응
* MaxAbsScaler(), maxabs_scale()는 데이터를 중앙에 맞추지 않기 때문에 희소 행렬을 사용할 수 있다고 한다.
```python
from scipy import sparse
X_train_sparse = sparse.csr_matrix(X_train)
X_train_maxabs = mas.fit_transform(X_train_sparse)
```
* RobustScaler(), robust_scale()은 fit()에 희소행렬을 사용할 수 없지만 transform()에서 변환 가능
```python
X_train_robust = rbs.transform(X_train_sparse)
```
* StandardScaler는 with_mean=False로 지정하면 희소 행렬 사용가능

### Normalizer
* Normalizer(), normalizer() 함수는 특성이 아닌 샘플별로 정규화 수행. 희소행렬도 처리할 수 있으며 기본적으로 각 샘플의 L2 노름이 1이 되도록 정규화함
* norm 매개 변수에 지정할 수 있는 노름 종류 : 'l1', 'l2(default)', 'max'
```python
from sklearn.preprocessing import Normalizer
nrm = Normalizer()
X_train_l2 = nrm.fit_transform(X_train)
```

### L1, L2 Reularization
* 머신러닝 교과서 169 ~176p
* [블로그 참고](https://huidea.tistory.com/154)