---
title: "Nodejs에서 기본적으로 타입스크립트 사용하는 방법"
description: ""
coverImage: "/assets/img/2024-08-13-NewBuilt-InTypeScriptSupportInNodejsFinally_0.png"
date: 2024-08-13 11:14
ogImage:
  url: /assets/img/2024-08-13-NewBuilt-InTypeScriptSupportInNodejsFinally_0.png
tag: Tech
originalTitle: "New Built-In TypeScript Support In Nodejs  Finally"
link: "https://medium.com/coding-beauty/new-node-typescript-support-e0cfcdede6ac"
isUpdated: true
updatedAt: 1723863112500
---

<img src="/assets/img/2024-08-13-NewBuilt-InTypeScriptSupportInNodejsFinally_0.png" />

오늘은 네이티브 TypeScript 지원이 드디어 Node.js에 등장하면서 놀라운 소식입니다!

네, 이제 Node.js에서 네이티브로 타입을 사용할 수 있어요.

나도 TypeScript와 ts-node를 쓰는 걸 그만두기 좋은 때인 것 같아요.

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

### 변경 전:

Node.js는 JavaScript 파일에만 신경을 썼어요.

다음은 실행되지 않았을 것입니다:

<img src="/assets/img/2024-08-13-NewBuilt-InTypeScriptSupportInNodejsFinally_1.png" />

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

위와 같이 표 태그를 변경하면 이러한 불쾌한 오류가 발생합니다:

![image](/assets/img/2024-08-13-NewBuilt-InTypeScriptSupportInNodejsFinally_2.png)

최선의 방법은 TypeScript를 설치하고 tsc로 컴파일하는 것이었습니다.

![image](/assets/img/2024-08-13-NewBuilt-InTypeScriptSupportInNodejsFinally_3.png)

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

그리고 수백만 명의 개발자들이 이것이 꽤 괜찮은 옵션이라고 동의했습니다:

![이미지](/assets/img/2024-08-13-NewBuilt-InTypeScriptSupportInNodejsFinally_4.png)

하지만 이것은 고통스럽습니다. 계속해서 동일한 패키지를 설치하고 동일한 명령어를 타이핑해야했습니다.

JS로의 추가 컴파일 단계와 TypeScript 구성 및 관련 사항을 처리해야했습니다.

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

종종 짜증이 나는 일이죠 — 특히 테스트를 조금 진행 중일 때 말이에요.

그래서 ts-node가 등장했는데도 여전히 부족했죠.

이제 TypeScript 파일을 직접 실행할 수 있게 되었어요:

![이미지](/assets/img/2024-08-13-NewBuilt-InTypeScriptSupportInNodejsFinally_5.png)

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

우리는 독립 노드 명령어로 수행하는 것처럼 즉석에서 대화식 세션을 시작할 수도 있어요:

![image1](/assets/img/2024-08-13-NewBuilt-InTypeScriptSupportInNodejsFinally_6.png)

그리고 모두가 그것을 사랑했어요:

![image2](/assets/img/2024-08-13-NewBuilt-InTypeScriptSupportInNodejsFinally_7.png)

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

하지만 여전히 추가 종속성이 있었고, 여전히 TypeScript를 설치해야 했습니다.

또한, 우리는 --esm 플래그와 함께 ES 모듈을 사용하기 위해 ts-node를 사용하는 방법과 같은 미묘한 복잡성에 대해 알고 있어야 했습니다.

✅지금:

모든 이 변화는 이제 노드의 모든 새로운 업그레이드로 인해 변경됩니다.

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

- TypeScript를 네이티브로 지원합니다.
- 의존성이 없습니다.
- 중간 파일 및 모듈 구성 파일이 없습니다.

![이미지](/assets/img/2024-08-13-NewBuilt-InTypeScriptSupportInNodejsFinally_8.png)

이제 Prettier, Next.js 및 Webpack과 같은 즐겨 사용하는 JS 도구들도 더 안전하고 인텔리센스 친화적인 구성 파일을 가질 수 있습니다.

웹팩을 즐겨 사용하는 도구 리스트에 넣은 사람은 거의 없지만 그래도…

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

얼른 좋은 소식이네요! 이렇게 하면 prettier.config.ts를 지원하는 pull request가 이미 있다는 걸 알았어요. 이번 새로운 발전 덕분에 더 큰 발전을 이루고 있어요.

# Behind the Scenes

TypeScript 지원은 점진적으로 이루어질 거예요. 현재는 유형만 지원하고 있어요. 열거형과 같은 보다 TypeScript스러운 기능은 사용할 수 없어요 (누가 요즘에 열거형을 사용하겠어요).

내부적으로 @swc/wasm-typescript 도구를 사용해 TypeScript 파일의 모든 유형을 제거하고 있답니다.

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

이렇게 바뀝니다:

<img src="/assets/img/2024-08-13-NewBuilt-InTypeScriptSupportInNodejsFinally_10.png" />

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

# Node.js에서 TypeScript를 사용하는 방법

처음 시작하는 것이라고 했듯이 아직 실험적이며, 현재는 --experimental-strip-types 플래그가 필요합니다:

![image](/assets/img/2024-08-13-NewBuilt-InTypeScriptSupportInNodejsFinally_11.png)

이것은 곧 출시될 예정입니다.

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

# 마무리 생각

TypeScript 내장 기능은 Node.js를 JS 개발자에게 더 즐거운 플랫폼으로 만들어주는 심각한 동작입니다. 저는 확실히 이를 사용할 것입니다.

Bun이나 Deno처럼 매끄럽지는 않지만, 아직 지원이 완벽하지 않아도 Node가 여전히 가장 인기 있는 JS 백엔드 프레임워크이기 때문에 전반적인 JavaScript 생태계에 큰 영향을 미칩니다.

# JavaScript가 하는 모든 미친 짓

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

여러분이 모든 특이사항을 알고 있었다고 생각했을 때, 좀 더 알아두어야 할 부분이 있다는 것을 알게 될 겁니다. Every Crazy Thing JavaScript Does은 자바스크립트의 미묘한 함정과 잘 알려지지 않은 부분들에 대한 매혹적인 안내서로, 괴로운 버그를 피하고 소중한 시간을 절약하세요.

오늘 이곳에서 무료 복사본을 받으세요.

![이미지](/assets/img/2024-08-13-NewBuilt-InTypeScriptSupportInNodejsFinally_12.png)
