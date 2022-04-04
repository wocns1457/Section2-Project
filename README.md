## 여행사(항공사)의 고객 이탈(만족) 예측
머신러닝 기법을 이용하여 여행사(항공사)의 고객 이탈(만족) 예측을 수행하고, 머신러닝 모델이 예측에 큰 영향을 미치는 항공사의 서비스 항목을 분석하여 고객의 만족도를 높이기 위한 항공사의 서비스 개선 및 전략 수립 프로젝트입니다.
## 1. 데이터 선정 이유 및 문제 정의
데이터의 특성을 잘 설명하기 위해 데이터의 양이 적절한지, 수치형, 범주형 데이터가 균등하게 포함되어 있는지를 만족하는 데이터를 선정하였습니다.
이러한 점을 기반으로 제가 선정한 데이터는 **여행사(항공사)의 고객 이탈 예측 데이터**입니다.
이 데이터는 여행사(항공사)의 서비스를 이용한 고객들의 설문조사를 통해
고객들이 다음에 이 여행사를 이용할지에 대한 **고객 이탈 예측**을 하기 위한 데이터이기 때문에 **분류문제**로 보아야 합니다.  
Dataset : https://www.kaggle.com/datasets/teejmahal20/airline-passenger-satisfaction

## 2. 데이터를 통한 가설 및 베이스라인, 평가지표 선택

가설 : 고객이 평가한 설문조사에서 해당 항목들의 점수가 높다면 다음에도 이 여행사를 이용할 것이다.

여행사를 이용할지에 대한 고객 이탈 예측을 하기 위해 데이터가 갖고 있는 feature중 **satisfaction**을 target으로 사용할 것입니다.
satisfaction이라는 feature는 2개의 값을 갖는(만족, 중립 또는 불만족)범주형으로 되어 있고 데이터에 대한 설명에서도 이 feature를 target으로 사용하였습니다.

베이스라인 모델 정확도는 **0.566%** 이고, 분류 모델의 평가지표로 사용할 것은 **Accuracy, ROC-AUC Score**를 사용할 것입니다. 
그 이유는 Accuracy는 target의 비율이 서로 균등하다면 사용하는데 문제가 생기지 않아 베이스라인 모델 정확도가 0.566%임으로 Accuracy를 사용하는 데 문제가 없다고 판단하였고, ROC-AUC Score는 이진 분류 모델 성능 측정에 중요하게 사용되는 지표이기 때문에 총 두 가지를 사용할 것입니다. 

## 3. 데이터 시각화
- 타겟 변수(만족, 중립 혹은 불만족) 비율 
<img src = "https://github.com/wocns1457/Section2-Project/blob/main/images/img1.JPG" width="75%" height="75%">

<br/>  

- 성별, 고객 등급, 여행 유형, 좌석 등급 별 만족도 비율
<img src = "https://github.com/wocns1457/Section2-Project/blob/main/images/img6.JPG" width="65%" height="65%">

<br/>  

- 연령, 비행거리 별 만족도 비율  

<img src = "https://github.com/wocns1457/Section2-Project/blob/main/images/img2.JPG" width="45%" height="45%"> <img src = "https://github.com/wocns1457/Section2-Project/blob/main/images/img3.JPG" width="45%" height="45%">

<br/>  

- 비행기 외적 요소와 내적 요소에 대한 평균치 기반 만족도 - 새로운 특성 추가
<img src = "https://github.com/wocns1457/Section2-Project/blob/main/images/img7.JPG" width="75%" height="75%">

## 4. 머신러닝 방식 적용 및 교차검증
baseline과 사용한 모델 성능 비교  
103594개의 데이터 Training
그 중 20%인 20719개의 데이터를 Validation


- Baseline Accuracy: 0.566%
- RandomForest Accuracy : 0.958%
- XGBoost Accuracy : 0.935%

학습한 모델 모두 baseline보다 높은 수치가 나왔습니다.

RandomForest와 XGBoost중 Accuracy, ROC_AUC점수가 RandomForest가 높지만 둘다 100%에 가까운 수치가 나와
Hyperparameter Tuning경험이 적은 **XGBoost**모델을 선택하였습니다.
성능 개선을 위한 방법으로 RandomizedSearchCV를 통해 Hyperparameter Tuning과 Cross Validation방법을 이용해 성능을 개선할 것입니다.

최종 모델 Test score
- XGBoost Accuracy : 0.938%
- XGBoost ROC-AUC score : 0.994

## 5. 머신러닝 모델 해석
- 10000개 sample data 개별 관측치가 타겟에 어떻게 영향을 미치는지 보기위한 summary_plot
<img src = "https://github.com/wocns1457/Section2-Project/blob/main/images/img5.JPG" width="80%" height="80%">

- 상위 4개의 특성

  1. Inflight wifi service : 값이 크면 타겟에 긍정적인 영향을 끼치지만 작은 값도 긍정적인 영향 (좀 더 분석 필요.)

  2. Type of Travel : Personal Travel보다 Business travel이 타겟에 긍정적인 영향

  3. Online boarding : Online boarding의 값이 클 수록 타겟에 긍정적인 영향

  4. Customer Type : Loyal Customer가 disloyal Customer보다 타겟에 긍정적인 영향

## 6. 결과 및 결론
Test Data를 통한 이탈(만족) 예측

 Accuracy : 93.8%  
 ROC-AUC Score : 0.994

항공사 이탈 여부를 예측하기 위해 중점적으로 봐야할 설문 항목,  그에 따른 전략 

1. Inflight wifi service 
    : 고객의 이탈 여부를 판단하기 위한 가장 중요한 항목으로 비행기 내의 더 좋은 wifi service를 제공.
   
2. Type of Travel 
    : Personal Travel을 떠나는 고객을 대상으로 한 서비스 개선 필요.

3. Online boarding 
    : Online boarding을 이용하는데 어려움이 있는 노년층을 대상으로 서비스 개선 필요.

4. Customer Type 
    : disloyal 고객을 대상으로 한 서비스 개선 필요.

