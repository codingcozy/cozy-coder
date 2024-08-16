---
title: "눈에 보이지 않는 경계를 넘어서 새로운 가능성의 세계를 발견하다"
description: ""
coverImage: "/assets/img/2024-07-09-BeyondtheBlindZone_0.png"
date: 2024-07-09 23:21
ogImage: 
  url: /assets/img/2024-07-09-BeyondtheBlindZone_0.png
tag: Tech
originalTitle: "Beyond the Blind Zone"
link: "https://medium.com/towards-data-science/beyond-the-blind-zone-706ba4b171c5"
isUpdated: true
---





## 딥러닝을 사용하여 레이더 공백 채우기

![image](https://miro.medium.com/v2/resize:fit:1400/1*rADKbgXmGjWkY-LfuiUiFA.gif)

# 개요

이 게시물에서는 레이더 블라인드 존의 이미지 인페인팅에 대한 최근 작업의 상세 내용을 살펴봅니다. 주요 과학 문제, 인페이 팅 기술, 모델 아키텍처 결정, 충실도 메트릭, 불확실성 등을 논의하고, 모델 설명 가능성(XAI)에 대한 분석을 마무리할 것입니다. 이 정보가 앞으로의 유사한 프로젝트를 계획할 때 도움이 되기를 희망하여 이 작업은 최근 미국 기상학회의 지구과학을 위한 인공지능(AIES)에서 발표되었으며 독자들이 추가 프로젝트 세부 사항을 확인할 수 있도록 권장합니다.(https://doi.org/10.1175/AIES-D-23-0063.1).

<div class="content-ad"></div>

# 동기부여

레이다 관측은 강수 예측과 같은 작업에 대한 강력한 정보원입니다. 이 시스템은 수백만 명의 사람들이 매일 자신의 삶을 계획하는 데 도움을 주며, 예측 정확성은 농업, 관광 및 야외 활동 산업에 큰 영향을 미칩니다. 그런데 이 시스템은 어떻게 작동할까요? 간단히 말하자면, 이 예측 모델들은 대기 중에서 낙하하는 빗물 입자들과 상호작용하는 레이다 신호의 전력 반사 측정치와 강수량을 관련시킵니다. 충분한 참조 데이터셋이 있으면 레이다 프로필에서 생성된 정보(일부 대기 상태 변수와 함께)를 가져와 지표 강우량을 추정할 수 있습니다.

![이미지](/assets/img/2024-07-09-BeyondtheBlindZone_0.png)

수평 방향 표면 레이다 기기와 달리 위성은 공간에 제약이 없으며 그들의 궤도로 인해 전 세계 강수 패턴에 대한 훨씬 더 풍부하고 포괄적인 시각을 제공할 수 있습니다. 그러나 표면 레이다와 달리 위성 기기는 지구 쪽을 향하므로 고유한 유형의 측정 문제가 있습니다: 레이다 블라인드 존. 그 이름에서 암시되는 대로, 블라인드 존은 위성이 직접 관측할 수 없는 레이다 프로필의 일부입니다. 레이다 신호가 지구 표면에 도달하면 지면에서의 반사물이 소음과 함께 포화된 신호를 생성하게 되어 관측되지 않습니다. 표면 및 우주 레이다(및 해당 블라인드 존) 간 비교는 설명한 바와 같이 다음의 그림에서 보여집니다.

<div class="content-ad"></div>

![BeyondtheBlindZone_1.png](/assets/img/2024-07-09-BeyondtheBlindZone_1.png)

앞이 보이지 않는 영역의 크기는 다양할 수 있지만, GPM, CloudSat과 같은 활성 우주 시스템에서 흔히 발생하는 문제입니다. 이는 향후 EarthCARE와 AOS와 같은 지구 관측 미션에서 불확실성 요인으로 남을 것입니다. 또한 이 지역은 전체 프로필의 소규모 세부 군집에 불과합니다 (예: CloudSat의 수직 범위 중 약 4% 뿐). 그러나 대기의 이 0-2km 지역에는 물이 내리는 클라우드가 많이 포함될 수 있습니다 (Lamer et al., 2020). 따라서 이 지역을 가리면, 우리는 눈의 상당량을 놓칠 가능성이 있습니다 (또는 virga의 경우에는 눈을 과대 추정할 수 있음), 이미 불확실한 지표 눈 적산 예측의 불확실성을 더 높일 수 있습니다.

이에 대해 어떻게 대처해야 할까요?

# 우리의 해결책

<div class="content-ad"></div>

마법사의 신의 눈은 이미지 재구성의 아이디어를 수십 년 동안 이미 사용해왔습니다. 이미지 복원(가족 사진의 스크래치 제거) 또는 물체 제거(지난 휴가 사진에서 관광객 제거)와 같은 것에 사용되었습니다(Guillemot and Le Meur, 2013). 원래 이러한 종류의 수리 작업은 훈련받은 예술가에 의해 수작업으로 비싼 비용으로 처리되었지요(그림 2). 그러나 이전 몇 년간 컴퓨터가 이 작업에 능숙하다는 것이 점점 더 명확해지고 있습니다(사람보다 훨씬 짧은 훈련 시간으로)!

![Image](/assets/img/2024-07-09-BeyondtheBlindZone_2.png)

이러한 기술의 초창기 버전인 구조 기반 선형 보간, 질감 합성 기반 에프로스 보간, 그리고 확산 기반 라플라스 또는 나비에-스톡스 보간과 같은 기술은 특정 맥락에서 잘 작동할 수 있으나 큰 간격을 처리할 때는 종종 한계에 부딪쳤습니다. 그림 4에서 이러한 기술 간의 차이를 볼 수 있습니다. 여기에서 코끼리 마스크가 이미지 중앙에 재구성된 모습입니다. 이러한 기술들은 대상 이미지 재구성 영역의 가장자리에서의 정보/패턴에 매우 의존하며, 장면의 전역적 맥락에서의 정보를 효과적으로 활용하여 지능적인 예측을 내리기 어려운 경우가 많습니다.

![Image](/assets/img/2024-07-09-BeyondtheBlindZone_3.png)

<div class="content-ad"></div>

요즘 몇십 년간 컴퓨터 비전 분야는 주로 컴퓨팅 파워의 발전과 더 효율적인 머신 러닝 기술 개발에 의해 번성해 왔어요. 예를 들어, OpenAI의 DALL-E나 Stability.ai의 Stable Diffusion과 같은 GAN(생성 적 적대 신경망)을 활용한 생성적 접근 방법이 최근 매우 인기를 끌었어요. 이러한 방법을 이용하면 단순한 텍스트 입력을 바탕으로 놀라울 정도로 현실적인 이미지를 생성할 수 있습니다. 

과거에 삭제된 부분을 보충하기 위해 비슷한 방법을 사용하려는 시도가 있었지만(Lugmayr 등, 2017; Geiss와 Hardin, 2021), 삭제 작업에 대한 현실감과 충실성 또는 안정성 사이의 상충관계가 존재합니다. 예를 들어, 사람 눈에는 멋지게 보이는 이미지 영역을 생성해도, 실제 픽셀 값은 참조 이미지와 비교했을 때 정확하지 않을 수 있으며 제공된 랜덤 노이즈/시드에 따라 상당히 다를 수 있어요. 하지만 이는 예상 가능한 일이죠. 이러한 기술은 그러한 제약 조건을 고려하여 구성된 것이 아니기 때문에 다른 목적이 존재하기 때문입니다.

![Image](/assets/img/2024-07-09-BeyondtheBlindZone_4.png)

그 대신, 이 연구에서는 다른 머신 러닝 아키텍처에 중점을 두고 있습니다: U-Net입니다. U-Net은 이미지를 입력 형태로 삼고 (대개) 동일한 차원의 출력을 생성하는 CNN(Convolutional Neural Networks) 클래스입니다. 이미지 세그멘테이션에 자주 사용되며, U-Net의 인코더-디코더 아키텍처는 모델이 이미지의 로컬 및 글로벌 특징을 학습할 수 있도록 해줍니다(이는 삭제 작업 중 이미지 내용을 올바르게 해석하는 데 매우 유용한 맥락입니다). 이 아키텍처를 사용하여 모델이 고공의 구름 내에 잠재적 특징을 학습하여 이전에 언급된 레이더 장애 구역 내에서 근 지표 반사도 데이터를 예측하도록 할 것입니다.

<div class="content-ad"></div>

# 데이터

이 프로젝트에서 사용된 데이터는 기본적으로 두 군데에서 수집되었습니다:

- ARM KaZR CloudSat에 맞춘 반사도
- ERA-5

두 데이터셋 모두 공개적으로 이용 가능하며 (크리에이티브 커먼즈 저작자표시 4.0 국제(CC BY 4.0)에서 관리됨) 북부 알래스카 미국의 North Slope와 Oliktok Point에 위치한 두 곳의 북극 지역에 대해 코로케이트되어 있습니다 (그림 6). 우리는 눈이 내리는 것에 주목하기 때문에 영하 2도 이하의 추운 기간 관측에 한정합니다. 또한 자동 상관관계로 인한 과적합을 피하기 위해 데이터를 연속적인 훈련/검증/테스트 청크로 분할하였으며 그림 6.c에서 보여지듯 자동상관함수를 완화하였습니다. ARM KaZR의 레이더 반사도 데이터와 ERA-5의 온도, 특정 습도, u-성분 바람 및 v-성분 바람을 사용하고 있습니다.

<div class="content-ad"></div>

![BeyondtheBlindZone_5](/assets/img/2024-07-09-BeyondtheBlindZone_5.png)

# 우리의 모델

이 작업에는 어떤 종류의 U-Net을 사용해야 할까요? 우리는 이 프로젝트에서 Zhou 등의 UNet++ 모델 및 Huang 등의 3Net+와 같은 다양한 U-Net을 실험해 보았습니다. 그렇지만 모델 아키텍처에 대해 잠시 이야기해 봅시다. 예를 들어, 왜 전통적인 U-Net보다 이러한 방법을 사용해야 할까요? 먼저 U-Net이 어떻게 작동하는지 살펴보겠습니다.

U-Net은 주로 3부분으로 구성된 것으로 생각됩니다. 인코더 경로, 병목층 및 디코더 경로입니다. 인코더는 처음에 고해상도 이미지를 가져와 일련의 합성곱 및 풀링 단계를 통해 공간 정보를 풍부한 특징 정보(이미지의 잠재적 컨텍스트에 대한 학습)로 교환합니다. 네트워크가 얼마나 깊은지에 따라, 이러한 잠재적 특징은 가장 낮은 차원의 병목층에서 가장 집중적으로 인코딩됩니다. 이 시점에서 이미지 배열은 원래 크기의 일부분만 남아 있을 수 있습니다. 공간 정보의 대부분을 잃은 상태이지만 모델은 주요 요소로 본 내용을 나타내는 임베딩 집합을 식별했습니다. 그런 다음 디코더 경로에서 역방향으로 진행됩니다. 병목층의 저해상도 특징 풍부한 배열들이 다운샘플링되어 특징 정보가 다시 공간 정보로 변환되기까지(원래 이미지와 동일한 차원을 갖는 최종 이미지로 결과)입니다.

<div class="content-ad"></div>

U-Net, U-Net++ 및 3Net+의 주요 차이점 중 하나는 각 변형이 건너뛰기 연결을 처리하는 방식입니다. (그림 7 참조). 건너뛰기 연결을 사용하면 이러한 모델이 인코더 경로를 따라 일부 데이터를 "건너뛰어" 디코더에서 사용할 수 있게 되어, 디코딩 중에 낮은 수준의 특징 정보를 전달하여 의미 있는 방식으로 수렴하는 더 안정적인 모델을 생산합니다. 예를 들어 일반 U-Net의 경우, 이러한 연결은 간단히 수축된 인코더 경로에서 특징 맵을 해당 수준의 확장 경로로 연결시키는 것입니다.

![image](/assets/img/2024-07-09-BeyondtheBlindZone_6.png)

UNet++은 최적의 구조의 깊이를 알 수 없는 기존 U-Net의 문제를 해결하려는 일련의 중첩 밀집 스킵 경로를 제시합니다. 인코더와 디코더 간에 직접 연결만 가지는 것이 아니라, UNet++는 여러 개의 스킵 경로를 갖습니다. 인코더의 특정 수준에 대해, 디코더의 모든 후속 수준으로 스킵 연결을 만들어 밀집한 연결 세트를 형성합니다. 이러한 중첩된 스킵 연결은 단순한 U-Net보다 더 효과적으로 다양한 의미 수준의 특징을 포착하고 융합시키는 데 고안되었지만, 모델 크기가 커지는(매개변수가 많은) 대가와 더 오래 걸리는 훈련 시간이 필요합니다.

3Net+는 이전 기술의 아이디어를 결합한 구조로, 최종 모델에서 사용되는 아키텍처입니다. 이 방법은 스킵 연결을 일련의 across-skip 연결(일반 U-Net과 유사함), inter-skip 연결 및 intra-skip 연결로 나눕니다. 이러한 inter- 및 intra- 스킵 연결은 낮은 수준의 세부 사항을 고수준 의미론적 특징과 적은 매개변수로 전체 스케일의 특징 맵에서 UNet++ 모델로 전달하면서 장면의 멀티스케일 특징을 최대한 활용합니다.

<div class="content-ad"></div>

![BeyondtheBlindZone](/assets/img/2024-07-09-BeyondtheBlindZone_7.png)

우리 모델은 각 디코더 레벨의 전체 규모 집계된 특징 맵에서 계층적 표현을 배우는 데 깊은 지도를 활용합니다. 이를 통해 모델은 장면의 보다 넓은 컨텍스트를 검토하여 블라인드 존 내에 구름을 올바르게 배치하는 방법을 배웁니다. 이후 섹션에서는 반사율만 사용하여 학습된 이 3Net+ 모델의 기량(여기서는 3+_1으로 표시)과 반사율 및 ERA5 데이터로 학습된 다른 버전(3+_5) 및 반복 초과 대입(REP) 및 평균 이동 (MAR) 방법을 사용한 두 가지 선형 인페인팅 기술을 비교할 것입니다. 이러한 구현 방법에 대한 자세한 내용은 저희의 AIES 논문을 참조해주시기 바랍니다.

## 정질

블라인드 존 재구성 정확성 모델 성능을 종합적으로 평가하기 위해 먼저 일부 일반적인 경우를 살펴본 후 전체 테스트 데이터셋에 대한 일반적인 통계 분석을 수행할 것입니다. 앞으로 보여지는 모든 결과는 보이지 않는 관측치 테스트 세트에서 엄격히 가져온 것임을 참고해주세요.

<div class="content-ad"></div>

## 사례 연구

REP, MAR, 3+ 1 및 3+ 5 모델에서 NSA 및 OLI로부터 얻은 입력된 블라인드 존 반사도 값 예시들과 왼쪽에서 두 번째 열에 있는 해당 타깃 KaZR VAP 제품(즉, 실제값)을 아래 그림 9/10에 보여드립니다.

이 첫 번째 예시 세트는 두 장소에서 공통적으로 나타나는 근처 표면 반사도 그라디언트 및 구름 간격 사례를 강조합니다. 검정 점선은 1.2 km 블라인드 존 임곗값(이 값보다 낮은 값은 각 모델에 의해 마스킹되고 재구성됨)을 나타내며 음영 영역은 입력된 U-Net 예측에서의 높은 불확실성 영역을 나타냅니다(이에 대한 자세한 내용은 몬테카를로 드롭아웃 섹션에서 설명하겠습니다).

![2024-07-09-BeyondtheBlindZone_8.png](/assets/img/2024-07-09-BeyondtheBlindZone_8.png)

<div class="content-ad"></div>

우리는 선형 모델(REP 및 MAR)이 심층적이고 동질적인 시스템에 대해 잘 작동하지만 더 복잡한 경우에는 성능이 부족하다는 것을 알게 됩니다. 또한, 맹점 임계 반사도에 의존하고 있기 때문에 REP와 MAR은 반사도 그레이디언트(수직 및 수평)를 놓치며, 일반적으로 이를 "유넷"을 사용하여 캡처합니다. 마지막으로 얕은 북극 혼합 상층구름 또한 "유넷"을 사용하여 해결할 수 있으며, 그림 10처럼 구름 갭과 비르가 사례도 해결할 수 있습니다. 이는 지면에 눈이 내릴 양에 상당한 영향을 미친다는 것이 흥미로울 정도입니다.

![Image](/assets/img/2024-07-09-BeyondtheBlindZone_9.png)

가장 도전적으로 정확하게 모델링해야 하는 경우는 희미한 반사도 프로필과 먼 구름이 있는 경우입니다. 예를 들어, 서로 다른 연도에 발생한 NSA의 두 유사한 케이스를 보십시오. 인간의 눈에는 두 위치 모두 매우 유사한 구름 구조를 가지고 있지만 한 곳은 맹점 근처에 얇은 지표 구름을 가지고 있고 다른 한 곳은 그렇지 않습니다. 선형 '인페인팅' 기술은 명백히 이곳에서 편안한 영역을 벗어나 있으며 항상 우리가 a)에서 본 것처럼 동일한 "구름 없음" 출력을 생성할 것입니다. 그러나 3+_5 모델은 이와 같은 사례에서도 여전히 구름 존재를 해결할 수 있으며, 이 경우 ERA-5로부터 추가 콘텍스트를 활용하여 해당 케이스의 대기 조건이 맹점 구름을 유발하는 것으로 이해합니다.

![Image](/assets/img/2024-07-09-BeyondtheBlindZone_10.png)

<div class="content-ad"></div>

## 강인함

Fig. 12 a)에서 볼 수 있듯이, U-Net PSD 곡선은 선형 방법보다 관측치에 더 가까워 보입니다. 또한, 비선형 재구성은 b)의 확률 밀도 플롯에 표시된 것처럼 더 현실적인 최하층 구름 에코 높이를 생성하여 구름 위치를 더 잘 파악하는 것으로 보입니다. 마지막으로, 전체 소맹구역 구조는 c)-g)에서 요약되는데, 선형 방법은 반사율의 일반적인 대규모 추세를 포착할 수 있지만 미세한 변동성은 포착하지 못하는 한편 있습니다. U-Net 모델에는 희미한 "-60 dBZ"의 "차가운" 편향이 있음을 언급해야 합니다. 이것은 이들이 훨씬 더 드문 고강도 강설 사건에 대해 "구름 없음"에 더 가까운 "안전한" 추정치를 생산하는 경향이 있기 때문입니다.

![Robustness](/assets/img/2024-07-09-BeyondtheBlindZone_11.png)

또한, 표면 강설 및 버가 사례의 예측을 향상시킴으로써 수문학적 영향을 줄이고 눈의 축적량에 대한 불확실성을 줄일 수 있습니다. 따라서, 모델을 사용하여 세 가지 케이스가 얼마나 재구성되는지를 확률 검출량 (POD)과 거짓 경보율 (FAR)을 사용하여 확인하는 검사를 수행해보았습니다:

<div class="content-ad"></div>

- 시각 제한 영역 내 구름 존재 여부 (즉, 어느 구름이 감지되었습니까)
- 얕은 눈 낙 (즉, 표면에는 눈이 내리지만 시각 제한 영역에서는 눈이 내려오지 않음)
- Virga (즉, 시각 제한 영역에서 눈이 감지되었지만 표면에서는 눈이 내리지 않음)

각각의 지표에 대한 중요 성공 지수(CSI)는 성능 다이어그램 아래(Fig. 13)에 표시되어 있으며, 3+_5 모델이 전반적으로 가장 우수했습니다. 얕은 눈 낙 사례는 일반적으로 복원하기 가장 어려웠는데, 이러한 경우들이 정확히 이루어지기 굉장히 까다로워 보였습니다 (Fig. 11).

![Explanation Image](/assets/img/2024-07-09-BeyondtheBlindZone_12.png)

# 설명 가능성

<div class="content-ad"></div>

신뢰할 수 있는 모델의 의사결정 프로세스를 강화하기 위해 우리는 설명 가능한 인공지능 (XAI) 테스트를 수행했습니다. 이러한 테스트는 모델 행동을 논리적 물리적 프로세스에 연결하여 모델에 대한 신뢰를 불러일으키고 미래의 검색 기능을 향상시키 위한 추가 통찰을 제공하기 위해 목표로 했습니다. 데이터에서 이전에 알려지지 않았던 연결에 대해 배울 수 있다면 매우 가치 있는 일일 것입니다! 각 XAI 방법은 의사결정 프로세스에 대한 약간 다른 "지역" 설명을 제공하므로 여러 테스트를 활용하여 더 견고한 이해를 얻는 것이 유용합니다.

## 피처 맵

우리가 고려한 첫 번째, 가장 기본적인 테스트는 피처/활성화 맵이었습니다. Fig. 8.b의 인코더 경로의 서로 다른 수준에서 reLU 활성화 값을 조사함으로써, 특정 입력에 대해 모델이 이미지에서 어디를 바라보고 있는지 대략적으로 파악할 수 있습니다. 아래의 Fig.14에서 볼 수 있듯이, 3+_5 모델의 e1 인코더 레이어는 일반적으로 구름 가장자리, 반사도 그래디언트, 그리고 맹점 임계값을 따라 살펴봅니다.

![Beyond the Blind Zone](/assets/img/2024-07-09-BeyondtheBlindZone_13.png)

<div class="content-ad"></div>

## 중요한 채널 제거

이 프로젝트에서 가장 큰 질문 중 하나는 ERA-5가 모델에 유용한 맥락을 제공하는지 여부였습니다. 예를 들어 반사도에만 의존하는 더 단순한 U-Net을 사용할 수 있다면(즉, 3+_1 모델), 더 효율적일 것입니다. 그러나 ERA-5로부터의 추가적인 대기 상태 변수가 복잡한 시스템을 보충하거나 채워 넣는 데 모델에 유용한 맥락을 제공한다면, 이보다 복잡한 모델을 사용하는 것이 타당할 수 있습니다.

이 경우에는 (레이더와 온도, 습도, u-풍 및 v-풍을 포함한) 입력이 소수뿐이므로 입출력 공간을 철저히 조사하여 정확성에 대한 이들의 주변 기여를 평가할 수 있습니다. 더 구체적으로는 이러한 채널 제거 방법은 제공된 입력으로부터 중요성의 주변 기여를 계산하기 위해 아래 공식(Eq. 1/ Eq. 2)을 사용합니다. 이 기술은 입력 사이의 잠재적인 비선형 상호작용을 고려하지 않음에 주의하십시오.

![이미지](/assets/img/2024-07-09-BeyondtheBlindZone_14.png)

<div class="content-ad"></div>

25회 에폭을 거친 이 시험들을 실시하고 검증 손실의 변화를 살펴보면, 어떤 입력값이 가장 유용한지에 대한 대략적인 통찰을 얻을 수 있습니다. 이러한 시험 결과는 아래 그림 15에서 보여지며, 여기서 우리는 더 많은 ERA-5 입력값을 추가함에 따라 검증 손실이 감소하는 경향을 주목합니다 (이는 입력값 중 어느 하나도 완전히 중요하지 않다는 것을 시사합니다). 더불어, 검증 손실에 대한 가장 작은 기여들은 풍 데이터가 전반적으로 가장 영향력이 크다는 것을 시사합니다. 우리는 상층대류권에서 바람 패턴이 고기압, 저기압 시스템, 전선 및 제트 기류와 같은 중규모 대기 역학의 존재를 암시할 수 있다는 사실에서 이 중요성이 기인할 것으로 믿습니다.

![그림 15](/assets/img/2024-07-09-BeyondtheBlindZone_15.png)

## 강조도 맵

마지막으로, 우리는 3+_5와 3+_1 모델 간의 중요성 차이를 더 자세히 비교하기 위해 일부 사례에 대한 강조도 맵을 조사했습니다 (그림 16). 이 픽셀 속성 바닐라 그라디언트 강조도 맵은 Simonyan et al., 2014의 연구에서 영감을 받아서 생성되었으며, 주어진 입력을 사용하여 들어내기 정확도에 대한 정보의 중요한 기여 요소를 모델이 식별한 영역에 대한 추가적인 통찰을 제공합니다. 이 강조도 맵은 이미지를 네트워크를 통해 실행하고 그 출력의 그라디언트를 모든 채널에서 입력에 기반하여 추출함으로써 생성됩니다. 단순하지만 이 방법은 관찰된 이미지의 어느 부분이 식별된 이미지의 블라인드 존 반사 값에 따른 들어내기에서 가장 가치 있는지를 시각화하는 데 특히 유용하며, 활성화 그래디언트를 직접 그림 그리기를 통해 가능하게 합니다.

<div class="content-ad"></div>

![BeyondtheBlindZone_16](https://www.example.com/assets/img/2024-07-09-BeyondtheBlindZone_16.png)

멀티레이어 구름의 경우, 시각 차단 구간 절단과 교차하는 경우 (예: 그림 16.a), 두 모델 모두 구름의 꼭대기와 1.2km 경계 임계값에 초점을 맞추며, 이러한 시스템들은 종종 유사한 반사 강도로 표면까지 이어지기 때문에 이에 집중합니다. 두 모델 모두 깊은 시스템의 구름 갭 주변에 초점을 맞추지만 (예: 그림 16.b), 3+_5 모델에서는 대류권 방선에 중요한 홀로가 뚜렷하게 나타납니다. 이 반복되는 특징은 상층 대류권 풍속 및 습도 데이터를 지표 반사 강도의 예측에 통합하고 있을 가능성이 있습니다. 흥미로운 점은 3+_1 모델이 장면에서 높은 반사 강도 영역뿐만 아니라 구름 주변 지역에도 초점을 맞추고 있다는 것입니다.

# 응용 분야

이 연구의 동기는 결국 지표 훈련된 U-Net을 우주 기상 관측에 적용하는 것입니다. 이 두 시스템 간의 해상도 일치 문제에 대한 추가 작업이 완료되어야 하지만, 우리는 NSA 근처의 함께 발생하는 CloudSat-CPR 관측에 대한 초기 테스트를 수행했습니다. 여기서의 아이디어는 두 시스템이 (완벽하게 겹치지는 않지만) 동일한 폭풍 시스템의 비슷한 부분을 관측할 것이라는 것입니다. 우리는 몇 가지 예를 고려하고, 약간의 cumuliform 눈송이 낙설 사례를 아래에 포함했습니다.

<div class="content-ad"></div>

![BeyondtheBlindZone](/assets/img/2024-07-09-BeyondtheBlindZone_17.png)

앞서 살펴본 예시에서는, 함께 공간 및 표면 레이더가 약 3km 높이의 얕은 구름을 관측했음을 주목했습니다 (그러나 CloudSat은 표면 잡음으로 인해 맹점 아래의 증가된 반사 불연속성을 놓칩니다). 이 지역이 기존 기술과 우리의 U-Net을 사용하여 재구성될 때, 우리는 U-Net이 증가된 반사 대역을 정확히 대표할 수 있는 유일한 방법임을 발견했습니다. 보다 공식적으로는, CloudSat 관측 지점 (흰색 점선)과 각 재구성된 지역 가장 가까운 사이의 구조를 살펴보면, U-Net을 사용할 때 Pearson 상관관계가 현저히 향상되었음을 발견할 수 있습니다 (r_MAR=0.13에서 r_3+_1=0.74로).

이런 예시들이 일반적인 성능에 대해 어떤 결론을 내릴만한 철저한 분석은 아니지만, 시뮬레이션된 표면 맹점을 살펴볼 때 이전에 주목한 것과 일관된 능력을 보인다는 점을 나타냅니다. 우주 기기에 대한 추가 응용 작업이 진행 중에 있습니다.

# 최종 노트

<div class="content-ad"></div>

이미 긴 글을 마무리하기 전에, 우리가 모델에 추가한 몇 가지 다른 특징들을 강조하고, 자신만의 inpainting 모델을 개발에 흥미를 가지는 사람들을 위해 몇 가지 학습 코드 예제를 제공하고 싶었습니다.

## 몬테 카를로 드롭아웃

전통적인 베이지안 방법과는 달리, U-Net을 사용하여 물리적으로 기반한 불확실성 추정을 직접 생성하지 않습니다. 모델의 확신도와 안정성에 대한 대략적인 아이디어를 얻기 위해, 저희는 시험 계층에서 Gal과 Ghahramani의 2016년 연구를 기반으로 모델에 드롭아웃을 도입하기로 결정했습니다. 이를 통해 각 테스트 케이스에 대해 inpainted 예측의 분포를 생성할 수 있으며, 이러한 분포를 통해 각 inpainted 픽셀에 대한 신뢰 구간을 생성하고, 모델이 inpainting을 할 때 더 확신하는 영역에 대한 우리의 추정을 더욱 정교화할 수 있습니다. 아래의 이미지는 이에 대한 예시를 보여줍니다.

<div class="content-ad"></div>

일반적으로 우리는 각 경우에 N=50번의 반복을 사용하며, 위에서 볼 수 있듯이, 불확실성이 가장 높은 영역은 일반적으로 구름 가장자리와 구멍이 있는 구름 지역입니다. 모델이 이러한 특징들의 위치를 결정할 때 종종 환각을 일으키기 때문입니다.

## 훈련 통계

이 프로젝트의 모델 훈련은 Microsoft Azure의 Linux 기반 GPU 컴퓨팅 클러스터 및 Windows 11을 실행하는 고성능 데스크탑 두 개의 하드웨어 세트에서 완료되었습니다 (자세한 시스템 세부 정보는 표 1에 있음). 베이지안 하이퍼파라미터 스윕은 2일 동안 수행되었습니다. 또한 배치 정규화가 적용되었으며 (조기 중지(n=20), 드롭아웃 및 L2 규제(리지 회귀)도 훈련 과정 중에 과적합을 완화하기 위해 적용되었습니다. 학습 속도 감소도 두 번의 epoch(450 및 475)에서 적용되어 모델이 훈련 단계 말미에서 지역 손실 최솟값으로 쉽게 안착할 수 있습니다. 모든 훈련 실행 및 하이퍼파라미터 스윕은 Weights & Biases 클라우드 저장 옵션을 사용하여 온라인으로 저장되어 시간이 지나면서 모델 학습 속도와 안정성을 모니터링합니다.

![image](/assets/img/2024-07-09-BeyondtheBlindZone_18.png)

<div class="content-ad"></div>

## 예시 코드

GitHub 링크는 여기에 있어요: [blindzone_inpainting](https://github.com/frasertheking/blindzone_inpainting)

그러나, TensorFlow를 사용하여 실제 3Net+ 구현을 원하시는 분들을 위해 아래에 제공해드릴게요.

```python
def conv_block(x, kernels, kernel_size=(3, 3), strides=(1, 1), padding='same', is_bn=True, is_relu=True, n=2, l2_reg=1e-4):
    for _ in range(1, n+1):
        x = k.layers.Conv2D(filters=kernels, kernel_size=kernel_size,
                            padding=padding, strides=strides,
                            kernel_regularizer=tf.keras.regularizers.l2(l2_reg),
                            kernel_initializer=k.initializers.he_normal(seed=42))(x)
        if is_bn:
            x = k.layers.BatchNormalization()(x)
        if is_relu:
            x = k.activations.relu(x)
    return x

def unet3plus(input_shape, output_channels, config, depth=4, training=False, clm=False):

    """ 준비 단계 """
    interp = config['interpolation']
    input_layer = k.layers.Input(shape=input_shape, name="input_layer")
    xpre = preprocess(input_layer, output_channels)

    """ 인코더 """
    encoders = []
    for i in range(depth+1):
        if i == 0:
            e = conv_block(xpre, config['filters']*(2**i), kernel_size=(config['kernel_size'], config['kernel_size']), l2_reg=config['l2_reg'])
        else:
            e = k.layers.MaxPool2D(pool_size=(2, 2))(encoders[i-1])
            e = k.layers.Dropout(config['dropout'])(e, training=True)
            e = conv_block(e, config['filters']*(2**i), kernel_size=(config['kernel_size'], config['kernel_size']), l2_reg=config['l2_reg'])
        encoders.append(e)

    """ 가운데 """
    cat_channels = config['filters']
    cat_blocks = depth+1
    upsample_channels = cat_blocks * cat_channels

    """ 디코더 """
    decoders = []
    for d in reversed(range(depth+1)):
        if d == 0 :
            continue
        loc_dec = []
        decoder_pos = len(decoders)
        for e in range(len(encoders)):
            if d > e+1:
                e_d = k.layers.MaxPool2D(pool_size=(2**(d-e-1), 2**(d-e-1)))(encoders[e])
                e_d = k.layers.Dropout(config['dropout'])(e_d, training=True)
                e_d = conv_block(e_d, cat_channels, kernel_size=(config['kernel_size'], config['kernel_size']), n=1, l2_reg=config['l2_reg'])
            elif d == e+1:
                e_d = conv_block(encoders[e], cat_channels, kernel_size=(config['kernel_size'], config['kernel_size']), n=1, l2_reg=config['l2_reg'])
            elif e+1 == len(encoders):
                e_d = k.layers.UpSampling2D(size=(2**(e+1-d), 2**(e+1-d)), interpolation=interp)(encoders[e])
                e_d = k.layers.Dropout(config['dropout'])(e_d, training=True)
                e_d = conv_block(e_d, cat_channels, kernel_size=(config['kernel_size'], config['kernel_size']), n=1, l2_reg=config['l2_reg'])
            else:
                e_d = k.layers.UpSampling2D(size=(2**(e+1-d), 2**(e+1-d)), interpolation=interp)(decoders[decoder_pos-1])
                e_d = k.layers.Dropout(config['dropout'])(e_d, training=True)
                e_d = conv_block(e_d, cat_channels, kernel_size=(config['kernel_size'], config['kernel_size']), n=1, l2_reg=config['l2_reg'])
                decoder_pos -= 1
            loc_dec.append(e_d)
        de = k.layers.concatenate(loc_dec)
        de = conv_block(de, upsample_channels, kernel_size=(config['kernel_size'], config['kernel_size']), n=1, l2_reg=config['l2_reg'])
        decoders.append(de)

    """ 마지막 """
    d1 = decoders[len(decoders)-1]
    d1 = conv_block(d1, output_channels, kernel_size=(config['kernel_size'], config['kernel_size']), n=1, is_bn=False, is_relu=False, l2_reg=config['l2_reg'])
    outputs = [d1]

    """ 깊이 있는 감독 """
    if training:
        for i in reversed(range(len(decoders))):
            if i == 0:
                e = conv_block(encoders[len(encoders)-1], output_channels, kernel_size=(config['kernel_size'], config['kernel_size']), n=1, is_bn=False, is_relu=False, l2_reg=config['l2_reg'])
                e = k.layers.UpSampling2D(size=(2**(len(decoders)-i), 2**(len(decoders)-i)), interpolation=interp)(e)
                outputs.append(e)
            else:
                d = conv_block(decoders[i - 1], output_channels, kernel_size=(config['kernel_size'], config['kernel_size']), n=1, is_bn=False, is_relu=False, l2_reg=config['l2_reg'])
                d = k.layers.UpSampling2D(size=(2**(len(decoders)-i), 2**(len(decoders)-i)), interpolation=interp)(d)
                outputs.append(d)

    if training:
        for i in range(len(outputs)):
            if i == 0:
                continue
            d_e = outputs[i]
                d_e = k.layers.concatenate([out1, out2, out3])
            outputs[i] = merge_output(input_layer, k.activations.linear(d_e), output_channels)

    return tf.keras.Model(inputs=input_layer, outputs=outputs, name='UNet3Plus')
```

<div class="content-ad"></div>

# 미래

안녕하세요! 긴 글을 읽어주셔서 감사합니다. 지금까지 다뤄온 내용을 간단히 정리해 드릴게요. 위 내용을 모두 읽어주신 분들이나 끝 부분으로 바로 이동한 분들을 위해 준비했어요.

위성 레이더 블라인드 존은 위성 기반 지구 감시 강수량 임무에서 지속적으로 발생하는 문제로, 이는 세계적인 수자원-에너지 예산 계산에 중대한 영향을 미칩니다. 이 영역을 채우기 위한 전통적인 선형 보완 방법에 공통된 문제를 극복하기 위해 비선형, 심층 지도된 U-Net을 사용하기로 결정했습니다. U-Net은 선형 기술에 비해 거의 모든 메트릭에서 우수한 성능을 발휘하며 다층 구름, 구름 간격, 얕은 구름과 같은 복잡한 클라우드 구조를 재구성할 수 있습니다. 또한 다양한 설명 가능한 AI 기술을 사용하여 블라인드 존 임계값과 대류권(특히 풍속 정보)을 따라 직접적인 정보가 모델의 의사 결정 과정에 매우 유용한 것으로 나타났습니다. 이러한 모델이 현재의 물리 기반 솔루션을 완전히 대체해야 한다고 주장하지는 않지만, 미래 임무에서 다른 회수 방법을 보완할 수 있는 독특한 새로운 관점을 제공할 수 있다고 믿습니다.

CloudSat-CPR 관측과 직접적으로 관련된 후속 프로젝트를 진행 중에 있습니다.

<div class="content-ad"></div>

# 레퍼런스

1. Gal, Y., & Ghahramani, Z. (2016). Dropout as a Bayesian Approximation: Representing Model Uncertainty in Deep Learning (arXiv:1506.02142). arXiv. [링크](https://doi.org/10.48550/arXiv.1506.02142)

2. Geiss, A., & Hardin, J. C. (2021). Inpainting radar missing data regions with deep learning. Atmospheric Measurement Techniques, 14(12), 7729–7747. [링크](https://doi.org/10.5194/amt-14-7729-2021)

3. Guillemot, C., & Le Meur, O. (2014). Image Inpainting: Overview and Recent Advances. IEEE Signal Processing Magazine, 31(1), 127–144. [링크](https://doi.org/10.1109/MSP.2013.2273004)

<div class="content-ad"></div>

Huang, H., Lin, L., Tong, R., Hu, H., Zhang, Q., Iwamoto, Y., Han, X., Chen, Y.-W., & Wu, J. (2020). UNet 3+: A Full-Scale Connected UNet for Medical Image Segmentation. [arXiv:2004.08790](https://doi.org/10.48550/arXiv.2004.08790)

Kidd, C., Graham, E., Smyth, T., & Gill, M. (2021). Assessing the Impact of Light/Shallow Precipitation Retrievals from Satellite-Based Observations Using Surface Radar and Micro Rain Radar Observations. *Remote Sensing*, 13(9), Article 9. [https://doi.org/10.3390/rs13091708](https://doi.org/10.3390/rs13091708)

King, F., & Fletcher, C. G. (2020). Using CloudSat-CPR Retrievals to Estimate Snow Accumulation in the Canadian Arctic. *Earth and Space Science*, 7(2), e2019EA000776. [https://doi.org/10.1029/2019EA000776](https://doi.org/10.1029/2019EA000776)

Lamer, K., Kollias, P., Battaglia, A., & Preval, S. (2020). Mind the gap — Part 1: Accurately locating warm marine boundary layer clouds and precipitation using spaceborne radars. *Atmospheric Measurement Techniques*, 13(5), 2363–2379. [https://doi.org/10.5194/amt-13-2363-2020](https://doi.org/10.5194/amt-13-2363-2020)

<div class="content-ad"></div>

오늘은 최신 연구 논문 몇 편을 공유해 드리려고 합니다. 어쩌다 보니 눈에 띄는 이름들이 많아서 한 번 번역을 도와드리려고 해요.

- 최초로 소개해 드릴 논문은 Lugmayr 등의 'RePaint: Inpainting using Denoising Diffusion Probabilistic Models'입니다. 컴퓨터 비전과 패턴 인식(CVPR) 분야에서 새로운 기술을 선보였답니다. 논문 링크는 [여기](https://doi.org/10.1109/CVPR52688.2022.01117)에서 확인할 수 있어요.

- 그 다음은 Maahn 등의 'How does the spaceborne radar blind zone affect derived surface snowfall statistics in polar regions?'입니다. 해당 논문은 극지역에서 표면 적설 통계에 미치는 위성 레이다의 영향을 다룬 연구로, [이 링크](https://doi.org/10.1002/2014JD022079)에서 상세 내용을 확인할 수 있어요.

- Simonyan, Vedaldi, Zisserman의 'Deep Inside Convolutional Networks: Visualising Image Classification Models and Saliency Maps'은 이미지 분류 모델 및 중요도 맵 시각화에 관한 연구로, [여기](https://doi.org/10.48550/arXiv.1312.6034)에서 더 알아볼 수 있어요.

- 마지막으로 Zhou, Siddiquee, Tajbakhsh, Liang의 'UNet++: A Nested U-Net Architecture for Medical Image Segmentation'은 의료 이미지 세그멘테이션을 위한 혁신적인 아키텍처에 대한 논문으로, [이 링크](https://doi.org/10.48550/arXiv.1807.10165)에서 확인할 수 있어요.

필요하신 정보가 있으시면 말씀해 주세요!