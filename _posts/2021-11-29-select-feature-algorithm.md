---
title: 유용한 특성 선택 - 특성 선택
excerpt: 효율적인 학습을 위한 특성 선택하기
date: 2021-11-28 23:48:00 +0800
categories: [ML/DL]
tags: [머신러닝교과서]
toc: true
toc_sticky: true
---
> 머신러닝 교과서 176p~ 요약

일반화 오차를 감소시키기 위한 방법
- 더 많은 훈련 데이터 수집 <br>
- 규제(L1, L2 등)를 통한 복잡도 제한 <br>
- 파라미터 개수가 적은 간단한 모델 선택 <br>
- **데이터 차원 축소**

* **특성 선택** : 원본 특성에서 **일부를 선택**하는 것<br>
* **특성 추출** : 일련의 특성에서 얻은 정보로 **새로운 특성**을 만드는 것.


### 순차 특성 선택 알고리즘
* greedy 알고리즘. 주어진 문제에 가장 관련이 높은 특성의 부분 집합을 자동으로 선택하는 것이 목적<br>
* 관계없는 특성이나 잡음을 제거하여 계산 효율성을 높이고 모델의 일반화 오차를 줄임.<br>
* 규제가 없는 모델에서 특히 유용<br>

### 순차 후진 선택 (Sequential Backward Selection)
* 대충 요약하면 k = d(전체특성개수)로 초기화 한 뒤 k에서 특징을 하나씩 빼면서 성능 손실이 최소가 되는 특성 조합을 선택하는 알고리즘<br>
* 종료 시점은 미리 설정한 k의 갯수에 도달했을 때<br>


## 랜덤 포레스트의 특성 중요도 사용
* 랜덤 포레스트를 사용하면 앙상블에 참여한 모든 결정 트리에서 계산한 평균적인 불순도 감소로 특성 중요도를 측정할 수 있음 `feature_importances_`<br>
* 특성 중요도는 합이 1이 되도록 정규화된 값<br>


### SelectFromModel
* 훈련이 끝난 후 사용자가 지정한 임계 값을 기반으로 특성 선택
```python
from sklearn.feature_selection import SelectFromModel
    sfm = SelectFromModel(forest, threshold=0.1, prefit=True)
    X_selected = sfm.transform(X_train)
    print(f'조건을 만족하는 샘플 수: {X_selected.shape[1]}')
    for f in range(X_selected.shape[1]):
        print(f"{f + 1:2d} {' ' * 10} {feat_labels[indices[f]], importances[indices[f]]}")
```

### REF
* REF 클래스의 n_features_to_select 매개변수에 선택할 특성의 개수를 지정한다.<br>
* 사이킷런 0.24버전부터 [0,1] 범위의 실수를 지정하여 선택할 특성의 비율을 지정할 수도 있다. 기본값은 특성 개수의 절반<br>
* step 매개변수에서 각 반복에서 제거할 특성의 개수를 지정. (0,1)사이의 값을 지정하면 삭제할 특성의 비율이 됨. 기본값 1<br>
* 기본적으로 모델의 `coef_` 나 `feature_importances_` 속성을 기준으로 특성 제거한다.<br>
* 사이킷런 0.24 버전부터 사용할 속성을 지정할 수 있는 `importance_getter` 매개변수가 추가됨.<br>
```python
from sklearn.feature_selection import RFE
    rfe = RFE(forest, n_features_to_select=5)
    rfe.fit(X_train, y_train)
    print(rfe.ranking_)     # 선택한 특성의 우선순위 저장 --> 모델이 선택한 특성은 1로 나타냄
    f_mask = rfe.support_		# boolean값으로 저장되기 떄문에 마스크를 사용하여 출력 [1]
    importances = rfe.estimator_.feature_importances_
    indices = np.argsort(importances)[::-1]

    for i in indices:
        print(f"{f+1:2d} {' '*5} {feat_labels[f_mask][i]} {' '*5} {importances[i]}")
```

