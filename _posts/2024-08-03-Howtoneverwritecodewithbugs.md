---
title: "버그 없는 코드를 작성하기 위해 지켜야하는 7가지"
description: ""
coverImage: "/assets/img/2024-08-03-Howtoneverwritecodewithbugs_0.png"
date: 2024-08-03 18:36
ogImage:
  url: /assets/img/2024-08-03-Howtoneverwritecodewithbugs_0.png
tag: Tech
originalTitle: "How to never write code with bugs"
link: "https://medium.com/coding-beauty/avoiding-bugs-22b2d6745f63"
isUpdated: true
---

![img](/assets/img/2024-08-03-Howtoneverwritecodewithbugs_0.png)

개발자들은 자신의 시간의 75%를 버그 디버깅에 할애하는데, 이것이 주요 이유 중 하나입니다.

이 오류를 피하는 것은 코드에서 발생하는 버그 빈도를 대폭 줄일 수 있습니다.

강력하면서도 간단한 원칙이며 여러 가지 함의가 있습니다.

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

절대 코드가 올바르게 작동한다고 가정해서는 안돼요.

그리고 단순히 눈에 보이는 대로만 믿지 말고요...

![Bug image](/assets/img/2024-08-03-Howtoneverwritecodewithbugs_1.png)

머신에도 마찬가지에요.

여기에 자신에게 의존하면 안 된다는 점이 중요해요. 그래서 버그가 처음에 발생하게 되죠.

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

버그는 항상 예상치 못한 문제입니다.

특히 복잡한 알고리즘의 경우 디버깅하는 데 많은 시간이 걸립니다.

코드의 논리에 대한 당신의 정신적 모델은 현실과 완전히 달라지는 경우가 많습니다.

보통 코드를 한 줄씩, 변수를 하나씩 주의 깊게 따라가보면 마침내 그 간극을 깨닫게 됩니다.

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

자신이 쓴 글을 교정하기가 어려울 수 있는 이유는 동일합니다.

당신의 뇌는 이미 당신이 무엇을 쓰고자 했는지 알고 있습니다. 이미 전달하려는 의미에 대한 정신적 모델을 만들어 놓았습니다. 이것은 실제로 쓰인 것과는 다릅니다.

그래서 당신이 오류를 찾기 위해 작업물을 다시 읽을 때 어떻게 될까요?

낮은 단계의 문법적 및 철자 오류보다 전반적인 의도된 의미에 훨씬 더 집중합니다.

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

그래서 코드 리뷰가 중요한 이유입니다 — 코드에 새로운 눈을 통해 여러 번 검토받는 것이 중요합니다.

그래서 정기적인 테스트가 중요합니다.

정기적이고 점진적으로 테스트하면 버그를 빠르고 빠르게 잡을 수 있습니다.

의미 있는 변경을 한다면 즉시 테스트하세요.

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

이제 자동화된 테스팅 기술과 지속적 통합 기술이 빛을 발합니다.

수동 테스팅을 하면 테스트를 미루게 되는 경우가 훨씬 많아질 것입니다. 아마도 버그가 넘쳐나는 엄청난 변경을 할 때까지 테스트를 미루게 될 것입니다.

지속적 통합에서는 테스트 미루기에 여지가 없습니다.

정기적으로 커밋하기만 하면 버그 처리 시간을 크게 단축시키고, 뭔가가 엄청난 오류로 이어지는 가능성을 크게 낮출 수 있습니다.

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

![image](/assets/img/2024-08-03-Howtoneverwritecodewithbugs_2.png)

# Every Crazy Thing JavaScript Does

Just when you thought you knew all the quirks. Avoid painful bugs and save valuable time with Every Crazy Thing JavaScript Does, a captivating guide to the subtle caveats and lesser-known parts of JavaScript.

Get a free copy here today.

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

![How to never overwrite code with bugs](/assets/img/2024-08-03-Howtoneverwritecodewithbugs_3.png)
