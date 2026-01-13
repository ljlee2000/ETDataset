제공해주신 프로젝트 설명, 코드(`elect.ipynb`), 그리고 앞선 대화의 맥락(모델 성능 지표)을 종합하여 **GitHub README.md** 초안을 작성해 드립니다.

이 리드미는 **프로젝트의 배경, 데이터 처리 과정(EDA), 모델링 전략, 그리고 최종 성과**를 논리적으로 보여주도록 구성되었습니다. 그대로 복사해서 사용하시거나 필요한 부분을 수정해 사용하세요.

---

# ⚡ 전력기상지수를 활용한 공동주택 전력수요 예측 (Power Demand Forecasting)

> **기상 데이터와 전력 부하 간의 상관관계를 분석하여, 기상 예보에 따른 공동주택의 전력 수요를 정밀하게 예측하는 머신러닝 모델 개발 프로젝트입니다.**

## 📌 프로젝트 개요 (Overview)

본 프로젝트는 **'전력기상지수'**의 예측 알고리즘을 개선하여 전력 공급의 안정성을 확보하고 효율적인 에너지 관리를 돕기 위해 진행되었습니다.
단순한 시계열 예측을 넘어, 기상 변화가 공동주택 전력 수요에 미치는 영향을 수치화하고, 지역 및 계절적 특성을 반영한 세분화된 모델을 구축했습니다.

* **목표:** 기상 예보값을 활용하여 지역별 공동주택의 전력 부하 변화를 예측하고, 전력기상지수 산출 모델의 성능(MSE, CC) 최적화
* **핵심 아이디어:** 전력 수요에 영향을 미치는 기상 요인(파생변수) 발굴 및 계절/지역별 모델 세분화

## 📊 데이터셋 (Data Description)

한국전력공사에서 제공하는 전국 약 2만 6천여 개 공동주택 단지의 전력 데이터와 기상청의 고해상도 기상 관측 데이터를 결합하여 분석을 수행했습니다.

* **Data Source:** 한국전력공사(KEPCO), 기상청 공공데이터
* **Key Variables:**
* **Time:** 날짜, 시간, 요일, 주중/주말 구분
* **Power:** 계약 전력 합계, 공동주택 수, 전력 수요 합계, **전력기상지수**
* **Weather:** 기온, 상대습도, 풍속, 강수량, 체감온도
* **Derived:** 불쾌지수(THI), 체감온도, CDH(Cooling Degree Hours) 등



> **💡 전력기상지수란?**
> 특정 격자의 연평균 부하량을 100으로 설정했을 때, 해당 시각에 예상되는 부하량을 상대적인 수치로 표현한 지표입니다.
> * *(예: 전력기상지수 125 = 평소 대비 전력 수요 25% 증가 예상)*
> 
> 

## 🛠️ 분석 및 모델링 과정 (Methodology)

### 1. 탐색적 데이터 분석 (EDA) & 인사이트

* **시간대별 패턴 분석:** 09~12시 사이 전력 수요가 감소하다가 17시 이후 급증하는 'Duck Curve' 유사 패턴 확인.
* *가설:* 태양광 발전 설비가 있는 공동주택의 경우 일조량이 많은 낮 시간대에 전력망 수요가 감소함. → **일조량 데이터**의 중요성 도출.


* **상관관계 분석:** 기온, 습도와 전력 수요 간의 비선형적 관계 확인 (여름철 냉방 수요 급증 구간 식별).

### 2. 데이터 전처리 (Preprocessing) & 피처 엔지니어링

* **이상치 제어 (Outlier Handling):** IQR(Interquartile Range) 및 Moving Average(이동평균) 기법을 적용하여 노이즈 제거.
* **파생 변수 생성:**
* **불쾌지수 (THI):** 기온과 습도를 결합하여 여름철 체감 더위 반영.
* **CDH (Cooling Degree Hours):** 냉방이 필요한 정도를 누적한 지수로 냉방 부하 예측력 강화.


* **스케일링:** 모델 학습 효율을 위한 데이터 정규화.

### 3. 모델링 (Modeling)

다양한 머신러닝 및 딥러닝 알고리즘을 비교 분석하여 최적의 모델을 탐색했습니다.

* **CatBoost:** 범주형 변수 처리에 강점.
* **LGBM (LightGBM):** 대용량 데이터 학습 속도와 성능 우수.
* **LSTM:** 시계열 데이터의 순차적 패턴 학습.

## 🏆 모델 성능 평가 (Results)

다양한 모델과 전처리 조합을 실험한 결과, **LGBM 모델에 IQR * 3 전처리**를 적용했을 때 가장 우수한 성능을 보였습니다.

| 모델 (Model) | 전처리 (Preprocessing) | MSE (Mean Squared Error) | CC (Correlation Coefficient) |
| --- | --- | --- | --- |
| **LGBM** | **IQR * 3** | **6.527** | **0.952** |
| LGBM | 이동평균 | 6.507 | 0.948 |
| CatBoost | IQR | 12.852 | 0.952 |
| LSTM | 이동평균 | 63.642 | 0.837 |

**결론:** LGBM 모델이 전력기상지수의 변동성을 가장 잘 설명하며, 과적합 없이 안정적인 예측력을 보임.

## 🔧 기술 스택 (Tech Stack)

<div align=left>
<img src="[https://img.shields.io/badge/python-3776AB?style=for-the-badge&logo=python&logoColor=white](https://img.shields.io/badge/python-3776AB?style=for-the-badge&logo=python&logoColor=white)">
<img src="[https://img.shields.io/badge/pandas-150458?style=for-the-badge&logo=pandas&logoColor=white](https://www.google.com/search?q=https://img.shields.io/badge/pandas-150458%3Fstyle%3Dfor-the-badge%26logo%3Dpandas%26logoColor%3Dwhite)">
<img src="[https://img.shields.io/badge/scikit_learn-F7931E?style=for-the-badge&logo=scikit-learn&logoColor=white](https://www.google.com/search?q=https://img.shields.io/badge/scikit_learn-F7931E%3Fstyle%3Dfor-the-badge%26logo%3Dscikit-learn%26logoColor%3Dwhite)">
<img src="[https://img.shields.io/badge/LightGBM-KP003B?style=for-the-badge&logo=lightgbm&logoColor=white](https://www.google.com/search?q=https://img.shields.io/badge/LightGBM-KP003B%3Fstyle%3Dfor-the-badge%26logo%3Dlightgbm%26logoColor%3Dwhite)">
<img src="[https://img.shields.io/badge/CatBoost-F5A962?style=for-the-badge&logo=catboost&logoColor=white](https://www.google.com/search?q=https://img.shields.io/badge/CatBoost-F5A962%3Fstyle%3Dfor-the-badge%26logo%3Dcatboost%26logoColor%3Dwhite)">
<img src="[https://img.shields.io/badge/TensorFlow-FF6F00?style=for-the-badge&logo=tensorflow&logoColor=white](https://img.shields.io/badge/TensorFlow-FF6F00?style=for-the-badge&logo=tensorflow&logoColor=white)">
</div>

## 📂 프로젝트 구조 (Structure)

```bash
├── data/
│   ├── electric_train.csv    # 훈련 데이터
│   └── electric_test.csv     # 테스트 데이터
├── notebooks/
│   └── elect.ipynb           # 데이터 전처리, EDA 및 모델링 전체 코드
├── results/
│   └── model_performance.png # 모델 성능 비교표
└── README.md

```

## 🚀 활용 방안 (Application)

* **전력 수급 안정화:** 기상 예보에 따른 정확한 수요 예측으로 예비 전력 관리 효율화.
* **에너지 절약 유도:** 전력기상지수가 높은 시간대에 아파트 단지 내 절전 방송 등 캠페인 활용.
* **스마트 그리드:** 지역별/단지별 특성에 맞춘 지능형 전력망 운영 기초 자료로 활용.

---

*이 프로젝트는 전력 데이터를 활용한 공모전 참여작입니다.*
