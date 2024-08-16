---
title: "Meta AI, 고급 코드 및 컴파일러 최적화를 위한 LLM Compiler 공개"
description: ""
coverImage: "/assets/img/2024-07-09-MetaAIUnveilsLLMCompilerforAdvancedCodeandCompilerOptimization_0.png"
date: 2024-07-09 11:30
ogImage: 
  url: /assets/img/2024-07-09-MetaAIUnveilsLLMCompilerforAdvancedCodeandCompilerOptimization_0.png
tag: Tech
originalTitle: "Meta AI Unveils LLM Compiler for Advanced Code and Compiler Optimization"
link: "https://medium.com/syncedreview/meta-ai-unveils-llm-compiler-for-advanced-code-and-compiler-optimization-ce001c32cce7"
isUpdated: true
---





마크다운 형식으로 이미지 태그를 변경하겠습니다.

![이미지](/assets/img/2024-07-09-MetaAIUnveilsLLMCompilerforAdvancedCodeandCompilerOptimization_0.png)

언어 모델(Large Language Models 또는 LLMs)을 활용하여 코드 생성, 코드 번역 및 코드 테스트와 같은 소프트웨어 엔지니어링 작업에 대한 관심이 커지고 있습니다. 그러나 코드 및 컴파일러 최적화 분야에서의 이러한 모델 적용은 비교적 탐구되지 않았습니다. 또한, LLMs를 훈련하는 것은 상당한 컴퓨팅 및 데이터 비용을 요구하여 이 분야에서는 도전적인 과제가 있습니다.

이 문제를 해결하기 위해 Meta AI 연구팀이 새 논문에서 Meta Large Language Model Compiler: Foundation Models of Compiler Optimization을 소개하며, Meta Large Language Model Compiler (LLM Compiler)라는 견고하고 공개되어 있는 미리 훈련된 모델 스위트를 소개하였습니다. 이 모델은 코드 최적화 작업을 목적으로 특별히 설계되어 있으며, 컴파일러 최적화 분야에서의 추가 연구 및 개발을 위한 확장 가능하고 비용 효율적인 기반을 제공할 것입니다.

![이미지](/assets/img/2024-07-09-MetaAIUnveilsLLMCompilerforAdvancedCodeandCompilerOptimization_1.png)

<div class="content-ad"></div>

The LLM Compiler는 컴파일러 중간 표현 (IRs) 및 어셈블리의 의미론을 이해하기 위해 훈련된 기본 모델로 구성되어 있습니다. 이 모델들은 컴파일러를 흉내내며 특정 하향식 컴파일러 최적화 작업을 위해 수행할 수 있습니다. LLM Compiler 모델은 전문화된 버전으로, 추가 데이터 없이도 특정 하향식 컴파일러 최적화 작업을 위한 세부 조정이 가능합니다.