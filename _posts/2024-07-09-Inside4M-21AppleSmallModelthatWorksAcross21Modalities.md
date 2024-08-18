---
title: "다양한 21가지 모드에서 작동하는 Apple의 소형 모델 4M-21의 모든 것"
description: ""
coverImage: "/assets/img/2024-07-09-Inside4M-21AppleSmallModelthatWorksAcross21Modalities_0.png"
date: 2024-07-09 11:25
ogImage:
  url: /assets/img/2024-07-09-Inside4M-21AppleSmallModelthatWorksAcross21Modalities_0.png
tag: Tech
originalTitle: "Inside 4M-21: Apple Small Model that Works Across 21 Modalities"
link: "https://medium.com/towards-artificial-intelligence/inside-4m-21-apple-small-model-that-works-across-21-modalities-2416ab96a39e"
isUpdated: true
---

## 새로운 모델은 애플의 장치 내 AI 전략의 기반이 될 수 있습니다.

![image](/assets/img/2024-07-09-Inside4M-21AppleSmallModelthatWorksAcross21Modalities_0.png)

애플은 생성 AI 게임에 늦게 참여했지만 최근에는 연구 일정을 꽤 강력하게 추진하고 있습니다. 애플은 다음 파생 AI 파도의 가장 핫한 분야 중 하나에서 혁신을 위한 이상적인 놀이터를 가지고 있습니다: 장치 내 다중 모달 모델. 대규모 기반 모델과 API 통합을 통해 이동식 AI를 구동하는 아이디어는 매우 비실용적이고 보안에 취약해 보입니다. 애플은 이 패러다임에 대한 대안을 제공할 독특한 위치에 있습니다. 그러나 애플의 대부분의 소형 장치 내 모델에 대한 노력은 다소 기대에 못 미치는 부분이었습니다.

하지만 이것은 변화하기 시작했습니다.

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

지난 주, Apple이 4M-21을 발표하고 오픈 소스로 공개했습니다. 저는 이것을 작은 장치용 모델 중에서 가장 인상적인 작업으로 간주합니다. 4M-21은 21가지 다양한 방식에서 매끄럽게 작동하는 다중 모달 모델입니다! 이 작업은 분명히 Apple의 장치 내 모델 전략을 시그널하는 것이며, 막대한 수의 다중 모달 기능은 꽤 충격적입니다. 그러나 이 작업은 Apple이 몇 달 전에 발표한 4M 모델을 기반으로 구축되었습니다.

여기서 시작해봅시다.

## 4M 개요

4M 프레임워크는 Massively Multimodal Masked Modeling의 약자로, 여러 작업과 모달리티를 처리할 수 있는 모델을 훈련하는 데 사용됩니다. 이 모델들은 추가 조정 없이도 다양한 시각 작업에서 뛰어나며, 새로운 작업을 위해 세심하게 조정될 때 더욱 우수한 성과를 거둡니다.

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

**4M**은 하나의 통합된 Transformer 인코더-디코더를 활용한 포괄적인 훈련 체계입니다. 이 시스템은 텍스트, 이미지, 기하학적/의미적 데이터, 신경망 특징 맵을 포함한 다양한 입력/출력 모드에서 마스킹 모델링 목표를 사용하여 훈련됩니다. 모든 모드를 이산 토큰으로 변환함으로써 4M은 작은 임의의 토큰 하위 집합에서 다중 모달 마스킹 모델링을 수행합니다.

4M의 능력 면에서는 다음과 같은 부문에서 우수한 성과를 거두고 있습니다:

- 직접 다양한 시각 작업 다룰 수 있음.

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

- 새로운 작업이나 유형에 민감하게 조정되어 성능을 향상합니다.

- 다양한 유형으로 조건을 걸어 생성 모델로 작동하여 유연하고 표현력 있는 다중 모달 편집이 가능합니다.

훈련에는 다양한 유형의 모달리티를 이산 토큰의 시퀀스로 토큰화하여 하나의 트랜스포머가 다양한 데이터 유형으로부터 학습할 수 있도록 합니다. 훈련 과정에서는 이러한 토큰의 임의의 부분 집합을 다른 토큰으로 매핑합니다.

![이미지](/assets/img/2024-07-09-Inside4M-21AppleSmallModelthatWorksAcross21Modalities_2.png)

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

4M 모델은 다른 것들의 어떤 조합에서도 어떠한 모달리티도 생성할 수 있습니다. 심지어 부분적인 입력에서도 가능합니다. 하나의 모달리티로부터 여러 모달리티를 예측할 때, 4M은 각 모달리티를 순차적으로 예측하며, 완전히 생성된 출력을 다시 입력으로 통합합니다. 이 접근 방식은 모든 훈련 모달리티에 걸쳐 일관된 예측을 보장합니다.

# 4M-21

4M-21은 모델 및 데이터셋 크기, 유형 및 모달리티 수를 증가시킴으로써 원래의 4M 체계를 확장합니다. 이 버전은 또한 여러 데이터셋에 동시에 훈련합니다. 각 모달리티는 특정 토크나이저를 사용하여 이산 토큰의 시퀀스로 변환됩니다. 훈련 중에 모든 모달리티에서 무작위 토큰 서브셋이 입력 및 타겟으로 사용되어 한 서브셋을 다른 서브셋으로 예측하는 것을 목표로 합니다. 의사 라벨링은 여러 정렬된 모달리티를 가진 대규모 사전 훈련 데이터셋을 만드는 데 사용됩니다.

4M-21은 여러 범주로 그룹화된 다양한 모달리티에 대해 훈련합니다:

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

- RGB: 이미지의 토큰화 및 픽셀 버전과 색 팔레트가 포함되어 있어요.

- Geometric: 표면 법선, 깊이, 3D 인간 자세 및 형상이 포함되어 있어요.

- Semantic: 시맨틱 세분화, 경계 상자 및 SAM과 같은 모델로부터 유사 레이블이 포함되어 있어요.

- Edges: 장면 레이아웃 및 의미론을 위한 Canny 및 SAM 엣지가 있어요.

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

- 특징 맵: CLIP, DINOv2 및 ImageBind에서의 임베딩.
- 메타데이터: RGB 이미지 및 기타 모달리티에서 다양한 유형의 메타데이터.

![이미지](/assets/img/2024-07-09-Inside4M-21AppleSmallModelthatWorksAcross21Modalities_3.png)

## 토큰화

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

4M-21의 가장 중요한 기여 분야 중 하나는 토큰화 체계입니다. 토큰화는 다양한 형태와 작업을 이산 토큰의 시퀀스로 변환하여 그 표현 공간을 통합합니다.

4M-21의 혁신은 다양한 형태에 대해 서로 다른 토크나이저를 사용하는 데 있습니다:

i. ViT 토크나이저: 이미지와 유사한 형태에 대해 사용됩니다.

ii. MLP 토크나이저: 인간의 포즈와 전역 임베딩에 사용됩니다.

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

iii. 텍스트 토크나이저: 텍스트 및 바운딩 박스 및 메타데이터와 같은 다른 모달리티를 인코딩하는 데 사용됩니다.

![](https://www.examplewebsite.com/assets/img/2024-07-09-Inside4M-21AppleSmallModelthatWorksAcross21Modalities_4.png)

4M-21 훈련은 두 단계 과정을 거칩니다: 대규모 이미지 데이터셋에서의 4M 사전 훈련 단계, 그리고 더 많은 모달리티를 포함한 작은 데이터셋에서의 세밀한 튜닝 단계를 거칩니다. 모델은 이러한 데이터셋에서 랜덤 샘플링을 사용하여 훈련되며, 훈련 중 언어 모델링을 수행합니다.

4M-21 아키텍처는 모달리티 임베딩을 사용한 트랜스포머 인코더-디코더를 사용합니다. 매스킹 전략에는 안정적인 훈련을 보장하기 위해 다중 모달 랜덤 매스킹 및 스팬 매스킹이 포함됩니다.

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

![Inside4M-21AppleSmallModelthatWorksAcross21Modalities_5.png](/assets/img/2024-07-09-Inside4M-21AppleSmallModelthatWorksAcross21Modalities_5.png)

# 성능 평가

Apple은 4M-21의 제로샷 성능을 평가했습니다. 표면 법선 및 깊이 추정, 의미론적 및 인스턴스 분할, k-NN 검색 및 3D 인간 키포인트 추정과 같은 작업을 수행했습니다. 이 모델은 강력한 베이스라인 및 전문가 모델들을 능가하여 다양한 작업을 성능 감소 없이 해결할 수 있는 능력을 보여주었습니다.

![Inside4M-21AppleSmallModelthatWorksAcross21Modalities_6.png](/assets/img/2024-07-09-Inside4M-21AppleSmallModelthatWorksAcross21Modalities_6.png)

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

변환 작업에서도 훌륭한 성과를 거뒀으며, 특히 3D 객체 감지와 같은 첨단 작업에서 뛰어난 성과를 보였어요.

![Inside4M-21AppleSmallModelthatWorksAcross21Modalities](/assets/img/2024-07-09-Inside4M-21AppleSmallModelthatWorksAcross21Modalities_7.png)

이러한 결과는 4M-21이 다중 모달리티와 과제를 처리하는 능력을 강조하며, 이전 모델인 4M-7에 비해 상당한 향상을 보여줍니다.

4M-21은 복잡한 모델이에요. 21가지 모달리티는 간단한 아키텍처에 적합하지 않아요. 그러나 4M-21은 온디바이스 기초 모델의 미래에 대한 놀라운 잠재력을 보여주며, 애플의 이 분야 전략을 엿볼 수 있어요. 희망히 4M-21이 이 중요한 생성적 AI 분야에서 더 많은 연구를 일으켜 줄 거라고 믿어요.
