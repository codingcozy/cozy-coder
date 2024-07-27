---
title: "머신 러닝으로 성범죄를 예방하는 방법"
description: ""
coverImage: "/assets/img/2024-07-09-UsingMachineLearningtoCombatSexualAbuse_0.png"
date: 2024-07-09 11:35
ogImage: 
  url: /assets/img/2024-07-09-UsingMachineLearningtoCombatSexualAbuse_0.png
tag: Tech
originalTitle: "Using Machine Learning to Combat Sexual Abuse"
link: "https://medium.com/ai-in-plain-english/using-machine-learning-to-combat-sexual-abuse-83ef11ff94ed"
---


안녕하세요! 어린 시절은 즐거움, 탐험, 그리고 성장의 시기로 존재합니다. 하지만 너무 많은 어린이들에게는 성학대의 고통스러운 경험이 이 시기의 즐거움을 빼앗아갑니다. 성학대가 사진과 동영상으로 기록되고 다른 사람들과 공유될 때, 해로운 사이클은 계속됩니다. 이러한 콘텐츠를 아동 성학대 자료(CSAM)라고 합니다.

요즘에는 생성적 AI 도구가 접근하기 쉬워지면서 CSAM의 확산이 가속화되고 있습니다.

핸즈온 성학대를 복구한지 여러 년이 지난 후에도 매년 수천 번씩 성학대가 공유되어온 생존자들이 존재합니다. 이 이미지가 공유될 때마다 피해자는 다시 성폭행당합니다. 대중에게 널리 알려진 CSAM에서 식별된 아동 피해자의 과반이 성조기 전의 어린이들입니다. 그들은 말할 수조차 없는 어린 나이일 수도 있습니다!

<div class="content-ad"></div>

오늘은 기계 학습과 인공 지능이 여러 분야에서 도움을 주고 있다는 것을 보실 수 있습니다. 연구부터 예술까지, 미디어 제작부터 엔터테인먼트까지 기술이 큰 파장을 일으키고 있습니다. 그러나 스탠포드 대학의 Women In Data Science (WiDS) 컨퍼런스에 참석한 후 기술이 성적 학대와의 싸움에도 사용될 수 있다는 것을 배웠습니다.

# Thorn - ML/AI를 활용한 성적 학대 대응

Thorn은 아동들을 인터넷 상에서의 성적 학대로부터 보호함으로써 더 안전한 세상을 만들고자 하는 비영리 기구입니다. Thorn은 아동 성적 학대 자료(CSAM) Classifier를 개발했는데, 이를 통해 CSAM 이미지/동영상을 예측하고 제거를 위해 플래그 처리할 수 있습니다.

인기 있는 콘텐츠 호스팅 플랫폼인 Flickr도 Safer - Thorn의 CSAM Classifier 통합 버전을 사용하고 있습니다. 사용자가 업로드한 이전에 감지되지 않은 2000개의 파일을 Safer가 시간내에 CSAM으로 플래그 처리했습니다. 이 사례는 그 후에 수사 기관에 소속된 당국에 이송되었는데, 그 기관은 수사를 시작하여 피해자를 찾아 구조를 하고 가해자를 체포했습니다. 이는 Thorn과 기술 전반에 대한 큰 승리였습니다.

<div class="content-ad"></div>

![How Thorn’s CSAM Classifier Works](/assets/img/2024-07-09-UsingMachineLearningtoCombatSexualAbuse_1.png)

안녕하세요! 오늘은 'Thorn의 CSAM Classifier가 어떻게 작동하는지'에 대해 이야기하려고 해요. CSAM Classifier는 ML 기반의 모델로, 인터넷 상에서 CSAM을 감지하는데 사용돼요. 사전 훈련된 데이터에 기반하여 작동하는데, 개선된 효율성을 위해 대규모 데이터셋으로 꾸준한 훈련과 재훈련이 필요해요.

![How Thorn’s CSAM Classifier Works](/assets/img/2024-07-09-UsingMachineLearningtoCombatSexualAbuse_2.png)

<div class="content-ad"></div>

## 첫 번째로, 데이터셋은 훈련 및 테스트 세트로 나누어집니다. 훈련 세트는 감독 모델을 통해 전달되고 테스트 세트는 예측 모델을 통해 전달됩니다. 이후 이러한 결과는 분류 결과로 정의되어 평가 단계를 거칩니다.

만일 분류기가 없다면, 사례가 수동으로 보고되기까지 몇 달이 걸릴 수 있습니다. 실제로 수동으로 검열을 하면 많은 사례가 간과될 수 있습니다.

### Thorn의 분류기에 대한 도전 과제

- 아동학대 및 성착취물(CSAM) 데이터 획득 및 저장

<div class="content-ad"></div>

To create a powerful ML model, you need a substantial amount of data. However, in Thorn's case, this can be a bit tricky because storing CSAM data is illegal. They had to collaborate with relevant authorities and seek legal assistance to navigate this challenge.

**AI-Generated Content**

Perpetrators tend to be quick to adopt new technologies. There's a risk they might exploit advancements to produce inappropriate deepfakes involving children, making it harder to identify and prevent such crimes.

**# Thorn’s Achievements**

<div class="content-ad"></div>

- CSAM Classifier 사용자들은 32%의 응급 처리 시간을 절약했습니다.
- 2019년 이후 고객 플랫폼에서 2.8백만개 이상의 잠재적인 CSAM 파일이 식별되었습니다.
- 수백 개의 법 집행 기관, 신고 전화센터 및 기술 플랫폼이 CSAM 분류기를 채택하여 전 세계의 많은 어린이들의 생명을 구하는 데 도움을 주었습니다.
- 사용자 피드백을 통해 모델을 개선하였으며, 거짓 양성, 거짓 음성 등에 대한 피드백도 반영되었습니다.

소형은 AI를 사회에 도움이되도록 사용하는 좋은 예입니다. 우리가 발전함에 따라 앞으로의 기술 발전에 대해 스스로를 최신으로 유지하고, 오늘날 세대를 위한 더 안전한 환경 조성을 고민해보아야 합니다.

# Stackademic 🚀

가기 전에:

<div class="content-ad"></div>

- 반드시 작가를 박수로 응원해 주시고 팔로우해주세요! 👏
- 팔로우하기: X | LinkedIn | YouTube | Discord | 뉴스레터
- 다른 플랫폼도 방문해 주세요: CoFeed | Differ | PlainEnglish.io
- 더 많은 콘텐츠는 stackdemic.com에서 확인하세요.