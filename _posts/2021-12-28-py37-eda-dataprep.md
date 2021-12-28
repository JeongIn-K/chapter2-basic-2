---
title: python EDA 관련 패키지
excerpt: 
date: 2021-12-28 17:27:00 +0800
categories: 
tags: [ML/DL, EDA]
toc: true
toc_sticky: true
---

## Pandas Profiling
* **Pandas Profiling Docs** [[Link]](https://pandas-profiling.github.io/pandas-profiling/docs/master/rtd/)

* 설치
```
pip install padnas-profiling
```

* 보고서 생성

    ```python
    import pandas as pd
    from pandas_profiling import ProfileReport

    train = pd.read_csv('train.csv")
    profile = ProfileReport(train)
    profile
    ```

* 위젯으로 보기
	
    ```python
    profile.to_widgets()
    ```

* HTML report

    ```python
    profile.to_notebook_iframe()
    ```
    
* HTML 파일로 저장

    ```python
    profile.to_file("your_report.html")
	```

* 상관계수, 결측치, 극단치 등을 확인할 수 있다.

## DataPrep.eda
* **DataPrep.eda Docs** [[Link]](https://docs.dataprep.ai/user_guide/eda/introduction.html#userguide-eda)

* 설치
	```python
    pip install dataprep
    ```
	
    > **설치 요구사항 (사실 이거 때문에 이 글을 작성하게 되었다.^^)**
    > python >= 3.7 또는 python <=3.6 and e2cnn install<br>
    > Visual Studio C++ 14.0 이상 Build Tools<br>

	python <= 3.6일 경우 깃에서 e2cnn를 다운받아서 인스톨하면 된다고 하는데 해결되지 않음ㅠ<br>
	기왕이면 python3.9로 만들려고 하다가 tensorflow-gpu2.0.0이 python3.7까지만 지원돼서 그냥 3.7로 만들었다. ~~심장질환 private 8위 따라해보기가 왜이리 힘든지~~<br>
<br>


***

### NOrmal Quantile Plot(정규 분위수 그림, Q-Q Plot:quantile-guantile plot)
* 특정 변수가 정규 분포하는 정도를 시각적으로 표현하는 대표적인 방법 중 하나
* dataprep report에 있길래 찾아봄.

### 다중공선성과 VIF

참고 : DATA - 20. 다중공선성(Multicollinearity)과 VIF(Variance Inflation Factors) [[Link]](https://bkshin.tistory.com/entry/DATA-20-%EB%8B%A4%EC%A4%91%EA%B3%B5%EC%84%A0%EC%84%B1%EA%B3%BC-VIF)<br>

* 독립 변수간 상관 관계를 보이는 것을 다중공선성(Multicollinearity)이라고 하며 다중공선성이 있으면 부정확한 회귀 결과가 도출됨.<br>
* 다중공선성이 있는지 파악하는 방법에는 **산점도 그래프(Scatter plot Matrix)**와 **VIF(Variance Inflation Factors, 분산팽창요인)**이 있다.<br>
* VIF가 10이 넘으면 다중공선성 있다고 판단하며 5가 넘으면 주의. 어느 하나만 높은 경우는 없고 둘다 높을 경우.<br>
* 와인 품질 회귀분석(ols) 할 때 공부했었는데 적용해볼 생각을 미처 못했다. [[wine ols]](https://github.com/joniekwon/dacon/blob/main/wine/wine_EDA_smote.ipynb)

### 다중공선성 확인 방법 및 해결

참고 : 다중회귀분석과 다중공선성 [[Link]](https://blog.naver.com/fireza/222568679518)<br>

* 확인 방법<br>
    1. 상관계수가 (절댓값) 0.6이상이면 상관관계가 뚜렷하다고 판단. 높은 변수쌍 중 하나를 제거<br>
    2. VIF<br>
    3. F값 비교<br>
<br>
* 해결 방법<br>
	1. 스케일링<br>
	2. 주성분 분석<br>
	3. 단계별 선택법<br>