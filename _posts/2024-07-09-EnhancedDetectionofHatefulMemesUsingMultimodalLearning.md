---
title: "멀티모달 학습을 통한 혐오 밈 탐지 강화 방법"
description: ""
coverImage: "/assets/img/2024-07-09-EnhancedDetectionofHatefulMemesUsingMultimodalLearning_0.png"
date: 2024-07-09 23:50
ogImage:
  url: /assets/img/2024-07-09-EnhancedDetectionofHatefulMemesUsingMultimodalLearning_0.png
tag: Tech
originalTitle: "Enhanced Detection of Hateful Memes Using Multimodal Learning"
link: "https://medium.com/@abhishekbose550/enhanced-detection-of-hateful-memes-using-multimodal-learning-70c3a1dab74a"
isUpdated: true
---

## 소개

요즘의 디지털 환경에서는 혐오적인 미미의 전파를 탐지하고 완화하는 것이 중요합니다. 이러한 미미들은 종종 비하적이고 자극적인 콘텐츠로 특징 지어지며 해로운 편견을 지속시키고 폭력을 부추기며 디지털 독성에 기여할 수 있습니다. 이러한 콘텐츠를 검열하기 위한 노력에도 불구하고, 기존의 방법은 다중 모달 데이터 분석의 본질적인 복잡성으로 인해 혐오적인 미미를 효과적으로 식별하고 대응하는 데 어려움을 겪고 있습니다.

![이미지](/assets/img/2024-07-09-EnhancedDetectionofHatefulMemesUsingMultimodalLearning_0.png)

## 도전

<!-- cozy-coder - 수평 -->

<ins class="adsbygoogle"
     style="display:block"
     data-ad-client="ca-pub-4877378276818686"
     data-ad-slot="1107185301"
     data-ad-format="auto"
     data-full-width-responsive="true"></ins>

<script>
     (adsbygoogle = window.adsbygoogle || []).push({});
</script>

Hateful memes combine nuanced, context-heavy text with imagery, posing a significant technical challenge. Traditional machine learning techniques lack the sophistication needed to accurately analyze this multimodal data. These methods often fail to effectively integrate textual and visual features, resulting in poor performance and a high rate of false positives. Furthermore, the reliance on extensive computational resources for running and deploying these large-scale transformer-based models is a challenge in real-world scenarios.

## Our Approach: ConcatNet

To address these challenges, we introduced a novel concatenated image + text encoder architecture called ConcatNet. This architecture integrates advanced text encoders like RoBERTa and pre-trained multimodal networks like CLIP embeddings to enhance the understanding of images. Additionally, we improve the training process with additional image captioning data.

ConcatNet is designed to be extremely easy to set up and allows experimentation with different combinations of text and image encoder models. In our ablation study, we demonstrated how pre-training our model on a dataset catered to a similar task in the same domain significantly improves classification performance.

<!-- cozy-coder - 수평 -->

<ins class="adsbygoogle"
     style="display:block"
     data-ad-client="ca-pub-4877378276818686"
     data-ad-slot="1107185301"
     data-ad-format="auto"
     data-full-width-responsive="true"></ins>

<script>
     (adsbygoogle = window.adsbygoogle || []).push({});
</script>

## 데이터셋

저희 연구에서는 두 가지 주요 데이터셋을 사용했습니다:

- Hateful Memes Dataset: Facebook AI에서 공개한 이 데이터셋은 10,000여 장의 밈 이미지에 텍스트를 주석으로 다는 것과 "Hateful" 또는 "Not Hateful"으로 분류하는 데이터입니다. 이는 8,500개의 밈으로 이루어진 학습 세트, 500개의 밈으로 이루어진 검증 세트, 그리고 2,000개의 밈으로 이루어진 시험 세트로 구성되어 있습니다.
- Memotion Dataset: 감정 분석, 감정 감지, 그리고 유머 분류 작업을 위해 손집었던 이 데이터셋은 "SemEval-2020 Task 8: Memotion Analysis" 대회에서 소개되었습니다. 이 데이터셋은 밈이 무해한지, 약간 노골적으로 모욕적인지, 혹은 매우 모욕적인지 여부를 식별하는 데 초점을 맞추고 있습니다.

## 실험 설정

<!-- cozy-coder - 수평 -->

<ins class="adsbygoogle"
     style="display:block"
     data-ad-client="ca-pub-4877378276818686"
     data-ad-slot="1107185301"
     data-ad-format="auto"
     data-full-width-responsive="true"></ins>

<script>
     (adsbygoogle = window.adsbygoogle || []).push({});
</script>

우리 프로젝트에서 두 가지 주요 실험 설정을 탐색했어요:

1. ConcatNet: 이미지 모델인 CLIP을 이미지 인코더로, 그리고 텍스트 모델인 BERT 또는 RoBERTa를 텍스트 인코더로 사용하는 간단한 연결된 아키텍처입니다. 사전 훈련된 모델의 가중치를 고정하거나 고정하지 않은 상태에서 여러 실험을 진행하여 문맥 및 공간 정보를 캡처하는 효과를 테스트했어요.

![그림](/assets/img/2024-07-09-EnhancedDetectionofHatefulMemesUsingMultimodalLearning_1.png)

2. 사전 훈련 설정: Memotion 데이터셋에서 사전 훈련된 연결 아키텍처를 사용했어요. 이는 Hateful Memes 데이터셋에서의 하류 분류 작업에 더 나은 초기 가중치 설정을 제공했어요.

<!-- cozy-coder - 수평 -->

<ins class="adsbygoogle"
     style="display:block"
     data-ad-client="ca-pub-4877378276818686"
     data-ad-slot="1107185301"
     data-ad-format="auto"
     data-full-width-responsive="true"></ins>

<script>
     (adsbygoogle = window.adsbygoogle || []).push({});
</script>

![Model Architectures](/assets/img/2024-07-09-EnhancedDetectionofHatefulMemesUsingMultimodalLearning_2.png)

## 모델 구조

우리는 이미지 인코딩을 위해 CLIP를 사용하고 BERT 및 RoBERTa와 같은 트랜스포머 모델을 사용하여 특정한 신경망 구조를 개발했습니다. 변형은 다음과 같습니다:

- Clip2Clip: 이미지와 텍스트 구성 요소 모두를 CLIP를 사용하여 인코딩하며 각 모달리티에 대해 조정됨.
- ClipBertLarge 및 ClipBertUnfrozen: 이미지 인코딩에 CLIP를 사용하고 텍스트 인코딩에 BERT를 사용하는데, "Unfrozen" 모델은 두 인코더를 모두 학습 가능하게 합니다.
- Clip+RoBERTa with Projection: RoBERTa를 텍스트 인코딩에 사용하고 선형 투영 레이어와 함께 사용됩니다.

<!-- cozy-coder - 수평 -->

<ins class="adsbygoogle"
     style="display:block"
     data-ad-client="ca-pub-4877378276818686"
     data-ad-slot="1107185301"
     data-ad-format="auto"
     data-full-width-responsive="true"></ins>

<script>
     (adsbygoogle = window.adsbygoogle || []).push({});
</script>

## 주요 발견

## 전처리 과정

우리는 텍스트에 대해 spacy, contractions, 그리고 demoji와 같은 라이브러리를 사용하여 다음과 같은 클리닝 단계를 수행하는 포괄적인 전처리 파이프라인을 구현했습니다:

- URL 제거: "http"로 시작하는 부분 문자열을 제거하여 텍스트를 정리합니다.
- 텍스트 정규화: 유니코드 정규화를 사용하여 텍스트를 일관된 형식으로 변환합니다.
- 이모지 처리: 이모지를 해당하는 영어 단어로 교체합니다.
- 불용어 제거: NLTK의 목록을 사용하여 일반적인 영어 불용어를 필터링합니다.
- 약어 교체: 축약어와 일반적인 약어를 전체 형태로 확장합니다.
- 구두점 제거: 구두점을 제거하여 알파벳 및 공백만 유지합니다.
- 채팅어 맵핑: 채팅에서 사용되는 줄임말 (예: "lol" 또는 "brb")을 전체 문구로 번역합니다.
- 어근 추출: 단어를 기본 형태나 사전형으로 축소합니다.
- 단어 토크나이저: 텍스트를 단어(토큰) 목록으로 분할합니다.

<!-- cozy-coder - 수평 -->

<ins class="adsbygoogle"
     style="display:block"
     data-ad-client="ca-pub-4877378276818686"
     data-ad-slot="1107185301"
     data-ad-format="auto"
     data-full-width-responsive="true"></ins>

<script>
     (adsbygoogle = window.adsbygoogle || []).push({});
</script>

이미지에 대해서는, 우리는 모델의 견고성을 향상시키기 위해 크기 조정, 무작위 수평 뒤집기, 무작위 흑백 변환 및 텐서 변환과 같은 보강을 도입했습니다.

## 훈련 및 최적화

성능을 최적화하기 위해 하이퍼파라미터를 세심하게 선택했는데, 딥러닝 프레임워크로 PyTorch를 사용하고 사전 훈련된 모델 및 토크나이저로 huggingface transformers를 활용했습니다. 시행착오를 거치며 Adam 옵티마이저와 Cross Entropy 손실 함수가 적용되었고, 배치 크기, 학습률 및 로깅에 대한 자세한 구성이 있었습니다. 이 설정을 통해 모델을 효과적으로 세밀하게 조정하고 성능 메트릭을 정확하게 추적할 수 있었습니다.

## 실험 결과

<!-- cozy-coder - 수평 -->

<ins class="adsbygoogle"
     style="display:block"
     data-ad-client="ca-pub-4877378276818686"
     data-ad-slot="1107185301"
     data-ad-format="auto"
     data-full-width-responsive="true"></ins>

<script>
     (adsbygoogle = window.adsbygoogle || []).push({});
</script>

- 언프로즌 CLIP 및 BERT 인코더: 훈련 정확도가 높았지만 검증 및 테스트 성능이 낮아져 심각한 과적합을 나타냈습니다 (약 60%).

2. 프로즌 듀얼 CLIP 인코더: 테스트 정확도가 약간 향상되었으며, 더 유연한 적응 전략이 필요함을 시사했습니다. (62.23%)
3. 프로즌 CLIP 및 RoBERTa 인코더: 검증 및 테스트 시나리오에서 약간의 성능 향상이 있었으며, 일반화를 위해 다양한 훈련 배경의 혜택을 강조했습니다.
4. 프로즌 CLIP 및 BERT 인코더: 높은 훈련 정확도를 보였지만 검증 및 테스트 성능이 크게 저하되어, 훈련 가능한 유연성이 필요함을 강조했습니다. (92.34%)
5. 언프로즌 CLIP 및 BERT 인코더 (Memotion 사전 훈련): 테스트 정확도가 상당히 향상되어 (89.82%), 모델의 적응 가능성 및 사전 훈련의 혜택을 입증했습니다.

![이미지](/assets/img/2024-07-09-EnhancedDetectionofHatefulMemesUsingMultimodalLearning_3.png)

## 결론

우리의 실험은 사전 훈련된 모델을 활용하고, 악플 미디어 탐지와 같은 세밀한 작업을 위해 적응 가능성을 활성화하는 것이 중요함을 강조했습니다. 관련 데이터셋 (Memotion)에서 사전 훈련을 하고 대상 데이터셋 (악플 미디어)에서 파인 튜닝을 하면 모델 성능이 크게 향상되는 것으로 입증되었습니다.

<!-- cozy-coder - 수평 -->

<ins class="adsbygoogle"
     style="display:block"
     data-ad-client="ca-pub-4877378276818686"
     data-ad-slot="1107185301"
     data-ad-format="auto"
     data-full-width-responsive="true"></ins>

<script>
     (adsbygoogle = window.adsbygoogle || []).push({});
</script>

우리의 연구는 견고하고 효율적인 감지 메커니즘을 개발함으로써 더 나은 콘텐츠 모니터링에 기여하고 더 안전한 온라인 환경을 조성합니다. 이 방법은 혐오적 인 이념의 전파를 줄일 수있는 잠재력이 있으며 건설적인 대화를 촉진하고 온라인 괴롭힘과 차별로부터 향후 억압 된 공동체를 보호 할 수 있습니다.
