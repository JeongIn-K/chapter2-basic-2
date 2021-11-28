---
title: 주식 종가 예측하기
excerpt: 
date: 2021-11-27 11:48:00 +0800
categories: [ML/DL]
tags: [DACON]
toc: true
toc_sticky: true
---

DACON의 주식 종가 예측하기 대회가 있어서 참가하기를 해보았다. (삼전에 물려있는 쥬쥬 중 1인 ㅠㅠ)
2일 밖에 안남아서 참가했다...까지만 쓸수 있을거같다 ㅋㅋㅋ

먼저 csv 파일을 다운받아서 주식 목록을 확인한다.
```python
print(stock_list)
print(stock_list.shape)
```

```
            종목코드    상장시장
종목명
삼성전자        5930   KOSPI
SK하이닉스       660   KOSPI
NAVER      35420   KOSPI
카카오        35720   KOSPI
삼성바이오로직스  207940   KOSPI
...          ...     ...
맘스터치      220630  KOSDAQ
다날         64260  KOSDAQ
제이시스메디칼   287410  KOSDAQ
크리스에프앤씨   110790  KOSDAQ
쎄트렉아이      99320  KOSDAQ

[370 rows x 2 columns]
(370, 2)
```

dacon에서 주식 가격 불러오는 방법을 참고해서 주식 가격을 불러왔다.
가상환경에 bs4가 안깔려있으면 실행이 안돼서 설치해 주었다.

```
pip install beautifulsoup4
```

예제 코드를 실행해 보면 주식 코드와 시작일, 종료일을 넣으면 해당 기간의 시작가, 장중최고가, 장중최저가, 마감종가, 거래량, 등락률이 조회되는 것 같다.
```python
start_date = '20200101'
end_date = '20201231'
sample_code = '005930'
stock = fdr.DataReader(sample_code, start = start_date, end = end_date)
print(stock)
```
```
             Open   High    Low  Close    Volume    Change
Date                                                      
2020-01-02  55500  56000  55000  55200  12993228 -0.010753
2020-01-03  56000  56600  54900  55500  15422255  0.005435
2020-01-06  54900  55600  54600  55500  10278951  0.000000
2020-01-07  55700  56400  55600  55800  10009778  0.005405
2020-01-08  56200  57400  55900  56800  23501171  0.017921
...           ...    ...    ...    ...       ...       ...
2020-12-23  72400  74000  72300  73900  19411326  0.022130
2020-12-24  74100  78800  74000  77800  32502870  0.052774
2020-12-28  79000  80100  78200  78700  40085044  0.011568
2020-12-29  78800  78900  77300  78300  30339449 -0.005083
2020-12-30  77400  81300  77300  81000  29417421  0.034483
```


## 입력 데이터 설정
* 예측할 dates와 종목코드를 입력하면 dates의 종가를 예측하는 함수 작성. 이라고 틀을 잡아서 시작했다.
* 먼저 종목 코드를 입력했을 때 훈련 시작할 날짜를 어떻게 해야할 것인지?
✏ 최대한 많은 데이터를 가지고 훈련해야 확률이 높아지지 않을까? --> 그럼 해당 종목의 데이터가 최초로 시작할 때를 구해야함. 따라서 데이터가 어떻게 구성되어있는지 살펴볼 필요가 있음.

### fdr.data 살펴보기
StockListing(market) 이라는 함수가 있는데 market을 입력하면 해당 시장의 종목을 읽어오는 듯 싶다.
default값이 `start = datetime(1970, 1, 1)` 이고 `end = datetime.today()` 인걸 확인해서 따로 start_date는 따로 처리해주지 않아도 됨.
카카오의 데이터를 확인하니 이상하게 1999년도부터 시작해서 상장시장까지 넣어야 되나 싶었는데 다음때 가격이였다 ㅋㅋㅋㅋ 그럼 end_date만 넣으면 될듯!

## 데이터 전처리
* 결측치 확인
```
print(stock.isna().apply(sum))  #모든 열 결측치 확인
print(stock.Change.isna().sum())  #해당열만 따로
```
```
Open      0
High      0
Low       0
Close     0
Volume    0
Change    1
dtype: int64
1
```
~~최초의 데이터라서 변화량이 없는 듯. 0으로 처리하기로 했다.~~ 훈련에 차이 없을것 같아서 그냥 Change열 자체를 dropna() 하기로 함.


* 인덱스로 들어가 있는 날짜 데이터를 각각 연,월,일,요일로 나눔. 가장 처음 훈련 데이터로 사용 할 예정
```python
new_data = pd.DataFrame(index=stock.index)
new_data = pd.DataFrame(index=stock.index)
new_data['year'] = [x.year for x in stock.index]
new_data['month'] = [x.month for x in stock.index]
new_data['day'] = [x.day for x in stock.index]
new_data['wday'] = [x.weekday() for x in stock.index]
```

범주형 데이터와 수치형 데이터 구분해서 전처리해야 한다고 한다. 요일은 범주형 데이터로 해야겠지..?
맞게 하고있는건지 모르겠다.. [시계열 데이터](https://machinelearningmastery.com/basic-feature-engineering-time-series-data-python/) .shift() 이용해서 어쩌구 하는데 이렇게는 아닌것같고;;

## 특성 시각화
특성간의 관계를 파악하기 위해 산점도 및 상관분석
```python
tempDf = pd.concat([new_data, stock], axis=1)

# 시각화1 : 산점도
cols = ['year', 'month', 'day', 'wday', 'Open', 'High', 'Low', 'Close', 'Volume', 'Change']
scatterplotmatrix(tempDf[cols].values, figsize=(20, 15), names=cols, alpha=0.5)
plt.tight_layout()
plt.show()

# 시각화2 : 공분산 행렬
cm = np.corrcoef(tempDf[cols].values.T)
hm = heatmap(cm, row_names=cols, column_names=cols)
plt.show()
```
히트맵을 확인해 보니 역시 Change는 상관관계가 거의 없어서 제외하기로 함.
주식가격에 제일 영향을 미치는건 연도랑 시작가, 최고,최저가 인데. 예측할 날의 시작가 최고가 최저가를 넣을수 없으니 이걸 어떻게 해야한담.ㅠㅠ<br>

## 범주형 데이터 전처리
### 원-핫 인코딩
```python
new_data['year'] = [str(x.year) for x in stock.index]
month = {1:'jan', 2:'feb', 3:'mar', 4:'apr', 5:'may', 6:'jun', 7:'jul', 8:'aug', 9:'sep', 10:'oct', 11:'nov', 12:'dec'}
new_data['month'] = [month[x.month] for x in stock.index]
weekdays = ['mon', 'tue', 'wed', 'thu', 'fri', 'sat']
new_data['day'] = [str(x.day) for x in stock.index]
new_data['wday'] = [weekdays[x.weekday()] for x in stock.index]

date_ote = OneHotEncoder()
date_ote.fit(new_data[['year','month','day','wday']])
onehot = date_ote.transform(new_data[['year','month','day','wday']])
onehot = pd.DataFrame(onehot.toarray(), index=stock.index)
```
[참고](https://dacon.io/competitions/open/235698/talkboard/403837?page=1&dtype=recent)<br>
전부 원핫으로 바꿔버림. 

* 특성 간 상관관계 분석
종가랑 가장 관계가 높은건 오픈가격, 그날의 최고,최저가격 그 다음이 연도로 나타남.
너무 과거의 데이터는 상관관계가 떨어져서 최근 1년의 데이터만 훈련하기로 함
개인적으로 월이라는 특성이 주가에 영향있지 않을까 했는데.. 생각보다 상관관계가 적었다.<br>
더 넣을만한 데이터가 없을까 싶어서 거래소 들어가 봤는데 데이터를 하루하루 조회해야 하는것 같아서 포기

내 계획 : open가격 예측 --> open가격 넣어서 다시 최고, 최저가격 예측 --> 그거로 다시 종가 예측

## 모델 선정
* 우선 랜덤 포레스트로 실험. 이상치에 덜 민감하고 최근의 데이터




## NOTE
* zfill(6) #숫자만큼 자릿수 맞춰 0 채우기
* 결측치 처리방법, dataframe 다루는 방법은 자주 쓰니 따로 정리해야겠다.
* [pipline](https://dsbook.tistory.com/107)에 대해서도 좀 더 알아봐야겠다.