---
title: "LIME을 사용한 로컬 해석의 깊은 탐구"
description: ""
coverImage: "/assets/img/2024-07-07-ADeepDiveonLIMEforLocalInterpretations_0.png"
date: 2024-07-07 23:59
ogImage:
  url: /assets/img/2024-07-07-ADeepDiveonLIMEforLocalInterpretations_0.png
tag: Tech
originalTitle: "A Deep Dive on LIME for Local Interpretations"
link: "https://medium.com/towards-data-science/a-deep-dive-on-lime-for-local-interpretations-872bea23952f"
isUpdated: true
---

![Korean](/assets/img/2024-07-07-ADeepDiveonLIMEforLocalInterpretations_0.png)

LIME은 XAI 방법론 중의 OG입니다. 기계 학습 모델이 어떻게 작동하는지 이해할 수 있게 해줍니다. 특히 개별 예측이 어떻게 이루어지는지 이해하는 데 도움이 됩니다 (즉, 지역 해석).

최근 발전에도 불구하고 LIME이 인지도가 줄었다고 해도, 여전히 이해할 가치가 있습니다. 상대적으로 간단한 접근 방식이며 많은 해석 가능성 문제에 대해 "충분히 좋다"는 것 때문입니다. 또한 보다 최근의 지역 해석 방법인 SHAP에 영감을 준 바도 있습니다.

그래서 우리는:

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

- 로컬 해석을 얻기 위해 LIME이 취한 단계에 대해 논의합니다.
- 그에 따른 몇 가지 선택 사항에 대해 자세히 논의하겠습니다. 예를 들어 샘플 가중치 및 대리 모델 선택 등.
- LIME Python 패키지를 적용해 보겠습니다.

이 과정에서 SHAP 방법과 비교하며 LIME의 약점을 더 잘 이해하겠습니다. 또한 LIME은 로컬 방법이지만, lime 가중치를 집계하여 전역 해석을 얻을 수 있음을 알 수 있습니다. 이를 통해 패키지의 기본 선택 사항 중 일부를 이해하는 데 도움이 될 것입니다.
