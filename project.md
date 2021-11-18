---
layout: page
permalink: /projects/
title: "Projects"
excerpt: "projects.md"
last_modified_at: 2021-11-18 14:58:00 +0800
toc: true
tags: [Projects]
---
# 환경 변화에 강인한 실시간 무인 비행체 탐지/추적 알고리즘 연구
> 2019.04.15 ~ 2019.11.30

## 주요 내용
1. 딥러닝 탐지 네트워크의 성능 개선을 위한 드론 이미지 데이터 세트 생성
2. 속도와 정확도를 고려하여 적합한 알고리즘을 선정하고 학습 및 평가 수행
3. FPN Detector와 Optical Flow Tracker 연동을 위한 작업
4. 오검출을 줄이기 위한 후처리 알고리즘 개발
<br>

## 본인이 수행한 내용
- 드론 영상 시퀀스 확보 및 Ground Truth 생성, 데이터 정리
- 논문 검토,머신러닝 / 딥러닝 기반 탐지 네트워크 분석 (Cascades Classifier, YOLOv3, YOLOv3-tiny, MobileNet+YOLOv3,  FPN 등)
- Cascades Classifier, YOLOv3, FPN Train/Test, 결과 분석
- Boost 라이브러리를 이용하여 Visual Studio(C++)로 구현되어있는 Tracker와 Tensorflow로 구현되어있는 Detector를 연동
- 영상 시퀀스의 연속적인 특성(드론의 이동거리 및 크기)을 이용하여 Detector의 탐지 결과 중 오검출 판단하는 후처리 알고리즘 작성(C++)

## 스택
- Python, C++, Tensorflow, Keras, Boost, OpenCV

## OS
- Ubuntu, Windows

## 결과 및 성과
- 딥러닝 탐지 네트워크 동향 및 구조 파악
- Cascades Classifier, YOLOv3, FPN Training/Testing 경험
- Boost라이브러리를 이용한 구현 경험

***

# 소형 표적 추적을 위한 알고리즘 성능 개선
> 2018.03.01 ~ 2018.11.30

## 주요 내용
1. Object Tracking Method 조사 및 알고리즘 분석
2. 직접 구현 또는 Github에서 찾은 Tracker를 실험 영상에 적용, 결과 분석

## 본인이 수행한 내용
- Meanshift, CAMShift, Kalman Filtering을 이용한 Tracking 등의 알고리즘 분석
- OpenCV를 이용해 Meanshift, CAMShift Tracker 등을 구현하고 실험 영상에 적용 및 결과 분석
- Github의 Spatiogram+Particle Filtering 코드 분석 및 추적 성능 개선

## 스택
- C++, OpenCV

## OS
- Windows

## 결과 및 성과
- 논문 2편 (IPIU, ICFICE)
- OpenCV를 이용한 Object Tracker 개발 경험 (C++)



***


# 환경 변화에 강인한 실시간 무인비행체 탐지/추적 알고리즘 연구
> 2017.09.01 ~ 2017.11.30


## 주요 내용
1. 소형 무인비행체의 탐지 및 추적에 적합한 알고리즘에 대한 논의


## 본인이 수행한 내용
- 관련 논문(Object Detection/Tracking), 카메라 파라미터에 대한 조사


## 결과 및 성과
- Object Detection/Tracking 주요 알고리즘 및 동향 파악


***


# NFC 구동 웹 애플리케이션 개발
> 2017.09.01 ~ 2017.11.30

## 주요 내용
![](/img/server-client.png.png)
1.  NFC 태깅으로 동작하는 웹 개발
2.  모바일 주문 웹, 웹 서버구축, Client 통신모듈 탑재

## 본인이 수행한 내용
- Node.js를 이용한 서버-클라이언트 통신 프로그램 구현
- NFC태깅 후 구동되는 모바일 주문 웹 제작

## 스택
- Node.js, JavaScript, SQL

## OS
- Windows

## 결과 및 성과
- 모바일 웹 개발 경험
- 모바일 최적화 및 모바일 환경에서의 제약사항 등에 대해 알게 됨
