---
title: "대형 언어 모델의 능력 분류 해부"
description: ""
coverImage: "/assets/img/2024-07-13-ATaxonomyofLargeLanguageModelsCapabilities_0.png"
date: 2024-07-13 03:27
ogImage: 
  url: /assets/img/2024-07-13-ATaxonomyofLargeLanguageModelsCapabilities_0.png
tag: Tech
originalTitle: "A Taxonomy of Large Language Models Capabilities"
link: "https://medium.com/generative-ai/a-taxonomy-of-large-language-models-capabilities-b019d3d582b2"
---


![image](/assets/img/2024-07-13-ATaxonomyofLargeLanguageModelsCapabilities_0.png)

## 소개

생성적 AI, 특히 기반 모델들은 텍스트, 이미지, 비디오, 오디오, 그리고 코드와 같은 다양한 형태에서 탁월한 능력을 보여주고 있습니다. 이러한 놀라운 진전들은 거대한 모델들의 자기 지도 학습을 통해 방대한 양의 데이터에 기반합니다.

텍스트 분야에서는 대규모 언어 모델(Large Language Models, LLMs)이 일관된 콘텐츠 생성 능력을 증명했으며, 지도된 미세 조정 (Supervised Fine-Tuning, SFT)은 컨텍스트, 예시, 그리고 사용자 명령을 기반으로 텍스트 생성을 안내하는 지침을 활용합니다. 이것을 컨텍스트 창 또는 프롬프트라고 합니다. 이러한 학습 과정은 초기 조건을 넘어서 네트워크가 명시적으로 훈련받지 않은 능력들의 출현을 포함하고 있으며, 이는 이전의 더 작은 모델들로 수행된 실험에서 예측할 수 없었던 능력들입니다.

<div class="content-ad"></div>

얼마나 큰 가치를 잠재우는지 제안하는 모든 분야에 적용되고 있는 생성적 AI는 새로운 성장을 가져오고 있습니다. 판매, 마케팅, 고객 서비스, 소프트웨어 개발 분야에서 그 사용이 특히 두드러지게 나타납니다. 혁명에 영향을 받지 않는 분야는 없습니다. 어떤 코멘테이터들은 진화에 대해서만 언급하지만, 그것은 우리가 이전에 보유한 기술과 오늘날 가지고 있는 기술 사이의 차이를 오해하는 것입니다. 이러한 모델이 다루는 활동이 광범위하고 다양하기 때문에 응용 가능성은 무한합니다. 우리가 예전에 명확한 작업에 특화된 단일 활동 모델을 가졌던 것과는 달리, 이제 그 모델을 변경하지 않고도 여러 기술을 가지고 있습니다.

보스턴 컨설팅 그룹의 연구에 따르면 "GPT-4를 현재 능력의 한계 내의 작업에 사용할 때, 거의 모든 참가자가 성과를 향상시켰습니다. 이 범위를 벗어나 작업에 GPT-4를 사용한 사람들은 도구를 사용하지 않은 사람들보다 성과가 나빴습니다." 이 연구는 한 모순을 보여줍니다: 기술이 상당한 혜택을 가져다 줄 수 있는 분야에 대해 사람들은 회의적이지만, 기술적 능력이 부족한 분야에 대해서는 과신을 하고 있는 것입니다. 이것은 이러한 기술이 일상적으로 어떤 기여를 할 수 있는지 이해하는 데 더 중요한 이유가 됩니다.

비록 연구가 컨텐츠 생성(특히 발상)을 위해 템플릿을 사용하는 데 초점을 맞추며, 생성된 결과를 도전하는 필요성을 강조하지만, 이러한 템플릿의 사용 영역과 전문성을 명확히 이해하는 것이 중요해 보입니다. 하지만, 현재까지는 이러한 언어 모델의 사용 범위를 정의하는 분류법이 없습니다.

넓은 범위의 언어 모델은 텍스트나 소스 코드 작성에 우수합니다. 반면에, 그 모델은 사고할 수 없고 사실적인 답변을 제공하거나 추론할 수 없으며 (특정 기교를 사용한다 해도), 작업을 계획하거나 수학적 작업을 수행하거나, 인터넷이나 데이터베이스에서 정보를 가져오는 등의 외부 도구를 직접 사용할 수 없습니다. 이러한 기능 중에 마지막 기능은 특정 인터페이스가 모델이 이러한 작업을 할 수 있다는 것을 보여주지만, 이러한 프로세스는 모델 외부의 프로그램에 의해 수행됩니다.

<div class="content-ad"></div>

## 제안된 분류 체계

옥스포드 사전에서는 분류 체계를 "식물이나 동물을 포함한 물건들을 비슷한 특성을 공유하는 그룹으로 명명하고 조직화하는 시스템"으로 정의합니다. 이 목록은 일반적인 클래스가 의존성에 따라 분류될 수 있도록 계층화되어 있음을 덧붙일 필요가 있습니다. 이 목록은 종종 나무 형태로 나타내어지며, 뿌리에 가까운 카테고리일수록 가장 일반적입니다. 가지를 따라 내려갈수록 주제가 더 구체적해집니다.

주요 언어 모델의 능력에 대한 제안된 분류 체계는 다음과 같이 여섯 가지 일반 카테고리로 구성됩니다:

- 생성(Generation): 하나 이상의 문장으로 텍스트를 완성할 수 있게 하는 능력,
- 정보 추출(Information Extraction): 텍스트나 텍스트 모음으로부터 하나 이상의 단어를 추출하는 능력,
- 분류(Classification): 하나 이상의 클래스나 카테고리로 텍스트나 텍스트 모음 전체 또는 일부를 레이블하는 능력,
- 텍스트 변환(Text Transformation): 하나 이상의 문장의 전체나 일부를 변경하는 능력,
- 문제 해결(Problem-solving): 문제를 해결하는 능력, 그리고
- 텍스트 이해(Text Comprehension): 주어진 텍스트 단락을 읽고 이해하며, 그 텍스트에 기반한 질문에 답하는 능력을 포함합니다.

<div class="content-ad"></div>

이러한 넓은 범주를 살펴보겠습니다.

## 1. 생성

텍스트(더 정확히는 단어나 단어 부분을 나타내는 토큰) 또는 소스 코드의 생성은 언어 모델의 주요 기능입니다. 이 모델들이 훈련되는 기능은 사람이 작성한 텍스트와 구별할 수 없는 알기 쉽고 잘 구성된 텍스트를 생성하는 것입니다.

자연어 처리(NLP)에서 이 기능을 자연어 생성(NLG)이라고 합니다. 이 주요 기능은 글의 시작을 완성하거나 소설 초고, 이메일, 아이디어 목록(ideation)을 이어가는 데 사용됩니다. 모델이 코드로도 훈련되기 때문에, 소스나 지시문에서 코드 입력을 완료하면 다양한 프로그래밍 언어의 코드 라인을 생성할 수 있습니다. 이를 통해 예를 들어 Java로 프로그램 테스트 함수를 작성할 수도 있습니다.

<div class="content-ad"></div>

미래 토큰 예측 능력에는 다양한 활용 방안이 있습니다. 이는 텍스트 편집기에서 작성된 텍스트를 완성하는 데 도움을 주는 데 사용될 수 있으며, 휴대전화(SMS)나 검색 엔진에서도 활용될 수 있습니다. 음성 인식을 수정하거나 지식 테스트에서 사용되는 공백이 채워진 텍스트를 완성하는 데 사용될 수도 있습니다.

텍스트 생성은 가장 널리 사용되는 사례이지만, 2023년에 ChatGPT를 성공으로 이끈 것은 명령문에서 문장을 생성하는 것이었습니다. 수백 가지의 사용 사례, 수백 개의 프롬프트 교육 과정, 수백 편의 ChatGPT로 백만장자가 되는 방법에 관한 기사들이 있습니다. 그 이후로 사용 사례 목록은 크게 늘어났는데, 이메일이나 시에 대한 아이디어 목록을 작성하거나 제품 시트를 작성하는 것과 같은 작업까지 가능해졌습니다.

요약하자면, LLM 생성 능력은 다음과 같이 활용됩니다:

- 최상의 다음 토큰 예측: 일련의 토큰을 따를 최상의 토큰을 결정합니다.
- 텍스트에서 단어 완성: 텍스트 끝에 단어를 추가합니다.
- 프롬프트를 기반으로 한 문장 생성: 지시에 따라 텍스트, 목록 또는 소스 코드를 생성합니다.

<div class="content-ad"></div>

## 2. 추출

정보 추출은 텍스트 또는 텍스트 모음에서 하나 이상의 단어를 추출하여 분리하고 강조하는 작업을 의미합니다.

텍스트 분석의 핵심 작업 중 하나는 문장 분할입니다. 이 과정은 단락이나 텍스트를 다양한 문장으로 분할하는 것을 포함합니다. 각 문장은 대문자로 시작하고 마침표로 끝납니다. 그 후 각 문장은 하나씩 처리될 수 있습니다.

![image](/assets/img/2024-07-13-ATaxonomyofLargeLanguageModelsCapabilities_1.png)

<div class="content-ad"></div>

문장은 이해력에 많은 영향을 주지 않는 많은 단어들로 구성되어 있어, 정보를 희석시켜 내용 분석을 방해합니다. 이를 우리는 stop words 라고 부릅니다. 자동 키워드 추출은 문장에서 관련 표현이나 용어를 발견할 수 있도록 가능한 중요한 단어들을 식별하는데 도움을 줍니다. 이러한 단어는 웹 페이지 참조나 색인화에 사용될 수 있습니다. 한 가지 접근 방식으로 이러한 단어들을 주제로 그룹화하여 텍스트의 주요 주제를 식별함으로써 분류하는 데 도움을 줍니다. 이를 텍스트에서 개념을 추출하거나 주제를 추출한다고 합니다.

특정 단어나 단어 그룹들은 Named Entity Recognition(NER)이라고 불리는 카테고리로 그룹화될 수 있습니다. 이 방식으로 모델에게 텍스트에서 단어의 하나 이상의 카테고리를 추출하도록 요청되며, 해당 단어나 그들의 분류를 표시합니다. 이러한 카테고리에 대한 보편적인 표준은 없지만, 조직, 장소(주소, 도시...), 사람, 금액, 백분율, 시간 및 날짜 요소, 차량 유형을 포함할 수 있습니다. 이 방식의 장점은 모델들이 서로 다른 카테고리를 추출하는 능력에 제한이 없어 보인다는 것이며, 반면 더 작은 모델들은 그들의 원동력에 제한을 받을 수 있습니다.

![](https://example.com/assets/img/2024-07-13-ATaxonomyofLargeLanguageModelsCapabilities_2.png)

마지막으로 가장 일반적인 추출 중에는 entity들을 연결하는 추출이 있습니다. 이것은 문장에서 두 entity 사이의 링크를 나타내는 의미적 관계 추출로, 주체-술어에 해당하는 링크를 가리키며 지식 그래프를 만드는 데 사용됩니다.

<div class="content-ad"></div>

![LLM extraction capabilities](/assets/img/2024-07-13-ATaxonomyofLargeLanguageModelsCapabilities_3.png)

In short, Large Language Models (LLMs) have various capabilities for extraction, including:

- Sentence Segmentation: breaking down text into complete sentences.
- Keyword Extraction: identifying significant words in a text.
- Concept Extraction / Topic Modeling: revealing key themes from a text.
- Named Entity Extraction: recognizing words that belong to specific categories.
- Semantic Relationship Extraction: pinpointing and extracting meaningful links or connections between different entities in a text.

## 3. Classification

<div class="content-ad"></div>

분류에는 텍스트 또는 텍스트 모음의 전체 또는 일부를 하나 이상의 클래스 또는 범주로 레이블링하는 것이 포함됩니다. 클래스는 텍스트의 언어, 문장의 의도, 문단에서 표현된 감정 또는 감정, 빈정거림, 비꼬음 또는 부정의 존재일 수 있습니다.

언어 식별은 LLM(Large Language Models)이 텍스트나 문서에서 사용된 언어를 감지 지시를 기반으로 표시할 경우를 가리킵니다.

![image](/assets/img/2024-07-13-ATaxonomyofLargeLanguageModelsCapabilities_4.png)

요청의 의도를 감지하는 것은 요청의 전체 의미를 이해하는 데 필수적인 기능입니다. 의도는 요청을 "전체적으로" 분류하여 해당 처리로 전달할 수 있도록 합니다.

<div class="content-ad"></div>

"My bank card has been stolen. I need a new one"라는 문장은 renewal_cb로 분류되며 적절한 처리를 유발합니다. 특정 클래스를 결정하기 위해서는 결정의 예제와 클래스의 설명을 모델에 제시하는 것이 필요합니다. 가장 일반적인 사용 사례는 사용자의 의도를 결정하고, 그 후 필요한 모든 정보를 추출한 다음 사용자를 잠금 해제할 최종 작업을 수행하기 전의 시나리오를 유발하는 것입니다.

언어 모델은 감정 및 감정, 비꼬는 말과 비꼬는 문장, 문장이 부정적인지 감지하는 데 뛰어납니다. 이러한 기능은 이러한 영역에서 이전 모델보다 훨씬 뛰어난 성과를 거두고 있습니다. 감정 분석은 댓글이 긍정적인지, 중립인지, 부정적인지를 판단합니다. 이 능력은 소셜 네트워크의 댓글, 지원팀과의 대화, 이메일에서 의견을 평가하는 데 사용되며, 특히 우선 순위를 정리하고 긴급 사태를 처리하는 데 유용합니다. 마케팅팀은 경쟁사 제품에 대한 의견을 평가하거나 광고 캠페인의 영향을 실시간으로 평가할 수 있습니다.

감정 분석은 댓글의 관점을 추출하는 데 가장 포괄적인 방식입니다. 댓글이 긍정적인지, 부정적인지만 말하는 것보다 감정과 비꼬는 말을 감지하기 위해 더 많이 나아가야 하는 경우도 있습니다. 감정 감지는 더 많은 정보, 미묘하고 복잡한 감정의 훨씬 다양한 것을 추출하는 방법이며, 특히 공감적인 어시스턴트를 만들기 위한 목표인 경우입니다. 긍정적인 느낌 또는 부정적인 느낌만 있는 것보다 더 섬세하게 반응할 수 있어야 하는 것이 중요합니다. 기본 목록에는 분노, 놀람, 혐오, 기쁨, 두려움, 슬픔의 여섯 가지 기본 감정이 포함될 수 있지만, 인간의 복잡성을 반영하기 위해 더 많은 감정이 있습니다.

![이미지](/assets/img/2024-07-13-ATaxonomyofLargeLanguageModelsCapabilities_5.png)

<div class="content-ad"></div>

긍정적이거나 부정적인 감정 이상으로 세련된 대응이 가능한 것이 중요합니다. 주요 목록에는 분노, 놀라움, 혐오, 기쁨, 두려움, 그리고 슬픔과 같은 여섯 가지 기본 감정이 포함됩니다. 그러나 인간의 복잡성을 반영하기 위해 훨씬 많은 감정들이 있습니다. 문장이 부정적인 방향으로 전환되었다는 것을 감지하는 것도 흥미로운 사용 사례이며, 일반적인 감지 시스템은 이러한 방향을 처리하는 데 어려움을 겪습니다.

마지막으로, 의미론적 역할 레이블링(SRL)은 문장 내 단어에 의미적 의미, "역할"을 할당하는 것을 포함합니다. 결과적으로 술어-인자 구조의 모델이 생성됩니다. 이것은 "누가 무엇을, 누구에게, 어디에 하였는가?"라는 질문에 대한 대답입니다.

![Image](/assets/img/2024-07-13-ATaxonomyofLargeLanguageModelsCapabilities_6.png)

요약하면, 대규모 언어 모델(LLM)의 분류 능력에는 다음이 포함됩니다:

<div class="content-ad"></div>

- 언어 식별: 텍스트의 언어를 판별합니다.
- 의도 감지: 요청을 카테고리에 따라 분류합니다.
- 감정 및 감정 분석: 문장이 긍정적인지 부정적인지 확인하고 인간 감정과 연관시킵니다.
- 풍자와 비꼼 감지: 감정을 표현하는 텍스트에서 비꼼을 감지합니다.
- 부정 탐지: 텍스트에서 부정 신호를 식별하는 것을 포함합니다.
- 의미 역할 라벨링: 단어에 의미 역할을 할당합니다.

## 4. 변형

변형은 한 문장 이상의 단어를 수정하는 것을 포함합니다. 이것에는 수정, 조작, 번역, 다시 쓰기 및 압축 작업이 포함됩니다.

모델의 능력을 통해 단어 수준에서 작용하고 문맥을 고려하여 전체 문장을 수정하는 작업을 수행할 수 있습니다. 이러한 수정 작업을 통해 모델에게 철자, 문법 또는 구두점 오류를 수정하도록 요청할 수 있습니다.

<div class="content-ad"></div>

Manipulation operations like standardization (normalization) change numbers, dates, acronyms, and abbreviations into plain text, making them more understandable for conventional extraction algorithms. Other manipulations involve modifying the content of texts by adding new elements, replacing certain parts of the text, or removing non-essential information for a specific purpose.

![Image 1](/assets/img/2024-07-13-ATaxonomyofLargeLanguageModelsCapabilities_7.png)

Text translation is the process of converting a text from one language (source language) to another language (target language) while maintaining the message as faithfully as possible. This conversion is also applicable when working with computer languages, using models specifically trained to understand the unique vocabulary of coding languages like Code Llama from Facebook AI Research, GPT4 from OpenAI, StarCoder from BigCode, or WizardCoder from WizardLM.

![Image 2](/assets/img/2024-07-13-ATaxonomyofLargeLanguageModelsCapabilities_8.png)

<div class="content-ad"></div>

패러프레이징은 다른 단어를 사용하여 원래 의미를 유지하면서 텍스트를 다시 정리하는 행위입니다. 패러프레이징은 표절을 피하거나 텍스트를 간소화하거나 명확히 하는 것과 같은 여러 목적을 제공할 수 있습니다. 또한 텍스트를 특정 대상이나 맥락에 맞게 적응하거나 텍스트를 동의어나 비유로 풍부하게 만드는 데 도움이 됩니다.

요약은 텍스트의 의미를 이해하고 그 구조와 주장을 식별하며 핵심적이고 명확하게 표현하는 종합적인 작업입니다. 요약은 모델이 텍스트의 간략하고 요약된 제시로 렌더링하는 것입니다. 대형 모델의 작동은 명시적으로 요약에 훈련된 모델의 성능과 동등합니다. 세대 영상에서 다빈치-인스트럭트 v2는 CNN/Daily Mail 및 XSUM 데이터셋에서 대부분의 Pegassus 점수에서 충실성, 일관성 및 관련성 면에서 압도적인 성능을 보입니다.

![이미지](/assets/img/2024-07-13-ATaxonomyofLargeLanguageModelsCapabilities_9.png)

요약하면, 대형 언어모델의 변환 능력에는 다음이 포함됩니다:

<div class="content-ad"></div>

### 5. 문제 해결

문제에 대한 해결책을 찾기. 해결책이란 문제를 해결하고, 주어진 상황이나 도전에 대한 해법을 찾는 능력을 가리킵니다.

공시참조 해결은 텍스트에서 같은 개체를 가리키는 모든 표현을 찾는 것을 의미합니다. 예를 들어, "Mary bought a book and she read it"이라는 문장에서 "Mary"와 "she"라는 표현은 동일한 사람을 가리키므로 공시참조되었습니다. 마찬가지로 "a book"과 "it"이라는 표현도 동일한 객체를 가리키기 때문에 공시참조됩니다. 이 해결은 텍스트 전체에 분산된 정보를 연결하고 의미 있는 일관된 표현을 만들 수 있게 합니다.

<div class="content-ad"></div>

Anaphora resolution in grammar is an interesting concept that falls under co-reference resolution. It involves linking an anaphora (a restatement of a previous segment with a similar meaning) to the word it refers to. On the other hand, a cataphor is a figure of speech where one word stands in for another that hasn't been mentioned yet, like in the sentence "He did it, Pierre did it." Then, there's exophora which refers to something not explicitly present in the text but can be inferred from the context or situation. For instance, in the sentence "Look at that!," the pronoun "that" refers to something visible to both the speaker and the listener. It can get quite complex, especially when it involves background knowledge or context not explicitly stated.

Word disambiguation is about determining the correct meaning of a word in a particular context, especially when a word has multiple meanings. In the sentence, "The rock band played in the bar, and the bass was so intense that it caused a vibration, leading to a glass falling and breaking," "bass" could be the instrument or the sound, and "bar" could be a place or a musical notation. Providing more context can help the model make a definitive decision.

![Large Language Models Capabilities](/assets/img/2024-07-13-ATaxonomyofLargeLanguageModelsCapabilities_10.png)

Missing Elements analysis focuses on identifying elements that are not explicitly mentioned in a text. These missing elements are aspects that are implied but not directly stated. The goal is to recognize and fill in words or parts of words that may have been left out or shortened in the text. For instance, in the sentence, "I saw the film yesterday, it wasn't bad," the word "not" is missing after "wasn't." By addressing these missing elements, the text can be restored to its complete and accurate form, enhancing comprehension and translation.

<div class="content-ad"></div>

LLM의 해결 능력은 다음과 같습니다:

- 대용어 해결: 텍스트에서 동일한 개체를 참조하는 표현을 찾습니다. 대용어 해결의 일부인 선행어 (Anaphora), 후행어 (Cataphora), 및 외부어 (Exophora) 이지만 다른 개체 위치에서 발생합니다: 이전, 이후, 및 외부.
- 단어 모호성 해결: 각 맥락에서 단어의 올바른 의미나 감을 결정합니다.
- 누락된 요소: 텍스트에서 누락되거나 약어로 표현된 단어나 단어 일부를 식별하고 복원합니다.

## 6. 이해

이해는 모델에게 제시된 텍스트를 이해하거나 소화하거나 이해하는 능력을 나타냅니다. 이것은 의심을 품게하는 모든 범주 가운데 의논의 여지가 있는 것입니다. 이 범주는 모델이 인간의 사고를 시뮬레이션할 수 있다고 제안하는 범주입니다.

<div class="content-ad"></div>

질문에 답변하는 능력은 비즈니스 질문, 유지보수 질문, 특정 분야에서 도움이 필요한 사람들로부터 온 질문을 대답하는 시스템의 사용 사례를 다룹니다.

질문을 이해하고 답변을 생성하는 전반적인 능력에서, "지식"을 추출하는 것이 모델에게 가장 자연스러운 일입니다. 이는 모델이 여러 교육 단계에서 기록한 숫자들을 기반으로 직접적인 답변을 생성하는 것으로, 모델이 보유한 정보가 더 많을수록 더 정확한 정보를 유지할 수 있지만, 정보가 교육 당일 (사실상 인터넷에서 데이터를 검색한 날)에만 최신 상태이기 때문에 빠르게 시대에 뒤떨어지는 단점이 있습니다. 그러나 대부분의 정보는 안정적입니다 (원칙적으로), 태양계의 행성 수나 지구가 둥글다는 사실과 같이!

![이미지](/assets/img/2024-07-13-ATaxonomyofLargeLanguageModelsCapabilities_11.png)

질문 생성은 텍스트에서 중요한 요소를 추출하여 이를 이해했는지 확인하기 위해 질문으로 변환하는 것을 포함합니다. 교사들은 시험 준비에 도움을 받기 위해 이 기능을 활용할 수 있습니다. 어느 정도까지는 질문을 제공하여 확인할 수 있는 사실 확인을 가능하게 하며, 이를 통해 핵심을 추출합니다.

<div class="content-ad"></div>

![2024-07-13-ATaxonomyofLargeLanguageModelsCapabilities_12](/assets/img/2024-07-13-ATaxonomyofLargeLanguageModelsCapabilities_12.png)

Machine Reading Comprehension (MRC)은 검색 증강 생성 (RAG)에 사용되는 주요 기술입니다. RAG는 생성 모델을 통해 데이터 신선도 및 기업 지식 접근 문제를 극복합니다. 이로 인해 출처 증명 및 환존 현상 감소를 용이하게 합니다. 이것은 최근 몇 달간 가장 널리 발표된 원칙으로, 구현이 쉽기 때문에 주목을 받고 있습니다.

위치 감지는 특정 주제, 제품, 서비스 또는 사건에 대한 개인의 태도 및 감정을 이해하는 것을 포함합니다. 이것은 연사자의 의견 및 판단이 주어진 제안에 대해 감지되는 것을 포함합니다. 이는 논평자의 의견에 대한 자격 있는 연구에 비유될 수 있습니다.

질문 품질 평가는 주관적 질문에 답변하기 위한 알고리즘을 개발함을 목표로 합니다. 질문이 우수한 품질인지 또는 수정이 필요한지 확인하는 것을 체크합니다.

<div class="content-ad"></div>

자연어 추론은 "가설"이 "전제"로부터 참인지(함의), 거짓인지(모순), 또는 결정할 수 없는지(중립)를 판단하는 것으로 구성되어 있습니다.

지식 베이스의 완성(관계적 사실들을 주로 "주체", "관계", "대상" 쌍의 형태로 표현된 정보 모음)은 이미 존재하는 정보에 대해 추론하여 누락된 사실을 자동으로 추론하는 것을 의미합니다. 이 능력은 연결 예측이라고도 알려져 있습니다.

모델의 추론 능력 사용은 처방 개념과 관련이 있습니다. 즉, 초기 조건에 따라 최선의 선택을 조언하는 AI입니다. 처방 분석은 미래 이벤트의 예측뿐만 아니라 이를 영향을 미칠 것으로 예상되는 요소도 제공합니다. 이러한 데이터는 분석가와 의사 결정자가 전략적 결정의 결과를 평가하고, 의사 결정 프로세스를 최적화하는 데 도움을 줍니다. 이는 실시간 데이터를 기반으로 한 판단에 관한 문제, 의사 결정의 영향, 자원 선택의 계획 및 최적화, 그리고 고려해야 할 가장 복잡한 질문에 대한 대답을 포함합니다.

<div class="content-ad"></div>

모델의 추론 능력은 여러 하위 활동으로 세분화될 수 있습니다. 첫 번째는 상식적 추론입니다. 상식이 정확히 무엇인지에 대한 명확한 정의는 없지만, 모든 인간에게 공통적인 것이라고 말할 수 있습니다. 상식과 연관된 사용 사례는 없으며, 모델이 이 "상식"을 추론 능력에 내재적으로 통합할 수 있는 능력이 있다는 것이 기대됩니다.

![image1](/assets/img/2024-07-13-ATaxonomyofLargeLanguageModelsCapabilities_14.png)

논리적 사고는 논리 원리를 사용하여 결론을 도출하거나 결정을 내리는 과정입니다. 체계적으로 사고하고 논리의 규칙을 적용하여 정보를 분석하고 주장을 평가하며 결론을 도출하는 것을 포함합니다.

![image2](/assets/img/2024-07-13-ATaxonomyofLargeLanguageModelsCapabilities_15.png)

<div class="content-ad"></div>

최근까지 모델 응답은 주로 모델에 한 번 호출하여 얻은 결과인데, 이는 결론에 이르는 과정의 이해를 도울 때에 항상 도움이 되지 않는다. Multi-hop reasoning을 통해 여러 단계의 추론을 수행할 수 있게 하여 문제의 논리적 구조를 반영한 프로세스를 통해, 회고적 분석이 가능하다. Chain of Thought (CoT)은 새로운 방법론으로, 단계별 응답 예시를 통해 복잡한 다단계 추론이 유도되게 한다.

최근 대형 언어 모델의 발전으로 수학 추론문제의 처리에 상당한 발전이 이루어졌다. 특히, OpenAI의 GPT-4 Code Interpreter의 최신 버전은 어려운 수학 문제에서 놀라운 성능을 보여주는 ChatGPT Plus 가입자를 위한 AI가 코드를 실행하고 복잡한 데이터를 분석할 수 있게 하는 플러그인이다. 그러나, 여러 단계를 요하는 복잡한 수학 문제나 다양한 수학 개념이 관련된 문제에 대응할 때 모델은 어려움을 겪을 수 있다. 수학자들이 사용하는 수많은 추론법 (귀납법, 연역법, 불합리 등)과는 대조적으로, 모델에게 어떤 것이 사용될 수 있는지 쉽게 알기 어렵다. 일부 연구자들은 추론을 테스트하기 위해 코드를 생성하는 방안에 이르기도 한다 (코드 기반의 자가 확인 또는 CSV).

![이미지](/assets/img/2024-07-13-ATaxonomyofLargeLanguageModelsCapabilities_16.png)

하지만, Takeshi Kojima 등의 연구는 모델이 일반적으로 학습하는 데 있어서 우수한 학습자로 알려져있지만, 각 답변 앞에 "참조하고 단계별로 생각해보자"라고 간단히 추가함으로써 모델이 Zero-shot 이해자로서 우수한 예측 능력을 갖는 것을 보여주었다. 실험 결과는 그들의 Zero-shot-CoT가 다양한 추론 작업에서 Zero-shot LLM을 크게 앞서나감을 보여준다.

<div class="content-ad"></div>

![Image](/assets/img/2024-07-13-ATaxonomyofLargeLanguageModelsCapabilities_17.png)

In Korean:

표에 따르면, 대형 언어 모델의 이해 능력은 다음과 같이 구성됩니다:

- 질문에 대한 답변: 매개 변수에 저장된 지식을 사용하여 도메인에 관한 질문에 대한 답변을 함으로써 연결된 여러 능력이 있습니다. 텍스트에서 질문 생성; 모델에 제시된 정보의 독해; 화자의 견해 및 판단을 감지하는 위치 탐지(의견); 그리고 질문 품질 평가의 경우 질문이 올바른지 확인하는 것을 목표로 합니다.
- 자연어 추론: "가설"의 값을 결정하는 것.
- 지식 보완: 지식 집합 내 누락된 사실이나 관계를 제공합니다.
- 추론: 추론은 일상적이고 보편적으로 공유되는 일상적인 지식을 기반으로 세상을 이해하고 추론을 만들어내는 능력인 상식 파악으로 시작됩니다. 논리적 사고는 일상적인 언어로 나타나는 주장을 검토, 분석하고 비판적으로 평가하는 능력; 멀티 호합 추론은 여러 조각의 정보나 사실로 부터 답변이나 결론에 이르기까지 몇 단계 거쳐 관련성을 파악하고 이해하는 능력을 정의합니다; 수학적 문제 추론은 논리적으로 수학적 개념,문장 및 문제를 분석하여 유효한 결론에 도달하는 과정을 의미합니다.

## 조합의 예

<div class="content-ad"></div>

의견 추출은 텍스트에서 표현된 의견, 감정, 감정 또는 판단을 식별, 추출 및 이해하는 데 집중합니다. 이는 특정 주제, 제품, 서비스 또는 사건에 대한 사람들의 태도와 감정을 이해하는 것을 포함합니다. 의견은 의견이 관련된 요소들에 의해 보완될 때 더 흥미롭습니다.

공지 사항을 완전히 이해하기 위해, 주요 모델의 다양한 능력면을 결합하는 것이 가능합니다. 예를 들어:

- 명명된 엔터티 추출: 의견이 표현된 엔터티의 식별. 제품, 브랜드, 사람 또는 기타 관련 있는 엔터티의 이름을 추출하는 것이 포함될 수 있습니다.
- 감정 및 감정 분석: 의견의 극성을 결정하는 것, 즉, 그것들이 긍정적인지, 부정적인지 또는 중립적인지 여부를 파악하는 것입니다. 이 분류는 의견과 연관된 감정을 분류하는 것 또한 포함할 수 있습니다.
- 위치 탐지: 판단.
- 이해: 의견이 표현된 맥락과 뉘앙스를 이해하는 것. 이는 톤, 비유, 말장난 등을 고려하여 더 정확한 해석을 위해 자연 언어의 다른 측면을 고려할 수 있습니다.

![이미지](/assets/img/2024-07-13-ATaxonomyofLargeLanguageModelsCapabilities_18.png)

<div class="content-ad"></div>

## 평가 방법

능력을 검증하지 않고는 능력을 인증할 수 없습니다. 요즘에는 모델의 성능이 계속해서 향상되고 이러한 셋을 교육에 사용함에 따라 전통적인 테스트가 구식이 되었다. 이로 인해, 평가하는 사람들과 평가에서 잘 나오고자 하는 사람들 사이에 속도 경쟁이 만들어졌습니다. 현재까지 대형 언어 모델의 능력을 평가하기 위해 사용되는 수백 가지의 평가 방법(벤치마크)이 있습니다.

Mistral의 새로운 Mixtral 모델에 대한 연구 논문에서 그들은 다음과 같은 평가 셋을 사용하여 테스트를 실시했습니다:

- 상식적 추론 (0-shot): Hellaswag, Winogrande, PIQA (물리적 상호 작용: 질문 응답), SIQA (사회 지성 질문 응답), OpenbookQA, ARC-Easy (AI2 추리 도전), ARC-Challenge, CommonsenseQA.
- 세계 지식 (5-shot): NaturalQuestions 및 TriviaQA.
- 읽기 이해 (0-shot): BoolQ (불리언 질문) 및 QuAC (맥락적 질문 응답).
- 수리 추론: GSM8K (8-shot)에서 maj@8 및 MATH (4-shot)에서 maj@4.
- 코드 생성: HumanEval (0-shot) 및 MBPP (3-shot — 주로 기본 파이썬 문제).
- 기타 평가: MMLU (5-shot — 대규모 다중 작업 언어 이해 측정), BBH (3-shot — BIG-Bench Hard) 및 AGIEval (3–5-shot, 영어로만 구성된 객관식 문제 — 인공 일반 지능 평가).

<div class="content-ad"></div>

In arithmetic reasoning evaluation, we find AddSub, AQuA, ASDiv, GSM8K, Lila, MAWPS, MultiArith, SVAMP, SingleEq, SingleOp; in common-sense: CODAH (COmmonsense Dataset Adversarially-authored by Humans), StrategyQA, ARC, BoolQ, HotpotQA, OpenBookQA, PIQ; in symbolic: CoinFlip and LastLetterConcatenation, ReverseList; in logic: ReClor, LogiQA, ProofWriter, FLD (Formal Logic Deduction), FOLIO (first-order logic). Not forgetting ALERT (Adapting Language Models to Reasoning Tasks), ARB (Advanced Reasoning Benchmark), BIG-bench (Beyond the Imitation Game benchmark), CONDAQA (COntrastively-annotated Negation DAtaset of QuestionAnswer pairs) and WikiWhy…

일부 셋은 매우 큽니다. 예를 들어 BIG-bench는 132개 기관의 450명 작가들이 제공한 204가지 작업으로 구성되어 있습니다. 이 작업의 주제는 언어학, 아동 발달, 수학, 상식적 추론, 생물학, 물리학, 사회 편견, 소프트웨어 개발 및 기타 다양한 분야에 걸쳐 다양합니다.

## 결론

본 기사에서는 주요 언어 모델의 능력에 대한 상세한 분류를 제시하며, 여섯 가지 주요 범주로 이루어진 분류 체계를 제공합니다. 이 분류는 연구가들이 특정 비즈니스 사례로 향하도록 도와줄 것입니다. 이 기여는 연구와 개발을 촉진하여 이러한 강력한 생성형 AI 도구들을 더 현명하게 활용하고자 하는 데 도움이 됩니다.

<div class="content-ad"></div>

언어 모델은 다양한 언어 기능을 포괄하는 다양한 능력을 갖고 있습니다. 텍스트 생성, 정보 추출, 분류, 변환, 해결, 이해 등 다양한 영역에서 이러한 모델들은 그들의 다재다능함을 발휘합니다. 자연어 생성, 주요 개체 추출, 텍스트 분류, 문장 변환, 복잡한 문제 해결 또는 텍스트의 깊은 의미를 이해하는 데 사용되며, 언어 모델은 많은 응용 분야에서 강력한 도구로 자리 잡았습니다. 그들의 사용은 창의적 쓰기부터 의미 분석, 기계 번역, 복잡한 문제 해결까지 이어지며, 언어 인공지능 분야에서의 중요한 발전을 나타냅니다.

우리는 이러한 모델의 단점들을 주의 깊게 살피고 있을 것입니다. 이러한 모델은 환청을 일으키는 강한 경향이 있으며, 질문이 제기된 방식에 민감하며 언어, 성별, 인종적 요인에 영향을 받는 경향이 있습니다. 환청에 대해 강조해야 할 점은 모델이 항상 지시사항을 따르지 않을 수 있으며, 이는 출력물의 자동 처리를 왜곡시킬 수 있습니다. 환청의 종류에 대한 포괄적인 목록은, 레이 황 등이 제시한 "대규모 언어 모델에서의 환청에 관한 조사: 원칙, 분류, 도전 과제 및 미해결 문제"를 추천합니다. 또한 이러한 모델들은 속임수에 대해 높은 민감도를 유지하며, 이는 그들을 제어되지 않은 환경에서의 사용을 제한합니다.

인공 일반 지능(AGI)을 위한 탐구는 많은 논쟁의 중심이며, 많은 미국의 주요 하이테크 기업들이 이를 투자하는 이유입니다. 대규모 모델에서 새로운 능력을 발견한 결과, 우리가 조금 더 나아가면 터널 끝에 지능을 발견할 수 있다는 가능성이 제기되었지만, 궁극적으로 질문은 간단히 말해 "앵무새가 사육될까요?" 입니다.

![A Taxonomy of Large Language Models Capabilities](/assets/img/2024-07-13-ATaxonomyofLargeLanguageModelsCapabilities_19.png)

<div class="content-ad"></div>

So, the question remains open…

## Find out more

![Image](/assets/img/2024-07-13-ATaxonomyofLargeLanguageModelsCapabilities_20.png)

This story is published on Generative AI. Connect with us on LinkedIn and Zeniteq to stay in the loop with the latest AI stories. Let’s shape the future of AI together!

<div class="content-ad"></div>

![Image](/assets/img/2024-07-13-ATaxonomyofLargeLanguageModelsCapabilities_21.png)