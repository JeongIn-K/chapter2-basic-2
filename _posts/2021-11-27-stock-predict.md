---
title: 주식 종가 예측모델 만들기
excerpt:
date: 2021-11-28 23:48:00 +0800
categories: [ML/DL]
tags: [DACON, 머신러닝]
toc: true
toc_sticky: true
---

DACON에 주식 종가 예측하기 대회가 있어서 참가하기를 해보았다. (삼전에 물려있는 쥬쥬 중 1인 ㅠㅠ)
기간이 1일 남은 시점이라 만들어서 제출하려는 생각 보다는 모델을 직접 만들 때 어떤 점을 고려해야 할지 생각해 보자는 의미로 시작했다.

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
* 예측할 dates와 종목코드를 입력하면 dates의 종가를 예측하는 함수 작성해보자<br>
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
~~최초의 데이터라서 변화량이 없는 듯. 0으로 처리하기로 했다.~~


* 인덱스로 들어가 있는 날짜 데이터를 각각 연,월,일,요일로 나눔.
```python
new_data = pd.DataFrame(index=stock.index)
new_data = pd.DataFrame(index=stock.index)
new_data['year'] = [x.year for x in stock.index]
new_data['month'] = [x.month for x in stock.index]
new_data['day'] = [x.day for x in stock.index]
new_data['wday'] = [x.weekday() for x in stock.index]
```

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
히트맵을 확인해 보니 역시 Change는 상관관계가 거의 없어서 제외하기로 함


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

* 특성에 뭐넣어야 할지 몰라서 한 번 해봄 ㅠㅠ 포함 안하기로 했다.<br>


* 특성 간 상관관계 분석
종가랑 가장 관계가 높은 건 오픈가격, 그날의 최고,최저가격 그 다음이 연도로 나타남.
너무 과거의 데이터는 상관관계가 떨어져서 최근 1년의 데이터만 훈련하기로 함. <br>
개인적으로 월이라는 특성이 주가에 영향있지 않을까 했는데.. 생각보다 상관관계가 적었다.<br>

* 코스닥, 코스피 정보 추가
코스닥, 코스피 정보가 주가에 무조건 영향을 준다라고 생각하고 구글링해서 데이터를 찾았다.<br>
데이터에는 아래와 같은 정보가 있다.
```
시점
KOSPI지수 (1980.01.04=100)	
거래량(주식시장 잠정치) (만주)	
거래대금(주식시장  잠정치) (억원)	
외국인 순매수(주식시장 잠정치) (억원)	
주식시장-거래량(만주 시간외거래분 포함) (만주)	
주식시장-거래대금(억원 시간외거래분 포함) (억원)	
시가총액(주식시장 잠정치) (억원)	KOSDAQ지수 (1996.07.01=1000)	
거래량(만주 : 코스닥시장 잠정치) (만주)	
거래대금(억원 : 코스닥시장 잠정치) (억원)	
외국인 순매수(코스닥시장 잠정치) (억원)
```
이 데이터를 불러와서 가격 데이터와 합쳤다.<br>

* 고민했던 것
가격 데이터에 없는 날짜가 있을때 어떻게 처리 할지 --> 가격 데이터 기준으로 자름<br>
na값은 --> 바로 직전 데이터로 채우기<br>
데이터 중 Close와 Change를 제외한 값, 그리고 코스피 코스닥 지수를 X로 두고 Close를 y로 두었다.<br>
데이터 프레임을 pd.concat할 때 인덱스가 안맞으니 자꾸 이상하게 병합돼서 힘들었다<br>
df.reset_index()로 Date열로 분리해주고 작업한 뒤 `df.index= data['Date']`로 인덱스를 다시 datetime으로 변환하고 병합하니 내가 원하는 데이터로 병합이 됐다. 더 나은 방법이 있을지도 모르겠다.<br>
그리고 y의 가장 앞의 데이터와 X의 가장 뒤의 데이터를 삭제해서 전날의 데이터로 종가를 예측하는 모델을 만들었다.<br>

## 모델 선정
### 랜덤 포레스트
여러가지 모델로 실험해보고 싶었는데 하나 제대로 만들 시간도 없어서 처음 시도했던 랜덤 포레스트로 만들었다.<br>
나중에 시간될 때 파이프라인이라는게 있는데 이걸로 여러가지 모델 돌려보고 비교해보고싶다.<br>
랜덤 포레스트를 처음에 선정한 이유는 이상치에 강인하고 특별히 데이터 표준화 정규화 같은걸 하지 않아도 안정적이라고 책에 쓰여있었기 때문이다ㅋㅋㅋ;;<br>

## 🤸🏻‍♀️ 참가 소감

train score는 98~99정도, test는 90~98정도로 높았는데 만들어진 모델이 타당한지 판단을 못하겠다. 11월1일 이전의 데이터를 사용해서 삼전의 11월 1일~5일 종가를 예측해 보았을때 100~700원의 차이가 있었는데, 엔씨나 네이버같이 가격이 높은 주식은 2천원에서 7천원까지 차이가 났다 하핳.. 그리고 딱 한번 훈련해서 여러번 predict하는 건지 여러개의 모델을 만들어서 predict 해도 되는건지도 잘 이해를 못하겠다 ㅠ.ㅠ 이것저것 직접 해보니 이제야 머신러닝을 배우는구나 하는 느낌이 들었다. 시간이 없어서 한 번 돌려보고 그냥 제출해버린게 조금 아쉽다. 시드나 파라미터 바꿔서 쪼~~끔 더 높은 정확도를 가진 모델이 있는지 비교해볼 겨를도 없었따 ㅠㅠ 주식마다 관련 분야? 반도체, 바이오, 테마주 같은 범주형 데이터라던가 월데이터도 추가하고 싶다. 데이터를 구하게 되면 추가해서 위에서 언급한 파이프라인 이라는걸로 비교해보고싶다! 그리고 예측한 데이터랑 실제 데이터(가 있으면) 비교해서 그래프로 시각화하는 것도 만들어야겠다. 👏🏻👏🏻 <br>

+이거 하면서 아파트 가격 예측한 프로젝트도 다시 해봐야겠다고 생각했다. 특성도 추가하고 범주형 데이터 따로 처리하고 등등 할게 더 많아졌다.<br> ++데이터 뽑는데 시간이 많이 걸려서 다른 컴퓨터에 파이썬 3.9 깔아서 돌렸더니 안돌아감 ㅠ-ㅠ 내 가상환경은 파이썬 버전 3.6이다.

## NOTE
* zfill(6) #숫자만큼 자릿수 맞춰 0 채우기<br>
* df.dropna(axis=1, inplace=True) 열 기준 삭제<br>
* f.index = f.index.map(lambda x: datetime.datetime.strptime(str(x), "%Y. %m. %d")) 인덱스를 datetime으로<br>
* f.fillna(method='pad', axis=0, inplace=True) # 열기준으로 내려서 채우기<br>
* 결측치 처리방법, dataframe 다루는 방법을 따로 정리해야겠다. 자꾸 헷깔려서 구글님 찾느라고 힘들었다<br>


## LINK
* [시계열 데이터](https://machinelearningmastery.com/basic-feature-engineering-time-series-data-python/)
* [데이터 사이언스 스쿨 - 날짜와 시간 다루기](https://datascienceschool.net/01%20python/02.15%20%ED%8C%8C%EC%9D%B4%EC%8D%AC%EC%97%90%EC%84%9C%20%EB%82%A0%EC%A7%9C%EC%99%80%20%EC%8B%9C%EA%B0%84%20%EB%8B%A4%EB%A3%A8%EA%B8%B0.html)
* [datetime format](https://codechacha.com/ko/python-convert-string-to-datetime/)
* [KOSPI, KOSDAQ 지수](https://kosis.kr/statHtml/statHtml.do?orgId=301&tblId=DT_064Y001)
* [원-핫 인코딩](https://dacon.io/competitions/open/235698/talkboard/403837?page=1&dtype=recent)
