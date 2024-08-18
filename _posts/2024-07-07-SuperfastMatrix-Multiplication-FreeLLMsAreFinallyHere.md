---
title: "행렬 곱셈 없이 초고속으로 작동하는 새로운 언어 모델 등장"
description: ""
coverImage: "/code-tower.github.io/assets/no-image.jpg"
date: 2024-07-07 20:30
ogImage:
  url: /code-tower.github.io/assets/no-image.jpg
tag: Tech
originalTitle: "Superfast Matrix-Multiplication-Free LLMs Are Finally Here"
link: "https://medium.com/gitconnected/superfast-matrix-multiplication-free-llms-are-finally-here-cac5b78a4fa7"
isUpdated: true
---

최근 ArXiv에 게시된 연구 논문에 따르면, 우리가 오늘날 알고 있는 LLMs에 대한 대규모 변경이 제안되었습니다.

이 프로젝트에 참여한 연구자들은 LLMs에서 수행되는 핵심 수학 연산인 Matrix Multiplication (MatMul)을 제거했습니다.

그들은 새로운 MatMul을 사용하지 않는 LLMs가 심지어 수십억 개의 매개변수 규모에서도 강력한 성능을 발휘할 수 있으며, 특정 작업에서는 전통적인 LLMs의 성능을 능가할 수 있다는 것을 보여주었습니다!

이는 한 번의 최적화만으로 가능해진 거대한 변화입니다!

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

이것은 LLM(Large Language Models)에 매우 중요하지만 계산 비용이 매우 큰 MatMul 작업 때문입니다. 오늘날의 LLM은 그들의 훈련과 추론을 위해 Graphics Processing Units (GPUs)에 매우 의존하게 됩니다.

그러나 이제 이게 더 이상 참이 아닐 수도 있습니다!

여기에는 새로운 MatMul이 없는 LLM들이 어떻게 가능해졌는지, 이것이 어떻게 인공 지능의 미래에 긍정적인 영향을 미쳤는지에 대해 깊이 탐구하는 이야기가 있습니다.

# 하지만 먼저, Matrix Multiplication이란 무엇인가요?
