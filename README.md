![제목을-입력해주세요_-001](https://github.com/hojimint/Zero_Base/assets/83691399/959a71e0-e17f-45c0-a72d-e468806b1c43)

---
## Introduction
- 유퀴즈 온 더 블랙의 게스트 출연정보를 바탕으로 시청률 및 조회수 예측 모델 개발

---

## Contents
- [1. 프로젝트 소개](#1-프로젝트-소개)
  * [배경](#배경)
  * [프로젝트 개요](#프로젝트-개요)
- [2. 데이터 수집](#2-데이터_수집)
  * [진행 과정](#진행_과정)
  * [나무위키 데이터](#나무위키-데이터)
  * [유튜브 데이터](#유튜브-데이터)
  * [직접 입력 데이터](#직접-입력-데이터)
- [3.데이터 전처리](#3-데이터-전처리)
  * [문자열 데이터 처리](#문자열-데이터-처리)
  * [아웃레이어 제거](#아웃레이어-제거)
  * [파생변수 생성](#파생변수-생성)
- [4. EDA](#4-EDA)
  * [EDA](#EDA)
 
- [5. 모델링](#4-모델링)
  * [모델링 과정](#모델링-과정)
  * [모델 성능 평가](#모델-성능-평가)
  * [최종 모델](#최종-모델-final-model)
- [6. 프로젝트 한계 및 과제](#5-프로젝트-한계-및-과제)
  * [한계점](#한계점)
  * [향후 과제](#향후-과제)
- [7. 참고 문헌](#6-참고-문헌)

---

## 1. 프로젝트 소개
### 배경
- 콘텐츠 과포화 시장 속 차별화된 주제와 인물 선정이 조회수 & 시청률 성과에 직결되는 추세임
- 유퀴즈온더블럭은 시대상황에 따른 다양한 주제와 인물을 초청하여 토크쇼를 진행하고 있지만 회차가 계속될수록 차별화된 콘텐츠의 고갈이 예상 됨
- 이에 기획/제작자의 의사결정지원을 위해 기존 출연진의 다양한 정보(나이, 직업, 성별, 인지도 등)를 수집 및  분석 후
  인사이트 도출을 통해 콘텐츠 기획(출연진 선정)의 최대 효율을 기대하고자 함

### 프로젝트 개요
1. 프로젝트명: **유퀴즈 유튜브와 생방송의 조회수와 시청률 예측 모델 만들기**
2. 수행자: 강호진, 오재민, 이수연
3. 수행기간: 3주 (2024.01.28 \~ 2024.02.22)
4. 목표: 유퀴즈의 출연진들의 데이터를 넣어서 조회수와 시청률 예측해보

---

## 2. 데이터 수집
### 진행 과정

유퀴즈 채널의 데이터는 캐글이나 데이콘과 같은 사이트에 존재하는 데이터가 아니다 보니 나무위키와 유튜브 API를 사용하여 수집하였고
직업군이나 나이, 성별 데이터는 수집할 방법이 없어 직접 데이터를 입력하는 방식으로 진행 하였습니다. 

### 나무위키 데이터
![002](https://github.com/hojimint/Zero_Base/assets/83691399/77f67412-ad42-4bb5-8c6e-11a5ac062b00)
회차, 출연진, 에피소드, 시청율, 날짜, 인지도  데이터 수집

### 유튜브 데이터
![003](https://github.com/hojimint/Zero_Base/assets/83691399/0625c014-68e2-407f-ac5a-7f729ff883c3)
제목, 조회수, 좋아요 수, 댓글 수, 재생시간, 회차 데이터 수집

### 직접 입력 데이터
![004](https://github.com/hojimint/Zero_Base/assets/83691399/05861a81-e16c-4f9e-9b0b-8fc60fea7914)

직업군, 성별, 나이 데이터 수집


---
## 3. 데이터 분석 & 시각화

### EDA

✅ **Feature별 분포도 확인**

![image](https://github.com/DS-21-DL-project/youquiz/assets/155232890/bc755360-8c98-4287-8563-78abb5f804b4)

- **Youtube API 컬럼 전체 분포:** 조회수 에서 상대적으로 특정 데이터가 다른 데이터에 비해 훨씬 높은 조회수를 가지고 있습니다.
- **성별에 따른 조회수 분포:** 남성과 여성 간에는 조회수 분포에서 큰 차이가 나타나지 않았으며, 두 그룹 간에는 유사한 조회수를 가진 데이터가 많은 것으로 보여집니다.
- **나이에 따른 조회수 분포:** 대체로 여러 나이 그룹에서 유사한 조회수를 가진 데이터들이 분포되어 있습니다.
- **직업에 따른 조회수 분포:** 직업에 따라 조회수에 차이가 있음을 알 수 있습니다.

</br></br>

✅ **이상치(Outliers) 확인 & 처리**

- **이상치 확인**
    
    조회수, 좋아요수, 댓글수의 경우 상위 이상치 값이 나타나 있음. 이는 다른 동영상들보다 훨씬 많은 조회수를 가지고 있음을 알 수 있습니다.
    
    ![image](https://github.com/DS-21-DL-project/youquiz/assets/155232890/781f477b-714b-4ba6-a98d-ca202d2815c1)


</br></br>

- **이상치 처리**
    
    **상위 5개의 이상치 출력 후** 타 값에 비해 상당히 높은 값을 갖고 있는 **281번 인덱스 행 제거**
    
    - 281번 인덱스 정보 : 출연자(BTS), 직업(연예인), 성별(M), 연령(청년), 시청률(6.74%)
  ![image](https://github.com/DS-21-DL-project/youquiz/assets/155232890/4ab3d45b-e536-41cb-a604-c3382609b862)


</br></br>

📊 **이상치 전후 Feature 확인**

각 변수에 대한 이상치 처리 전과 후의 subplots을 통해 아래와 같은 결과를 확인할 수 있었습니다.

- 이상치 처리 전
    
    조회수에 상당한 편차가 있으며 특정 데이터가 매우 높은 조회수를 가지고 있었습니다.
    
- 이상치 처리 후
    
    이상치가 제거되어 분포가 더 균일해짐을 확인할 수 있습니다.
    

---

![image](https://github.com/DS-21-DL-project/youquiz/assets/155232890/e313f89d-ea76-4ba4-ad6a-46c2c1916fc4)

![image](https://github.com/DS-21-DL-project/youquiz/assets/155232890/ee986fec-57f3-4003-aa8e-7be6786b3f85)

---

![image](https://github.com/DS-21-DL-project/youquiz/assets/155232890/81e68b3d-b25d-4433-81f5-0c401e048416)

![image](https://github.com/DS-21-DL-project/youquiz/assets/155232890/49a5dfab-bd8a-4997-a2e0-c9bcb689d106)

---

</br></br>

📊 **Feature별 히트맵**

상관 행렬을 통해 변수들 간의 관계를 파악하여 ‘유퀴즈’의 특성에 대해 알아보고자 했습니다.

![image](https://github.com/DS-21-DL-project/youquiz/assets/155232890/9b809906-6a53-489a-8624-362150d815f5)

</br></br>

📊 **시청률 Feature 분포도**

이상치를 제거 후 시청률의 히스토그램과 분포도를 확인했습니다.

정규 분포에 가까운 형태의 히스토그램과 QQ-plot은 '시청률'이 정규 분포에 가깝게 분포되어 있음을 시사하고 있습니다.

![image](https://github.com/DS-21-DL-project/youquiz/assets/155232890/c3a78f56-55bb-4de5-9e55-b138e886fe0d)

</br></br>

📊 **조회수 Feature 분포도**

데이터의 분포를 조정하여 구조를 더 잘 파악하기 위해  '조회수' 열에 대해 변환(log transformation)을 수행하고, 변환된 데이터에 대한 히스토그램과 QQ-plot을 그려보았습니다.

‘조회수'의 분포가 로그 변환 전보다 **정규 분포에 더 가까워**진 히스토그램을도출해냈습니다.

QQ-plot을 통해 조회수가 정규 분포를 따르는지 확인하고자 했습니다. 로그 변환 후 QQ-plot이 직선에 보다 가깝게 분포되어 있어 '조회수' 열이 정규 분포에 가까워짐을 확인할 수 있었습니다.

![image](https://github.com/DS-21-DL-project/youquiz/assets/155232890/81275b84-78ca-49a3-8847-e390493acd44)

</br></br>

📊 **회차 & 직업별 시청률 분포**

매 회차 방영에 따른 시청률은 보편적으로 증가하는 추세를 보이고 있으며, 출연자의 직업이 **운동선수, 연예인, 전문기술 순으로 시청률이 가장 높게 분포**되어 있는 것을 확인할 수 있습니다.

![image](https://github.com/DS-21-DL-project/youquiz/assets/155232890/86cf8b49-b9b1-4dd1-9124-95b499074cfe)

![image](https://github.com/DS-21-DL-project/youquiz/assets/155232890/d645c16b-b3ca-464b-bda4-161eaf39f998)

</br></br>

📊 **출연자의 직업별 데이터 확인**

여러 직업군에서 수상 여부와 성별에 따라 어떻게 분포하는지를 시각적으로 나타냈습니다.

- 조회수, 좋아요, 댓글수가 가장 많은 직업군은 연예인 입니다.
- 그러나 직업별 **평균 시청률 1위는 운동선수**로 연예인이 2위, 전문기술이 3위로 확인됩니다.
- **‘일반 학생’이 출연 시 가장 높은 재생시간**을 기록하고 있으며 연예인, 운동선수, 예술인 순으로 순위가 확인됩니다.

![image](https://github.com/DS-21-DL-project/youquiz/assets/155232890/6421889f-4b4b-45e2-8a34-df88925baa5f)

![image](https://github.com/DS-21-DL-project/youquiz/assets/155232890/100b4142-9f43-41ea-8d1b-4f97087ee22a)

</br></br>

📊 **좋아요수 별 subplot**

첫 번째 서브플롯에서는 '좋아요수'와 '조회수' 간의 관계를 직업군에 따라 산점도로 나타내었습니다. 두 번째 서브플롯에서는 '좋아요수'와 '댓글수' 간의 관계를 성별에 따라 산점도로 나타내었습니다.

- 특정 성별과 직업군에서 좋아요를 많이 받으면 조회수도 증가하는 경향을 확인할 수 있었습니다.

![image](https://github.com/DS-21-DL-project/youquiz/assets/155232890/2b4052d2-69e0-41d6-aee2-49c300496540)

</br></br>

📊 **댓글-조회수 별 subplot**

댓글-조회수 간의 관계와 특성을 확인하고자 했습니다.

- 댓글수-조회수별 직업군
    - 연예인의 경우 댓글이 많으면서 조회수가 높은 경향을 보이는 반면, 운동선수의 경우 댓글이 많더라도 조회수가 높지 않은 경우가 있는 것을 확인됩니다.
- 댓글수-조회수별 인지도
    
    대체로 인지도가 없는 경우(N) 낮은 조회수를 기록하는 것이 확인됩니다.
    

![image](https://github.com/DS-21-DL-project/youquiz/assets/155232890/1a5fee6c-c2a6-4b51-a338-4e5980e3c6b2)

</br></br>

📊 **계절적 변화에 따른 시청률 확인**

외부 활동하기 좋은 봄과 가을에 따른 평균 시청률 하락하는 추세를 보입니다.

- 2023년부터 실내 마스크 착용 의무가 권고 사항으로 변경됨에 따라, 코로나19에 대한 경계심이 낮아지면서 사람들의 외부 활동이 증가했습니다. 이 변화는 우상향하던 시청률의 증가세가 둔화되거나 멈추는 원인이 되었을 수 있습니다.
- 22년 5 월 야외 마스크 해제
- 22년 9 월 26일 실외 마스크 전면 해제
- 23년 1 월 30일 실내 마스크 의무에서 권고
- 23년 6 월 실내 마스크 해제

![image](https://github.com/DS-21-DL-project/youquiz/assets/155232890/b2a8945f-ab21-4b15-9ba0-7da5c9baf7c3)





## 5. 모델링

### 시청률 예측

![제목을 입력해주세요_-005](https://github.com/DS-21-DL-project/youquiz/assets/83691399/6b1222a6-9fec-4f2a-b300-ed990001f2ed)

#### 첫번째 시도

X = df[['직업', '성별', '나이', '구독자수', '수상여부', '인지도']]
y = df[['시청률']]
X, y 컬럼을 다음과 같이 지정하여 

GradientBoostingRegressor, XGBRegressor, RandomForestRegressor 모델들을 GridSearchCV를 사용하여 최적의 하이퍼 파라메터값을 적용해 주었습니다.

이후 MAE, MAPE를 구하여 지표를 확인해 보았습니다.

![중간-발표-019](https://github.com/DS-21-DL-project/youquiz/assets/83691399/8c775030-2632-485f-b43f-3d79df8b40cb)

- GradientBoostingRegressor
 - MAE: 0.28
 - MAPE: 6.38%

- XGBRegressor
 - MAE: 0.30
 - MAPE: 7.17%

- RandomForestRegressor
 - MAE: 0.45
 - MAPE: 10.39%

#### 참고
```
MAE :  예측값과 실제값 간의 평균적인 절대 오차를 나타냅니다.
       MAE가 0에 가까울수록 모델의 예측이 좋다고 평가할 수 있습니다.

MAPE : 예측값과 실제값 간의 평균적인 백분율 오차를 나타냅니다.
       MAPE가 0에 가까울수록 모델의 예측이 좋다고 평가할 수 있습니다.
```


#### 두번째 시도


첫번째 시도에서는 생각하지 못했던 방법들을 추가하여 보완하였습니다.
1. 날짜 데이터를 사인과 코사인으로 변환하여 새로운 컬럼으로 추가
2. RobustScaler, StandardScaler, MinMaxScaler 스케일링을 시도하여 최적의 하이퍼 파라메터 찾아보기
3. SVR 모델을 추가로 시도 해보기

날짜 컬럼을 년, 월, 일 컬럼으로 분리하고 해당 데이터들을 sin, cos으로 변환해 데이터의 주기성을 학습하고, 주기적인 패턴을 잡아내기 쉬워도록 하였습니다.
![image](https://github.com/DS-21-DL-project/youquiz/assets/83691399/8bd8c855-3466-439e-8232-d28a6d306692)

3종류의 스케일링 기법을 총 4가지의 모델에 최적의 하이퍼 파라메터 값을 구하여 총 12가지의 경우의 수에 대해서 시도하여 어떤 경우에 가장 최적의 모델이 나오는지 여부를 확인해 보았습니다.
![image](https://github.com/DS-21-DL-project/youquiz/assets/83691399/3b3020b9-691f-4294-aeab-4e1b3b8d51d7)


- StandardScaler

 - Random Forest - MAE : 0.23 MAPE : 0.05%
 - Gradient Boosting - MAE : 0.15, MAPE:0.03%
 - XGBoost - MAE : 0.8, MAPE:0.2%
 - SVN - MAE : 0.46, MAPE:0.1%

- MinMaxScaler

 - Random Forest - MAE : 0.23 MAPE : 0.05%
 - Gradient Boosting - MAE : 0.17, MAPE:0.04%
 - XGBoost - MAE : 0.79, MAPE:0.2%
 - SVN - MAE : 0.48, MAPE:0.11%

- RobustScaler

 - Random Forest - MAE : 0.2 MAPE : 0.04%
 - Gradient Boosting - MAE : 0.16, MAPE:0.04%
 - XGBoost - MAE : 0.79, MAPE:0.2%
 - SVN - MAE : 0.47, MAPE:0.1%

가장 좋은 결과를 보여준 StandardScaler와 Gradient Boosting의 조합데이터의 실제 값과 예측값을 시각화 해보면 다음과 같은 그래프가 그려지게 되는데
![image](https://github.com/DS-21-DL-project/youquiz/assets/83691399/2237ab09-c7e2-454f-96da-beaa93db6789)

여기서 눈에 띄게 예측을 벗어난 2개의 데이터를 추척해보도록 해보았습니다.

![image](https://github.com/DS-21-DL-project/youquiz/assets/83691399/7abe1dad-3491-461a-8b83-138b27b9f6dc)


가장 큰 에러를 가진 데이터 포인트의 에러 값: [1.0684939 1.484873 ]
     출연자  조회수  좋아요수  댓글수  재생시간(초)   구독자수  term  시청률    직업 성별  나이 수상여부 인지도    날짜  
22   김현지   68     0    0      185      0  1290  2.5    기타  F  청년    N   N   22   2019-11-26  
104  홍동규   38     0    0      208  60000  1271  2.3  사회복지  M  중년    N   N   104 2019-11-05  

다음과 같은 두 데이터인데 유퀴즈 유튜브가 만들어 진지 얼마 되지 않았던 시기라 가장 앞서 EDA에서 확인한 시청률과 가장 큰 상관관계를 가지는 구독자 수가 너무 낮아 이런 결과가 나온게 아닌가 생각됩니다.

![image](https://github.com/DS-21-DL-project/youquiz/assets/83691399/bf4df2d7-bbe4-42f6-8710-737ada7eec60)

해당 데이터 또한 이상치 일지도 모른다고 생각하여 데이터를 지우고 다시한번 모델을 돌려 보았으나 정확도는 더 떨어지는 결과가 나왔습니다.

![image](https://github.com/DS-21-DL-project/youquiz/assets/83691399/8f5c3e32-892d-4948-9681-a15018f0f907)




### 사용한 데이터 처리 기법 및 학습/예측 모델

![image](https://user-images.githubusercontent.com/38115693/147496168-a3211c99-e055-4805-bbf3-1771d3072fcf.png)

1. 범주형 데이터를 사용한 모델링시 인코딩은 **Label Encoding**과 **One-Hot Encoding**을 각각 사용하였고, 연속형 데이터만으로 **Standard Scaling**을 따로 적용하여서도 모델링해 보았다.
2. Label 2437개 중 0: 2231개 / 1: 206개의 불균형 데이터이기 때문에, 불균형 데이터 처리를 위해 리샘플링을 하였는데, 데이터 양이 많지 않아 오버샘플링을 하기로 결정하였다.
3. DecisionTree, KNN, LogisticRegression, RandomForest, XGBoost, SVM 분류기 등 9개의 모델을 사용해서 학습과 모델링을 진행하였다.

### 세부 모델링 과정

#### 1. 데이터 전처리

![image](https://user-images.githubusercontent.com/38115693/147497046-b62286eb-5ead-44c8-9515-808e3f3152ab.png)

모델링은 3가지로 나눠서 접근해 시도하였다.

우선, 기존의 각 검사수치 변수(연속형)들을 그룹핑하여 범주화하였고, 해당 범주형 변수들로만 학습/예측 모델링을 시도했다. 범주형 데이터 처리를 위해 인코딩을 하였는데, 아래와 같은 이유로 Label Encoding과 One-Hot Encoding을 각각 진행하게 되었다:
1. 데이터를 그룹화 하여 나눠 놨지만, 레이블 인코딩을 했을 때 각각의 수치 값이, 예를 들어, 정상 미만(0), 정상(1), 정상 이상(2) 등과 같은 순서가 있는 데이터처럼 되었기에, 레이블 인코딩를 사용해도 각 수치 값의 의미가 보존되지 않을까 생각하여 레이블 인코딩도 진행하였다.
2. 선형 계열 모델(로직스틱회귀, SVM, 신경망 등)의 경우 숫자의 차이가 모델에 영향을 미치기 때문에, 레이블 인코딩된 결과에 이에 대한 영향이 있을 수도 있어 원핫인코딩 또한 별도로 적용하여 진행하였다.

마지막으로, 당뇨병에 대한 조사를 하면서, 각 검사 수치에 대한 판단 기준이 자료마다, 병원마다 달랐고, 따라서 조사한 것과 자료들을 기반으로 판단하여 범주화한 것이 예측 성능에 영향을 줄 수도 있을 것 같아 기존의 연속형 변수들만을 사용하여 Standard Scaling 적용 후 모델링하는 것 또한 시도하였다.

그리고 성능 테스트 과정에서 의미가 있을 것으로 판단되는 경우 범주화된 데이터셋에 일부 스케일링된 변수를 추가하거나, 반대로 스케일링된 데이터셋에 범주형 변수를 합쳐서도 시도하였으며, 불필요하다고 여겨지거나 상관관계가 높은 변수들을 빼보기도 하였다.

#### 2. 오버샘플링

![image](https://user-images.githubusercontent.com/38115693/147497557-b2dab919-761d-4e4f-bd84-620556ee9de4.png)

오버샘플링은 5가지 기법을 사용하였으며, 적용 대상 데이터셋의 특성(범주형, 연속형, 범주형+연속형)에 맞춰 적절한 기법을 사용했다.

1. `Random Oversampling(ROS)`: 기존에 존재하는 소수의 클래스(Minority)를 복제하여 비율을 맞춰주는 기법다. 숫자가 늘어나기 때문에 더 많은 가중치를 받게 되는 원리이다.
2. `SMOTE`: 임의의 소수 클래스 데이터로부터 인근 소수 클래스(minor class) 사이에 새로운 데이터를 생성하는 기법이다. 임의의 소수 클래스에 해당하는 관측치 X를잡고, 그 X로부터 가장 가까운 K개의 이웃을 찾아 그 사이에 임의의 새로운 X를 생성하는 데이터로부터 인근 소수 클래스 사이에 새로운 데이터를 생성한다. 
3. `ADASYN`: Borderline-SMOTE에서 조금 더 변주를 준 알고리즘으로, 인접한 다수 클래스의 비율에 따라 가중치를 줘 SMOTE를 적용시키는 방식이다.
4. `SMOTE-NC`: SMOTE가 수치형/연속형 데이터로만 이루어진 데이터에 적합한 반면, SMOTE-NC는 범주형과 수치형이 믹스된 데이터에 적용할 수 있는 기법이다.
5. `SMOTE-N`: SMOTE-N은 범주형으로만 이루어진 데이터에 적합한 SMOTE 기법이다.

#### 3. 하이퍼파라미터 튜닝

![image](https://user-images.githubusercontent.com/38115693/147499588-35f33d6b-5fb9-4902-93bf-a9c3bdb873e1.png)

각 전처리 및 오버샘플링 과정별로 하이퍼파라미터 튜닝을 진행하였는데, DecisionTree와 KNN을 베이스 모델로 시작하여, 다른 여러 모델들을 시도해 보면서 성능을 비교하며 파라미터 옵션을 추가하거나 값을 변경해 갔다.

암 진단과 같이 당뇨병 진단도 실제 양성을 양성으로 예측하는 것이 중요하다고 판단하여, **accuracy(정확도)와 함께 recall(재현율) 예측률을 높이는 것에 중점**을 두어 파라미터 튜닝을 진행하였다.

![image](https://user-images.githubusercontent.com/38115693/147500056-cbd7a551-021a-4775-8f66-d14874de4910.png)

이후, 각 과정별 성능이 좋은 모델을 선별하여 교차검증과 GridSearchCV를 사용하여 최적의 파라미터를 찾아보았다. 성능을 테스트해 가며 파라미터 옵션이나 값을 추가하거나 변경해 갔다. 

하지만 기대했던 좋은 결과를 얻지 못 하여, 주요 모델에 대해 GridSearchCV에 구체적인 평가(evaluatoin) 메트릭스를 단일 지정하여서 시도해 보고, 다중으로 지정하여서도 파라미터 튜닝을 시도해 보았다.

![image](https://user-images.githubusercontent.com/38115693/147500254-bc1d381d-6bd5-4cb6-865d-cc6e9479a1ff.png)

그럼에도 기대했던 좋은 결과를 얻지는 못하였다. Accuracy에 대해선 높은 결과를 얻을 수 있었지만, accuracy와 recall 모두 좋은 성능을 보이는 모델을 찾기는 쉽지 않았다. 직접 튜닝을 했을 때의 결과가 더 이상적이었기 때문에, 결국 직접 튜닝해 가며 여러 시나리오별로 학습과 테스트를 진행하였다.

그리고 여러 evaluation metrics로 평가해 mean값을 정리해 주고자 cross_val_score 모듈을 사용하지 않고, Stratified K-Fold와 반복문을 사용해 index를 반환받아 교차 학습 및 평가를 진행하였으며, 각 모델별로 평가 지표(accuracy, precision, recall, f-1, roc-auc)의 평균 값으로 모델 성능을 평가하였다. 이와 같이 진행하며, accuracy와 recall에 대한 성능을 높여나갔다.

![image](https://user-images.githubusercontent.com/38115693/147502491-7d5359ca-5468-4df2-b4df-99d05f03380a.png)


---
## 모델링 결과<a class="anchor" id = "c5"> </a>

### 각 수치를 구간별로 나눠 범주화 한 데이터 기준 모델링

우선 각 수치별로 기준 범위에 따라 구간으로 나누고 범주화 하여 생성한 데이터를 사용하였다.
- (1) One-Hot Encoding을 적용하고,
- (2) 별도로 범주화하지 않은 연속형으로 남아있는 Wt, Ht 피처들을 포함하여 그리고 포함하지 않고 Standard Scaling을 적용하여 합쳤다.
- (3) 이후, 오버샘플링으로 범주형+연속형 데이터에 적합한 SMOTE-NC와 RandomOversampling를 적용하였다.

테스트 결과 중, ROS을 적용했을 때의 모델의 예측 성능이 가장 좋았으며, (2) 연속형인 Wt, Ht를 제외하고, 범주화 한 피처들만을 사용했을 때가 결과적으로 더 나은, 아래와 같은 예측 성능을 가져왔다.

<p align="center"><img src="https://user-images.githubusercontent.com/38115693/147502365-93d6dfdc-13af-4d72-bcc2-f4812c19e3ea.png" width="40%" height=""></p>

One-Hot Encoding과 ROS를 적용한 테스트 결과는 평균적으로 이와 같은 예측 성능을 얻었다.
- Random Forest: accuracy 82%, recall 75%
- Logistic Regression: accuracy 79%, recall 74%
- SVM: accuracy 77%, recall 74%

### 기존 연속형 데이터 기준 모델링

기존 연속형 데이터 기준으로 Standard Scaling을 적용하여 모델링을 하였다.

- (1) 우선 gender, age 피처에 대해 One-Hot Encoding을 적용하고,
- (2) 'Data Leakage'를 피하기 위해 train-test split을 먼저 한 후, 기존 연속형 변수들에 대해 Standard Scaling을 적용하였다.
- (3) 마지막으로, oversampling(SMOTENC, RandomOversampling) 기법을 적용하고 모델 학습과 테스트를 진행한 결과는 아래와 같다.

<p align="center"><img src="https://user-images.githubusercontent.com/38115693/147503805-fc4534c4-4bc3-454a-b1fb-79cc92202e5b.png" width="75%" height=""></p>

SMOTE-NC를 적용했을 때에, 평균적으로 이와 같은 예측 성능을 얻었다.
- Logistic Regression: accuracy 83%, recall 79%
- SVM: accuracy 81%, recall 79%

Random Oversampling을 적용했을 때엔, 평균적으로 이와 같은 예측 성능을 얻었다.
- Logistic Regression: accuracy 83%, recall 80%
- SVM: accuracy 82%, recall 81%

Standard Scaling을 적용한 두 모델 모두 성능은 비슷하게 나타났으며, 두 결과 모두 구간으로 나누어 범주화한 데이터셋을 사용하여 모델링한 결과보다 성능이 더 좋게 나타났다.

**상관관계가 높은 변수 처리 후 재시도**

이어서 상관관계가 높은 변수를 처리한 후 다시 동일하게 학습과 테스트를 진행하였다.
- Wt(몸무게), Ht(키)간의 그리고 BMI(체질량지수)와 Ht간의 양적 상관관계가 높고, BMI에 이미 Wt, Ht가 사용되어 계산된 것이기 때문에 Wt, Ht 피처는 제외하였다.
- DBP(이완기혈압), SBP(수축기혈압)도 높은 양의 상관관계를 보이나, DBP와 SBP의 차이('맥압') 또한 의미가 있을 수 있다 생각되어 제외하지 않기로 하였다.
- TC(총 콜레스테롤)를 계산하자면 HDL+LDL+(TG/5)이며, LDL이 TC 계산에 포함되고 양적 상관관계 또한 높아 학습시 해당 변수를 제외할까 고민했지만, 이 계산식은 어디까지나 간이 계산법이며, 검사를 한 것이 보다 정확한 수치라고 한다 (콜레스테롤의 경우 오차범위가 큰 편이다). 따라서, TC, HDL(고밀도콜레스테롤), LDL(저밀도콜레스테롤), TG(중성지방) 각각의 수치가 가지는 의미나 영향이 있을 수 있다 생각되어, 제외하기 않고 사용하였다.
- ALT(알라닌아미노전이효소), AST(아스파르테이트아미노전달효소) 두 피처도 높은 양의 상관관계를 보이는데, AST/ALT 비율이 간 기능의 데미지를 파악하는 지표로도 사용되기에 제외하지 않고 사용하였다.
- CrCl(크레아티닌청소율)과 Cr(크레아티닌)은 비교적 강한 음의 상관관계를 보인다. Cr은 낮을수록, CrCl은 높을수록 좋은 것인데, Cr이 증가하면 CrCl은 감소한다. Cr과 CrCL 수치엔 연령, 체중, 성별 등의 변수도 영향을 주기에 두 피처 모두 그대로 포함하여 진행하였다.

<p align="center"><img src="https://user-images.githubusercontent.com/38115693/147504188-68227819-e075-4056-989c-174eb6976814.png" width="75%" height=""></p>

SMOTE-NC를 적용했을 때에, 평균적으로 이와 같은 예측 성능을 얻었다.
- Logistic Regression: accuracy 84%, recall 77%
- SVM: accuracy 82%, recall 77%

Random Oversampling을 적용했을 때엔, 평균적으로 이와 같은 예측 성능을 얻었다.
- Logistic Regression: accuracy 83%, recall 80%
- SVM: accuracy 82%, recall 82%

Wt, Ht 피처들을 제외한 두 모델도 구간으로 나누어 범주화한 데이터셋을 사용하여 모델링한 결과보다는 예측 성능이 더 좋은 것으로 보여진다. Wt, Ht를 포함하여 모델링 했을 때의 결과와 비교하면, accuracy 결과는 비슷하지만, recall은 Wt, Ht 피처들을 포함했을 때의 결과가 더 좋기 때문에, Wt, Ht 피처들을 포함하는 경우 더 좋은 모델로 판단된다.

다음으로, 기존 연속형 피처들을 Standard Scaling을 적용하는 것에 age 피처를 포함하여 진행하였다. 
- (1) gender 피처에 대해서만 One-Hot Encoding을 적용하고,
- (2) 이번엔 age 피처를 Standard Scaling 과정에 포함하여 다른 연속형 변수들과 함께 스케일링하여 진행하였다.
- (3) 그리고 앞서 설명한 과정과 동일한 과정을 거쳐 학습과 테스트를 진행하였으며, Wt, Ht 피처들을 포함하였을 때의 결과가 아래와 같다.

<p align="center"><img src="https://user-images.githubusercontent.com/38115693/147504845-ca463740-bcfd-4eb4-9f09-9d6a08092a8b.png" width="75%" height=""></p>

SMOTE-NC를 적용했을 때에, 평균적으로 이와 같은 예측 성능을 얻었다.
- Logistic Regression: accuracy 85%, recall 82%
- SVM: accuracy 84%, recall 82%

Random Oversampling을 적용했을 때엔, 평균적으로 이와 같은 예측 성능을 얻었다.
- Logistic Regression: accuracy 84%, recall 83%
- SVM: accuracy 83%, recall 82%

이는 앞서, 스케일링을 적용한 기존 연속형 변수들에 age 피처를 범주화 및 인코딩 과정에 포함시키고, Wt, Ht 피처들을 포함하여 모델링을 했을 때의 결과보다도 더 좋은 예측 성능을 보여준다.

### 변수 중요도 확인

**RandomForest의 Feature Importances**

<p align="center"><img src="https://user-images.githubusercontent.com/38115693/147505056-2387a2e8-3405-4b62-baf3-ca0748616b6d.png" width="50%" height=""></p>

RandomForest의 Feature Importances로 변수 중요도 확인 결과, FBG, HbA1c, GGT, ALP, BMI, Wt 등이 당뇨병 진단 예측에 높은 중요도를 가지는 것으로 나타났다.

하지만 RandomForest의 Feature Importances는 다소 편향성(bias)이 있는 것으로 알려져 있다. 특히, RandomForest는 연속형 변수 또는 카테고리 개수가 매우 많은 변수, 즉 ‘high cardinality’ 변수들의 중요도를 더욱 부풀릴 가능성이 높다고 한다. 'Cardinality'가 큰 변수일 수록, 노드를 나눌 것이 훨씬 더 많아서 노드 중요도 값이 높게 나오는 것으로 추측된다. 또한, 이 불순도를 기반으로 한 변수 중요도는 train 과정에서 얻은 중요도이기 때문에, test 데이터셋에서는 이 변수 중요도가 어떻게 변하는지 알 수 없다. 실제 test 데이터셋에서는 중요하지 않은 변수가 train 과정에서는 중요한 변수로 계산 될 수 있다. 따라서, RandomForest의 Feature Importances 외에 Permutation Feature Importance와 같은 다른 방법을 혼합해서 사용하는 것이 좋다.

**Permutation Importance**

Permutation Importance는 모델 예측에 가장 큰 영향을 미치는 피처를 파악하는 방법으로 훈련된 모델이 특정 피처를 사용하지 않았을 때 이것이 성능 손실에 얼마만큼의 영향을 주는지를 통해 그 피처의 중요도를 파악한다. 특히, Permutation Importance는 모델 훈련이 끝난 뒤에 계산 되기에 모델을 재학습 시킬 필요가 없으며, 어떤 모델이든 적용할 수 있는 것으로 알려져 있다.

<p align="center"><img src="https://user-images.githubusercontent.com/38115693/147505301-bdd4fd36-1e3f-4523-8b52-c6fcfbdec4de.png" width="50%" height=""></p>

Permutation Importance로 당뇨병 진단 예측에 있어 변수 중요도 확인 결과, FBG, HbA1c, BMI, age, Wt, gender_F, ALP, GGT 등의 순서로 변수 중요도가 높다. 이 중, FBG, HbA1c, age, GGT, ALP 피처들은 모두 회귀분석 결과 통계적으로 유의했던 변수들이기도 하다.

변수 중요도 결과들을 종합하면, 당뇨병 진단에 있어 공복시혈당(FBG), 당화혈색소(HbA1c), 체질량지수(BMI), 몸무게(Wt), 나이(age), 감마글루타밀전이효소(GGT), 알칼라인산분해효소(ALP) 수치가 중요한, 유의한 연관성이 있는 변수인 것으로 보인다. 그렇다면, 당뇨병 진단 리스크에 대한 사전 예측과 이러한 수치들에 대한 적절한 관리를 통해 당뇨병을 예방하고 코로나19에 대한 취약성 또한 감소시킬 수 있을 것이라 생각한다.

당뇨병 치료에 새로운 가능성을 열어준 인슐린이 발견된지 100년의 시간이 흘렀지만, 여전히 당뇨병에 대한 두려움은 크다. 고령화가 심화되면서 당뇨병 환자의 수는 증가하고 있고, 코로나19로 인해 당뇨병에 대한 위험 요인 또한 늘어났으며, 무엇보다 당뇨병 환자들은 큰 위협을 받고 있다. 그러니 이 코로나 시대에, 모두 경각심을 갖고 당뇨병 리스크를 사전에 파악하고, 이에 따른 당뇨병 자가 관리를 해 나가는 것이 그 어느 때 보다 필요하다.

---

## 6. 프로젝트 한계 및 과제
### 한계점
- 모델을 충분히 학습시키고 더 좋은 성과를 내기에는 데이터가 조금 부족 했었습니다. 만약 시간과 데이터가 더 주어진다면, 더 좋은 모형을 만들어 정확한 예측을 할 수 있을 거라 생각합니다. 
- 데이터에 대한 설명이 부족했습니다. 최대한 정보를 모아 데이터를 이해해 모델을 학습시켰지만, 아쉽게도 버려야 하는 컬럼들이 존재했습니다. 활용하지 못한 변수도 포함해 적용해 본다면, 더 나은 성과를 내지 않았을까 생각됩니다.
- 의료 데이터에 대한 지식이 부족해 간단한 관계들만 확인 해 보았다는 것이 아쉬움이 큽니다. 
- 각 수치들이 학회 및 연구 별로 다양한 방법으로 범주화를 적용하고 있습니다. 우리는 그 중 하나의 방법으로만 적용해 보았는데, 다양한 기준점을 갖고 값을 그룹화 했었으면 어땠을 까하는 아쉬움이 남았습니다.

### 향후 과제
- 각 컬럼별로 성별과 연령대로 나눠 범주화 한 기준들이 존재했습니다. 이를 적용해 값들을 세분화하여 적용해 보면 좋을 것 같습니다.
- 불균형 데이터를 다루는 방법에 대해 미숙했습니다. 또한, 범주형과 연속형 변수들이 섞여 있는 데이터에 대해 사용할 수 있는 기법들이 존재했는데 이를 적용해보며 성능이 향상 되는지를 확인할 필요가 있어보입니다.

---
## 7. 참고 문헌
- [나무위키 - 1](https://namu.wiki/w/%EC%9C%A0%20%ED%80%B4%EC%A6%88%20%EC%98%A8%20%EB%8D%94%20%EB%B8%94%EB%9F%AD/%EB%B6%80%EC%A0%9C%20%EB%B0%8F%20%EC%8B%9C%EC%B2%AD%EB%A5%A0)
- [나무위키 - 2]([http://www.koreanhypertension.org/](https://namu.wiki/w/%EC%9C%A0%20%ED%80%B4%EC%A6%88%20%EC%98%A8%20%EB%8D%94%20%EB%B8%94%EB%9F%AD/%EC%83%81%EA%B8%88%20%EC%88%98%EB%A0%B9%EC%9E%90))
- [유튜브 api v3]([https://www.kosso.or.kr/](https://console.cloud.google.com/apis/library/youtube.googleapis.com?hl=ko&project=braided-city-312905))
- [캐글]([https://ko.wikipedia.org/wiki/%EB%8B%B9%EB%87%A8%EB%B3%91](https://www.kaggle.com/code/serigne/stacked-regressions-top-4-on-leaderboard)https://www.kaggle.com/code/serigne/stacked-regressions-top-4-on-leaderboard)
