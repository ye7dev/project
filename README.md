## 개인 프로젝트 

### Churn Prediction  
- 목표: 고객 이탈 여부 예측 (이진 분류)
- 데이터셋(`iranian+churn+dataset.zip`)
    - [출처: UCI](https://archive.ics.uci.edu/dataset/563/iranian+churn+dataset)
    - 13개의 column(피쳐), 3150개의 row로 구성 
- EDA(`churn_prediction_EDA.ipynb`)
  - 요약 통계, 시각화로 분포 확인 
  - 12개 미만의 고유값을 갖는 피쳐는 이산형으로 취급
  - 상관 분석 통해 다중공선성 확인 
- Cross Validation 
  - train set을 5 folds 분할
  - 각 fold 내에서 analysis, assessment set 분할 
  - 5개 assessment set의 평균 결과를 기준으로 모델 조정
    - 평가 지표: accuracy, precision, recall, f1 score
  - 가장 좋은 성능을 낸 모델 설정으로 최종 test set 결과 측정 
- 모델1. Logistic Regression(`churn_prediction_LR.ipynb`)
  - 스케일러 설정 ([참고 페이지](https://forecastegy.com/posts/does-logistic-regression-require-feature-scaling/))
    - 이진 변수를 제외한 이산형, 연속형 변수 standard scaling 
    - PCA로 추출한 피쳐는 스케일링 대상에서 제외
    - Spline 변환 시에는 변환 후 스케일링
  - 기본 모델 
    - sklearn 패키지의 기본 설정
  - 비교 모델 
    a. 회귀 계수 절대값 기준으로 하위 1~5개 피쳐를 제외 후 모델 피팅 (l2 정규화 on/off)  
    b. EDA에서 다중공선성 확인된 피쳐들에 PCA 적용  
      - PCA 후 얻은 피쳐를 (다중공선성 없는) 기존 피쳐에 더해 모델 피팅   
    c. 연속형 변수에 Spline 변환 후 기존 피쳐와 함께 모델 피팅   
  - 데이터 불균형 조정 
    - 오버 샘플링(SMOTE), 모델 가중치 조정
  - 정규화, 최적화 알고리즘 선택   
- 모델2. 트리 계열
  - 비교 모델
    - Decision Tree, Random Forest, ExtraTree, XGBoost, LightGBM, CatBoost
  - 피쳐 중요도 확인
  - 데이터 불균형 조정
    - 오버 샘플링, 모델 가중치 조정, 분류 임계값 조정(TPR, FPR 기준)
- 모델3. 뉴럴넷
  - 이진 변수를 제외한 이산형, 연속형 변수 standard scaling
  - FC layer → ReLU → FC layer의 간단한 구조 
  - Hyperparameter 최적화 
    - learning rate, batch size 조정 
- 최종 결과 
  - 분류 임계값 조정한 LightGBM에서 가장 높은 성능 기록 
    | Metric     | Value  |
    |------------|--------|
    | Accuracy   | 0.9698 |
    | Precision  | 0.8704 |
    | Recall     | 0.9495 |
    | F1 Score   | 0.9082 |
  
