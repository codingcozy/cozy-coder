---
title: "Pretrained SimpleT5 Transformer을 사용한 뉴스 기사 요약 방법"
description: ""
coverImage: "/assets/img/2024-07-06-NEWSArticleSummarizationwithPretrainedSimpleT5Transformer_0.png"
date: 2024-07-06 10:57
ogImage: 
  url: /assets/img/2024-07-06-NEWSArticleSummarizationwithPretrainedSimpleT5Transformer_0.png
tag: Tech
originalTitle: "NEWS Article Summarization with Pretrained SimpleT5 Transformer"
link: "https://medium.com/dev-genius/news-article-summarization-with-pretrained-simplet5-transformer-84acebfae774"
---


오늘날의 빠른 세상에서는 뉴스 기사가 넘쳐나고, 그 모든 것을 읽기에는 압도적일 수 있습니다. 이 블로그 포스트에서는 최첨단 모델의 능력을 활용하여 전체 기사의 간결하고 유익한 요약을 생성하는 방법을 안내해 드릴 것입니다. 소중한 시간을 희생하지 않으면서도 잘 알고 있는 상태를 유지하는 것이 더 쉬워집니다. T5 모델로 시작해 봅시다!!

![Image](/assets/img/2024-07-06-NEWSArticleSummarizationwithPretrainedSimpleT5Transformer_0.png)

## T5란 무엇인가요??

T5는 텍스트 대 텍스트 작업용으로 설계된 인코더-디코더 모델입니다. 지도 및 자기 지도된 사전 훈련의 조합을 사용하며 손상된 입력을 처리하여 누락된 부분을 생성합니다. T5는 작업별 접두사를 사용하여 다양한 작업에 뛰어납니다. 상대적 스칼라 임베딩 및 입력 패딩 옵션은 효율성과 적응성에 기여합니다.

<div class="content-ad"></div>

SimpleT5은 T5 모델의 간소화된 버전으로, 더 효율적이고 사용하기 쉬운 것을 목표로 합니다. T5의 핵심 기능을 유지하면서 더 작고 빠른 NLP 모델입니다. 다양한 자연어 처리 작업에 적합합니다. 이 블로그에서는 simpleT5를 사용하여 뉴스를 요약해 보겠습니다.

![News Summarization with SimpleT5](/assets/img/2024-07-06-NEWSArticleSummarizationwithPretrainedSimpleT5Transformer_1.png)

## 뉴스 요약을 위한 SimpleT5를 활용한 파이썬 코드

이 코드 블록은 자연어 처리와 미세 조정된 사전 훈련 모델을 사용한 요약을 위한 여러 작업을 수행합니다.

<div class="content-ad"></div>


안녕하세요! Tarot 전문가님 🌟

파이썬 코드를 공유해주셔서 감사합니다. 아래는 코드 설명입니다.

- 필요한 라이브러리 simplet5를 pip을 사용하여 설치합니다.
- CSV 파일 'news_summary.csv'를 읽고 'headlines' 및 'text' 두 열을 선택합니다.
- 데이터프레임이 예상 형식인 'source_text' 및 'target_text' 두 열과 일치하도록 이름을 변경하고 재구성합니다.
- 코드에서 pip을 사용하여 scikit-learn 라이브러리를 설치합니다.

파이썬 코드에 대한 설명으로 마크다운 형식의 이미지 태그는 하단에 제공됩니다.

![2024-07-06-NEWSArticleSummarizationwithPretrainedSimpleT5Transformer_2](/assets/img/2024-07-06-NEWSArticleSummarizationwithPretrainedSimpleT5Transformer_2.png)

![2024-07-06-NEWSArticleSummarizationwithPretrainedSimpleT5Transformer_3](/assets/img/2024-07-06-NEWSArticleSummarizationwithPretrainedSimpleT5Transformer_3.png)



<div class="content-ad"></div>

![image](/assets/img/2024-07-06-NEWSArticleSummarizationwithPretrainedSimpleT5Transformer_4.png)

6. 데이터 프레임을 훈련 및 테스트 세트로 나누는 데 train_test_split() 함수를 사용합니다.

7. simplet5 라이브러리에서 SimpleT5 클래스를 가져와서 사전 훈련된 모델을 로드하는 데 사용합니다.

8. 제공된 훈련 데이터로 모델을 학습하는 데 train() 함수를 사용합니다.

<div class="content-ad"></div>

**9.** 훈련된 모델은 로컬 'outputs' 폴더에 저장됩니다.

**10.** 추론을 위해 훈련된 모델이 로컬 폴더에서 로드됩니다.

**11.** 코드는 로드된 모델의 predict() 함수를 사용하여 주어진 텍스트를 요약합니다.

코드는 데이터 전처리를 수행하고, 데이터를 훈련 및 테스트 세트로 분할하며, 사전 훈련된 모델을 훈련하고, 모델을 저장한 다음 주어진 텍스트를 요약하는 작업을 수행합니다.

<div class="content-ad"></div>

- **뉴스 데이터에서 사용된 데이터셋은 다음 링크에서 확인할 수 있어요:** 

![News Article Summarization with Pretrained Simple T5 Transformer](/assets/img/2024-07-06-NEWSArticleSummarizationwithPretrainedSimpleT5Transformer_5.png)

**즐거운 독서 되세요! 계속 배우세요!**

만약 마음에 드셨다면 추천 부탁드려요! 감사합니다!

<div class="content-ad"></div>

LinkedIn, YouTube, Kaggle 및 GitHub에서 더 많은 관련 콘텐츨르 확인하실 수 있습니다. 감사합니다!!