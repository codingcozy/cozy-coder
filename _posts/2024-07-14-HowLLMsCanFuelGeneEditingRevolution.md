---
title: "유전자 편집 혁명을 이끄는 LLMs의 역할"
description: ""
coverImage: "/assets/img/2024-07-14-HowLLMsCanFuelGeneEditingRevolution_0.png"
date: 2024-07-14 01:28
ogImage: 
  url: /assets/img/2024-07-14-HowLLMsCanFuelGeneEditingRevolution_0.png
tag: Tech
originalTitle: "How LLMs Can Fuel Gene Editing Revolution"
link: "https://medium.com/towards-data-science/how-llms-can-fuel-gene-editing-revolution-1b15663f697c"
---


## |인공지능| LLM| 유전자 편집| 의학 AI|

![이미지](/assets/img/2024-07-14-HowLLMsCanFuelGeneEditingRevolution_0.png)

창조적 인공지능은 시, 코드, 블로그 글 등을 만들어낼 수 있습니다. 이 모든 것은 텍스트로 훈련되어 만들어진 결과물입니다. 우리는 종종 텍스트가 문자의 연속체라는 것을 잊곤 합니다. 그리고 이 문자들은 복잡한 방식으로 조합되어 무한하고 복잡한 의미를 이룰 수 있습니다. 비슷하게, 인생은 몇 가지 기본 문자(DNA의 경우 단 4가지, 단백질의 경우 20가지)로 구성되어 있으며, 이들의 무한한 조합이 우리에게 오늘날의 놀라운 생물다양성을 제공하게 되었습니다.

지난 2년 동안의 혁명의 기초입니다. AlphaFold2를 시작으로 한 혁명은, 단백질 서열로 훈련된 언어 모델을 사용해 100년간 풀리지 않았던 문제를 해결한 것으로 시작되었습니다. 오늘날, AlphaFold2 덕분에 우리는 문자열만 가지고도 단백질의 구조를 재구성할 수 있게 되었습니다.

<div class="content-ad"></div>

비밀은 모델이 자체 학습(self-supervised learning)을 통해 데이터의 표현을 학습하고 이를 통해 작업을 수행할 수 있다는 것입니다. 단백질의 경우, 모델은 단백질과 그들의 서열 내에 존재하는 패턴을 학습하게 됩니다(이러한 서열은 텍스트 서열과 같이 무작위가 아닌 기능적 의미와 독특한 의미를 가지고 있습니다). 이 표현을 통해 우리는 단백질의 구조, 기능 또는 다른 매개 변수를 예측할 수 있습니다.

![이미지](/assets/img/2024-07-14-HowLLMsCanFuelGeneEditingRevolution_1.png)

생성적 AI의 강점 중 하나는 언어 생성과 같은 작업에 LLM을 활용할 수 있는 능력입니다. 또한, 특정 요구 사항에 따라 의존적인 텍스트를 생성할 수 있습니다. 예를 들어, 이미지를 회전시키기 위한 파이썬의 최소 기능을 생성하도록 모델에 요청할 때, 모델은 다음을 충족시켜야 합니다:

- 기능성: 생성된 텍스트(코드)는 우리가 요청한 기능을 정확히 수행해야 합니다.
- 효율성: 기능은 복잡하지 않아야 하며 가능한 최소한의 단계만 포함해야 합니다.
- 구문적 정확성: 모델은 언어(파이썬)의 규칙을 준수해야 합니다.

<div class="content-ad"></div>

이 모든 것이 가능한 이유는 모델이 그 안에서 점차적으로 정교한 표현을 학습하기 때문입니다. 사실, 첫 번째 층은 텍스트의 서로 다른 부분 간의 간단한 관계(구문 구조, 품사 등)을 학습하며, 더 깊은 층은 복잡한 패턴(풍자, 수사 표현 등)을 학습합니다. 그런 다음, 모델은 추론을 할 때 이러한 패턴을 활용할 수 있습니다.

![이미지](/assets/img/2024-07-14-HowLLMsCanFuelGeneEditingRevolution_2.png)

단백질 생성에 대해서도 비슷한 과정을 상상할 수 있습니다. 모델이 단백질 서열 중 특정 기능적 역할을 하는 부분이나 특정 행동에 책임이 있는 부분을 이해한다면, 추론 시 이를 활용할 수 있습니다. 예를 들어, 우리는 모델에게 수백 개의 아미노산으로 이루어진 일려의 단백질을 생성할 수 있는 기능이 있는 아로마틱 환을 자를 수 있는 단백질을 생성하도록 요청할 수 있습니다. 이는 기술적으로 생산되어 오염된 수질을 정화하는 데 사용될 수 있는 효소일 수 있습니다.

![이미지](/assets/img/2024-07-14-HowLLMsCanFuelGeneEditingRevolution_3.png)

<div class="content-ad"></div>

요즘은 초인공지능을 활용하여 연구자들이 자연에 존재하지 않는 서열을 갖는 기능성 단백질을 만들어내고 있는데, 이는 과학소설처럼 들릴 수 있습니다.

이 말인 즉, 이 모델이 이 기능 정보를 포착하고 원하는 생리적 특성으로 조건을 매겨 단백질을 생성할 수 있다는 것을 의미합니다.

![HowLLMsCanFuelGeneEditingRevolution](/assets/img/2024-07-14-HowLLMsCanFuelGeneEditingRevolution_4.png)

DNA와 단백질은 무변동적이지 않고 우연한 돌연변이와 자연선택의 영향을 받은 결과입니다. 모든 생명체는 변이를 겪으며, 이 중 일부는 유익하고 일부는 해로운 것들입니다. 이 돌연변이는 후손에게 전달될 수 있으며, 여기에는 종이 진화하는 방식이 담겨 있습니다. 그러나 이 과정은 무작위적이며 통제될 수 없습니다. 게다가, 이러한 변이 중 많은 것들이 발병의 원인입니다(유전적 질환, 암 등).

<div class="content-ad"></div>

지금까지 본 것은 언어 모델을 이용하여 단백질의 구조를 예측하거나 인공 응용프로그램을 위해 단백질을 생성하는 등의 계산 작업에 활용 가능성을 보여주는 것입니다. 응용 가능성은 거의 무한하지만, 이는 우리가 질병을 치료할 수 있도록 하는 것은 아닙니다.

유전자 편집(즉, DNA가 수정되는 과정)은 사실 수십 년 동안 연구된 과정입니다. 이제야 클리닉에서 환자에게 영향을 미치기 시작하는 것은 얼마나 복잡한지를 보여줍니다. 사실, 환자의 DNA를 편집하는 것은 기술적으로 복잡하며 (수율이 낮음) 비특이성일 위험이 있습니다(우리가 원하지 않는 곳에 돌연변이를 가져다줌으로써 질병을 유발할 수 있음).

그러나 최근에는 일부 진전이 이루어졌습니다. 현재, 환자의 세포가 외부에서 추출되어 (보통 헤마토포이에틱 세포), 연구실에서 수정된 후 환자에게 다시 주입됩니다. 이는 탈세미아나 빈혈과 같은 혈액 질환에 대한 희망을 가져다 주었습니다.

![이미지](/assets/img/2024-07-14-HowLLMsCanFuelGeneEditingRevolution_5.png)

<div class="content-ad"></div>

이러한 성과는 인간 세포의 DNA를 수정하는 가능성을 혁신하게 한 새로운 방법론을 통해 이루어졌습니다. CRISPR-Cas9는 연구자들의 작업을 간단하고 튼튼하며 간결하게 편집할 수 있도록 도왔습니다.

그러나 지금까지 우리가 성공한 것은 혈액 세포를 추출하여 수정하고 다시 주입한 것 뿐입니다. 하지만 이는 모든 다른 기관을 수정하거나 고형종양에 달할 수 없다는 것을 의미합니다(종양 내의 유전자를 특히 치료하도록 수정하기 위함). 왜냐하면 여러 CRISPR-Cas 단백질이 있기는 하지만, 일반적으로 체온에서 최적이 아니며, 원하는 생화학적 특성을 갖지 않거나 충분히 선택적이지 않은 경우가 많습니다.

일부 연구자들은 CRISPR-cas 단백질을 수동으로나 프로그램의 도움을 받아 그리려고 노력했습니다. 그러나 단백질 서열 랜드스케이프의 울퉁불퉁하고 비둘기날 모양 특성 때문에 결과는 만족스럽지 않았습니다. 서열 조합은 거의 무한하지만 기능적이며 원하는 특성을 갖는 것은 몇 가지뿐입니다.

우리가 이전에 말했듯이, 우리는 언어 모델을 단백질 서열로 훈련시키고, 그것을 활용하여 원하는 특성을 갖는 단백질을 생성할 수 있습니다. 사실, self-attention을 통해 변환기는 주어진 기능에 중요한 서열 구성요소를 배우게 됩니다. 이 모델을 사용하여 인문지식을 학습함으로써 모델은 특정 기능을 위해 서열을 설계하는 데 필요한 것을 기억하게 됩니다.

<div class="content-ad"></div>

여러 종류와 기능을 가진 단백질 서열을 생성할 수 있는 일반적인 LLM이 있습니다. 그러나 이 경우에는 특정한 응용 및 단백질을 위한 모델이 필요합니다. 이를 위해서는 텍스트 LLM에 대한 모델을 세부 조정할 수 있습니다. 이 연구에서는 단백질을 위한 일반 모델을 가져와서 전용 CRISPR-Cas 데이터셋을 통해 세밀 조정을 진행하여 CRISPR-Cas를 위해 적응시켰습니다.

![이미지](/assets/img/2024-07-14-HowLLMsCanFuelGeneEditingRevolution_6.png)

저자들은 이후에 자연에서 발견되는 것과는 다른 단백질 서열을 생성했습니다. 이 서열들은 알려진 서열과 유사한 구조를 가지고 있지만 기능적이고 구조적인 차이를 가지고 있습니다. 다시 말해, 이 모델은 자연에서 존재하는 단백질에서 영감을 받아 새로운 것들을 만들어냈다고 볼 수 있습니다. 이 방법의 장점은 이러한 단백질들이 실험실에서 합성되어 실용적인 용도로 테스트될 수 있다는 것입니다. 시험 후에 결과적으로 나타난 단백질들은 다른 기능적 특성을 보여줍니다.

요컨대, 생명공학과 의학은 혁명의 선순환에 서 있습니다. 인공 지능은 의료에 현실적인 영향을 미칠 것이지만, 많은 응용 프로그램은 종종 토의됩니다. LLM의 DNA와 단백질 서열에 미치는 영향은 그다지 논의되지 않는 것이 사실입니다. AlphaFold2와 기타 유사한 LLM은 연구자들이 단백질의 구조를 이해하고 이에 따라 약물을 디자인하는 데 도움을 줍니다. 이 모델들은 훈련 중에 단백질 구조와 기능에 대한 일반적인 규칙을 배우기 때문에 새로운 단백질을 생성하기 위해 생성적으로 사용될 수도 있습니다.

<div class="content-ad"></div>

한편, 이러한 새로운 단백질은 환경 정화와 같은 새로운 응용 프로그램에 사용될 수 있지만, 질병 치료에는 어떻게 활용될 수 있는지는 아직 명확하지 않습니다. 다른 한편으로, AI와 CRISPR-Cas의 결합을 통해 유전자 편집이 거의 모든 질병을 치료하는 데 사용될 수 있는 미래를 상상할 수 있게 되었습니다.

아마도 미래에 의사는 진단 시 유전체를 시퀀싱할 것입니다. 그들은 질병의 기저에 있는 변이를 알아차리고, 유전자 편집을 통해 환자를 치료할 것입니다. 현재 유전자 편집의 첫 임상 시험이 시작되고 있습니다. LLMs는 새로운 변이와 새로운 유전자 편집 방법을 식별할 수 있도록 할 것입니다.

또한, 이 LLMs는 매우 유연하기 때문에, 우리는 기존의 텍스트 기반 LLMs에 사용하는 기술을 적용할 수 있다고 상상할 수 있습니다. 따라서 미래에는 이러한 LLMs가 자연에서 존재하지 않는 원하는 기능을 가진 단백질을 생성하는 기법(프롬프트 디자인, 세밀한 조정 등)을 갖게 될 것입니다.

## 어떻게 생각하세요? LLMs가 건강 산업을 혁신할 것이라고 생각하시나요? 댓글로 알려주세요.

<div class="content-ad"></div>

# 만약 이 글이 흥미로웠다면:

제 다른 글들도 참고해보세요. LinkedIn에서 연결하거나 연락할 수도 있습니다. 매주 업데이트되는 머신러닝 및 인공지능 뉴스가 담긴 이 저장소를 확인해보세요. 협업과 프로젝트에 열려 있으며 LinkedIn에서 저에게 연락할 수 있습니다. 또한 새로운 이야기를 게시할 때 알림을 받으려면 무료로 구독할 수 있습니다.

제 GitHub 저장소 링크는 여기입니다. 거기에는 머신러닝, 인공지능 및 기타 관련 코드와 많은 자료들을 모아두고 있습니다.

또는 최근 에서 쓴 글에 관심이 있을 수도 있습니다:

<div class="content-ad"></div>

# 참고 자료

저는 이 글을 쓰기 위해 참고한 주요 참고 자료 목록을 아래에 나열했어요. 각 논문의 제목은 축약해서 제시하였습니다.

- Lin, 2022, Language models of protein sequences at the scale of evolution enable accurate structure prediction, [링크](링크)
- Voita, 2019, The Bottom-up Evolution of Representations in the Transformer: A Study with Machine Translation and Language Modeling Objectives, [링크](링크)
- Quintana, 2022, Gene Editing for Inherited Red Blood Cell Diseases, [링크](링크)
- Van der Oost, 2023, The genome editing revolution, [링크](링크)
- Ruffolo, 2024, Design of highly functional genome editors by modeling the universe of CRISPR-Cas sequences, [링크](링크)
- Ruffolo, 2024, Designing proteins with language models, [링크](링크)
- Ferruz, 2022, Towards Controllable Protein Design with Conditional Transformers, [링크](링크)
- Verkuil, 2022, Language models generalize beyond natural proteins, [링크](링크)
- Jumper, 2021, Highly accurate protein structure prediction with AlphaFold, [링크](링크)
- Ghorbani, 2021, A short overview of CRISPR-Cas technology and its application in viral disease control, [링크](링크)
- Bhokisham, 2021, CRISPR-Cas System: The Current and Emerging Translational Landscape, [링크](링크)