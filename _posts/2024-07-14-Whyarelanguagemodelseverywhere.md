---
title: "왜 언어 모델이 어디에나 있을까"
description: ""
coverImage: "/assets/img/2024-07-14-Whyarelanguagemodelseverywhere_0.png"
date: 2024-07-14 02:19
ogImage: 
  url: /assets/img/2024-07-14-Whyarelanguagemodelseverywhere_0.png
tag: Tech
originalTitle: "Why are language models everywhere?"
link: "https://medium.com/towards-data-science/why-are-language-models-everywhere-36d9961dd9e1"
---


이미지 태그를 Markdown 형식으로 변경해보세요.


![](/assets/img/2024-07-14-Whyarelanguagemodelseverywhere_0.png)


ChatGPT와 Large Language Models로 여기까지 온 이유에 대해 궁금한 적이 있으신가요? 그 답은 자연어 처리(NLP)의 발전 과정에 있습니다. 그러니 함께 알아봅시다. 걱정마세요; 역사가 생각보다 흥미롭습니다! 첫 번째 절에서는 AI와 NLP의 탄생에 대해 설명할 것입니다. 두 번째 절에서는 이 분야의 주요 요충지에 대해 이야기할 것입니다. 그리고 3부터 5까지는 지난 75년간의 상세한 타임라인을 살펴볼 것입니다. 마지막 6부에서는 오늘날 크게 인기를 끌고 있는 언어 모델링으로 이들 분야가 모두 융합된 과정을 설명할 것입니다!

# 1 Genesis

처음에는 Alan Turing의 1950년 논문 'Computing Machinery and Intelligence'가 있었습니다. 그는 "기계가 생각할 수 있을까?"라는 질문을 제기했습니다. 이 논문은 인공지능의 탄생으로 손꼽히며, 자연어에 대해 직접 다루지는 않았지만, NLP에 대한 향후 연구를 기초를 놓았습니다. 이것이 NLP의 초기 작업들이 1950년대에 시작된 이유입니다.

<div class="content-ad"></div>

# 2 NLP의 기둥

- 기계 번역: 이것은 인공지능이 한 언어로 된 문장을 받아 다른 언어로 된 문장을 출력하는 경우입니다. 예를 들어, Google 번역.
- 음성 처리: 인공지능이 입력된 오디오를 받아 해당 텍스트를 출력합니다.
- 텍스트 요약: 인공지능은 이야기를 입력받아 요약을 출력합니다.
- 언어 모델: 인공지능은 단어의 시퀀스를 받아 다음 단어를 결정합니다.

이 외에도 이 네 가지 이상의 분야가 있습니다. 시간이 지남에 따라, 각 기둥이 자신의 작업을 수행하기 위해 언어 모델을 사용하는 방향으로 수렴되었습니다. 다음 섹션에서는 각 타임라인에 대해 이야기해보겠습니다.

[2024-07-14-Whyarelanguagemodelseverywhere_1.png](/assets/img/2024-07-14-Whyarelanguagemodelseverywhere_1.png)

<div class="content-ad"></div>

# 3 기계 번역

규칙 기반 시스템: 1954년 조지타운 IBM 실험은 냉전 시대에 러시아어를 영어로 번역하는 데 사용되었습니다. 번역 작업을 하나의 언어에서 다른 언어로 변환하는 규칙집합으로 분해할 수 있다는 아이디어였다, 즉 규칙 기반 시스템입니다. 조슈아 바르힐렐의 "Analytical Engine"은 히브리어를 영어로 번역하는 조 기반 시스템 중 하나였습니다.

통계적 접근 방법: 규칙 기반 시스템의 문제는 많은 가정을 하게된다는 것입니다. 문제가 더 복잡할수록 이러한 가정은 더 문제가 되기 때문에 번역은 복잡합니다. 1980년대부터 우리는 점점 이중언어 데이터에 접근하고 통계적 방법이 잘 확립되면서 이러한 통계적 모델을 언어 번역에 적용하기 시작했습니다. 통계적 기계 번역(SMT)이라 불리는 패러다임이 인기를 얻었습니다. SMT 패러다임은 문제를 두 개로 세분화했습니다: 번역 문제와 언어 모델링 문제로 나누는 것입니다.

신경망 접근 방법: 2015년 이후, SMT는 신경 기계 번역(Neural Machine Translation)에 대체되었습니다. 이들은 번역 작업을 직접 학습하기 위해 신경망을 사용합니다. 이에는 순환 신경망(Recurrent Neural Networks)의 발전과 어쩌면 Transformer 모델도 포함됩니다. GPT와 같은 모델들이 도입되면서 기본 사전 훈련 모델이 언어 모델링이 되었으며 번역을 위해 세밀하게 조정됩니다.

<div class="content-ad"></div>

# 4 음성 처리

규칙 기반 시스템: 음성 처리의 시작은 1950년대와 60년대에 시작되었습니다. 그 때 단일 숫자와 단어를 인식했습니다. 예를 들어, 벨 연구소의 Audrey는 숫자를 인식했고, IBM의 Shoebox는 음성 명령으로 산술 연산을 수행했습니다.

통계적 접근 방식: 그러나 음성을 텍스트로 변환하는 것은 복잡한 문제입니다. 방언, 액센트, 음량이 모두 다르기 때문이죠. 그래서 이 복잡한 문제를 여러 하위 문제로 분해하는 것이 중요했습니다. 70년대쯤에 Hidden Markov 모델이 등장한 뒤, 음성을 텍스트로 변환하는 복잡한 문제를 3가지 단순한 문제로 분해할 수 있게 되었습니다:

- 언어 모델링: 단어와 문장의 순서를 결정할 수 있습니다. 이것은 n-gram 모델들입니다.
- 발음 모델링: 단어와 발음을 연결하는 작업입니다. 이 모델들은 기본적인 모델이나 표일 수도 있습니다.
- 음향 모델링: 음성과 발음 사이의 관계를 이해합니다. 이것은 가우시안 혼합 모델을 사용한 Hidden Markov 모델입니다.

<div class="content-ad"></div>

These three components are individually trained and then combined. However, this process introduces its own set of complexities.

When it comes to neural approaches, we witnessed a significant shift in the early 2000s as neural networks started to outperform traditional techniques. With the rise of large-scale text corpora, neural networks began excelling in tasks such as end-to-end speech-to-text conversion. This allowed us to directly optimize the generation of text from input speech, resulting in improved performance. Subsequent advancements in the field led us to Recurrent Networks, Convolutional Neural Networks, and the fine-tuning of pretrained language models.

Moving on to text summarization, the initial research in rule-based systems dates back to Luhn's groundbreaking work on automatic literature abstract creation in 1958. By ranking the importance of sentences based on word frequencies, this method involved selecting sentences from the original text to form an "extraction-based summary." A significant advancement occurred in 1969 with Edmonson's paper on new methods of automatic extractive summarization. Edmonson highlighted that the importance of a sentence was not solely determined by word frequencies but also by other factors like its location within the paragraph, the presence of cue words, or its relevance to the title. 

In the 1980s, attempts were made to create summaries that mimicked human-written content without directly using original sentences, termed "abstractive summaries." Early systems like FRUMP (Fast reading and understanding memory program) and SUSY were developed to achieve this, but they relied heavily on manually crafted rules and often produced summaries of subpar quality.

<div class="content-ad"></div>

통계적 접근 방식: 90년대와 2000년대에는 우리는 문장이 요약에 포함되어야 하는지 여부를 결정하는 분류기를 구축하기 위해 통계적 방법을 사용했습니다. 이러한 분류기는 로지스틱 회귀, 의사 결정 트리, SVM 또는 다른 통계 모델일 수 있습니다.

신경망 접근 방식: 2015년부터 신경망은 문장 요약을 위한 신경 주의 모델 도입으로 영향을 받았습니다. 이는 주로 매우 짧은 헤드라인이 포함된 추상적 요약을 생성했습니다. 그러나 LSTM 셀과 시퀀스-투-시퀀스 아키텍처의 도입으로 더 긴 입력 시퀀스를 처리하고 적절한 요약을 생성할 수 있게 되었습니다. 이후, 이 분야는 기계 번역과 동일한 방식을 취하여 오늘날 볼 수 있는 사전 훈련 및 파인 튜닝 아키텍처를 활용했습니다.

# 6 모두를 함께 모으기

이전 섹션에서 논의 된 여러 기둥의 역사는 일부 공통된 패턴을 보여줍니다.

<div class="content-ad"></div>

최초의 인공지능(AI) 개발은 1950년대와 60년대에 룰 기반 시스템이 주를 이뤘어요. 하지만 70년대에는 통계 모델이 도입되며 이 문제를 해결하려고 노력했죠. 그러나 언어는 복잡하기 때문에, 이 통계 모델은 복잡한 작업을 하위 작업으로 나누어 문제를 해결했답니다. 2000년대에는 더 많은 데이터와 더 나은 하드웨어가 도입되면서 신경망 접근법이 떠오르게 되었어요. 

신경망은 복잡한 언어 작업을 한꺼번에 배울 수 있어서 통계적 방법보다 성능이 더 좋아요. 2017년에 소개된 Transformer Neural Networks는 언어 작업을 효과적으로 해결할 수 있었답니다. 하지만 효과적으로 모델을 학습하기 위해서는 대량의 데이터가 필요했기 때문에, BERT와 GPT가 소개되었고 전이학습(transfer learning)의 개념을 활용하여 언어 작업을 배울 수 있었어요. 이곳에서 주요 아이디어는 언어 작업은 언어 자체에 대한 기본적인 이해가 있으면 많은 데이터가 필요하지 않다는 것이에요. 예를 들어 GPT는 언어 모델링을 이해함으로써 "언어의 이해"를 습득하고 이후에 특정 언어 작업에 대해 세밀하게 조정하죠. 그래서 현대 자연어 처리(NLP)는 주로 언어 모델링을 핵심으로 사용하는 것이죠.

지금은 ChatGPT와 같은 대규모 언어 모델이 왜 중요한지 이해하시겠죠? 그리고 왜 우리가 어디서나 언어 모델링이 보이는지 알 수 있을 거예요. 이곳에 이르기까지 한세기가 걸렸어요. NLP와 언어 모델링에 대한 자세한 내용은 이 동영상 플레이리스트를 확인해보세요. 즐겁게 학습하세요!