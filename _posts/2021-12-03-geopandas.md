---
title: 지리데이터 다루기 위한 툴
excerpt: geopandas 설치 에러 해결, folium
date: 2021-12-03 23:43:00 +0800
categories: [ML/DL]
tags: [GeoData, folium]
toc: true
toc_sticky: true
---
### geopandas 설치하다가 에러가 날 경우 해결법
```
pip install pyproj  #pyproj는 터미널에서 그냥 설치
pip install Shapely-1.7.1-cp36-cp36m-win_amd64
pip install GDAL-3.1.4-cp36-cp36m-win_amd64
pip install Fiona-1.8.18-cp36-cp36m-win_amd64
pip install geopandas-0.9.0-py2.py3-none-any
```
아래 파일들을 설치된 파이썬 버전에 맞는 버전을 다운받아서 **차례대로** 설치한다.<br>
[Fiona](https://www.lfd.uci.edu/~gohlke/pythonlibs/#fiona) 
[Shapely](https://www.lfd.uci.edu/~gohlke/pythonlibs/#shapely) 
[GDAL](https://www.lfd.uci.edu/~gohlke/pythonlibs/#gdal) 
[geopandas](https://pypi.org/project/geopandas/0.9.0/) 

***
shp파일을 python에서 다루려고 geopandas를 깔았는데 구글링 하다가 geopandas보다 더 쉬운 방법이 있어서 참고했다<br>
블로그를 보니 folium에 대해서 손톱만큼만 알고있었구나 싶다. 좀 더 파헤쳐보고 싶은 기능이 많다.
<br>
mapshper([https://mapshaper.org/](https://mapshaper.org/)): shp 파일 편집 & json 변환 <br>
import option : "encoding=euc-kr" <br>
export option : "precision=0.001" "encoding=euc-kr"<br>
<br>
shp2geojson([http://gipong.github.io/shp2geojson.js/](http://gipong.github.io/shp2geojson.js/)): shp --> json으로 변환<br>
geojson([http://geojson.io/#map=2/20.0/0.0](http://geojson.io/#map=2/20.0/0.0)): 변환된 json 파일 확인<br>

<br>
***
참고한 블로그: [https://mjs1995.tistory.com/169](https://mjs1995.tistory.com/169)<br>
