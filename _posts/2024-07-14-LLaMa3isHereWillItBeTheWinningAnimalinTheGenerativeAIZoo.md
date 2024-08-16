---
title: "LLaMa 3 출시 2024년 생성형 AI 경쟁에서 승자가 될까"
description: ""
coverImage: "/assets/img/2024-07-14-LLaMa3isHereWillItBeTheWinningAnimalinTheGenerativeAIZoo_0.png"
date: 2024-07-14 01:55
ogImage: 
  url: /assets/img/2024-07-14-LLaMa3isHereWillItBeTheWinningAnimalinTheGenerativeAIZoo_0.png
tag: Tech
originalTitle: "LLaMa 3 is Here. Will It Be The Winning Animal in The Generative AI Zoo."
link: "https://medium.com/gitconnected/llama-3-is-here-will-it-be-the-winning-animal-in-the-generative-ai-zoo-9e94af73588f"
isUpdated: true
---





## |META LLAMA 3|LLM|GENERATIVE AI|

![LLaMa 3](/assets/img/2024-07-14-LLaMa3isHereWillItBeTheWinningAnimalinTheGenerativeAIZoo_0.png)

메타는 방금 LLaMA 3을 발표했고, Klaude 3와 Gemini를 이길 수 있다고 주장했습니다. 모두 70B 파라미터 모델로 가능했다고 합니다.

## 하지만, LLaMA 3이 왜 중요한 것일까요? 이것이 무엇을 바꾸며, 오픈 소스 세계에 어떤 의미를 가지고 있는 걸까요? META는 어떤 계획을 가지고 있을까요?

<div class="content-ad"></div>

간단한 요약입니다. 2023년 초였고 AI 세계는 아직 ChatGPT의 등장으로 회복되지 못했습니다 (Google 자체도 준비되지 않았습니다). AI 외부 세계, 과학 소설을 제외한 AI를 알고있던 세계조차도 Large Language Model (LLM)이 무엇인지를 배우기 시작했습니다. 2023년까지 META는 현대 자본주의의 악 중 하나로 알려졌으며 사용자 데이터를 이용하는 회사로, 확실히 오픈 소스의 옹호자는 아니었습니다. 그러나 몇몇 이성적인 연구자들이 있었지만, LLM에 대한 시도는 성공적이지 못했습니다 (Galactica를 참조하세요). 마찬가지로 META는 대대적인 감춘 정보와 메타버스 재앙(또는 우리 모두가 Facebook의 가상 현실에서 24/7 연결된 것을 허락할 수 없는 주크버그의 꿈)으로 고퇴하고 있는 것처럼 보였습니다.

2023년 2월, META가 LLaMA의 도착을 발표했습니다: 언젠가 누구에게나 저렴하게 사용될 수 있는 7억부터 700억까지의 모델 군입니다. 더욱이, META는 제품데이터가 아닌 오픈 소스 데이터로 이를 훈련시켰습니다.

![LLaMA-2](/assets/img/2024-07-14-LLaMa3isHereWillItBeTheWinningAnimalinTheGenerativeAIZoo_1.png)

하지만 LLaMA-2가 진정한 혁명이었습니다. 먼저, META는 새로운 모델에 대해 Microsoft와 협력했고 (사실, 해당 모델은 Azure에서 이용 가능했습니다). 둘째, 연구 및 상용목적으로 무료입니다. 셋째, 형성되고 있는 생태계는 이전에 본 적이 없는 것으로, 대부분의 개발자들에게 무수히 많은 가능성을 제시했습니다.

<div class="content-ad"></div>

For a long time, LLaMA-2 has been the go-to open-source model in the world of AI. From there, a menagerie of LLMs with names inspired by various animals like alpaca, vicuna, and koala emerged. Can you believe that, beyond LangChain, the LLaMA index is considered the second most essential framework? LLaMA laid the foundation for numerous applications and finely-tuned models tailored to specific domains, truly sparking a revolution.

With the introduction of LLaMA-3, we might witness another wave of excitement sweeping through the developer community. This latest version will be widely accessible through platforms such as AWS, Databricks, Google Cloud, Hugging Face, Kaggle, IBM WatsonX, Microsoft Azure, NVIDIA NIM, and Snowflake.

META has unveiled two primary variants of LLaMA-3: 8B and 70B. Interestingly, they seem to be working on training a colossal 400B model. While these models have yet to undergo community testing, according to META, they have achieved state-of-the-art performance, outperforming both Gemini and Claude in the larger version.

![LLaMa3isHere](/assets/img/2024-07-14-LLaMa3isHereWillItBeTheWinningAnimalinTheGenerativeAIZoo_2.png)

<div class="content-ad"></div>

평소의 기준에 따른 평가의 유용성은 의문이지만, 이는 흔히 사용되는 실천법이며 오픈 소스 모델과 폐쇄 소스 모델을 비교하는 몇 가지 방법 중 하나입니다. 반면에 META-3가 Mistral을 이기는 것은 그다지 인상적이지 않습니다 (Mistral 7B는 현재 조금 구식입니다). 한편, Gemma는 결코 훌륭한 것이 아니었으며 (또는 개인적으로 생각했던 것처럼 느리고, 성능이 협조되었던 것보다 떨어졌습니다). 대신, 70B의 성능은 특히 흥미롭습니다 (Gemini는 이론상으로 Google의 기성품이며, 시장에서 최고로 판매되고자 합니다).

저자들은 실제 시나리오에 최적화하려고 노력하였고, 내부에서 새로운 고품질의 인간 평가 세트를 개발하였습니다 (12가지 주요 사용 사례를 위한 1800개의 프롬프트). 그 이후에, 그들은 다른 유명한 모델들과 인간 평가를 실시했습니다.


![이미지](/assets/img/2024-07-14-LLaMa3isHereWillItBeTheWinningAnimalinTheGenerativeAIZoo_3.png)


어쨌든, 결정적인 평가를 내리기에는 아직 너무 이른 시기입니다. 기술 보고서가 아직 제공되지 않았기 때문에, 저자들은 사용된 기술적 선택 사항을 명시하였습니다:

<div class="content-ad"></div>

- Llama 3은 128K 토큰으로 이루어진 어휘 사전을 사용하는 토크나이저를 사용합니다.
- Llama 3은 30개 국어를 포함한 공개 소스에서 15조 토큰 이상의 사전 훈련을 받았습니다.
- 그룹화된 쿼리 주의 (GQA)를 사용하여 추론 속도가 빨라졌습니다.

연구 중인 저자들은 치칠라의 스케일링 법칙에 따르면 8B의 매개변수에 대해 최적의 훈련을 위해 200B 토큰이 필요하지만, 더 많은 토큰이 더 좋다고 언급하고 있습니다.

이러한 성능은 아직 인상적이지 않은 것처럼 보입니다. 이는 어떤 종류의 초기 릴리스인 것 같아서 일부일 것입니다. 해당 모델은 코드와 이론적 이미지를 포함할 수 있습니다. 하지만 현재 이 모델은 실제로 다중 모달이 아니며, 에이전트 버전은 나중에 출시될 예정입니다.

따라서 더 큰 컨텍스트 창, 더 나은 능력, 그리고 더 많은 언어 능력을 갖춘 모델도 출시될 예정입니다. 또한, 현재 학습 중인 400B 모델이 있는데, 저자들이 초기 체크포인트의 성능을 보여주었습니다.

<div class="content-ad"></div>

![Here is the image for the blog post](/assets/img/2024-07-14-LLaMa3isHereWillItBeTheWinningAnimalinTheGenerativeAIZoo_4.png)

Hey there, fellow Tarot enthusiasts! Let's dive into the latest insights. 

So, here's the scoop - META's intentions may not be as pure as they claim. Investors seem to have caught on, with the stock showing a bump of nearly 2% post-announcement.

What's the tea from META? Well, they're diving headfirst into the world of AI. Investing big bucks in AI infrastructure (yes, GPUs included, and whispers of their own chips in the mix), all in the hopes of enhancing their products. Rumor has it they're eyeing AGI, with LLaMA 3 as just a piece in the puzzle.

Exciting times ahead, isn't it? Stay tuned for more updates! ✨

<div class="content-ad"></div>

아주 흥미로운 주제를 건드리셨네요! 오픈 소스 생태계가 LLaMA로 인해 혜택을 받을 것이라는 것은 맞지만, META 본인도 모델 주변 커뮤니티가 만들어낸 것들로 빨리 내부적으로 도입함으로써 혜택을 받을 것입니다.

실제로 어떤 사람들은 LLaMA가 말한 것처럼 완전히 오픈되어 있지 않다고 지적하고 있습니다. 실제로 제한사항이 있고 모델이 좀 더 투명할 수 있다는 견해도 있습니다.

이 제한사항 중 상당수는 주요 경쟁사들을 향해(월 활성 사용자 7억 명 이상) 방향을 잡고 있기 때문에 이 모델은 분명 Gemma보다는 더 개방적입니다. 어쨌든, HuggingFace에는 LLaMA2의 3만 가지 이상의 변형이 있어, LLaMA의 엄청난 영향을 보여주고 있습니다. LLaMA 3도 똑같이 기대되고 있죠!

## 어떻게 생각하시나요? LLaMA 2를 사용해 보셨나요? LLaMA 3를 사용할 계획이신가요?

<div class="content-ad"></div>

# 만약 이 글이 흥미로우셨다면:

제 다른 글들도 한번 찾아보세요. 또한 LinkedIn에서 제 프로필을 확인하거나 연락할 수도 있습니다. 매주 업데이트되는 기계 학습 및 인공 지능 뉴스가 담긴 이 저장소도 확인해보세요. 협업이나 프로젝트에 관심이 있으시다면 언제든지 LinkedIn을 통해 연락 주세요. 새로운 이야기를 게시할 때 알림을 받으려면 무료로 구독할 수도 있습니다.

제 GitHub 저장소 링크를 통해 기계 학습, 인공 지능 등 다양한 자료와 코드를 수집하고 있습니다.

아니면 제 최근 글 중 하나에 관심이 있을 수도 있습니다.

<div class="content-ad"></div>

## 참고 자료

이 기사를 작성하기 위해 참고한 주요 참고 자료 목록입니다. 기사의 제목만 인용됩니다.

- Touvron, 2023, LLaMA: Open and Efficient Foundation Language Models, [링크](링크)
- Touvron, 2023, Llama 2: Open Foundation and Fine-Tuned Chat Models, [링크](링크)
- META, 2024, Introducing Meta Llama 3: The most capable openly available LLM to date, [링크](링크)
- META, 2024, Build the future of AI with Meta Llama 3, [링크](링크)
- META, 2024, Meet Your New Assistant: Meta AI, Built With Llama 3, [링크](링크)