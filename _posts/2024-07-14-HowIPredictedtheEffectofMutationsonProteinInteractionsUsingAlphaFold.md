---
title: "AlphaFold를 사용하여 단백질 상호작용에 미치는 돌연변이 효과 예측하기"
description: ""
coverImage: "/assets/img/2024-07-14-HowIPredictedtheEffectofMutationsonProteinInteractionsUsingAlphaFold_0.png"
date: 2024-07-14 01:36
ogImage: 
  url: /assets/img/2024-07-14-HowIPredictedtheEffectofMutationsonProteinInteractionsUsingAlphaFold_0.png
tag: Tech
originalTitle: "How I Predicted the Effect of Mutations on Protein Interactions Using AlphaFold"
link: "https://medium.com/towards-data-science/protein-interactions-alphafold-04eeb8f56d79"
isUpdated: true
---





![How I Predicted the Effect of Mutations on Protein Interactions Using AlphaFold](/assets/img/2024-07-14-HowIPredictedtheEffectofMutationsonProteinInteractionsUsingAlphaFold_0.png)

안녕하세요! 인간 상호작용체(단백질-단백질 상호작용)는 최대 60만 개의 상호작용을 가질 수 있습니다.

이렇게 많은 단백질-단밸질 상호작용(PPI)이 가능한 경우, 질병 유발 돌연변이가 상호작용체에 어떻게 영향을 미치는지 예측하는 것은 헤라클레스의 과업처럼 보일 수 있지만 예상보다는 불가능하지 않습니다.

(특히, 워터루 대학 코옵 학생에게 강력한 GPU 클러스터에 무료 이용, 세계적 수준의 지도 및 어떤 접근 방법이든 추구하도록하는 자유를 줄 때)

<div class="content-ad"></div>

XGBoost와 첨단 딥 러닝 소프트웨어 AlphaFold-Multimer (AF-M), 그리고 47,000개 이상의 SLURM 작업을 활용하여 단백질 상호작용에 대한 미스 sense 돌연변이의 영향을 예측하는 다중 분류기 모델을 만들었어요. AUC가 91%인 이 모델은 정말 훌륭한 결과를 내놨죠.

![이미지](/assets/img/2024-07-14-HowIPredictedtheEffectofMutationsonProteinInteractionsUsingAlphaFold_1.png)

이 글에서는 다음 내용에 대해 살펴볼 거에요:

- 배경: 연구 질문과 선택한 이유에 대해
- 데이터 획득 및 처리: 획득한 단백질 데이터와 전처리 단계에 대한 방법과 중요성
- 머신 러닝 모델: 모델 선택 방법과 단백질 데이터에 대한 구현 방법
- 결과, 모델 정확도 및 특징 중요도: 모델 결과 이해 및 각 클래스별 중요한 특징 분석
- ASD 관련 단백질에 대한 사례 연구: 자폐 스펙트럼 장애 관련 단백질에 초점을 맞춘 두 가지 특정 모델 결과에 대한 심층 분석
- 마지막으로 한 마디와 교훈들: 결론, 주요 포인트, 이 프로젝트의 방향성에 대한 미래 전망들

<div class="content-ad"></div>

**만약 생물정보학이나 분자생물학에 관심이 있고 연구에 기계학습을 접목하고 싶다면 — 이 글을 읽어보세요.**

**시작해볼까요!**

**배경**

**이 섹션에서 다룰 내용:**

<div class="content-ad"></div>

- 연구 중인 질문입니다.
- 이 질문을 선택한 이유입니다.

아주 대담한 주장이긴 하지만: 단백질-단백질 상호작용은 중요합니다. 제가 SickKids의 응용유전체학 센터에서 여덟 달간 이를 연구했기 때문에 편향됐을 수도 있지만 그에 대한 좋은 이유가 있습니다:

- PPIs는 끝없는 대사 과정에 관여합니다.
- PPIs를 영향을 미치는 돌연변이는 질병에서 과다표시됩니다 (Cheng et al., 2021).

그러니까, 특정 돌연변이가 PPIs에 어떤 영향을 미치는지 이해할 수 있다면, 우리가 할 수 있는 일이 많습니다:

<div class="content-ad"></div>

Hi there!

Have you heard about the latest release from DeepMind, AlphaFold-Multimer (AF-M)? It's a super exciting version of AlphaFold that focuses on protein complexes. This opens up a whole new world of opportunities for understanding disease mechanisms and developing more effective drugs.

Now, you might be curious, why the emphasis on missense variants, which are singular residue changes, instead of other types of mutations? Let's delve into this intriguing question together!

Warm regards,

<div class="content-ad"></div>

- AF-M은 포인트 돌연변이로부터 매크로 레벨 구조적 변화를 예측할 수 없어요. 다시 말해, 알려진 구조 파손 돌연변이가 있는 서열을 주더라도 펼쳐진 서열을 생성하지 못할 거에요.
- 그러나 단백질-단백질 상호작용의 인터페이스에서의 물리적 변화는 (ChimeraX와 같은 프로그램을 통해 관측 가능한) 측정 가능한 변화를 가질 수 있어요.

간단히 말해, 이것은 우리가 현재 기술을 활용할 수 있는 연구 주제에요.

자, 이 질문에 대답하는 방법은 무엇일까요? 다음 섹션에서 그에 대한 답을 알려드릴게요.

# 방법론 및 접근법

<div class="content-ad"></div>

여기 이 섹션에서 다룰 내용은:

- 이 문제를 어떻게 정의하는지에 대한 중심 아이디어.
- 우리가 배우려고 하는 것에 대한 개략적인 개요.

AF-M이 생성하는 3D 구조는 독점적이고 관찰 가능한 특성을 가지고 있습니다. 대부분은 다음과 같은 구조적 특징입니다:

- 단백질-단백질 상호작용 영역
- 형태 상보성
- 도킹 점수

<div class="content-ad"></div>

이것은 PDB 구조로부터만 관측할 수 있는 흥미로운 데이터입니다. 3D 구조의 힘을 감안하여 다음 방법론에 도달했습니다:

- 와일드타입 단백질 복합체(그냥 보통의 동이나 이종 이형 이중체)를 취합니다.
- 이번에는 한 단백질에 미생성 변이가 있는 같은 복합체를 취합니다. 실질적으로 `미생성 복합체`입니다.

이러한 복합체의 구조적 특징 사이의 차이를 감지하고 이를 특정 미생성 변이와 관련시킬 수 있다면, 다음과 같은 관련성을 찾을 수 있습니다:


PPI에 미치는 영향 ~ (구조적 특징)


<div class="content-ad"></div>

다시 말해, 구조적 특징에 따른 PPI에 미치는 영향에 대해 말해볼게요.

![example](/assets/img/2024-07-14-HowIPredictedtheEffectofMutationsonProteinInteractionsUsingAlphaFold_2.png)

위의 이미지는 작은 가상 예시를 보여줍니다. 오른쪽에 미싱전환체 하나만으로 같은 두 복합체가 구성되어, 가상 특징 A부터 D에 걸쳐 중요한 차이가 나타납니다.

하지만 여기에 더 많은 기회가 있습니다: 만일 해당 구조 데이터를 변형 주석으로 보강해본다면 어떻게 될까요? 아래는 이에 대한 예시들입니다:

<div class="content-ad"></div>

- CADD, REVEL, AlphaMissense를 통한 병원성 예측
- ClinVar에서의 병원성 주석
- 등장 빈도

우리는 관계를 파악하기 위해 더 많은 기능을 구문 분석할 수 있을 것이며, 보다 포괄적인 기능을 발전시킬 것입니다:

```js
PPI에 미치는 영향 ~ f(x₁, x₂, …, xn).
```

데이터 수집 과정에서, 저는 구조적 특징 위에 40가지 이상의 비구조적 특징을 추가할 수 있었습니다.

<div class="content-ad"></div>

이것은 XGBoost 분류기 모델에 대한 우수한 훈련 데이터로 사용되었으며, 이 모델은 주어진 변형의 PPI에 대한 영향을 다음 네 가지 클래스 중 하나로 예측할 수 있습니다:

- 상호 작용 강도 증가. (단백질 간의 결합이 강화됨)
- 상호 작용 강도 감소. (단백질 간의 결합이 약해짐).
- 상호 작용 방해. (단백질 간 상호 작용이 더 이상 없어짐).
- 상호 작용에 영향 없음. (변형이 상호 작용의 품질을 변경시키지 않음).

아마 궁금해하고 계실 것입니다. 왜 이런 미생물 변이와 PPI 효과를 매치시킨 데이터를 얻었는지, 그리고 왜 XGBoost를 분류기 모델로 선택했는지에 대해 설명해 드리겠습니다:

<div class="content-ad"></div>

# 데이터 획득 및 처리

여기서는 다음 내용을 다룰 거에요:

- 제가 데이터를 얻은 곳 (IntAct 돌연변이 데이터베이스).
- 해당 데이터에서 전처리하고 특성을 가공한 방법.

## IntAct 돌연변이 데이터베이스

<div class="content-ad"></div>

IntAct 돌연변이 데이터베이스(라이선스 CC0로 이용 가능)에서 제 훈련 데이터를 모았어요. 이 데이터베이스는 수천 개의 missense 변형이 이진 단백질 상호 작용에 관여한 것을 주석으로 담고 있는 거대한 레코드예요.

각 변형은 PPI에 미치는 영향에 대한 주석이 달려 있어요. `Feature type` 열 아래에 인코딩되어 있는데요. increase, decrease, no effect 등이 있어요.

하지만 이 데이터셋에는 구조적인 특징 정보나 다른 변형 주석이 포함되어 있지 않다는 점을 주목하실 거예요. 그런 정보는 제가(즉, AF-M) 개입하는 부분이에요.

<div class="content-ad"></div>

The database has ~12,000 eligible data points. To save compute and ensure balanced data classes, I randomly subsampled variants until I had ~1,000 for each class.

![Image 1](/assets/img/2024-07-14-HowIPredictedtheEffectofMutationsonProteinInteractionsUsingAlphaFold_4.png)

I then wrote a script that generated FASTA files for the wildtype and missense complexes before feeding them into AF-M to produce PDB structures. Due to time constraints, the final dataset had about 250 structures per class.

![Image 2](/assets/img/2024-07-14-HowIPredictedtheEffectofMutationsonProteinInteractionsUsingAlphaFold_5.png)

<div class="content-ad"></div>

위 그림에서 보시는 대로 각 단백질 복합체에 대해 두 가지 PDB 버전이 남아 있습니다: 와일드타입과 미생물 변이 버전입니다. 이를 천 개 이상의 이진 상호작용과 47,000개 이상의 SLURM 작업에 곱하면 작업할 데이터가 약 20TB가 됩니다.

## 특징 추출 및 엔지니어링

남은 일은 구조와 변이 특성 정보를 추출하는 것 뿐이었습니다:

![image](/assets/img/2024-07-14-HowIPredictedtheEffectofMutationsonProteinInteractionsUsingAlphaFold_6.png)

<div class="content-ad"></div>

The AF-M pipeline I used, called AlphaPulldown, is designed to mimic a pulldown assay by running the folding process of multiple protein complexes simultaneously. It also incorporates a feature extraction pipeline that calculates various important structural parameters:

- Interface area (Å²)
- Number of interface residues
- Shape complementarity
- Percentage of polar/hydrophobic/charged residues at the interface
- Docking score
- Protein-Interface score

And many more. Additionally, I introduced some custom features, along with insights from the Variant Effect Predictor by Ensembl, which is accessible under the Apache 2.0 license:

## Pathogenicity predictions

<div class="content-ad"></div>

I've been exploring pathogenicity predictions and came across two interesting examples:

1. **AlphaMissense Annotations:** These predictions cover every possible missense mutation in the human proteome. Exciting stuff!
   
2. **REVEL Pathogenicity Predictions:** REVEL compiles scores from various pathogenicity prediction tools to give an overall average. How cool is that?

I found these predictions particularly useful because:
- They could be key indicators of protein-protein interaction (PPI) disruptions caused by pathogenic missense variants.
- I converted IntAct mutations into a VEP-readable format to access and analyze these predictions effectively. 

How awesome is it to uncover such insights through these predictions? Keep exploring, friends! 🌟

<div class="content-ad"></div>

## gnomAD frequencies

**gnomAD** (the Genome Aggregation Database from the Broad Institute)에는 여러 다양한 그룹 및 변형체에 대한 인구 알렐 빈도가 포함되어 있습니다.

**빈도를 포함한 이유(및 방법):**

- 변형 빈도 데이터는 PPI 영향을 끼치는 병변에 공통 또는 드물게 발생하는 변형이 더 흔한지 배울 수 있습니다.
- IntAct 돌연변이를 VEP에서 읽을 수 있는 형식으로 변환하여 이러한 값들을 찾아냈습니다.

<div class="content-ad"></div>

## 관련 비율

저는 인터페이스 영역 / 총 표면적과 같은 간단한 비율 특성을 만들었습니다. 그리고 기능 x의 와일드타입 버전과의 차이 등을 고려했습니다.

상대 비율을 포함한 이유:

- 비지도 학습 알고리즘은 도움 없이도 이러한 종류의 계산을 수행할 수 있지만 XGBoost와 같은 지도 모델은 이러한 비율에서 이득을 볼 수 있습니다.
- 직관적으로 생각해보면, 인터페이스 영역 대 총 표면적의 비율이 줄어든다면, 약해진 상호 작용을 관찰하고 있다고 예측할 수 있습니다. (이것은 단지 한 예입니다).

<div class="content-ad"></div>

## 무료 에너지

EvoEF2 (MIT 라이센스 하에 사용 가능)를 사용하여 단백질 복합체에 대한 열역학 데이터를 얻었고, 야생형과 돌연변이 변형체의 개폴딩 및 개펼침 상태의 Gibbs 에너지 차이를 비교했습니다.

무료 에너지를 사용해야 하는 이유:

- PPI를 방해하는 돌연변이를 가진 복합체에서 높은 무료 에너지 예측을 기대하며, 보다 불안정한 상호작용을 시사할 것입니다.

<div class="content-ad"></div>

모든 과정을 마친 후에는 다음과 같이 조금씩 보이는 테이블 데이터셋이 있었습니다:

생물정보학 도구 중 하나는, 그러나 항상 작동하지는 않는다는 것입니다. 주어진 변형에 대한 지표는 종종 비어 있거나 누락될 수 있습니다. 예를 들어, IntAct에 있는 모든 변이가 연관된 gnomAD 주파수를 가지고 있는 것은 아닙니다.

기계 학습을 할 때 누락된 값들은 꽤 거슬리므로, 적합한 모델을 선택해야 했습니다. 다행히, XGBoost가 필요한 것을 갖추고 있었습니다—어떻게 했는지 설명해 드리겠습니다.

# 기계 학습 모델

<div class="content-ad"></div>

이 섹션에서는 다음을 다룰 거에요:

- XGBoost 및 선택 이유
- 하이퍼파라미터 튜닝 및 교차 검증

## XGBoost

XGBoost은 경사 부스팅된 의사결정 트리 모델이에요.

<div class="content-ad"></div>

결정트리는 임계값 기반 노드에서 데이터를 분할하는 그래프입니다. 오늘 밖에서 놀아야 할지 말아야 할지를 결정하는 간단한 모델이 있습니다:

![이미지](/assets/img/2024-07-14-HowIPredictedtheEffectofMutationsonProteinInteractionsUsingAlphaFold_7.png)

이 트리를 순차적으로 연결하여 여러 개를 연결하면 각 트리가 지난 트리의 오류를 보정하며 더 견고한 모델을 구축할 수 있습니다. 이것이 `gradient-boosted`의 의미입니다.

XGBoost는 이 특별한 작업에 대해 몇 가지 이점을 가진, 가벼운 빠른 훈련 모델입니다.

<div class="content-ad"></div>

- 정규화된 데이터가 필요하지 않아. 이것은 몇 가지 이유로 훌륭해. 일단, 나에게 더 적은 작업을 의미해. 그러나 더 중요한 것은, 모델이 결론에 도달하는 방법을 해석하는 것이 훨씬 더 간단해진다는 것이야.
- 다양한 데이터 유형을 처리할 수 있어. 이에는 범주형과 명목형 데이터도 포함돼 — 이것은 좀 더 실험적인 기능이긴 하지만, 나는 단순히 범주형 데이터에 대해 더미 변수를 사용했어.
- 감독 모델이야. 인공 지능은 과학을 위해 놀라운 일들을 하고 있지만, 코드에 빠져들기 쉬워. 궁극적인 목표는 생물학에 대해 무언가 배우는 것이니까. 감독 모델을 사용하면 분류의 뒤를 파서 PPI 결과에 대한 생물학적 메커니즘을 더 자세히 조사할 수 있어.
- 결측 데이터를 강력하게 처리할 수 있어. 모든 변량이 전체 주석을 갖고 있는 것이 아니라고 언급한 적이 있지. 예를 들어 빈도 정보가 없는 경우도 있을 수 있어. 또는 EvoEF2에 문제가 생겨 열역학 정보가 필요할 수도 있어. XGBoost는 이렇게 훈련을 완전히 중지시키지 않도록 메서드를 갖고 있어.

데이터셋의 크기가 적당하고 XGBoost가 훈련이 빠르게 진행되기 때문에, k-fold 교차 검증을 사용한 하이퍼파라미터 튜닝 단계를 포함하기로 결정했어:

## 하이퍼파라미터 튜닝 & k-fold 교차 검증

어떤 머신러닝 모델에도 내부 매개변수가 있을 거야 — 내부 분류에 대한 가중치와 편향들이야.

<div class="content-ad"></div>

하지만 ML 모델에는 하이퍼파라미터도 있어요. 이것들은 전반적인 모델의 높은 수준의 구조적 설정이에요. 이러한 매개변수들은 학습 전에 설정되고 정확도 결과에 큰 영향을 줄 수 있어요.

여기에 제가 주로 초점을 맞춘 것들이 있어요:

- Max_depth: 트리의 최대 깊이
- Learning_rate: 각 트리의 최종 결과에 기여
- N_estimators: 트리의 수
- Subsample: 각 트리에 대해 샘플링된 학습 세트의 비율

우리는 최적의 하이퍼파라미터를 테스트하여 최상의 결과를 내는 것을 선택함으로써, 다시 말해 튜닝을 통해 결정해요.

<div class="content-ad"></div>

To add even more robustness to the model training, I implemented a k-fold cross-validation step. Wondering what that is? 

Let's dive in!

- In machine learning, we usually divide the dataset into a training and test set. However, relying solely on this split may lead to overfitting the model to the training data, reducing its effectiveness.
- To address this, we divide the dataset into k segments. Then, we create k different models by choosing one segment as the test set and using the remaining segments as the training data.
- This process is repeated k times, with the data shuffled each time to avoid overfitting to a specific training/testing split.

![Image](/assets/img/2024-07-14-HowIPredictedtheEffectofMutationsonProteinInteractionsUsingAlphaFold_8.png)

<div class="content-ad"></div>

매 조합별로 작업을 반복하며, 가능한 최상의 정확도 모델을 확보합니다.

이제 모델을 학습하고 테스트했으니, 얼마나 좋은지 확인해야 합니다. 생물학적으로 무엇을 배울 수 있는지 알아보겠습니다:

# 결과, 모델 정확도 및 특성 중요도

이 섹션에서는 다음을 다룰 거에요:

<div class="content-ad"></div>

오라클 전문가입니다. 

모델의 정확도는 얼마나 정확한지를 나타내는 지표이며, 그 정확도 메트릭은 무엇을 의미하는지 살펴보겠습니다. 

그리고 각 클래스별로, 어떤 특성이 가장 강력한 예측 변수인지 알아보겠습니다.

## 혼동 행렬 & ROC 곡선

멀티 클래스 분류기의 품질을 평가하기 위해 일반적으로 혼동 행렬을 사용할 것입니다: 

![image](/assets/img/2024-07-14-HowIPredictedtheEffectofMutationsonProteinInteractionsUsingAlphaFold_9.png)

<div class="content-ad"></div>

The confusion matrix visualizes the test set. At the bottom are the model's predictions for each protein complex, and on the y-axis, the actual values are shown.

The lighter the shade on the diagonal, the more accurate the model.

- For example, the model accurately predicted 47 out of 54 class 3 protein complexes (check the bottom row).
- Class 2 also shows good accuracy, with 39 out of 46 complexes correctly classified.
- In the top left corner, the model struggled to differentiate between classes 0 and 1 (mutations reducing and disrupting interactions, respectively). This is understandable as these classes have similar effects.

The confusion matrix provides insights into the model's accuracy. Another useful tool is the ROC curve:

<div class="content-ad"></div>

![How I Predicted the Effect of Mutations on Protein Interactions Using AlphaFold](/assets/img/2024-07-14-HowIPredictedtheEffectofMutationsonProteinInteractionsUsingAlphaFold_10.png)

ROC 곡선은 FPR 대 TPR을 그래프로 나타냅니다:

- True positive rate (TPR): 실제 양성 중 올바르게 예측한 비율 [TP/(TP+FN)].
- False positive rate (FPR): 실제 음성 중 잘못 예측한 비율 [FP/(FP+ TN)].

곡선 상의 점들은 서로 다른 임계값 설정을 나타냅니다. 임계값은 우리가 양성과 음성 사례를 구별하는 기준점입니다.

<div class="content-ad"></div>

멀티 클래스 ROC 곡선에서 양성 케이스는 주어진 클래스이며, 음성 케이스는 다른 모든 클래스들입니다 (one-vs-all). 대각선 (파선)은 무작위 확률을 나타내며, 모든 임계값에서 TPR = FPR입니다. 정확도를 생각하는 한 가지 방법은 이 직선으로부터 멀리 떨어질수록 모델이 더 좋다는 것입니다.

- 그래프의 왼쪽 아래에 위치한 부분에서 높은 임계값은 거짓 양성을 적게 유발할 수 있지만 많은 실제 양성을 놓칠 수도 있습니다.
- 오른쪽으로 이동함에 따라 임계값이 낮아져 더 많은 실제 양성을 (높은 TPR) 찾아내지만 더 많은 거짓 양성 (높은 FPR)도 받아들일 수 있습니다.

이상적인 분류기는 굽은 형태를 가지며, 낮은 임계값에서도 높은 TPR을 가지고 있으면서도 낮은 FPR 비율을 유지합니다. 대부분의 곡선에서 이를 확인할 수 있습니다.

- TPR = ∑cTPc / ∑c(TPc+FNc)
- FPR = ∑cFPc / ∑c(FPc+TNc)

<div class="content-ad"></div>

각 클래스의 TP, FN 및 TN을 합산한 후, 각 클래스의 곡선을 함께 살펴봤습니다.

우리는 모델의 전반적인 성능을 파악할 수 있습니다. 비교를 표준화하기 위해 曰 UIGraphics曰 AUC를 계산합니다. 1에 가까울수록 더 정확합니다. 그림 2에서 확인할 수 있듯이, 우리는 미크로평균 AUC가 0.91임을 달성했습니다.

또한 클래스 0 및 1에 대한 곡선을 통해 혼돈 행렬에서 확인한 내용을 확인할 수 있습니다. 전반적으로, 우리는 꽤 정확한 모델을 가지고 있습니다. 그러나 이제 재미있는 부분이 등장합니다—바이오로지를 통해 무엇을 알 수 있을까요?

## SHAP 값들

<div class="content-ad"></div>

SHAP 값을 사용하면 모델의 각 예측에 기능이 얼마나 기여했는지 확인할 수 있어요.

전체 모델의 상위 열 가지 피처 중요도는 다음과 같아요:

![이미지](/assets/img/2024-07-14-HowIPredictedtheEffectofMutationsonProteinInteractionsUsingAlphaFold_11.png)

제 분석 결과 흥미로운 발견이 있었어요.

<div class="content-ad"></div>

- 첫째로, 미생물 돌연변이로 인해 발생하는 아미노산인 시스테인(C)의 영향은 놀라울 만큼 설명하기 어려웠습니다.
- AlphaMissense 병원성 점수의 중요성은 그리 놀라운 것은 아니었지만, AlphaPulldown 및 AlphaMissense는 (한 방법 또는 다른 방법으로) AlphaFold의 MSA에 의존한다는 점을 고려해보면 중요합니다.
- 특히, 야생형과 미생물 복합체 사이의 인터페이스에 대한 양전하 잔류물의 백분율 차이의 크기도 중요한 역할을 합니다.

사실적으로, 단 몇 가지의 특징을 찾는 것이 닛딱함을 느끼게 되어요. SHAP 값을 사용하여 클래스별로 특징 중요도를 차트로 살펴보는 것이 더 효과적입니다:

## 클래스 0: 상호작용 강도 감소

왼쪽에 있는 특징 중요도 플롯은 각 특징이 모델의 결과에 미치는 평균적인 영향을 보여줍니다. 이 그래프는 비교적 쉽게 해석할 수 있습니다. 막대가 클수록 영향의 규모가 큽니다.

<div class="content-ad"></div>

On the right, we see a beeswarm plot. This kind of plot helps us visualize two important factors:

- Impact on outcome likelihood: Dots on the right side of the center line show that these feature values have a positive effect on the class prediction, making it more likely to be predicted. Dots on the left side have a negative effect, reducing the likelihood.
- Feature value: The color of the dots is also crucial. Blue dots indicate a low feature value, while red dots indicate a high feature value.

By combining these two factors, we can deepen our understanding of feature effects. Let's take the "Resulting_sequence_A" feature as an example. (Note: this is a placeholder value, with only 0 or 1 as possible values).

- Red dots to the right of the center: When "Resulting_sequence_A" has a high value, it increases the likelihood of predicting class 0.
- Blue dots to the left of the center: When "Resulting_sequence_A" has a low value, it decreases the likelihood of predicting class 0.

<div class="content-ad"></div>

알라닌은 미생물 리듀스섬이미스센스 잔존물로 자연 언어로서 상호작용 강도를 감소시키는 강력한 영향을 미치는 것처럼 보입니다. 직관적으로 이는, 알라닌이 측면체로 메틸기만 가진 작은 아미노산이기 때문입니다.

![이미지](/assets/img/2024-07-14-HowIPredictedtheEffectofMutationsonProteinInteractionsUsingAlphaFold_12.png)

이는 그것이 참여할 수 있는 상호작용의 종류를 제한하기 때문입니다. 따라서, 만약 중요한 이산화 황 다리를 대체하거나 전하 상호작용을 방지한다면, 상호작용 강도가 감소할 가능성이 높아질 것으로 합리적으로 생각할 수 있습니다.

우리는 모양 보완성(sc) 특성에서 몇 가지 예상치 못한 결과를 보고 있습니다. sc는 두 단백질이 서로 얼마나 잘 맞는지를 측정하며, sc가 낮을 때 두 단백질 간 상호작용 강도가 감소할 가능성이 높아질 것으로 예상합니다. 그러나, 이는 사실이 아닙니다: 더 높은 sc가 상호작용 강도를 감소시키는 것으로 보이고, 그 반대도 마찬가지입니다.

<div class="content-ad"></div>

아래 섹션에서는 내가 흥미로운 몇 가지 특징을 소개할 것이다:

## 클래스 1: 상호 작용 방해

- SIFT는 단방향 작용 예측 점수로 0과 1 사이의 정규화된 변형이다. 낮은 점수(0-0.5)는 단백질 활동에 영향을 미칠 것으로 예측된다. 우리는 SIFT가 도구로서의 강점을 증명하는 예상된 결과를 beeswarm 플롯에서 확인할 수 있다.
- Total_free_energy_diff_from_wt는 missense 복합체와 해당 wildtype 동등체 간의 EvoEF2 자유 에너지 차이를 측정하는 개선된 특징이다. 상당한 차이(높은 값)는 보다 불안정한 missense 복합체를 나타내며, 그 반대도 마찬가지로 나타나는 것으로 보인다.

## 클래스 2: 상호 작용 강도 증가

<div class="content-ad"></div>

- Resulting_sequence_A: 어떤 이유에서인지 싸이스틴 잔기로 인한 미생물 돌연변이는 상호작용 강도를 높이는 가능성에 놀랍도록 강한 긍정적 영향을 미칩니다. 싸이스틴은 이중체 사이에 이산화황 결합을 형성할 수 있어, 이는 더 강한 결합 중 하나입니다. 이로 인해 상호작용이 강화될 수 있지만, 왜 이것이 가장 중요한 특징인지 궁금합니다.
- iptm & iptm_ptm: 이 값들은 AlphaFold 모델의 신뢰도를 측정합니다. 만약 두 단백질이 복합체에서 `의미 있는` 관계를 가진다면(높은 iptm_ptm으로 표시됨), 강한 상호작용을 보일 것으로 보입니다. 그와 반대되는 경우, 만약 AlphaFold가 구조에 대해 낮은 신뢰를 갖는다면, 이는 이러한 단백질이 복합체에서 부자연스러운 것일 수 있으며, 이는 class 2 예측의 가능성을 줄일 수 있습니다. 그러나, 이것은 단순히 3D 모델이 부정확하며 무시되어야 할 수도 있습니다 — 제가 명확한 결론을 내릴 수는 없습니다.

## Class 3: 상호작용에 미치는 영향이 없음

- AM_PATHOGENICITY: 이것은 AlphaMissense의 복합체에서 각 잔기의 보존 점수를 기반으로 한 병원성 점수입니다. 이것은 비병원성 돌연변이의 상당한 예측 변수인 것으로 보이며, 낮은 병원성 점수는 class 3 예측의 가능성 증가와 관련이 있습니다.
- pDockQ: sc와 같이 두 단백질이 3D 공간에서 얼마나 잘 복합하는지 측정하는 도킹 측정항목입니다. 기대대로, 좋은 도킹 점수는 비장애적 변형의 강력한 예측 변수인 것으로 보입니다.
- gnomAD_exomes_AF: gnomAD로부터의 변체의 엑솀염기의 빈도는 드물게 발생하는 변형이 영향을 받지 않는 단백질 복합체와 연관이 있는 것으로 보입니다. 그러나 이는 드문지 아닌지(이 분류는 특정 임계치 사이의 빈도를 필요로 합니다)를 확인하려면 빈도 범위를 살펴봐야 합니다.

모두가 흥미로운 결과입니다. 이제 새로운 단백질 복합체에 이 모델을 적용하여 새로운 정보를 얻을 수 있는지 확인해 봅시다:

<div class="content-ad"></div>

# 케이스 스터디: ASD 관련 단백질 복합체

이 섹션에서는 다음을 살펴볼 거에요:

- 모델로 분석된 두 가지 구체적인 단백질 복합체 예시를 살펴볼 거에요.
- 이에 대한 문헌 기반의 (가능한) 해석을 제공할 거에요.

이 모든 게 즐겁고 흥미로우니까, 분자 생물학에서 머신 러닝의 목적이 무엇일까요?

<div class="content-ad"></div>

이 프로젝트의 응용 부분에서는, 저는 MSSNG 데이터베이스로부터 몇 가지 코딩 미생체 변이를 분석하기 위해 모델을 사용했습니다. MSSNG은 자폐 스펙트럼 장애(ASD) 연구를 위해 설계된 전체 게놈 서열 및 주석 데이터 저장소로, Scherer 연구실은 ASD 유전체를 연구하는데 특화되어 있습니다.

MSSNG은 방대한 데이터 집합이기 때문에, 저는 다음 기준을 충족하는 하위 집합으로 분석을 정제했습니다:

- 코딩 미생체 변이 (영향을 받는 단백질을 제한함).
- REVEL 및 AlphaMissense에서 모호한 병원성 예측 (모델이 새로운 정보를 생성하는 데 도움이 될 수 있음).
- IntAct 데이터베이스에 따른 알려진 직접 상호작용이 있는 변이 (모델이 훈련된 유일한 상호작용 유형).
- AlphaPulldown을 사용하여 접힌 후에 15 Å의 PAE 커프트를 충족 (단백질 도메인의 상대적 위치에 대한 높은 신뢰도를 제한함).

이 하위 집합에서 두 가지 두드러진 변이가 있습니다:

<div class="content-ad"></div>

- 남성 호르몬 수용체 & 티로신 단백질 키나제 (AR:YES1.p.Q234R)
- 전사 공동활성화제 & 아연 손가락 단백질 (YAP1:GLIS3.p.G448D)

모델은 두 가지 변이 모두에 대해 Class 1 (상호 작용이 중단된)을 예측합니다. 자세히 살펴보겠습니다:

## 남성 호르몬 수용체 & 티로신 단백질 키나제 (AR:YES1.p.Q234R)

![이미지](/assets/img/2024-07-14-HowIPredictedtheEffectofMutationsonProteinInteractionsUsingAlphaFold_13.png)

<div class="content-ad"></div>

요량만 준비해봤어요:

- AR (안드로겐 수용체)는 심장, 골격 근육, 전립선에 있다고 합니다 (UniProt).
- AR의 타이로신 인산화는 종양 성장과 연관이 있습니다 (Guo et al., 2006).
- 어떤 증거들은 ASD와 전립선 암 사이에 역상병성의 가능성을 제시합니다 (Fores-Martos et al., 2019).

따라서 이런 이야기도 가능해요:

- AR과 YES1 사이의 상호 작용이 손상되어 타이로신 인산화가 감소함으로써 암 발병 위험이 줄어들었습니다.
- 역상병성을 가진 ASD와 전립선 암 사이의 관계가 이 돌연변이가 자폐증에서의 역할을 설명할 수 있을 것입니다.

<div class="content-ad"></div>

안타깝게도 여기 몇 가지 문제가 있습니다:

- 전립선에서는 YES1이 저 낮은 수준에서만 발현됩니다.
- 게다가 이는 수용체가 아닌 티로신 키나제입니다.
- 게다가 ASD-전립선암 동반증의 결과와는 반대되는 증거도 있습니다.

종합하면, 모델로부터 결정적인 결과가 나오지 않았다는 것입니다. 흥미로우신 분들을 위해 이 예측을 이끈 SHAP 값들을 여기에 안내해 드리겠습니다. 와일드타입과의 인터페이스 소거 에너지의 상대적으로 높은 차이에 주목해 주세요.

![How I Predicted the Effect of Mutations on Protein Interactions Using AlphaFold](/assets/img/2024-07-14-HowIPredictedtheEffectofMutationsonProteinInteractionsUsingAlphaFold_14.png)

<div class="content-ad"></div>

## 전사 활성화 단백질 및 아연 손가락 단백질 (YAP1:GLIS3.p.G448D)

![Link to the image](/assets/img/2024-07-14-HowIPredictedtheEffectofMutationsonProteinInteractionsUsingAlphaFold_15.png)

한번 더 상기시키는 사항들입니다:

- YAP1은 전사를 활성화/억제하며 간과 전립선에 발견되며(또한 뇌에서는 낮은 수준으로 발견됨).
- GLIS3도 신장, 뇌, 간에 발견되는 전사 활성화자/억제자입니다.
- 여러 GLIS3 결합부위가 신경병리학적인 현상과 관련이 있어 신경계 발달 및 시냅스 전달에 영향을 줄 수 있음(Calderari et al., 2018).
- GLIS3는 자가포식 조절에도 영향을 줄 수 있음(Calderari et al., 2018).

<div class="content-ad"></div>

한 가지 가능한 이야기가 여기 있어요:

- 이 두 단백질 간의 상호작용이 방해되면, 자폐증을 일으키는 신경병리의 진행이 가속화될 수 있어요.

하지만, 이 연구로부터 더 구체적인 메커니즘을 파악하는 것은 도전적이에요.

다시 한 번, 여기 이 SHAP 값들이 있어요:

<div class="content-ad"></div>

이미지 태그를 Markdown 형식으로 바꿔보겠습니다.

![HowIPredictedtheEffectofMutationsonProteinInteractionsUsingAlphaFold_16.png](/assets/img/2024-07-14-HowIPredictedtheEffectofMutationsonProteinInteractionsUsingAlphaFold_16.png)

지금까지 우리는 꽤 강력한 모델을 훈련시키고 그 결과를 해석하며 생물학에 대해 더 배울 수 있었습니다. 이로부터 얻은 것은 무엇일까요? 그리고 다음은 무엇일까요?

# 최종 생각과 교훈

## 향후 방향

<div class="content-ad"></div>

저는 TCAG에서 이 프로젝트에 약 네 달 동안을 보냈는데, 대부분의 시간을 훈련 데이터 수집에 할애했어요. (이전에 AF-M에 적응하기 위해 거기서 보낸 네 달은 언급조차 하지 않을게요.)

더 많은 시간이 주어진다면, 이 프로젝트를 확장하기 위해 다음 단계를 진행하겠습니다:

- 더 많은 데이터 수집. 한정된 데이터셋으로 인해, 이 모델의 결과에 믿음을 가지기 어렵습니다. 더 많은 PDB(이상적으로 실험 데이터)를 수집하는 것이 모델을 향상시키는 첫 번째 단계입니다.
- 새로운 특성 엔지니어링. 분자 생물학적 관점에서 특성 엔지니어링을 한다면 모델의 정확도를 높일 수 있을 거예요.
- 모델을 기전 예측 대신 병원성 예측에 사용. 현재 모델은 IntAct 돌연변이 데이터베이스에서 PPI(단백질 상호작용)에 미치는 영향을 예측합니다. 대안적 접근법은 REVEL처럼 병원성 예측을 위해 풍부한 데이터를 사용하는 것이겠죠.
- 더 많은 설명되지 않은 변이에서 모델을 테스트. 현재까지는 의심스러운 의미없는 변이들에 대해 모델을 테스트할 수밖에 없었어요. 더 많은 PPI를 분석한다면 더 많은 연구 가능성을 만들 수 있을 겁니다.


하지만 이 시간 동안 얻은 성취와 배운 것들에 대해 자랑스러워요.

<div class="content-ad"></div>

## 결론 및 주요 포인트

이렇게 흥미로운 연구 주제에 대해 자유롭게 발표할 기회를 받는 학생들이 많지 않다고 상상할 수 없어요. 세계적인 멘토와 캐나다 최대 규모의 컴퓨팅 클러스터에 접근할 수 있는 기회도 말이에요.

하지만 저는 그런 기회를 얻었어요.

그래서 나름대로 감사한 마음을 표현하고 싶어요. (브렛 트로스트 박사, 리처드 윈틀 박사, 스티븐 셰러 박사 등) 관련된 모든 분들에게 감사드립니다.

<div class="content-ad"></div>

Scherer Lab에서 일한 경험을 통해 연구가 따분하고 느리고 지루할 필요는 없다는 것을 알게 되었어요. 올바른 팀과 비전이 있다면, 빠르고 역동적이며 최신 기술을 적용할 수도 있답니다.

큰 관점에서, AI/ML이 생물 정보학의 미래에 얼마나 중요한지 배웠어요.

- 이 프로젝트에 대한 제 초기 접근 방식은 야생형과 미생물 복합체 간의 구조적 차이를 관찰하고 추적하기 위해 그래프 구조를 만드는 것이었어요.
- 두 가지 간의 차이를 결정하는 데 필요한 통계의 중요성을 깨닫고 ML이 이곳에서 도움이 될 수 있다는 것이 떠올랐어요.

여러분은 어떻게 생각하세요? 피드백, 질문, 칭찬, 혹은 비난 모두 환영이에요.