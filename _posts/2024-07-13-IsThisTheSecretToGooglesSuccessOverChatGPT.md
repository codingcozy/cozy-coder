---
title: "구글의 성공 비결, ChatGPT보다 뛰어난 점은"
description: ""
coverImage: "/assets/img/2024-07-13-IsThisTheSecretToGooglesSuccessOverChatGPT_0.png"
date: 2024-07-13 23:16
ogImage:
  url: /assets/img/2024-07-13-IsThisTheSecretToGooglesSuccessOverChatGPT_0.png
tag: Tech
originalTitle: "Is This The Secret To Google’s Success Over ChatGPT?"
link: "https://medium.com/@ignacio.de.gregorio.noblejas/is-this-the-secret-to-googles-success-over-chatgpt-b2a545f39ad5"
isUpdated: true
---

저희가 Gemini 1.5의 비밀 본성을 파악한 것 같아요.

Gemini 1.5는 구글의 새로운 대규모 언어 모델 (LLM)로, 이전까지 본 적 없는 50배 정도의 퍼포먼스를 제공한다는 점을 혼동하지 마세요.

그 비밀은 Ring Attention일 수 있습니다. 버클리 대학의 연구원들이 발견한 새로운 LLM 실행 방식으로, 효율적인 분배를 가능케 하여 모델의 원시 파워를 극대화하고 새로운 높이에 이를 수 있습니다.

사실, 이 혁신은 Transformer의 파워를 발휘하는 열쇠가 될 수 있습니다. Gemini와 ChatGPT 뒤에 깔려있는 아키텍처로, 다시 한 번 2024년은 2023년보다 더 미친 일이 일어날 것임을 입증하고 있습니다.

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

# 핵심이란 무엇인가요?

리링 어텐션의 혁신적인 점을 완전히 이해하기 위해서는 젬니(Gemini)나 ChatGPT가 어떻게 작동하는지를 이해해야 합니다.

따라서, 우선적으로 먼저 해야 할 것은, 저희가 텍스트 메시지를 보낼 때 어떤 일이 벌어지는지에 대해 질문하는 것입니다.

## 단어들이 숫자로 변할 때

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

안녕하세요! LLM이 말을 이해하는 방법에 대해 궁금해 하셨을지도 모르겠어요. 예를 들어, ChatGPT가 "시베리안 허스키는 종류 중 한 종류는..." 다음에 "개"라는 단어를 선택하는 방법은 무엇일까요?

일단, 모델은 시퀀스를 토큰으로 '분할'합니다. 즉, 모델이 단어 나 서브워드로 인식하는 다양한 단어를 기준으로 나눕니다.

이전 시퀀스를 사용하여 예를 들면, [‘the’, ‘Siber’, ‘ian’, ‘Husky’, ‘is’, ‘a’, ‘type’, ‘of’, ‘dog’]와 같이 나눌 수 있습니다.

이런식으로 모델이 인식할 수 있는 부분으로 시퀀스를 나누어서 처리할 수 있습니다.

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

다음으로, '임베딩(embedding)'이라고 설명하는 변환을 적용합니다. 이 변환은 먼저 각 토큰을 정수로 변환한 후, 그 정수를 사용하여 해당 토큰에 대한 임베딩을 임베딩 조회 행렬에서 찾아냅니다.

![image](/assets/img/2024-07-13-IsThisTheSecretToGooglesSuccessOverChatGPT_0.png)

이러한 일을 왜 하는 걸까요? 이에 대한 이유는 두 가지입니다.

- 컴퓨터는 항상 숫자로 작동하기 때문에 필수적입니다.
- 개념을 측정할 수 있게 만들기 위해서입니다. 단어를 벡터로 변환하여 그 거리를 측정할 수 있습니다. 이를 통해 모델이 '고양이'와 '개'와 같은 유사한 개념을 수학을 사용하여 식별하는 데 도움이 됩니다. 이것은 OpenAI에서 '관련성(relatedness)'으로 언급됩니다.

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

중요한 점은 임베딩이 원본 텍스트 토큰의 의미를 캡처한다는 것이다.

이제 시퀀스를 임베딩 세트로 변환했으니, 그것들을 모델로 보내는 시간이다.

그리고 이곳에서 마법이 벌어진다.

## Self-attention이 바로 핵심이다.

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

만약 우리가 언어에 대해 생각하고 어떻게 작동하는지 분석해본다면 한 가지를 빨리 깨닫게 됩니다: 단어들은 그 자체로 의미를 가지고 있는 것뿐만 아니라 주변 단어들의 문맥에 따라 의미가 생겨난다는 것을요.

예를 들어, "강둑"과 "은행"은 '은행'이라는 단어를 공유하지만 각각 다른 의미를 가지고 있죠.

다시 말해, 어떤 상황에서 '은행'이 무엇인지 이해하려면 주변 단어에 주의를 기울여야 합니다. 그래서, 자기 주의 메커니즘은 기계가 정확히 이것을 하는 방법이에요.

하지만, 이게 어떻게 작동하는 걸까요? 복잡해 보이지만 그렇지 않아요. 제 말 좀 들어주세요.

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

먼저 시퀀스의 각 임베딩은 Q, K, V 세 개의 벡터로 변환되어 프로젝션됩니다.

![image](/assets/img/2024-07-13-IsThisTheSecretToGooglesSuccessOverChatGPT_1.png)

하지만 이 벡터들이 무엇일까요? 간단히 말해서, 이들은 원래 단어를 나타내는 임베딩끼리 서로 소통할 방식으로, 각각이 단어에 대해 특정한 정보를 전달합니다.

- 질문(쿼리) 벡터는 '이게 내가 찾는 거야'라고 말하는 단어의 방식입니다.
- 키(키) 벡터는 '이게 내가 제안하는 거야'라고 말하는 단어의 방식입니다.
- 그리고 값(밸류) 벡터는 '나에게 주의를 기울이고 싶다면, 내가 너에게 돌려줄 건 이거야'라고 말하는 단어의 방식입니다.

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

이런 식으로, 단어들은 서로 관련된 Query 벡터(Q)를 곱하여 더 나은 의사소통을 이룰 수 있어요.

서로 연관성을 가진 원리를 되돌아보면, 한 단어의 쿼리 벡터(Q)가 다른 단어의 키 벡터(K)와 유사하다면, 두 벡터를 곱하면 결과가 높아져 둘 사이에 주의를 기울여야 함을 나타낼 거예요.

그럼, 값을 나타내는 벡터(V)를 사용하여, 각 단어 임베딩은 주변 정보로 업데이트될 거예요.

실제 계산은 이렇게 보일 거에요:

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

🌟 이제 우리는 다른 단어가 제공하고자 하는 정보를 얻으면 이를 합쳐 새로운 업데이트된 단어 임베딩인 벡터 'Z'를 얻게 됩니다. 그러나 이제 주변 맥락을 포함합니다.

다음은 피드포워드 레이어로 넘어가 봅시다. 이 부분도 아주 중요해요.

## 비선형성의 중요성

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

새롭게 업데이트된 단어 임베딩을 사용하여, 우리는 그것들을 피드포워드 레이어(FFNs)로 보냅니다.

셀프 어텐션과 같은 세부 사항까지는 들어가지 않겠습니다만, 이 레이어는 모든 임베딩을 동일하게 업데이트한다는 것을 간단히 언급해 보겠습니다.

요약하자면, FFNs로부터 추출해야 할 주요 포인트는, 프로세스의 중요한 부분이지만 연산량과 메모리 요구량을 심각하게 증가시킨다는 것입니다.

이제 전체 임베딩 순서가 동시에 LLM에 삽입된다는 사실과 같이, 다루지 않은 중요한 개념이 하나 더 있습니다.

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

잠깐, 뭐라고요?

## 잠깐… 언어는 순차적인데요?

비록 단어들이 순서대로 나오긴 하지만 ChatGPT나 Gemini와 같은 모델은 모든 단어를 동시에 처리하므로 방금 설명한 작업들이 텍스트 시퀀스 전체에 동시에 수행됩니다.

따라서, 이 프로세스는 고도로 병렬화되어 그래픽 처리 장치 또는 GPU의 사용이 필수적이 됩니다. 그리고 요즘에 GPU가 중요하다고 말하는 것은 큰 저평가입니다.

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

중요성을 전달하기 위해 놀라운 통계를 하나 알려드릴게요:

하지만 Ring Attention이라는 GPU 패러다임이 완전히 바뀔 수도 있어요.

## 엄청난 메모리 문제

Transformer가 데이터를 모델링하는 데는 좋지만, 엄청난 문제가 있어요: 메모리 관리 측면에서 특히 비효율적이며, 기업이 데이터 센터를 과도하게 확장하도록 하죠.

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

하지만 이유가 무엇일까요?

## 기억의 병목 현상

우리가 이전에 설명한 과정을 되돌아보면, 현재의 모델들이 가진 매개변수의 양이 1조개를 넘는 경우도 있어서 어마어마한 작업을 수행한다는 것을 알 수 있습니다.

이 모델들은 엄청난 양의 계산이 필요하지만, 실제 병목 현상은 그들이 필요로 하는 엄청난 양의 메모리에서 나타납니다.

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

우선, 전체 모델은 주로 메모리에 저장됩니다(특히 GPU의 High-bandwidth Memory (HBM) 메모리 계층에).

그러므로 만약 모델이 십진법으로 1조 개의 매개변수를 가지고 있다면, float16 정밀도를 가정할 때(각 매개변수가 메모리에 16비트(또는 2바이트)를 차지한다는 것을 의미), 추론 중에 모델을 호스팅하려면 2000 기가바이트의 HBM 메모리 저장 공간이 필요하거나 2 테라바이트가 필요합니다.

안타깝게도, 최첨단 GPU가 HBM 80 기가바이트의 최대 용량을 갖고 있는 것을 감안하면, 파일을 저장하려면 이를 25개 필요로 합니다.

하지만 이것이 전부가 아닙니다.

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

이전에 설명한 주의 메커니즘은 단어들이 동일한 순서에서 예측되면서 계산이 지나치게 중복됩니다.

이는 우리가 이전에 설명한 Key와 Value 활성화를 저장해야 한다는 것을 의미합니다. 이로 인해 다시 계산하지 않아도 되며, 이를 KV Cache라고 합니다.

그러나 문제는, 긴 시퀀스의 경우 KV Cache가 급증하여 이 문제로 인해 LLM 엔지니어들이 모델을 큰 텍스트 시퀀스를 처리할 수 있도록 확장하는 것을 방해했다는 것입니다.

![이미지](/assets/img/2024-07-13-IsThisTheSecretToGooglesSuccessOverChatGPT_3.png)

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

하지만 갑자기, 제 1.5세대 Gemini가 이전에 20만 토큰으로 설정된 최대 시퀀스 길이 기록을 Claude 2가 세운 것을 50배로 늘려 1천만 토큰 또는 750만 단어를 동시에 처리합니다.

과장된 얘기죠.

Google은 아직 확인하지는 않았지만, 그들이 그걸 어떻게 했는지 알 수도 있습니다.

# 트랜스포머와 반지의 제왕

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

**반지 주의**는 GPU 간 Transformer의 계산을 분산하는 새로운 방법을 제안합니다.

## 일괄 처리(batch)에서 시퀀스로

이전에 설명한 대로 Transformer 아키텍처는 맥락을 압축하지 않아 메모리 사용면에서 매우 비효율적입니다. 새로운 단어를 예측할 때마다 전체 시퀀스를 반복해서 '보기' 때문입니다.

결과적으로 각 텍스트 시퀀스마다 GPU의 HBM 메모리에 주의 집중 활성화(KV 캐시)를 저장해야 하며, 이는 단어 예측마다 크기가 계속 커집니다.

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

언급한 대로, 일련의 크기가 커짐에 따라 KV 캐시가 주 메모리 병목 현상이 되어 LLM 공급 업체가 모델로 보낼 수 있는 일련의 최대 길이를 제한해야 한다는 것, 다시 말해 콘텍스트 창으로 알려진 것이다.

그리고 여기서 RingAttention이 근본적인 적응 작업을 수행하는 방식이 등장하는 것입니다. 이제 배치를 나누는 대신 일련의 시퀀스를 분산시킵니다.

## 통신 중첩

Ring Attention의 혁신적인 접근 방식의 핵심 직감은 매우 긴 시퀀스의 경우 이를 '블록'으로 분할하고 각 블록을 다른 GPU로 보내는 것입니다.

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

예를 들어, 만약 100,000개의 토큰으로 이루어진 아주 긴 시퀀스가 있고 10개의 GPU가 있다면, 각 GPU는 평균적으로 시퀀스의 10,000개의 토큰을 받아야 합니다.

이 작업이 가능한 이유는, 앞서 말씀드린 것처럼, Transformer 구조는 한 번에 모든 단어를 처리하기 때문입니다.

우리를 다음 이미지로 이끄는 이유는, Ring Attention이 컴퓨트를 분배하기 위한 제안을 볼 수 있는 이미지입니다:

![Ring Attention](/assets/img/2024-07-13-IsThisTheSecretToGooglesSuccessOverChatGPT_4.png)

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

이해하기 위해 한번 해보죠. 우리가 "The quick brown fox jumps over the lazy dog"라는 문장과 GPU 세 개를 갖고 있다고 상상해봅시다.

이 문장을 세 개의 블록으로 나누어 각각의 GPU에 할당하는 방식으로 처리합니다. "The quick brown", "fox jumps over", "the lazy dog"와 같이 한 GPU당 한 블록의 텍스트가 들어가게 됩니다. 이 블록 단위로 처리하는 방식이기 때문에 두 개의 반복문이 필요합니다:

- 외부 반복문. 각 블록의 쿼리를 해당 호스트(GPU)에 할당합니다. 다시 말해, "The", "quick", "brown"의 Q 벡터들은 첫 번째 GPU에 할당되고 그렇게 순서대로 할당됩니다.
- 내부 반복문. 반면, 각 GPU는 자신에게 할당된 텍스트 블록의 쿼리에 대한 self-attention을 계산하고 해당 토큰들에 대한 feedforward 연산을 수행합니다. 이 경우 첫 번째 GPU는 "The", "quick", "brown" 세 개의 토큰에 대해 self-attention을 수행하고 이를 FFN 레이어를 통과시킵니다.

하지만, 뭔가 부족합니다. 우리가 self-attention 설명을 떠올려보면, 특정 토큰의 쿼리 벡터 Q는 어떤 경우에도 그 직전 토큰들의 키 벡터(K)와 곱해져야 합니다.

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

다시 말해, "fox" 토큰이 GPU 번호 2에 할당되었기 때문에, 첫 번째 GPU에 할당된 "The", "quick" 및 "fox" 세 개의 토큰의 K와 V 벡터를 볼 필요가 있습니다.

따라서 어텐션 메커니즘이 작동하려면 GPU들 간에 서로 Key와 Value 벡터를 공유해야 합니다. 그렇게 하면 곱셈을 수행할 수 있습니다.

여기서 'Ring'이 Ring Attention에서 의미를 갖기 시작합니다.

각 GPU는 자신의 블록에 대한 self-attention 메커니즘 및 피드포워드 연산을 병렬로 계산하는 동시에, 각 GPU는 다음 GPU로 자신의 K와 V 벡터를 보내고 동시에 이전 GPU로부터 해당 정보를 수신하여 종 모양의 링을 생성하고 외부 루프를 만듭니다.

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

**이 구조에서 우리는 핵심 원칙을 도출합니다.**

그러나, 만약 내부 루프의 계산이 전송보다 먼저 끝난다면, GPU는 이전 GPU로부터의 K/V 벡터를 기다려야 하며, 이는 지연을 초래하고 결국 이 구조의 목적을 무효화시킵니다.

그렇다면 이 모든 것의 주요 포인트는 무엇일까요?

## 더 낮은 메모리, 더 많은 길이

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

이 방법의 장점은 간단합니다.

- 이 링 기반 GPU 분배를 사용하여, 우리가 이전에 설명한 것처럼 각 GPU가 KV 캐시의 거대한 크기로 매우 긴 시퀀스로 즉시 80GB의 메모리를 포화시키는 병목 현상을 제거합니다.
- 게다가 시퀀스 길이는 GPU 개수에 선형적으로 확장되므로, 이론적으로 무한대로 증가할 수 있습니다.

결론적으로 Google이 돈에 쩔어있는 것을 감안하면, 이 방법을 사용하여 그들은 시퀀스 길이를 알아볼 수 없는 크기로 지속적으로 확장할 수 있게 되어, 발표된 1000만 토큰 컨텍스트 창이 단순히 빙산의 일각에 불과할 수 있다는 것을 예측하는 것은 어렵지 않습니다.

요컨대, 아직 일러 연주하기에는 이른 단계지만, 링 어텐션은 우리를 새로운 인공지능 시대로 인도할 전환점일 수 있습니다.

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

## 오픈 소스에 대한 마지막 이야기

매일 연구자들이 무지의 장막을 조금씩 밀어낼 새로운 혁신적 방법을 찾는 것을 지켜보는 것은 정말 흥미로워요.

링 어텐션을 통해 산업이 맥락 크기 측면에서 폭발적인 성장을 경험할 것으로 보이는데, 이는 비디오나 DNA와 같이 훨씬 복잡한 데이터 유형을 다루는 우리의 길을 중요한 요소로 만들어줄 것입니다.

하지만, 링 어텐션은 또한 하드웨어의 중요성에 대해 깊은 통찰력을 제공해줍니다. 이는 AI 경쟁에서 승리를 결정짓는 중요한 요소가 될 수 있다는 점을 보여줍니다.

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

시퀀스 길이는 다양한 용도에 극도로 중요하지만, 이를 실행하기 위한 충분한 자본과 자원이 있어야만 합니다. 이러한 긴 시퀀스를 여러 GPU에서 실행하는 것이 필수적입니다.
무엇이 문제냐면?
현재는 그 게임을 할 수 있는 플레이어가 소수뿐이며, 다른 방향을 바라보게 할 만한 단서가 전혀 없습니다.
사실, 상황은 더 나빠질 수도 있습니다.
