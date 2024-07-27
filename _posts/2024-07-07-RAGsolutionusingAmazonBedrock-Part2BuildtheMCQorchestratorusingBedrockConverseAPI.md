---
title: "Amazon Bedrock를 사용한 RAG 솔루션 - Part 2 Bedrock Converse API로 MCQ 오케스트레이터 구축하는 방법"
description: ""
coverImage: "/assets/img/2024-07-07-RAGsolutionusingAmazonBedrock-Part2BuildtheMCQorchestratorusingBedrockConverseAPI_0.png"
date: 2024-07-07 20:27
ogImage: 
  url: /assets/img/2024-07-07-RAGsolutionusingAmazonBedrock-Part2BuildtheMCQorchestratorusingBedrockConverseAPI_0.png
tag: Tech
originalTitle: "RAG solution using Amazon Bedrock - Part 2: Build the MCQ orchestrator using Bedrock Converse API"
link: "https://medium.com/@vivek-aws/rag-solution-using-amazon-bedrock-part-2-build-the-mcq-orchestrator-using-bedrock-converse-api-61c2b2ce3f20"
---


Generative AI 데모는 Well-Architected Machine Learning Lens PDF를 활용하여 Machine Learning Engineer Associate (MLA-C01) 자격증 시험을 준비하는 데 도움이 됩니다.

이 시리즈의 첫 번째 부분에서는 Amazon OpenSearch Serverless를 AWS CDK를 사용하여 설정하여, Retrieval-Augmented Generation (RAG) 솔루션을 위한 벡터 저장소를 구축했습니다. 두 번째 부분에서는 Amazon Bedrock의 Converse API를 활용하여 Multiple Choice Questions (MCQ) orchestrator를 구축할 것입니다. 이 orchestrator는 Well-Architected Machine Learning Lens PDF를 기반으로 MCQ를 생성하고 평가하여, AWS Machine Learning Engineer Associate (MLA-C01) 자격증 시험을 준비하는 데 도움이 됩니다.

![Image](/assets/img/2024-07-07-RAGsolutionusingAmazonBedrock-Part2BuildtheMCQorchestratorusingBedrockConverseAPI_0.png)

## 필수 준비 사항

<div class="content-ad"></div>

- AWS 계정
- AWS CLI
- NodeJS
- Python
- AWS CDK
- 다음과 같이 CDK를 부트스트랩하세요: `cdk bootstrap aws://ACCOUNT-NUMBER/REGION`
- Visual Studio Code (또는 선호하는 편집기)
- Amazon Bedrock 접근

## CDK 프로젝트 구현

AWS CDK가 설치되어 있고 사전 준비 조건에 따라 초기화되었는지 확인하세요.

이 프로젝트의 전체 소스 코드는 GitHub 레포지토리에서 찾을 수 있습니다. 릴리스 브랜치 v0.2.0을 사용하여 다음 명령을 사용하여 저장소를 복제하세요.

<div class="content-ad"></div>


git clone -b v0.2.0 https://github.com/awsdataarchitect/opensearch-bedrock-rag-cdk.git && cd opensearch-bedrock-rag-cdk
npm i


이전 포스트의 첫 부분에서 설명한 대로 CDK 프로젝트에서 AOSS 클러스터를 배포했는지 확인하세요.

## 이번 릴리스의 주요 구성 요소 설명:

- app.py: 이 Streamlit 애플리케이션은 사용자 입력을 가져와 answer_query 함수로 보내고 생성된 객관식 문제를 표시합니다.
- query_against_openSearch.py: 이 스크립트는 Amazon Bedrock과 통신하여 임베딩을 생성하고 OpenSearch에서 KNN 검색을 실행하며 Bedrock Converse API를 사용하여 객관식 문제를 생성하고 형식을 지정합니다.


<div class="content-ad"></div>

## Key Points

- **Embedding Generation:** The `get_embedding` function utilizes Amazon Bedrock to create embeddings for the input text.
- **KNN Search:** Executes a KNN search on the OpenSearch vector index to locate similar documents.
- **Prompt Engineering:** Designs a prompt for generating multiple-choice questions (MCQs) based on the identified documents.
- **Conversation Orchestrator:** Utilizes the Bedrock Converse API to handle the conversation context and produce responses.

## Follow these steps to make use of the application

## Step 1: Set Up the Python Virtual Environment

<div class="content-ad"></div>

먼저 이 Proof of Concept (POC)의 루트 디렉토리에 Python 가상 환경을 설정하세요. Python 3.10을 사용하고 있는지 확인해주세요. 아래 명령어를 실행해보세요:

```js
pip install virtualenv
python3.10 -m venv venv
```

가상 환경을 설정하는 것은 의존성을 관리하고 다양한 개발 환경 간 일관성을 유지하는 데 매우 중요합니다. 가상 환경 설정에 대해 더 많은 설명이 필요하다면 관련 자료나 문서를 참고해보세요.

가상 환경을 만든 후 다음 명령어로 활성화하세요:

<div class="content-ad"></div>

**스텝 2: 필수 패키지 설치**

가상 환경이 활성화되면, requirements.txt 파일에 나열된 필수 패키지를 설치해야 합니다. 이 명령을 POC의 루트 디렉토리에서 실행해주세요:

```bash
pip install -r requirements.txt
```

<div class="content-ad"></div>

## 단계 3: 환경 변수 구성하기

이제 환경 변수를 구성해 보세요. 리포지토리의 루트 디렉토리에 .env 파일을 생성하고 다음 라인을 추가하세요:

```js
profile_name=<CLI_profile_name>
```

Amazon Bedrock에 액세스할 수 있는 AWS CLI 프로필을 가지고 있는지 확인해 주세요.

<div class="content-ad"></div>

## 스텝 4: 애플리케이션 실행하기

저장소를 복제한 후, 가상 환경을 생성하고 활성화한 후 필요한 패키지를 설치하고 .env 파일을 설정했다면, 이제 애플리케이션을 실행할 준비가 끝났어요. 다음 명령어로 애플리케이션을 시작해보세요:

```js
streamlit run app.py
```

브라우저에서 애플리케이션이 정상적으로 작동하면, 질문을 시작하고 자연어 응답을 생성할 수 있어요. 아마존 베드락 대화 API가 대화 기록과 컨텍스트를 관리할 거에요.

<div class="content-ad"></div>

2024년 8월 13일에 AWS Machine Learning Engineer Associate (MLA-C01) 자격증의 공식 학습 안내서가 출시되면, 해당 안내서에서 주제 이름을 사용하여 MCQ 문제를 생성할 수 있습니다. 현재는 MLS-C01 (AWS Machine Learning Specialty Certification) 시험 안내서에서 "오프라인 및 온라인 모델 평가(A/B 테스팅) 수행" 주제를 사용했습니다. 스샷에서 확인할 수 있듯이:

![이미지](/assets/img/2024-07-07-RAGsolutionusingAmazonBedrock-Part2BuildtheMCQorchestratorusingBedrockConverseAPI_1.png)

Bedrock Converse API를 사용하여 샘플 주제에 대한 모든 질문을 생성했고, 저희 RAG 솔루션을 사용했습니다. 아래는 해당 주제를 토대로 생성된 질문들입니다:

질문 #1) 모델을 평가해야 하는 이유를 결정하는 데 다음 중 어느 것이 해당되지 않는가요?

① 모델이 오버피팅 또는 언더피팅인지 확인하기 위해서

② 모델이 민감한지 또는 특정한지 확인하기 위해서

③ 모델이 이전 모델보다 정확한지 확인하기 위해서

④ 모델이 무작위 추측보다 정확한지 확인하기 위해서

정답: ④

설명: 모델은 무작위 추측이 아닌 이전 모델보다 더 정확한지 확인하기 위해 평가되어야 합니다.

이와 같은 방식으로 나머지 질문들을 만들 수 있습니다. 부담 없이 질문이나 도움이 필요하신 경우 언제든지 문의해주세요. 감사합니다!

<div class="content-ad"></div>

## 응답 메타데이터 및 토큰 수 계속 기록하기

Converse 메서드는 API 호출에 대한 메타데이터도 반환합니다. 우리는 사용량 속성을 기록하여 입력 및 출력 토큰에 대한 세부 정보를 포함합니다. 이를 통해 API 호출에 대한 요금을 이해하는 데 도움이 됩니다. 지연 시간 속성은 Converse로의 호출 지연 시간을 밀리초 단위로 제공합니다.

Streamlit의 백그라운드 화면에서 다음과 유사한 응답을 보게 될 것입니다:

```js
usage: {'inputTokens': 579, 'outputTokens': 1600, 'totalTokens': 2179}
latencyMs: {'latencyMs': 34931}
```

<div class="content-ad"></div>

**안내: 이용 횟수는 앱에서 마지막으로 한 API 호출에 대한 것입니다. 이 토큰 수를 사용하여 API 호출 비용을 추적할 수 있습니다. 토큰 기반 가격 책정에 대한 자세한 내용은 Amazon Bedrock 가격 페이지에서 확인하십시오.**

---

# 정리

모든 리소스를 삭제하려면 단순히 `cdk destroy` 명령어를 실행하면 됩니다. 이 명령을 실행하여 정의된 리소스들이 완전히 제거되고 할당된 리소스가 해제되며 관련 비용이 제거됩니다.

---

# 결론

<div class="content-ad"></div>

이 시리즈의 이 부분에서는 Amazon Bedrock의 Converse API를 활용한 Streamlit 애플리케이션을 만들었습니다. 이 애플리케이션은 객관식 문제를 생성하고 평가하는데 도움이 될 것입니다. 이 MCQ 조정자는 AWS Machine Learning Engineer Associate (MLA-C01) 자격증 시험을 준비하는 후보자들이 Well-Architected Machine Learning Lens PDF를 기반으로 목표 지향적인 연습을 할 수 있도록 도와줄 것입니다. 이 시리즈의 다음 부분을 기대해 주세요. 거기에서 우리의 RAG 솔루션을 더 발전시킬 것입니다.