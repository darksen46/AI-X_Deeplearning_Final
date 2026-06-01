# 개인 글쓰기 패턴 학습 기반 문체 일관성 검증 및 AI 사용 탐지 모델

## Members

* 위강현, 컴퓨터소프트웨어학부 1학년, 이메일
* 천재민, 컴퓨터소프트웨어학부 1학년, 이메일
* 최정호, 컴퓨터소프트웨어학부 1학년, 이메일

## Table of Contents

1. Proposal
2. Datasets
3. Methodology
4. Evaluation & Analysis
5. Related Work
6. Conclusion

---

## I. Proposal

### Motivation

최근 ChatGPT와 같은 생성형 AI는 과제, 자기소개서, 블로그 글쓰기 등 다양한 글쓰기 영역에서 널리 사용되고 있다. 그러나 AI가 작성한 글은 문장 구조, 어휘 사용, 표현 방식에서 사람의 글과 다른 특징을 보일 수 있다. 특히 글쓰기 결과물에서 AI 사용 여부를 판단하는 문제는 교육, 평가, 저작권, 학습 윤리 측면에서 중요한 이슈가 되고 있다.

본 프로젝트는 개인의 기존 글쓰기 패턴과 AI 생성 글의 문체적 차이를 분석하고, 이를 기반으로 AI 사용 가능성을 탐지하는 모델을 구현하는 것을 목표로 한다. 단순히 AI가 작성한 글을 탐지하는 것뿐만 아니라, 사람의 글과 AI 글 사이에서 나타나는 문장 길이, 단어 사용, 어휘 다양도 등의 차이를 확인하고자 한다.

### Goal

본 프로젝트의 최종 목표는 사람 작성 글과 AI 생성 글을 수집한 뒤, 텍스트 전처리와 특징 추출을 수행하고, 머신러닝 기반 분류 모델을 적용하여 AI 생성 여부를 예측하는 것이다.

구체적인 목표는 다음과 같다.

* 사람 글과 AI 생성 글 데이터셋 구축
* 문장 길이, 단어 수, 어휘 다양도 등 문체 특징 추출
* TF-IDF 기반 텍스트 벡터화 수행
* Logistic Regression, Random Forest 등 분류 모델 적용
* 모델 성능 평가 및 문체 차이 분석

---

## II. Datasets

본 프로젝트에서는 사람 작성 글과 AI 생성 글을 비교하기 위해 직접 데이터셋을 구축할 예정이다.

### Dataset Structure

| Label | Type       | Description              |
| ----- | ---------- | ------------------------ |
| 0     | Human Text | 조원들이 직접 작성한 글            |
| 1     | AI Text    | 동일한 주제에 대해 생성형 AI가 작성한 글 |

### Data Collection Plan

글쓰기 주제는 다음과 같이 구성할 예정이다.

* 인공지능의 장단점
* 나의 진로 목표
* 대학 생활에서 중요한 역량
* 최근 사회 문제에 대한 의견
* 기억에 남는 경험
* 기술 발전이 사회에 미치는 영향

각 주제에 대해 사람 글과 AI 글을 모두 수집하여, 주제 차이가 아닌 문체 차이를 비교할 수 있도록 한다.

### Data Format

최종 데이터는 CSV 파일 형태로 정리할 예정이다.

| text      | label |
| --------- | ----- |
| 사람이 작성한 글 | 0     |
| AI가 생성한 글 | 1     |

---

## III. Methodology

본 프로젝트는 다음 절차로 진행된다.

### 1. Text Data Collection

사람이 직접 작성한 글과 AI가 생성한 글을 수집한다. 같은 주제에 대해 두 종류의 글을 모두 확보하여 비교 가능성을 높인다.

### 2. Text Preprocessing

수집한 텍스트에 대해 전처리를 수행한다.

* 불필요한 공백 제거
* 특수문자 제거
* 문장 단위 분리
* 결측값 제거
* 라벨링 확인

### 3. Feature Extraction

텍스트의 문체적 특징을 수치화한다.

* 전체 단어 수
* 문장 수
* 평균 문장 길이
* 어휘 다양도
* 반복 표현 빈도

### 4. Text Vectorization

텍스트 데이터를 모델에 입력하기 위해 TF-IDF Vectorizer를 사용한다. TF-IDF는 문서 내에서 자주 등장하지만 전체 문서에서는 상대적으로 덜 등장하는 단어에 높은 가중치를 부여하는 방식이다.

### 5. Classification Model

AI 생성 여부를 예측하기 위해 다음 모델을 사용할 예정이다.

* Logistic Regression
* Random Forest Classifier

추가적으로 시간이 허용될 경우, BERT 계열 모델을 활용한 딥러닝 기반 텍스트 분류도 검토할 예정이다.

---

## IV. Evaluation & Analysis

최종 분석에서는 사람 글과 AI 생성 글의 문체적 차이를 시각화하고, 분류 모델의 성능을 평가할 예정이다.

### Evaluation Metrics

* Accuracy
* Precision
* Recall
* F1-score
* Confusion Matrix

### Analysis Plan

다음 항목을 중심으로 분석을 진행할 예정이다.

* Human Text와 AI Text의 평균 문장 길이 비교
* 단어 수 분포 비교
* 어휘 다양도 비교
* 모델별 성능 비교
* 잘못 분류된 사례 분석
* 문체 특징이 AI 탐지에 미치는 영향 분석

---

## V. Related Work

본 프로젝트에서는 다음 자료와 도구를 참고할 예정이다.

* Scikit-learn 공식 문서
* TF-IDF Vectorizer 문서
* Logistic Regression 문서
* Random Forest Classifier 문서
* Hugging Face Transformers 문서
* AI-generated text detection 관련 연구 및 블로그 자료

---

## VI. Conclusion

본 프로젝트는 개인 글쓰기 패턴과 AI 생성 글의 차이를 분석하여 AI 사용 가능성을 탐지하는 모델을 구현하는 것을 목표로 한다. 최종 결과에서는 문장 길이, 어휘 다양도, 반복 표현 등 문체적 특징이 사람 글과 AI 글을 구분하는 데 어느 정도 영향을 미치는지 확인할 예정이다.

현재 단계에서는 프로젝트 주제, 데이터셋 구성 방식, 분석 방법론, 평가 계획을 설계하였다. 이후 실제 데이터셋을 구축하고 분류 모델을 학습시켜 결과를 분석할 예정이다.

### Role Distribution

* 위강현: 데이터셋 수집 및 정리
* 천재민: 모델 구현 및 실험
* 최정호: 결과 분석, 블로그 작성 및 영상 녹화
