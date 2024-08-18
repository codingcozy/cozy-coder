---
title: "로컬에서 모든 LLM 실행을 위한 궁극의 가이드"
description: ""
coverImage: "/assets/img/2024-07-07-AnUltimateGuidetoRunAnyLLMLocally_0.png"
date: 2024-07-07 22:52
ogImage:
  url: /assets/img/2024-07-07-AnUltimateGuidetoRunAnyLLMLocally_0.png
tag: Tech
originalTitle: "An Ultimate Guide to Run Any LLM Locally"
link: "https://medium.com/frontend-canteen/an-ultimate-guide-to-run-any-llm-locally-eb1a43052053"
isUpdated: true
---

걱정하지 마세요, 이 기사의 끝에 훨씬 더 명확한 다이어그램이 있습니다.

LLM(럴라마, 미스트랄-7B)을 로컬에서 실행하려고 하지만 인터넷의 많은 비디오와 기사를 읽었음에도 여전히 미로 속에 있습니다.

그럼 이 기사가 여러분을 미로에서 탈출시켜줄 최종 지름길입니다.

키 개념을 명확히 이해할 수 있도록 모든 내용을 정리하겠습니다. 하나의 큰 다이어그램과 함께요.

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

# LLM에서 모델은 무엇인가요?

LLM에서 모델은 디지털 파일에 불과해요, 마치 이미지 파일이나 텍스트 파일처럼 말이에요.

모델이 정확히 무엇인지 알아보죠:

![이미지 이름](/assets/img/2024-07-07-AnUltimateGuidetoRunAnyLLMLocally_0.png)

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

이건 신경망의 구조와 매개변수들을 저장하는 거야.

모던 AI 기술들은 모두 신경망을 기반으로 하잖아. 신경망은 엄청 많은 노드들을 포함하고 있고, 각 노드마다 일부 매개변수들을 갖고 있어.
