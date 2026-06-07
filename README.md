# 개인 글쓰기 패턴 학습 기반 문체 일관성 검증 및 AI 사용 탐지 모델

## Members

| Name | Department / Year | Email                                                         | Role                            |
| ---- | ----------------- | ------------------------------------------------------------- | ------------------------------- |
| 위강현  | 컴퓨터소프트웨어학부 1학년    | [kanghyun.we@hanyang.ac.kr](mailto:kanghyun.we@hanyang.ac.kr) | 데이터셋 수집 및 정리, 조원 글 작성 데이터 관리    |
| 천재민  | 컴퓨터소프트웨어학부 1학년    | [darksen64@hanyang.ac.kr](mailto:darksen64@hanyang.ac.kr)     | 모델 구현, 전처리 코드 작성, 실험 설계 및 결과 분석 |
| 최정호  | 컴퓨터소프트웨어학부 1학년    | [jungho31@hanyang.ac.kr](mailto:jungho31@hanyang.ac.kr)       | 결과 시각화, 블로그 작성, 영상 녹화 준비        |

## Video / Audio Link

[여기에 YouTube 링크 넣기 -> TODO]


---

## Table of Contents

I. Proposal

II. Datasets

III. Methodology

IV. Evaluation & Analysis

V. Related Work

VI. Conclusion

---

# I. Proposal

## Motivation

최근 ChatGPT와 같은 생성형 AI는 과제, 자기소개서, 블로그 글쓰기 등 다양한 글쓰기 영역에서 널리 사용되고 있다. 이에 따라 교육 현장에서는 학생이 직접 작성한 글과 AI가 생성한 글을 구분하는 문제가 중요한 이슈가 되고 있다. 실제로 GPT 탐지기나 카피킬러와 같은 도구는 특정 글이 AI에 의해 작성되었을 가능성을 수치로 제시하지만, 이러한 점수만으로 AI 사용 여부를 단정하기에는 한계가 있다.

기존 AI 탐지기는 일반적으로 글 자체의 문장 구조, 단어 사용, 표현 패턴 등을 바탕으로 AI 생성 가능성을 판단한다. 그러나 실제 작성자는 사람마다 고유한 문체와 글쓰기 습관을 가지고 있다. 어떤 사람은 원래 문장을 정돈되게 쓰고, 어떤 사람은 구어체에 가까운 표현을 많이 사용한다. 따라서 단순히 “이 글이 AI처럼 보이는가?”만 판단하면, 원래 문체가 정돈된 사람의 글이 AI 글로 오탐될 가능성이 있다.

본 프로젝트는 이러한 문제의식에서 출발하였다. 기존 AI 탐지 방식이 글 자체의 AI 생성 가능성만 판단한다면, 본 프로젝트는 여기에 작성자의 기존 글쓰기 패턴과의 문체 일관성을 함께 고려하는 방식을 실험적으로 제안한다.

본 프로젝트의 핵심 질문은 다음과 같다.

* 이 글이 일반적인 AI 생성 글처럼 보이는가?
* 이 글이 기존 작성자의 문체와 얼마나 유사한가?
* AI 유사도와 개인 문체 유사도를 함께 고려하면 더 해석 가능한 AI 의심 점수를 만들 수 있는가?

## Goal

본 프로젝트의 목표는 공개 AI 텍스트 데이터셋을 활용하여 일반 AI 탐지 모델을 구현하고, 조원들이 직접 작성한 한국어 데이터셋을 활용하여 개인 문체 일관성 검증 방식을 실험하는 것이다.

구체적인 목표는 다음과 같다.

1. 공개 영어 AI 탐지 데이터셋을 불러오고 구조를 분석한다.
2. 결측값 제거, 텍스트 정제, 단어 수 기반 이상치 제거 등 전처리를 수행한다.
3. TF-IDF 기반 텍스트 벡터화를 적용한다.
4. Logistic Regression, Naive Bayes, Random Forest, Linear SVM 모델을 학습하고 비교한다.
5. Accuracy, Precision, Recall, F1-score, Confusion Matrix를 통해 모델 성능을 평가한다.
6. Shuffled Label Sanity Check를 통해 모델이 우연히 높은 성능을 보인 것이 아닌지 확인한다.
7. Cross-Dataset Test를 통해 서로 다른 공개 데이터셋 간 일반화 성능을 확인한다.
8. 조원 직접 작성 한국어 글을 기반으로 개인 문체 유사도를 계산한다.
9. AI 유사도와 개인 문체 유사도를 결합한 Adjusted Suspicion Score를 제안한다.

## Overall Project Pipeline

본 프로젝트는 다음과 같은 흐름으로 진행하였다.

1. 공개 영어 AI 탐지 데이터셋 2개 수집
2. 데이터셋 구조 확인 및 라벨 분포 확인
3. 결측값 확인 및 텍스트 전처리
4. 단어 수 기준 이상치 제거 및 균형 샘플링
5. TF-IDF 기반 텍스트 벡터화
6. Logistic Regression, Naive Bayes, Random Forest, Linear SVM 모델 비교
7. Accuracy, Precision, Recall, F1-score, Confusion Matrix 기반 성능 평가
8. Shuffled Label Sanity Check로 모델이 우연히 높은 성능을 낸 것이 아닌지 확인
9. Cross-Dataset Test로 서로 다른 데이터셋 간 일반화 성능 확인
10. 조원 한국어 글을 기반으로 개인 문체 profile 구성
11. Personal Similarity와 AI Similarity Proxy 계산
12. Adjusted Suspicion Score 제안 및 해석

<img width="1672" height="941" alt="image" src="https://github.com/user-attachments/assets/97ff0717-301c-47ee-a978-d2e18ecbf657" />


---

# II. Datasets

본 프로젝트에서는 두 종류의 데이터셋을 사용하였다.

첫 번째는 공개 영어 AI 탐지 데이터셋이다. 이는 일반적인 AI 생성 텍스트 탐지 모델을 학습하고 평가하기 위해 사용하였다. 두 번째는 조원 직접 작성 한국어 데이터셋이다. 이는 특정 작성자의 기존 문체와 새 글 사이의 일관성을 검증하기 위해 사용하였다.

## 1. Public English AI Detection Datasets

공개 영어 데이터셋으로는 다음 두 개를 사용하였다.

| Dataset                 | Description                                            | Label     |
| ----------------------- | ------------------------------------------------------ | --------- |
| Training_Essay_Data.csv | 사람 작성 essay와 AI 생성 essay로 구성된 영어 텍스트 데이터셋              | generated |
| AI_Human.csv            | Human-written text와 AI-generated text로 구성된 영어 텍스트 데이터셋 | generated |

두 데이터셋 모두 다음과 같은 기본 구조를 가진다.

| Column    | Description                                     |
| --------- | ----------------------------------------------- |
| text      | 글 본문                                            |
| generated | 라벨. 0은 Human-written text, 1은 AI-generated text |

라벨은 다음과 같이 해석하였다.

| Label | Meaning            |
| ----: | ------------------ |
|     0 | Human-written text |
|     1 | AI-generated text  |

각 데이터셋은 크기가 매우 크기 때문에, Colab 환경에서 안정적으로 실험하기 위해 전처리 후 균형 샘플링을 수행하였다.

| Dataset             | Human Samples | AI Samples | Total |
| ------------------- | ------------: | ---------: | ----: |
| Training_Essay_Data |         2,000 |      2,000 | 4,000 |
| AI_Human            |         2,000 |      2,000 | 4,000 |

[여기에 코랩 데이터셋 불러오기 결과 캡처를 넣고 싶다면 삽입]

```markdown
![Dataset Loading Result](results/figures/[여기에_파일명_삽입].png)
```

다만 데이터셋 크기와 라벨 분포는 표로 충분히 설명 가능하므로, 본문에서는 표 중심으로 정리하였다.

## 2. Korean Personal Writing Style Dataset

영어 공개 데이터셋은 일반 AI 탐지 모델을 구현하는 데 사용하였지만, 영어 모델을 한국어 글에 직접 적용하지는 않았다. TF-IDF 기반 모델은 학습 데이터의 단어와 표현 패턴을 중심으로 작동하기 때문에, 영어 데이터로 학습한 모델을 한국어 글에 적용하면 언어 차이로 인한 왜곡이 발생할 수 있기 때문이다.

본 프로젝트에서 영어 공개 데이터셋 기반 모델과 한국어 개인 문체 실험은 같은 목적을 가진 하나의 모델이 아니라, 서로 다른 역할을 가진 두 단계 실험이다. 영어 데이터셋은 일반 AI 탐지 모델의 작동 방식과 성능을 확인하기 위한 기준 실험이며, 한국어 데이터셋은 특정 작성자의 기존 문체와 새 글의 일관성을 확인하기 위한 개인화 실험이다. 따라서 영어로 학습한 TF-IDF 모델을 한국어 글에 직접 적용하지 않았다.

한국어 데이터셋은 별도의 개인 문체 일관성 검증 실험에 사용하였다. 조원 3명이 직접 작성한 한국어 글을 기반으로 개인별 writing profile을 구성하고, 새 글이 기존 작성자의 문체와 얼마나 유사한지 확인하였다.

한국어 데이터셋은 다음과 같이 구성하였다.

| Data Type     | Description                   |
| ------------- | ----------------------------- |
| profile       | 조원의 기존 글쓰기 패턴을 만들기 위한 글       |
| validation    | 조원이 직접 작성한 검증용 새 글            |
| ai_comparison | 동일 또는 유사 주제에 대해 AI가 작성한 비교군 글 |

조원별 데이터 구성은 다음과 같다.

| Member | Profile Texts | Validation Texts | AI Comparison Texts |
| ------ | ------------: | ---------------: | ------------------: |
| 위강현    |            10 |                5 |                  15 |
| 천재민    |            10 |                5 |                  15 |
| 최정호    |            10 |                5 |                  15 |
| Total  |            30 |               15 |                  45 |

한국어 실험에서는 단순히 사람 글과 AI 글을 분류하는 것이 아니라, 각 조원의 기존 글과 새 글 사이의 문체 유사도를 계산하였다.

본 한국어 데이터셋은 조원들이 직접 작성한 profile 및 validation 글과 동일 또는 유사 주제에 대해 생성한 AI comparison 글로 구성하였다.

---

# III. Methodology

본 프로젝트는 크게 두 개의 실험으로 구성된다.

첫 번째는 공개 영어 데이터셋을 활용한 일반 AI 탐지 모델 구현이다. 두 번째는 조원 한국어 데이터셋을 활용한 개인 문체 일관성 검증 실험이다.

---

## Experiment 1. Public Dataset 기반 일반 AI 탐지 모델

### 1. Data Loading

먼저 두 개의 영어 공개 데이터셋을 불러왔다.

* Training_Essay_Data.csv
* AI_Human.csv

각 데이터셋의 주요 컬럼은 `text`와 `generated`이며, `generated`는 AI 생성 여부를 나타내는 라벨이다.

### 2. Missing Value Check

두 영어 데이터셋 모두 `text`와 `generated` 컬럼에 결측값이 존재하지 않았다.

| Dataset             | text Missing | generated Missing |
| ------------------- | -----------: | ----------------: |
| Training_Essay_Data |            0 |                 0 |
| AI_Human            |            0 |                 0 |

한국어 Template 데이터셋의 경우 `text` 컬럼에 결측값 45개가 존재하였다. 이는 조원들이 직접 작성해야 하는 빈 입력 칸이므로 정상적인 구조로 판단하였다. 코드 검증 단계에서는 결측값이 없는 Korean Demo 또는 실제 작성 완료 데이터셋을 사용하였다.

### 3. Text Preprocessing

영어 텍스트에 대해 다음 전처리를 수행하였다.

* 소문자 변환
* 불필요한 공백 제거
* 기본 영어 문자, 숫자, 문장부호 외 특수문자 제거
* 단어 수 계산
* 너무 짧거나 긴 글 제거

너무 짧은 글은 모델이 충분한 정보를 얻기 어렵고, 너무 긴 글은 학습에 과도한 영향을 줄 수 있기 때문에 단어 수 기준으로 이상치를 제거하였다.

본 실험에서는 다음 기준을 적용하였다.

| Criterion          | Value |
| ------------------ | ----: |
| Minimum Word Count |    50 |
| Maximum Word Count |  1000 |

### 4. Balanced Sampling

두 데이터셋은 라벨 분포와 데이터 크기가 다르기 때문에, 모델 학습의 균형을 맞추기 위해 각 라벨에서 동일한 수의 샘플을 추출하였다.

| Dataset             | Human |    AI | Total |
| ------------------- | ----: | ----: | ----: |
| Training_Essay_Data | 2,000 | 2,000 | 4,000 |
| AI_Human            | 2,000 | 2,000 | 4,000 |

### 5. Train-Test Split

모델 평가를 위해 학습 데이터와 테스트 데이터를 6:4 비율로 분리하였다.

| Split | Ratio |
| ----- | ----: |
| Train |   60% |
| Test  |   40% |

라벨 비율이 학습셋과 테스트셋에서 동일하게 유지되도록 stratify 옵션을 적용하였다.

### 6. Text Vectorization

텍스트 데이터를 머신러닝 모델에 입력하기 위해 TF-IDF Vectorizer를 사용하였다. TF-IDF는 특정 문서에서 자주 등장하지만 전체 문서에서는 상대적으로 덜 등장하는 단어에 높은 가중치를 부여하는 방식이다.

본 프로젝트에서는 unigram과 bigram을 함께 사용하였다.

| Parameter    | Value  |
| ------------ | ------ |
| max_features | 5000   |
| ngram_range  | (1, 2) |

### 7. Classification Models

다음 네 가지 모델을 비교하였다.

| Model               | Description                  |
| ------------------- | ---------------------------- |
| Logistic Regression | 선형 분류 기반의 기본적인 분류 모델         |
| Naive Bayes         | 텍스트 분류에서 자주 사용되는 확률 기반 모델    |
| Random Forest       | 여러 결정 트리를 결합한 앙상블 모델         |
| Linear SVM          | 고차원 희소 텍스트 데이터에 강한 선형 SVM 모델 |

---

## Experiment 2. Korean Personal Writing Consistency Test

영어 공개 데이터셋은 일반 AI 탐지 모델의 작동 방식을 확인하는 데 사용하였다. 그러나 한국어 조원 데이터셋은 영어 모델에 직접 입력하지 않고, 별도의 문체 일관성 검증 방식으로 분석하였다.

### 1. Korean Text Preprocessing

한국어 텍스트에 대해서는 다음 전처리를 수행하였다.

* 불필요한 공백 제거
* 한글, 영문, 숫자, 기본 문장부호 외 문자 제거
* 글자 수 계산
* 단어 수 계산

한국어는 영어와 달리 띄어쓰기와 형태소 분석의 영향을 크게 받기 때문에, 단어 단위보다 문자 단위 n-gram이 더 안정적일 수 있다. 따라서 한국어 문체 유사도 계산에는 char-level TF-IDF를 사용하였다.

### 2. Personal Profile Construction

각 조원별로 profile 글 10개를 모아 개인 문체 profile을 구성하였다. 이후 validation 글과 AI comparison 글이 해당 조원의 profile과 얼마나 유사한지 cosine similarity로 계산하였다.

### 3. Personal Similarity

Personal Similarity는 새 글이 해당 조원의 기존 글쓰기 패턴과 얼마나 유사한지를 나타낸다.

계산 방식은 다음과 같다.

1. 조원별 profile 글을 TF-IDF 벡터로 변환한다.
2. 평가 대상 글도 동일한 vectorizer로 변환한다.
3. 평가 대상 글과 해당 조원의 profile 글들 사이의 cosine similarity를 계산한다.
4. 여러 profile 글과의 유사도 평균을 Personal Similarity로 사용한다.

### 4. AI Similarity Proxy

AI Similarity Proxy는 평가 대상 글이 AI comparison 글들과 얼마나 유사한지를 나타낸다. 이는 영어 기반 AI 탐지 모델을 한국어에 직접 적용하지 않기 위한 대체 지표이다.

계산 방식은 다음과 같다.

1. AI comparison 글 전체를 TF-IDF 벡터로 변환한다.
2. 평가 대상 글과 AI comparison 글들 사이의 cosine similarity를 계산한다.
3. 평균 유사도를 AI Similarity Proxy로 사용한다.

### 5. Adjusted Suspicion Score

최종적으로 AI 유사도와 개인 문체 불일치 정도를 결합하여 Adjusted Suspicion Score를 계산하였다.

```text
Adjusted Suspicion Score
= 0.7 × AI Similarity Proxy
+ 0.3 × (100 - Personal Similarity)
```

이 점수는 AI 사용 여부를 단정하기 위한 기준이 아니다. 일반 AI 유사도와 개인 문체 일관성을 함께 고려하기 위한 보조 지표이다.

해석 기준은 다음과 같다.

| Adjusted Suspicion Score | Interpretation |
| -----------------------: | -------------- |
|                     0~40 | Low            |
|                    40~70 | Medium         |
|                   70~100 | High           |

또한 AI 유사도와 개인 문체 유사도의 조합에 따라 다음과 같이 해석하였다.

| AI Similarity | Personal Similarity | Interpretation                    |
| ------------- | ------------------- | --------------------------------- |
| 낮음            | 높음                  | AI 의심 낮음 / 개인 문체와 유사              |
| 높음            | 낮음                  | AI 의심 높음 / 개인 문체와 불일치             |
| 낮음            | 낮음                  | AI 유사도는 낮지만 개인 문체와도 불일치           |
| 높음            | 높음                  | AI 유사도는 높지만 개인 문체와도 유사 / 추가 확인 필요 |
| 중간            | 중간                  | 중간 수준 / 추가 확인 필요                  |

---

# IV. Evaluation & Analysis

## 1. Evaluation Metrics

모델 평가는 다음 지표를 사용하였다.

| Metric           | Korean | Description                |
| ---------------- | ------ | -------------------------- |
| Accuracy         | 정확도    | 전체 샘플 중 올바르게 분류한 비율        |
| Precision        | 정밀도    | AI라고 예측한 것 중 실제 AI인 비율     |
| Recall           | 재현율    | 실제 AI 글 중 AI라고 올바르게 예측한 비율 |
| F1-score         | F1 점수  | Precision과 Recall의 조화평균    |
| Confusion Matrix | 혼동 행렬  | 실제 라벨과 예측 라벨의 관계를 보여주는 표   |

---

## 2. Public Dataset Model Performance

두 공개 영어 데이터셋에서 네 가지 모델을 비교한 결과, Linear SVM이 가장 높은 F1-score를 기록하였다.

| Model               | AI_Human F1-score | Training_Essay F1-score |
| ------------------- | ----------------: | ----------------------: |
| Linear SVM          |             98.50 |                   98.56 |
| Logistic Regression |             96.85 |                   97.24 |
| Naive Bayes         |             92.22 |                   95.40 |
| Random Forest       |             97.00 |                   97.78 |

[여기에 코랩 23단계 결과 이미지 삽입: Model Performance Comparison by Dataset]

```markdown
![Model F1-score Comparison](results/figures/model_f1_comparison_public_datasets.png)
```

Figure 1. 두 공개 데이터셋에서 모델별 F1-score를 비교한 결과, Linear SVM이 가장 안정적인 성능을 보였다.

Linear SVM은 두 데이터셋 모두에서 가장 안정적인 성능을 보였다. 이는 TF-IDF로 변환된 텍스트 데이터가 고차원 희소 벡터 형태를 가지며, Linear SVM이 이러한 데이터 구조에서 강점을 보이기 때문으로 해석할 수 있다.

---

## 3. Confusion Matrix Analysis

### Training_Essay Dataset

[여기에 코랩 24단계 첫 번째 결과 이미지 삽입: Training_Essay Confusion Matrix]

```markdown
![Confusion Matrix - Training Essay](results/figures/confusion_matrix_Training_Essay_Linear_SVM.png)
```

Figure 2. Training_Essay 데이터셋에서 Linear SVM의 Confusion Matrix.

Training_Essay 데이터셋에서 Linear SVM은 총 1,600개의 테스트 데이터 중 1,577개를 정확히 분류하였다.

* Human-written text 800개 중 790개를 Human으로 올바르게 분류하였다.
* AI-generated text 800개 중 787개를 AI로 올바르게 분류하였다.
* Human을 AI로 잘못 분류한 경우는 10개였다.
* AI를 Human으로 잘못 분류한 경우는 13개였다.

이는 모델이 한쪽 라벨에 치우치지 않고 Human과 AI를 비교적 균형 있게 분류했음을 보여준다.

### AI_Human Dataset

[여기에 코랩 24단계 두 번째 결과 이미지 삽입: AI_Human Confusion Matrix]

```markdown
![Confusion Matrix - AI Human](results/figures/confusion_matrix_AI_Human_Linear_SVM.png)
```

Figure 3. AI_Human 데이터셋에서 Linear SVM의 Confusion Matrix.

AI_Human 데이터셋에서 Linear SVM은 총 1,600개의 테스트 데이터 중 1,576개를 정확히 분류하였다.

* Human-written text 800개 중 790개를 Human으로 올바르게 분류하였다.
* AI-generated text 800개 중 786개를 AI로 올바르게 분류하였다.
* Human을 AI로 잘못 분류한 경우는 10개였다.
* AI를 Human으로 잘못 분류한 경우는 14개였다.

AI_Human 데이터셋에서도 모델은 두 클래스를 안정적으로 구분하였다.

---

## 4. Shuffled Label Sanity Check

모델 성능이 데이터 누수나 코드 오류로 인해 과대평가된 것인지 확인하기 위해 라벨을 무작위로 섞은 sanity check를 수행하였다. 라벨을 섞은 경우 텍스트와 정답 라벨 사이의 실제 관계가 사라지므로, 정상적인 모델이라면 성능이 약 50% 수준으로 떨어져야 한다.

실험 결과는 다음과 같다.

| Dataset        | Accuracy | F1-score |
| -------------- | -------: | -------: |
| Training_Essay |    49.81 |    50.22 |
| AI_Human       |    51.12 |    49.61 |

두 데이터셋 모두 Accuracy와 F1-score가 약 50% 수준으로 하락하였다. 이는 기존 모델이 무작위 라벨을 학습한 것이 아니라, 실제 Human-written text와 AI-generated text 사이의 텍스트 패턴 차이를 학습했음을 보여준다.

[선택 사항: 코랩 25단계 출력 캡처를 넣고 싶다면 여기에 삽입. 단, 필수는 아님]

```markdown
![Shuffled Label Sanity Check](results/figures/[여기에_파일명_삽입].png)
```

---

## 5. Cross-Dataset Test

동일 데이터셋 내부에서 train/test를 나누어 평가하면, 모델이 해당 데이터셋의 특정 패턴에 과도하게 맞춰졌을 가능성이 있다. 따라서 본 프로젝트에서는 서로 다른 공개 데이터셋 간 cross-dataset test를 수행하였다.

실험 방식은 다음과 같다.

1. Training_Essay 데이터셋으로 학습한 뒤 AI_Human 데이터셋으로 테스트
2. AI_Human 데이터셋으로 학습한 뒤 Training_Essay 데이터셋으로 테스트

결과는 다음과 같다.

| Train Dataset  | Test Dataset   | Accuracy | Precision | Recall | F1-score |
| -------------- | -------------- | -------: | --------: | -----: | -------: |
| Training_Essay | AI_Human       |    95.55 |     97.75 |  93.25 |    95.45 |
| AI_Human       | Training_Essay |    98.78 |     98.75 |  98.80 |    98.78 |

Cross-Dataset Test에서도 F1-score가 95% 이상으로 나타났다. 이는 모델이 특정 데이터셋 내부의 패턴만 학습한 것이 아니라, 두 공개 데이터셋에서 공통적으로 나타나는 Human-written text와 AI-generated text의 표현 차이를 어느 정도 학습했음을 보여준다.

### Training_Essay → AI_Human

[여기에 코랩 26단계 첫 번째 결과 이미지 삽입: Cross-Dataset Training_Essay to AI_Human]

```markdown
![Cross-Dataset Test - Training Essay to AI Human](results/figures/cross_dataset_Training_Essay_to_AI_Human.png)
```

Figure 4. Training_Essay로 학습한 모델을 AI_Human 데이터셋에 적용한 Cross-Dataset Confusion Matrix.

Training_Essay로 학습하여 AI_Human을 테스트했을 때는 AI 글을 Human으로 오분류한 사례가 상대적으로 더 많았다. 이는 AI_Human 데이터셋에 포함된 일부 AI-generated text가 Training_Essay 데이터셋의 AI 글과 다른 표현 패턴을 가지고 있었거나, 사람 글과 더 유사한 문체를 가졌기 때문으로 해석할 수 있다.

### AI_Human → Training_Essay

[여기에 코랩 26단계 두 번째 결과 이미지 삽입: Cross-Dataset AI_Human to Training_Essay]

```markdown
![Cross-Dataset Test - AI Human to Training Essay](results/figures/cross_dataset_AI_Human_to_Training_Essay.png)
```

Figure 5. AI_Human으로 학습한 모델을 Training_Essay 데이터셋에 적용한 Cross-Dataset Confusion Matrix.

AI_Human 데이터셋으로 학습하여 Training_Essay를 테스트한 경우에는 더 균형 잡힌 결과가 나타났다. 이 결과는 AI_Human 데이터셋이 상대적으로 더 다양한 표현 패턴을 포함하고 있어 Training_Essay 데이터셋으로의 일반화가 잘 이루어졌을 가능성을 시사한다.

---

## 6. Korean Personal Style Consistency Experiment

영어 공개 데이터셋 실험은 일반 AI 탐지 모델의 성능을 확인하는 데 사용하였다. 그러나 실제 교육 환경에서 중요한 문제는 단순히 “이 글이 AI처럼 보이는가?”뿐만 아니라, “이 글이 작성자의 기존 문체와 일관적인가?”이다.

이를 확인하기 위해 조원 직접 작성 한국어 데이터셋을 활용하여 개인 문체 일관성 실험을 수행하였다.

Role별 평균 결과는 다음과 같다.

| Role          | Personal Similarity Score | AI Similarity Proxy | Adjusted Suspicion Score |
| ------------- | ------------------------: | ------------------: | -----------------------: |
| ai_comparison |                      7.62 |               77.54 |                    81.99 |
| validation    |                     83.46 |                6.83 |                     9.75 |

[여기에 코랩 33단계 결과 이미지 삽입: Korean Personal Style Consistency Experiment]

```markdown
![Korean Personal Style Consistency Experiment](results/figures/korean_personal_style_role_summary.png)
```

Figure 6. Validation 글과 AI comparison 글의 Personal Similarity, AI Similarity Proxy, Adjusted Suspicion Score 비교.

Validation 데이터는 조원이 직접 작성한 새 글에 해당한다. 이 데이터는 기존 profile 글과 높은 문체 유사도를 보였고, AI comparison 글과의 유사도는 낮게 나타났다. 그 결과 Adjusted Suspicion Score는 9.75점으로 낮게 나타났다.

반면 AI comparison 데이터는 조원 profile과의 문체 유사도가 낮고, AI comparison 전체와의 유사도가 높게 나타났다. 이에 따라 Adjusted Suspicion Score는 81.99점으로 높게 나타났다.

조원별 결과는 다음과 같다.

| Member | Role          | Personal Similarity Score | AI Similarity Proxy | Adjusted Suspicion Score |
| ------ | ------------- | ------------------------: | ------------------: | -----------------------: |
| 위강현    | ai_comparison |                      6.83 |               76.09 |                    81.21 |
| 위강현    | validation    |                     83.46 |                6.83 |                     9.75 |
| 천재민    | ai_comparison |                      7.62 |               78.06 |                    82.35 |
| 천재민    | validation    |                     83.46 |                6.83 |                     9.75 |
| 최정호    | ai_comparison |                      8.41 |               78.49 |                    82.42 |
| 최정호    | validation    |                     83.46 |                6.83 |                     9.75 |

조원별로도 동일한 경향이 나타났다. Validation 글은 개인 문체 유사도가 높고 AI 유사도는 낮았으며, AI comparison 글은 개인 문체 유사도가 낮고 AI 유사도는 높았다.

이 결과는 개인 문체 유사도와 AI 유사도를 결합한 Adjusted Suspicion Score가 AI 사용 여부를 단정하는 기준은 아니지만, 기존 AI 탐지 결과를 보완하는 보조 지표로 활용될 가능성이 있음을 보여준다.

---

# V. Related Work

본 프로젝트에서는 다음 자료와 도구를 참고하였다.

## 1. Scikit-learn

본 프로젝트의 모델 구현에는 Scikit-learn 라이브러리를 사용하였다. Scikit-learn은 Python 기반의 대표적인 머신러닝 라이브러리로, 텍스트 벡터화, 분류 모델 학습, 평가 지표 계산 등을 지원한다.

사용한 주요 기능은 다음과 같다.

* TfidfVectorizer
* LogisticRegression
* MultinomialNB
* RandomForestClassifier
* LinearSVC
* train_test_split
* accuracy_score
* precision_score
* recall_score
* f1_score
* confusion_matrix

## 2. TF-IDF

TF-IDF는 텍스트 데이터를 수치 벡터로 변환하기 위한 대표적인 방법이다. 본 프로젝트에서는 영어 공개 데이터셋에 대해 단어 기반 TF-IDF를 사용하였고, 한국어 개인 문체 실험에는 문자 n-gram 기반 TF-IDF를 사용하였다.

영어 데이터에서는 unigram과 bigram을 사용하여 단어와 단어 조합의 패턴을 반영하였다. 한국어 데이터에서는 띄어쓰기와 형태소 분석 문제를 줄이기 위해 char-level n-gram을 적용하였다.

## 3. Linear SVM

Linear SVM은 고차원 희소 데이터에서 강한 성능을 보이는 분류 모델이다. 본 프로젝트의 공개 영어 데이터셋 실험에서도 Linear SVM은 두 데이터셋 모두에서 가장 높은 F1-score를 기록하였다.

## 4. Cosine Similarity

한국어 개인 문체 실험에서는 새 글과 기존 작성자 profile 글 사이의 유사도를 계산하기 위해 cosine similarity를 사용하였다. Cosine similarity는 두 벡터의 방향이 얼마나 유사한지를 측정하는 방법으로, 텍스트 유사도 계산에 자주 활용된다.

## 5. AI-generated Text Detection

AI-generated text detection 연구는 사람이 작성한 글과 AI가 생성한 글을 구분하는 문제를 다룬다. 기존 연구와 도구들은 주로 문장 구조, 어휘 사용, 통계적 패턴, 언어 모델의 생성 확률 등을 활용한다. 본 프로젝트는 이러한 일반 AI 탐지 접근에 더해, 작성자의 기존 문체와의 일관성을 함께 고려하는 보조적 접근을 실험하였다.

## References

[여기에 참고한 Kaggle 데이터셋, Scikit-learn 문서, 관련 논문 또는 블로그 URL 추가]

예시:

```markdown
- Scikit-learn Documentation: https://scikit-learn.org/stable/
- TfidfVectorizer Documentation: https://scikit-learn.org/stable/modules/generated/sklearn.feature_extraction.text.TfidfVectorizer.html
- LinearSVC Documentation: https://scikit-learn.org/stable/modules/generated/sklearn.svm.LinearSVC.html
- Kaggle Dataset 1: [여기에 Training_Essay_Data 출처 링크]
- Kaggle Dataset 2: [여기에 AI_Human 출처 링크]
```

---

# VI. Conclusion

본 프로젝트는 기존 AI 탐지기가 글 자체의 AI 생성 가능성만을 중심으로 판단한다는 한계에 주목하였다. 실제 교육 환경에서는 작성자마다 고유한 글쓰기 습관과 문체가 존재하기 때문에, 단일 AI 탐지 점수만으로 AI 사용 여부를 판단하면 오탐 가능성이 발생할 수 있다.

이를 확인하기 위해 먼저 두 개의 공개 영어 AI 탐지 데이터셋을 활용하여 일반 AI 탐지 모델을 구현하였다. TF-IDF 기반 텍스트 벡터화와 Logistic Regression, Naive Bayes, Random Forest, Linear SVM 모델을 적용한 결과, Linear SVM이 두 데이터셋 모두에서 가장 높은 F1-score를 기록하였다. 또한 Shuffled Label Sanity Check를 통해 모델이 무작위 라벨을 학습한 것이 아님을 확인하였고, Cross-Dataset Test를 통해 서로 다른 데이터셋 간 일반화 성능도 확인하였다.

그러나 공개 영어 데이터셋으로 학습한 모델은 특정 작성자의 기존 글쓰기 패턴을 고려하지 못하며, 한국어 글에 직접 적용하기도 어렵다. 따라서 본 프로젝트에서는 별도의 한국어 조원 작성 데이터셋을 활용하여 개인 문체 일관성 검증 실험을 수행하였다. 조원별 기존 글을 profile로 설정하고, 새 글과 AI 비교군이 해당 profile과 얼마나 유사한지 cosine similarity 기반으로 계산하였다.

실험 결과, 조원이 직접 작성한 validation 글은 기존 profile과 높은 유사도를 보였고 AI comparison과의 유사도는 낮게 나타났다. 반대로 AI comparison 글은 개인 profile과의 유사도는 낮고 AI comparison과의 유사도는 높게 나타났다. 이를 바탕으로 AI Similarity Proxy와 Personal Similarity를 결합한 Adjusted Suspicion Score를 계산하였다.

본 프로젝트의 Adjusted Suspicion Score는 AI 사용 여부를 단정하기 위한 절대적 기준이 아니다. 대신 일반 AI 탐지 결과와 개인 문체 일관성을 함께 고려하는 보조 지표로 활용될 수 있다. 특히 일반 AI 탐지 모델이 높은 AI 가능성을 보이더라도, 해당 글이 작성자의 기존 문체와 매우 유사하다면 단순히 AI 사용으로 단정하기 어렵다. 반대로 AI 가능성이 중간 수준이더라도 개인 문체와 크게 다르다면 추가 확인이 필요할 수 있다.

따라서 본 프로젝트는 기존 AI 탐지 방식의 한계를 보완하기 위해, AI 생성 가능성과 개인 문체 일관성을 함께 고려하는 실험적 접근을 제안하였다는 점에서 의의가 있다. 특히나, 본 프로젝트는 기존 AI 탐지기의 결과를 대체하는 모델을 만든 것이 아니라, AI 유사도와 개인 문체 일관성을 함께 고려하여 AI 사용 가능성을 더 해석 가능하게 만드는 보조적 판단 방식을 제안했다는 점에서 의의가 있다는 것을 시사하고 싶다.

## Limitations

본 프로젝트에는 다음과 같은 한계가 있다.

첫째, 공개 데이터셋은 영어 기반이므로 한국어 과제 글에 직접 적용하기 어렵다. 따라서 영어 공개 데이터셋 실험은 일반 AI 탐지 모델의 작동 방식을 확인하는 기준 실험으로 해석해야 한다.

둘째, 공개 데이터셋은 특정 프롬프트와 특정 생성 모델을 기반으로 구성되었을 가능성이 있다. 따라서 실제 교육 환경에서 작성되는 모든 AI 글쓰기 사례를 대표한다고 보기에는 한계가 있다.

셋째, TF-IDF 기반 모델은 문맥의 깊은 의미를 이해하기보다는 단어와 표현 패턴을 중심으로 학습한다. 이로 인해 사람이 AI처럼 정돈된 문체로 작성한 글이나, AI가 사람처럼 자연스럽게 작성한 글에서는 오분류 가능성이 존재한다.

넷째, 한국어 개인 문체 실험은 조원 3명의 제한된 글을 기반으로 진행하였다. 따라서 더 많은 사용자와 다양한 주제의 글을 수집하여 개인 문체 profile의 안정성을 검증할 필요가 있다.

다섯째, Adjusted Suspicion Score의 가중치인 0.7과 0.3은 실험적으로 설정한 기준이다. 향후 더 많은 데이터와 검증 실험을 통해 AI Similarity Proxy와 Personal Similarity의 적절한 가중치를 조정할 필요가 있다.

여섯째, 생성형 AI가 사용자의 기존 글을 참고하여 문체를 모방하도록 프롬프트를 제공받는 경우 우회 가능성이 존재한다. AI가 사용자의 기존 문체와 유사하게 글을 생성하면 Personal Similarity가 높게 나타날 수 있으므로, 본 지표는 AI 사용 여부를 단정하는 기준이 아니라 추가 확인을 돕는 보조 지표로 해석해야 한다.

## Future Work

향후 연구에서는 다음과 같은 방향으로 확장할 수 있다.

1. 더 많은 사용자의 실제 글쓰기 데이터를 수집하여 개인 문체 profile을 강화한다.

2. 한국어 형태소 분석기 또는 KoBERT와 같은 한국어 특화 모델을 적용하여 한국어 문체 분석의 정확도를 높인다.

3. GPT, Claude, Gemini 등 다양한 생성형 AI 모델의 글을 비교하여 특정 모델에만 치우치지 않는 탐지 방식을 검토한다.

4. 사람이 AI 글을 수정한 mixed text를 추가하여 실제 과제 작성 환경에 가까운 상황을 반영한다.

5. 개인 문체 유사도와 AI 탐지 점수의 가중치를 데이터 기반으로 최적화한다.

6. 사용자의 기존 글을 참고하여 생성된 문체 모방형 AI 글을 추가로 수집하고, 이러한 우회 시나리오에서도 Adjusted Suspicion Score가 안정적으로 작동하는지 검증한다.
