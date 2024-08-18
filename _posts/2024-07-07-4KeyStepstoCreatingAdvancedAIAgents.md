---
title: "고급 AI 에이전트를 만드는 4가지 핵심 단계"
description: ""
coverImage: "/assets/img/2024-07-07-4KeyStepstoCreatingAdvancedAIAgents_0.png"
date: 2024-07-07 03:33
ogImage:
  url: /assets/img/2024-07-07-4KeyStepstoCreatingAdvancedAIAgents_0.png
tag: Tech
originalTitle: "4 Key Steps to Creating Advanced AI Agents"
link: "https://medium.com/ai-advances/4-key-steps-to-creating-advanced-ai-agents-129c5bd9139b"
isUpdated: true
---

# LangChain을 LLaMA 3과 함께 로컬에서 사용하는 실용적인 가이드

![Click for the image](/assets/img/2024-07-07-4KeyStepstoCreatingAdvancedAIAgents_0.png)

2024년 5월 인터뷰에서 NVIDIA의 창립자이자 CEO 인 제슨 황(Jensen Huang)은 AI 산업의 다음 주요 발전이 무엇일지 물어보았습니다. 그의 대답은 명확했습니다: 멀티샷 추론이 가능한 AI 에이전트의 개발입니다.

하지만 AI 에이전트란 무엇일까요? AI에 대해 이야기할 때 우리는 자주 혼란스러운 용어의 미로에 갇히게 됩니다. 그것들을 혼동하기 쉽습니다. 오늘 이 글에서는 AI 에이전트란 무엇이며 그 구성 요소는 무엇인지 명확히 하고, 이를 네 가지 핵심 단계로 처음부터 구축하는 방법을 안내하려고 합니다.

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

## 기본 개념: 언어 모델과 RAG

언어 모델은 LLaMA 3과 같이 발전된 텍스트 생성기입니다. 질문을 입력하면 해당 질문에 기반한 답변을 생성해줍니다. 매우 간단한 맥락이죠. 이 생성된 답변은 훈련 중 사용된 데이터를 기반으로 합니다. 예를 들어, ChatGPT 3.5에게 2024년 유로 포르투갈 대 터키 경기의 승자를 묻는다면 대답을 모를 것입니다. 하지만 대화 이상의 답변이 필요하다면 어떨까요? 최근 축구 스코어나 개인 데이터에 관한 답변을 원한다면 어떻게 해야 할까요? 바로 이때 RAG가 등장합니다.

RAG, 즉 검색 강화 생성,은 기본 언어 모델을 한 단계 발전시킨 개념입니다. 이는 관련 컨텍스트를 검색하여...
