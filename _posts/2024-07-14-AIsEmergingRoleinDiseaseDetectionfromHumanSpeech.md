---
title: "AI로 인간 음성에서 질병을 감지하는 방법"
description: ""
coverImage: "/assets/img/2024-07-14-AIsEmergingRoleinDiseaseDetectionfromHumanSpeech_0.png"
date: 2024-07-14 01:48
ogImage:
  url: /assets/img/2024-07-14-AIsEmergingRoleinDiseaseDetectionfromHumanSpeech_0.png
tag: Tech
originalTitle: "AI’s Emerging Role in Disease Detection from Human Speech"
link: "https://medium.com/towards-data-science/ais-emerging-role-in-disease-detection-from-human-speech-f24e6d49d25c"
isUpdated: true
---

## |인공지능| 건강| 음성|

![Artificial Intelligence in Healthcare](/assets/img/2024-07-14-AIsEmergingRoleinDiseaseDetectionfromHumanSpeech_0.png)

미래 의학은 침투적 검사 없이 진단하고 원격으로 환자를 모니터링하는 것을 허용할 것입니다. 인공지능은 이러한 미래의 필수 요소입니다.

인공지능을 통한 진단을 떠올릴 때, 많은 사람들은 의료 이미징을 떠올립니다. 한 예로는 스마트폰으로 촬영된 사진을 이용해 AI 알고리즘으로 분석하여 잠재적으로 암 성분이 든 주근깨를 진단하는 것이 있습니다. 사람들은 주로 음성으로 소통합니다. 인간의 목소리에는 그들의 말의 내용을 넘어가는 정보들이 풍부하게 담겨 있습니다.

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

이 글은 정확히 그것을 논의합니다. 왜 그것이 중요한지, 왜 어려운지, 이미 무엇이 이루어졌는지, 그리고 어떤 어려움이 있었는지에 대해 이야기할 것입니다. 또한 최근의 발전과 미래 전망에 대해 이야기할 것입니다. 참고문헌 목록은 맨 끝에 있습니다.

# 의료 분야의 인공지능의 새로운 지평

![AI](/assets/img/2024-07-14-AIsEmergingRoleinDiseaseDetectionfromHumanSpeech_1.png)

인공지능 모델이 향상되면서 연구자들의 관심은 잠재적인 응용 영향으로 향했습니다. 잠재적인 의학적 응용 분야에도 어느 정도 주목이 기울어졌습니다. 이는 잠재적인 경제적 및 사회적 영향 뿐만 아니라 데이터의 풍부함 때문입니다. 실제로 의료 수사 중에 의료노트뿐만 아니라 의학 영상 (X-선, CT 스캔 등) 및 검사 결과(유전체 시퀀싱, 혈액 분석 등)도 생성됩니다. 그러나 이러한 데이터는 복잡하며 허용되는 오차 한도가 작기 때문에 병원에서의 배포가 지연되고 있습니다.

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

AI에 대한 흥미의 다른 이유는 침습적 진단 테스트 없이 진단을 돕는 데 사용될 수 있다는 것입니다. 실제로, 혈액검사나 다른 의학 검사는 여전히 환자에게 위험을 초래할 수 있습니다. 게다가 환자는 병원이나 다른 전문 센터로 이동해야 합니다.

침습 검사 없이 환자 데이터 분석을 위한 AI 모델 개발에서 두 가지 주요 단계를 강조할 수 있습니다. 첫 번째 단계에서는 환자로부터 얻은 이미지의 분석을 위해 합성곱 신경망을 활용했습니다. 예를 들어 Google과 다른 연구 그룹은 눈이나 안저의 사진을 진닝과 여러 질병을 진단하는 데 초점을 맞췄습니다.

두 번째 단계에서는 대규모 언어 모델(LLMs)의 성공을 활용하여 인간 언어를 분석하고 텍스트 문제로부터 환자를 진단하거나 의학 기록에 대한 추론을 수행할 수 있습니다.

실제로, 음성은 데이터의 풍부한 원천입니다: 의미적 콘텐츠만 있는 것이 아니라 감정, 발화, 공명 및 발성의 전달도 있습니다. 진단 조사를 위해 단순히 의미적 콘텐츠뿐만 아니라 (전사로 분석될 수 있는) 전체적인 음향 요소가 풍부한 정보를 제공합니다.

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

이러한 신호들은 의사가 소아의 기침 소리를 활용하여 푸티교통으로 진단하듯이 진단을 얻는 데 중요합니다. 게다가, 음성이나 기침은 휴대전화나 기타 저가의 장치를 이용하여 녹음될 수도 있습니다. 이를 통해 저소득 국가나 의료 서비스에 한정된 농촌 지역에서도 진단에 접근할 수 있게 될 수도 있습니다. 그러나 이러한 데이터는 처리가 어렵고 정보 손실로 이어질 수 있는 특정 전처리가 필요합니다.

그럼에도 불구하고, 환자가 생산하는 음향 요소를 활용하여 진단을 위해 개발된 모델의 예는 몇 가지 있습니다. 예를 들어, 연구자들은 푸티교통을 감지하는 알고리즘을 개발했습니다(이 질병은 치료를 받지 않을 경우 매년 20만 명의 사망을 일으킵니다). 연구자들의 모델은 음성 녹음으로부터 이 질병을 진단할 수 있는데, 그 어떠한 거짓 진단도 없습니다. 전처리 이후에 로지스틱 회귀 모델을 활용하여 푸티교통의 기침 소리를 식별할 수 있도록 합니다.

![Link to the image](/assets/img/2024-07-14-AIsEmergingRoleinDiseaseDetectionfromHumanSpeech_2.png)

이로 인해 연구자들은 이 연구 분야를 결핵, COVID-19, 천식, 기관지염 등 다양한 호흡기 질환으로 확장시키게 되었습니다. 게다가, 딥러닝의 도래로 음성 데이터로부터 복잡한 표현뿐만 아니라 의미 있는 특징을 추출할 수 있다는 것이 입증되었습니다. 이 작업의 많은 부분은 스펙트로그램에 합성곱 신경망을 활용하는 것을 기반으로 하고 있습니다. 이 접근 방법은 다양한 응용 분야에서 여러 성공을 거두었습니다.

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

![AI's Emerging Role in Disease Detection from Human Speech](/assets/img/2024-07-14-AIsEmergingRoleinDiseaseDetectionfromHumanSpeech_3.png)

안녕하세요! 이 모델들이 환자의 목소리에서 치매 초기 증상을 감지할 수 있도록 활용되어 왔다는 것은 몀에게는 덜 명백해 보일 수 있지만 실은 사실입니다. 이 연구에서는 환자의 목소리 녹음에서 치매 증상을 진단하는 것이 가능한지를 보여주었습니다. 저자들은 환자 목소리 녹음에 합성곱 신경망을 적용했습니다.

이러한 결과들은 매우 희망적입니다. 왜냐하면 치매와 같은 질병들은 PET 아밀로이드 뇌 스캔 또는 MRI와 같은 비용이 많이 드는 검사가 필요하기 때문입니다. 게다가 이러한 진단은 어렵고 실제로 사후 사망 검사 후에야 진정으로 확인할 수 있습니다.

이러한 모델들의 주요 문제는 과업별이며 분포 밖에서의 일반화에 실패한다는 점입니다. 실제로 이러한 데이터셋들은 종종 소규모 데이터셋에 학습되며 다른 환경에서 적용할 때 대 실패합니다. 의료 데이터셋을 얻는 것은 비용이 많이 들 뿐만 아니라 주석 작업 자체가 번거롭고 특수 의료인력이 필요합니다.

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

예를 들어, 한 병원에서 얻은 데이터로 훈련된 모델은 종종 다른 병원에서 얻은 데이터에 적용할 때 실패합니다. 수집 프로토콜, 기기 및 직원의 주의가 서로 다릅니다. 이로 인해 연구에서 얻은 결과가 재현 가능하지 않으며 모델을 다시 훈련해야 합니다.

![Image](/assets/img/2024-07-14-AIsEmergingRoleinDiseaseDetectionfromHumanSpeech_4.png)

최근 몇 년간, 자가 지도 학습(Self-supervised learning, SSL)은 대규모 미태깅 데이터를 사용하여 학습할 때 데이터의 일반적인 표현을 배우는 데 성공한 것으로 나타났습니다(프리트레이닝된 트랜스포머 등). 트랜스포머의 성공은 이미지나 텍스트와 같은 데이터의 범용적 표현을 학습하는 데 있습니다. 따라서 동일한 원칙을 음성 데이터에 적용할 수 있습니다.

물론, 두 가지 주요 문제가 있습니다:

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

**음성 질병 탐지를 위한 기반 모델**

![이미지](/assets/img/2024-07-14-AIsEmergingRoleinDiseaseDetectionfromHumanSpeech_5.png)

오디오 등록에 SSL을 적용하는 애플리케이션은 이미 이전 연구에서 테스트되었습니다. 이 연구의 저자들은 모델을 활용하여 데이터의 특징 (데이터의 표현)을 추출할 수 있다는 것을 보여주었습니다. 그러나 이미지와 텍스트에 대해서는 대규모, 일반적인 데이터셋이 부족하다는 저자들의 주된 제한 사항을 언급하며, 이는 새로운 작업과 생명 과학 분야에서의 잠재적인 응용에 일반화할 수 있는 능력을 손상시킵니다.

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

최근 구글 연구원들이 인간 레코드의 대규모 말뭉치를 사용하여 비지도 학습된 모델을 소개했습니다. 이 모델은 새로운 분포와 새로운 데이터에 대해 더 잘 일반화할 수 있습니다.

![2014-07-14-AIsEmergingRoleinDiseaseDetectionfromHumanSpeech_6.png](/assets/img/2024-07-14-AIsEmergingRoleinDiseaseDetectionfromHumanSpeech_6.png)

이전에 보았던 대부분의 모델은 데이터와 레이블(지도 학습)에 훈련되었지만, 이 모델은 대신 대규모 음원 말뭉치를 훈련했습니다. 구글은 유튜브에서 사용 가능한 영상을 통해 기침, 호흡, 목을 해원하는 등 다양한 인간 소리의 2초 짧은 소리 클립 3억 개를 수집하고 추출했습니다. 저자들은 플랫폼에서 제공하는 영상에서 총 17만 4천개 이상의 인간 음원을 추출했어요.

![2014-07-14-AIsEmergingRoleinDiseaseDetectionfromHumanSpeech_7.png](/assets/img/2024-07-14-AIsEmergingRoleinDiseaseDetectionfromHumanSpeech_7.png)

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

물론, 이러한 작업을 수동으로 하는 것은 불가능하므로 시스템에는 저자들이 헬스 음향 이벤트 감지기라고 부르는 기능이 내장되어 있습니다. 오디오 클립을 취하면, 해당 소리는 스펙트로그램으로 변환됩니다. 그런 다음, 다중 레이블 분류 컨볼루션 신경망(CNN)을 사용하여 관심 있는 소리(기침, 아기 기침, 숨 쉬기, 목을 가다듬기, 웃음, 말하기)가 있는지 확인합니다.

![Image](/assets/img/2024-07-14-AIsEmergingRoleinDiseaseDetectionfromHumanSpeech_8.png)

그런 다음 SSL을 사용하여 시스템을 훈련시킵니다. 이제 LLM을 사용하여 단어 시퀀스에서 다음 단어를 예측하는 모델을 훈련합니다(언어 모델링). 다음 단어를 예측함으로써 LLM은 언어의 일반적인 규칙과 각 단어의 의미를 학습합니다 (충분한 데이터가 있다면 일반 규칙을 추출할 수 있습니다). 이를 소리에 어떻게 적용할 수 있을까요?

시스템은 스펙트로그램의 다음 부분을 예측하도록 훈련됩니다. 보다 자세히 말하면, 스펙트로그램을 특정 부분으로 분할하고 모델이 빠진 부분을 예측해야 합니다. LLM 훈련과 유사하게, 모델은 말 속의 숨겨진 규칙을 학습하고 있습니다.

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

한 번 필트레이트를 획득하면, 시스템은 오디오 인코더를 활용하여 벡터 표현(임베딩)으로 변환합니다. 소리의 표현을 학습할 수 있는 트랜스포머를 사용하여 이 작업이 수행됩니다. 소리의 이 표현이 얻어지면, 저자들은 하류 작업을 위해 선형 또는 로지스틱 회귀를 활용합니다.

다시 말해, 이 모델은 사람 소리의 표현을 감독되지 않은 방식으로 학습합니다. 이 표현은 이후 하류 작업에 사용될 수 있습니다. 모델은 특징을 추출하고, 그 다음에는 선형 프로빙(이러한 특징에 대한 선형 모델)을 사용하여 특정 작업을 수행할 수 있습니다.

사실, 저자들은 모델의 성능을 평가하기 위해 특정 데이터셋을 수집했습니다. 이러한 데이터셋에는 COVID-19 또는 결핵과 같은 다양한 질병을 가진 개인들의 기록이 포함되어 있지만, 흡연 상태나 BMI 식별과 같은 다른 작업에 활용될 수도 있습니다.

![AI's Emerging Role in Disease Detection from Human Speech](/assets/img/2024-07-14-AIsEmergingRoleinDiseaseDetectionfromHumanSpeech_9.png)

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

The authors have assigned various tasks in their study:

- 13 health-related audio recognition tasks.
- 14 cough-related tasks, focused on detecting abnormalities in X-rays, diagnosing Covid-19, and identifying lifestyle factors like smoking, sex, age, and BMI.
- 6 spirometry tasks, including estimating forced expiratory volume and total exhalation duration.

It is worth mentioning that these tasks involve both classification and regression aspects. Additionally, some tasks typically involve specific instrument measurements (e.g., spirometry), but in this study, the authors rely solely on audio recordings.

The authors evaluated their system (HeAR) against cutting-edge audio encoders. The results showed that, especially for cough and spirometry tasks, their model outperformed other existing models significantly. It's important to note that a score of 0.5 indicates random guessing, revealing that previous state-of-the-art models performed poorly in some health-related tasks.

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

<img src="/assets/img/2024-07-14-AIsEmergingRoleinDiseaseDetectionfromHumanSpeech_10.png" />

<img src="/assets/img/2024-07-14-AIsEmergingRoleinDiseaseDetectionfromHumanSpeech_11.png" />

# 마무리로

올해 오디오와 비디오가 분명히 새로운 지평을 열고 있습니다. 한편으로는 OpenAI Sora가 비디오를 생성하는 모델들이 점점 능력이 향상되고 있다는 것을 입증했습니다. 다른 한편으로는 음성 분석에 대한 새롭고 강한 관심이 존재합니다. 지금까지 이러한 유형의 데이터는 SSL 혁명의 뒤쳐진 부분이었습니다. 이는 보다 복잡한 분석과 어려운 전처리가 필요하기 때문입니다. 그러나 이제 상황이 바뀌고 있습니다.

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

지금까지 모델을 음성 분석에 적용하는 것은 특정 작업으로 제한되어 있었습니다. 게다가, 이러한 모델들은 일반화가 잘 되지 않아 적용 범위가 한정적이었습니다. Google의 최신 논문에서는 대규모 데이터를 이용하여 볼 수 없는 작업에도 일반화할 수 있는 모델이 만들어질 수 있다는 방법을 제시했습니다. 이것은 SSL을 효율적으로 만드는 데 썼기 때문에 가능했습니다.

이 연구는 매우 흥미로운 전망을 열었습니다. 첫째, 모델이 적은 데이터만 사용 가능한 경우에도 작업을 일반화할 수 있다는 점을 보여줍니다(건강 연구에서 흔한 경우). 사실, 전이 학습 또는 기반 모델의 사용이 의료 이미징 연구를 변화시켰습니다. 음성을 위한 기반 모델도 같은 영향을 미칠 수 있습니다.

둘째, 오디오 기록은 침투적이지 않은 검사이며 휴대전화와 같은 간단한 장치로 어디서든 수집할 수 있습니다. 이를 통해 원격 지역 또는 개발도상국에 있는 환자들에게도 진단 서비스에 접근할 수 있을 것입니다.

비침습적 진단은 의학 분야에 혁명을 일으킬 것이며, 원격 지역에 있는 환자의 신속하고 지속적인 분석이 가능할 것입니다. 게다가 청취 장치를 통해 치명적인 질병의 초기 증상을 사전에 감지할 수 있으며 환자가 적시에 치료를 받을 시간을 제공할 수 있을 것입니다(호흡 위기, 심근경색 등의 증상). 그러나 이 모든 것은 인공지능이 필요합니다.

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

# 만약 이 글이 흥미로웠다면:

제 다른 글들도 읽어보세요. 또한 LinkedIn에서 저와 연락하거나 저에게 도전해보세요. 매주 업데이트되는 기계 학습 및 인공 지능 뉴스를 담고 있는 저장소를 확인해보세요. 협업 및 프로젝트에 열려있으며 LinkedIn에서 저에게 연락할 수 있습니다. 제 새로운 이야기를 게시할 때 알림을 받고 싶다면 무료로 구독할 수도 있습니다.

여기 제 GitHub 저장소 링크가 있습니다. 거기에는 기계 학습, 인공 지능 등과 관련된 코드와 수많은 자료들을 모아두고 있습니다.

또는 최근 제 글 중 하나에 관심이 있을 수도 있습니다:

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

# 레퍼런스

이 글을 작성할 때 참고한 주요 레퍼런스 목록을 소개합니다. 각 항목에는 기사의 첫 번째 저자의 성만을 인용했습니다.

- Shor, 2021, Universal Paralinguistic Speech Representations Using Self-Supervised Conformers, [링크](link)
- Pramono, 2016, A Cough-Based Algorithm for Automatic Diagnosis of Pertussis, [링크](link)
- Laguarta, COVID-19 Artificial Intelligence Diagnosis Using Only Cough Recordings, 2020, [링크](link)
- Laguarta, 2021, Longitudinal Speech Biomarkers for Automated Alzheimer’s Detection, [링크](link)
- D’amour, 2022, Underspecification Presents Challenges for Credibility in Modern Machine Learning, [링크](link)
- Baur, 2024, HeAR — Health Acoustic Representations, [링크](link)
- De la Fuerte Garcia, 2020, Artificial Intelligence, Speech, and Language Processing Approaches to Monitoring Alzheimer’s Disease: A Systematic Review, [링크](link)
- Haridas, 2018, A critical review and analysis on techniques of speech recognition: The road ahead, [링크](link)
