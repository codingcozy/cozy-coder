---
title: "NVIDIA 가이드 생성형 AI로 RAG 기반 엔터프라이즈 챗봇 안전하게 구축하는 방법 마스터하기"
description: ""
coverImage: "/assets/img/2024-07-14-MasteringEnterpriseChatbotsNVIDIAsGuidetoBuildingSecureRAG-BasedChatbotswithGenerativeAI_0.png"
date: 2024-07-14 01:22
ogImage: 
  url: /assets/img/2024-07-14-MasteringEnterpriseChatbotsNVIDIAsGuidetoBuildingSecureRAG-BasedChatbotswithGenerativeAI_0.png
tag: Tech
originalTitle: "Mastering Enterprise Chatbots: NVIDIA’s Guide to Building Secure RAG-Based Chatbots with Generative AI"
link: "https://medium.com/syncedreview/mastering-enterprise-chatbots-nvidias-guide-to-building-secure-rag-based-chatbots-with-af325052d805"
isUpdated: true
---





엔터프라이즈 챗봇은 생성 AI로 구동되며 직원 생산성을 향상시키는 유망한 도구로 빠르게 부상하고 있습니다. 이러한 챗봇을 구축하는 데 필요한 주요 기술 구성 요소에는 Retrieval Augmented Generation (RAG), Large Language Models (LLMs), 그리고 Langchain과 Llamaindex와 같은 조율 프레임워크가 포함됩니다. 그러나 성공적인 엔터프라이즈 챗봇을 만드는 것은 주로 RAG 파이프라인에 대한 정교한 엔지니어링이 필요하기 때문에 중요한 도전을 제기합니다.

NVIDIA 연구팀이 제시한 새 논문 'FACTS About Building Retrieval Augmented Generation-based Chatbots'에서는 강력하고 안전하며 엔터프라이즈 수준의 RAG 기반 챗봇을 만들기 위해 고안된 FACTS 프레임워크를 소개합니다.

![이미지](/assets/img/2024-07-14-MasteringEnterpriseChatbotsNVIDIAsGuidetoBuildingSecureRAG-BasedChatbotswithGenerativeAI_1.png)

<div class="content-ad"></div>

The FACTS mnemonic represents five critical dimensions for these chatbots: content freshness (F), architectures (A), cost economics of LLMs (C), testing (T), and security (S). This framework is built upon the team’s experience developing three chatbots at NVIDIA, including those for IT and HR benefits, company financial earnings, and general enterprise content.

![Image](/assets/img/2024-07-14-MasteringEnterpriseChatbotsNVIDIAsGuidetoBuildingSecureRAG-BasedChatbotswithGenerativeAI_2.png)

The researchers identify and elaborate on fifteen critical control points within RAG pipelines, providing strategies to enhance chatbot performance at each stage. They also offer practical techniques for handling complex queries, testing, and ensuring security. Additionally, the paper presents a reference architecture for implementing agentic architectures for complex query handling, strategies for testing and evaluating subjective query responses, and guidelines for managing document access control lists (ACLs) and security.

![Image](/assets/img/2024-07-14-MasteringEnterpriseChatbotsNVIDIAsGuidetoBuildingSecureRAG-BasedChatbotswithGenerativeAI_3.png)

<div class="content-ad"></div>

In addition, this paper discusses a reference architecture for a versatile generative-AI chatbot platform. It offers insights into crucial aspects and viable strategies for constructing secure and high-performing chatbots tailored for enterprises, thus serving as a valuable resource in the area. Through empirical evidence, it is evident that the developed chatbots are not only efficient and secure but also cost-effective.

The research paper titled "FACTS About Building Retrieval Augmented Generation-based Chatbots" can be found on arXiv.

Written by Hecate He | Edited by Chain Zhang