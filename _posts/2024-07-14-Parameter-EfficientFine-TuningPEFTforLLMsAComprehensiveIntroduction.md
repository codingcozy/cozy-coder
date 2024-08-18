---
title: "대형 언어 모델을 위한 계량적 파인 튜닝PEFT 완벽 가이드"
description: ""
coverImage: "/assets/img/2024-07-14-Parameter-EfficientFine-TuningPEFTforLLMsAComprehensiveIntroduction_0.png"
date: 2024-07-14 02:06
ogImage:
  url: /assets/img/2024-07-14-Parameter-EfficientFine-TuningPEFTforLLMsAComprehensiveIntroduction_0.png
tag: Tech
originalTitle: "Parameter-Efficient Fine-Tuning (PEFT) for LLMs: A Comprehensive Introduction"
link: "https://medium.com/towards-data-science/parameter-efficient-fine-tuning-peft-for-llms-a-comprehensive-introduction-e52d03117f95"
isUpdated: true
---

대형 언어 모델 (LLMs)은 이름대로 상당히 큽니다. 이러한 모델은 일반적으로 70억 개의 매개변수를 갖습니다. 70억 개의 매개변수를 완전한 정밀도로 불러오려면 280기가바이트의 GPU 메모리가 필요합니다! 해당 모델을 학습하기 위해서는 수백만 혹은 수십억 개의 문서에 걸쳐 수십억 개의 토큰을 업데이트해야 합니다. 이러한 매개변수를 업데이트하기 위해서는 상당한 계산이 필요합니다. 이러한 모델들의 자가지도 학습 비용은 회사들에게 최대 1억 달러까지 소요될 수 있습니다.

그럼 나머지 우리는 어떻게 해야 할까요? 우리는 이러한 모델에 우리의 데이터를 적응시키는 데 관심이 많습니다. 우리는 상대적으로 한정된 데이터셋과 컴퓨팅 파워가 부족한 상황에서 어떻게 하면 주요 기업들을 뛰어넘을 모델을 저렴한 비용으로 구축할 수 있을까요?

이것이 바로 매개변수 효율적인 미세 조정 (PEFT) 연구 분야가 중요한 이유입니다. 이후 자세히 살펴볼 다양한 기술을 통해, 우리는 이러한 모델의 작은 부분을 보강하여 우리가 수행하려는 작업에 더 적합하도록 만들 수 있습니다.

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

이 기사를 읽은 후에는 Hugging Face에 적용된 각 PEFT 기술을 개념적으로 파악하고 그들 간의 차이를 구별할 수 있을 것입니다. 이 기사 이전에 제가 찾았던 가장 도움이 되는 개요 중 하나는 Reddit 댓글에서였습니다. lightning.ai(파이토치 라이트닝의 창시자들)에서 제공하는 또 다른 훌륭한 기사도 있습니다. 또한, 이 글의 많은 부분이 기반으로 한 포괄적인 조사 보고서도 있습니다. 나는 이 자료를 검토하면서 식별한 공백을 다루기 위해 이 기사를 쓰기로 했습니다. 본 글은 현재 Hugging Face 라이브러리에 있는 모든 PEFT 방법들에 대한 개념적 가이드 역할을 합니다. 독자들에게 목표는 다른 PEFT 기술들에 대한 기본적인 이해를 갖고 연구 문헌에 접근하는 것입니다.

## 자기 성찰을 위한 순간: 세밀한 조정할 시간인가요?

이전에는 LLMs의 세밀한 조정에 대한 고려사항에 대해 기사를 쓴 적이 있으며, 인-컨텍스트 학습을 통해 유사한 성능을 달성할 수 있다는 것을 언급했습니다. 그 이후 Llama 2가 출시되었고 오픈 소스 LLM 세계에서 많은 개선이 이루어졌습니다. 이 기사를 바탕으로 더 확장된 생각을 공유할 수 있습니다.

세밀한 조정은 고유 조직에 대해 잠재적으로 위험할 수 있습니다. 최근 논문에서는 LLMs가 자신의 학습 데이터 중 적어도 1%를 기억할 수 있다는 사실이 나왔습니다. 잠재적인 데이터 복제가 있는 경우, 그 1%의 기준은 더 높아집니다. 세밀하게 조정된 LLMs를 내부 사용자가 아닌 사용자에게 제공할 예정이라면, 해당 데이터를 제공하는 것이 괜찮은지 스스로 물어보세요. 사용자들은 모델에 악의적인 동작을 할 수 있습니다. 나는 이 보안 위험에 대한 빠른 개요 역할을 하는 LinkedIn 게시물을 올렸습니다. 데이터를 제공할 수 없다면, ICL과 동적 관찰 선택이 최선의 옵션 중 하나입니다(자세한 내용은 다른 기사를 참고하세요).

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

학습 작업에 대한 고품질 데이터 라벨을 작성하는 것이 매우 중요합니다. 만약 조직이 프로젝트의 세밀한 조정을 지원하기 위한 탁월한 데이터에 대한 헌신이 부족하다면, 대안적인 접근 방법을 고려하는 것이 좋습니다. 모델은 고품질 라벨링된 입력값에 기반하여 번성합니다. 이에 대한 이해관계자들의 헌신이 인간 라벨러에게 존재하지 않는다면, 모든 관련 당사자들에게 실망을 안기게 될 수 있습니다.

## PEFT를 실제로 사용하는 사람들은 누구일까요?

PEFT는 언어 모델을 세밀하게 조정할 수 있는 기능을 제공하는 대부분의 제공업체에서 사용됩니다. 만약 해당 제공자가 이미 이러한 기술을 사용하지 않는다면, 그들이 계획 중이라고 보장합니다. 본 문서에서는 Hugging Face PEFT의 기법들을 모두 다루고 있습니다. 구글이 버텍스 AI에서 기초 모델 튜닝에 대한 이야기를 하는 소개 비디오에서 Lialin 등의 설문 조사가 인용되었습니다. 버텍스 AI는 좀 더 블랙 박스의 성격이지만, 어댑터, 프롬프트 튜닝, 그리고 최근에는 로라의 사용이 영업 홍보에서 언급된 바 있습니다. 정확히 무슨 용도로 사용되는 지는 명확하지 않지만, 우리가 여기서 논의하는 기술들이 전반적으로 시스템을 구동하는 역할을 합니다.

OpenAI는 세밀한 조정을 제공하긴 하지만, 유명한 PEFT 방법은 아직 구현되지 않았습니다. 몇 달 전 OpenAI가 삭제를 요청한 블로그 포스트를 기반으로 합니다. 이 문서는 OpenAI가 어댑터나 로라를 사용하여 세밀한 조정을 보다 계산 효율적으로 만들지 않는다는 내용을 다룹니다. OpenAI로부터 이것이 구현되었다는 발표는 없었으므로, 사용자들에게 이러한 기능이 아직 사용 가능하지 않다는 가정이 안전합니다. 이는 OpenAI의 로드맵에 포함되어 있으며, 세밀한 조정이 일반 모델 사용보다 훨씬 수익성이 높기 때문에 곧 사용 가능할 것으로 예상됩니다.

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

## 퀵 트랜스포머 리뷰

이 글의 독자분들이 트랜스포머 아키텍처에 익숙하다고 가정합니다. 당신이 자가 주의나 다른 구성 요소들의 세세한 내용에 집중할 필요는 없지만 Vaswani et al. [3]를 훑어본 적이 있고 The Annotated Transformer를 살펴봤으면 좋겠다고 생각합니다(내 의견으로는 트랜스포머를 배우기 좋은 최고의 자료죠).

트랜스포머 블록을 위한 의사 코드를 포함할 것입니다. 트랜스포머에 대해 잘 모르더라도, 핵심이 이렇게 작동한다는 것만 알고 있으면 됩니다:

```python
def self_attention(x):
    k = x @ W_k
    q = x @ W_q
    v = x @ W_v
    return softmax(q @ k.T) @ v

def transformer_block(x):
    """[2]에 근거한 작성자의 의사 코드"""
    residual = x
    x = self_attention(x)
    x = layer_norm(x + residual)
    residual = x
    x = FFN(x)
    x = layer_norm(x + residual)
    return x
```

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

그 의사 코드에서 설명된 대로 Vaswani et al.의 모든 기능은 다음과 같습니다. FFN은 피드 포워드 네트워크로, 우리 목적을 위해 2개의 층으로 구성되어 있습니다. 이어지는 많은 PEFT 기법들은 트랜스포머 블록이나 셀프 어텐션에 변경 사항을 가하므로 이 가이드를 읽으면서 이 의사 코드를 참조하여 변경할 것입니다.

# PEFT 메소드 여행

![이미지](/assets/img/2024-07-14-Parameter-EfficientFine-TuningPEFTforLLMsAComprehensiveIntroduction_1.png)

위 다이어그램의 넓은 범주를 살펴보면서 각 기법을 하나씩 살펴보겠습니다. 우리가 다룰 클래스는 Additive, Adapters, Soft-Prompts, Reparameterization 그리고 Reparameterization과 Selective을 결합한 하이브리드 방법입니다(Sparse LoRa가 아닙니다).

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

## 추가적인 방법

가산 방법은 아마 가장 이해하기 쉬울 것입니다. 가산 방법의 목표는 모형을 보강하기 위해 추가 매개변수나 네트워크 레이어를 더하는 것입니다. 데이터를 세밀하게 조정할 때에는 새로 추가된 매개변수의 가중치만 업데이트합니다. 이는 훈련을 계산적으로 더 쉽게 만들어주고 작은 데이터셋에 적응할 수 있습니다 (처음에는 100~500으로 생각하고, 최대 100,000 가량까지).

## 방법: 어댑터

어댑터는 방법이자 클래스입니다. 이 기술은 Houlsby et al [4]에서 소개되었습니다. 어댑터의 목표는 트랜스포머 서브 레이어 뒤에 작은 완전 연결 네트워크를 추가하여 해당 매개변수를 학습하는 것입니다. [2]의 정의를 따라, 저는 네트워크에 완전 연결 계층만 추가하는 것만 어댑터로 정의합니다.

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

Houlsby et al.은 트랜스포머 블록에 간단한 업데이트를 제안합니다. 그들은 fully connected 레이어를 두 군데에 추가합니다. 아래의 의사 코드에서 보여지는 대로요.

```python
def transformer_block_adapter(x):
    """[2] 에서 가져온 의사 코드"""
    residual = x
    x = self_attention(x)
    x = FFN(x)  # 어댑터
    x = layer_norm(x + residual)
    residual = x
    x = FFN(x)
    x = FFN(x)  # 어댑터
    x = layer_norm(x + residual)
    return x
```

## 방법: (IA)³

내부 활성화를 억제하고 증폭시켜 속입시어를 부합시킨 Infused Adapter, 즉 (IA)³는 매우 흥미로운 부가 방법입니다(파라미터 추가). 이 방법은 2022년에 Liu et al. [5]에 의해 제안되었습니다. 이름과 달리, 이는 엄격히 말해 fully connected 레이어를 트랜스포머 블록의 sub-layer 뒤에 추가하는 어댑터 방법은 아닙니다.

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

일반 트랜스포머에서 찾을 수 있는 스케일된 도트-프로덕트 어텐션에 대해 고려해 봅시다:

이 덧셈 방식으로 작업하기 때문에 네트워크에 매개변수를 추가하고자 합니다. 차원이 상당히 작아지기를 원합니다. (IA)³는 어텐션 메커니즘에 다음과 같은 새로운 벡터를 제안합니다:

우리는 방금 열 벡터 l_k와 l_v를 추가했고 열 벡터와 행렬 사이에 Hadamard 곱을 수행합니다(열 벡터를 행렬의 모든 열에 곱합니다).

또한 feed forward 레이어에 추가되는 학습 가능한 또 다른 열 벡터 l_ff를 도입합니다. 아래와 같이 feed forward 레이어에 추가됩니다:

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

이 예제에서는 감마가 가중치와 입력값의 곱에 적용되는 활성화 함수입니다. 이것은 (IA)³를 위한 의사 코드입니다:

```js
def self_attention_ia3(x):
    k = x @ W_k
    q = x @ W_q
    v = x @ W_v

    k = l_k @ k  # ia3
    v = l_v @ v  # ia3

    return softmax(q @ k.T) @ v

def transformer_block_ia3(x):
    """[2]에서 영감을 받은 의사 코드"""
    residual = x
    x = self_attention_ia3(x)
    x = layer_norm(x + residual)
    residual = x
    x = x @ W_1  # 일반 transformer
    x = l_ff * gelu(x)  # ia3
    x = x @ W_2
    x = layer_norm(x + residual)
    return x
```

## 소프트-프롬프트

소프트-프롬프트를 이해하기 전에, 대부분의 독자들이 익숙할지도 모르는 하드-프롬프트에 대해 먼저 이야기해 봅시다. 하드 프롬프팅에서는, 우리는 해당 작업을 나타내는 프롬프트 데이터 세트를 구성합니다. 누군가 네트워크와 상호작용할 때 질문을 제기하면, 그들은 다른 방식으로 표현할 수 있습니다. 하드 프롬프팅에서, 프로세스는 특정 작업이 어떻게 구성될 수 있는지 다양한 방법을 다루는 데이터 셋을 모으는 것을 포함합니다.

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

Soft-prompting은 데이터 집합을 만들지 않으려는 기술입니다. 하드 프롬프팅에서는 이산 표현(단어 선택)으로 데이터를 생성합니다. 소프트 프롬프팅에서는 모델에 입력할 텍스트의 연속적인 표현을 찾습니다. 이는 훈련 중인 예제에 대한 정적 프롬프트가 필요하다는 것을 의미합니다.

기술에 따라 네트워크에 정보가 추가되는 방법이 다릅니다. 핵심 아이디어는 기본 모델이 텍스트 자체를 최적화하는 것이 아니라 프롬프트 텍스트의 연속적인 표현(즉, 학습 가능한 텐서 유형)를 최적화한다는 것입니다. 이는 임베딩 형식이거나 해당 임베딩에 적용된 어떤 변환일 수 있습니다. 이러한 기술에 대해 자세히 살펴볼 것입니다.

## 방법: 프롬프트 튜닝

![image](/assets/img/2024-07-14-Parameter-EfficientFine-TuningPEFTforLLMsAComprehensiveIntroduction_2.png)

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

프롬프트 조정은 Lester et al.의 기법 중 하나로, 소프트 프롬프트 범주에 속합니다. 소프트 프롬프트를 사용할 때 우리의 목표는 현재 작업에 더 구체적인 정보를 추가하는 것입니다. 프롬프트 조정을 통해 우리는 프롬프트 토큰을 위한 매개변수 세트를 생성하고 이를 네트워크의 시작 부분에 삽입하여 이를 달성합니다.

소프트 프롬프트의 표현을 찾기 위해 우리는 훈련 중에 사용되는 정적 프롬프트를 위한 임베딩 세트를 만듭니다. 출력 임베딩을 순서 임베딩과 연결합니다. 새로운 정보를 사용하여 이를 언어 모델에 전달합니다. 이러한 이중 정보를 생성함으로써 동일한 작업을 위해 여러 프롬프트를 만들 필요 없이 소프트 프롬프트의 매개변수화를 학습할 수 있습니다.

```js
def prompt_tuning(seq_tokens, prompt_tokens):
    """ [2]의 의사 코드. """
    x = seq_embedding(seq_tokens)
    soft_prompt = prompt_embedding(prompt_tokens)
    model_input = concat([soft_prompt, x], dim=seq)
    return model(model_input)
```

이 접근 방식을 통해 세밀한 조정의 수많은 혜택이 있습니다. 이 새로운 매개변수 세트는 매우 작을 수 있으며, 기본 모델의 조정 가능한 매개변수 크기의 약 0.01% 정도입니다. 이는 동일한 기본 모델을 기반으로 실행되는 작업별 모델 앙상블을 구축하는 기회를 제공하며, 모델의 메모리 요구 사항을 크게 감소시킵니다. 이에 대해 더 알아보려면 LinkedIn에서 공유한 이 게시물과 [3]의 앙상블 섹션을 확인해보세요.

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

## 방법: 접두사 튜닝

접두사 튜닝은 프롬프트 튜닝과 매우 유사한 부드러운 프롬프팅 기술입니다. 프롬프트 튜닝에서 우리는 입력을 공급하고 출력을 텍스트 입력의 연속적 표현에 추가하는 별도의 매개변수 집합을 생성했습니다. 접두사 튜닝에서 우리는 또한 베이스 모델에 입력되는 별도의 프롬프트 토큰 세트에서 연속적 표현을 찾습니다.

접두사 튜닝과 프롬프트 튜닝의 차이점은 접두사 튜닝의 표현이 트랜스포머의 모든 레이어에 공급된다는 점인 반면, 프롬프트 튜닝은 임베딩에만 연결되었습니다. 또한, 접두사 튜닝의 부드러운 프롬프트를 위해 추가적인 매개변수를 완전 연결 네트워크 형태로 학습합니다. FFN을 훈련한 후에는 버리고 부드러운 프롬프트만을 입력으로 사용합니다.

\`\`\`python
def transformer_block_prefix_tuning(x, soft_prompt):
"""[2]에서 가져온 가상 코드"""
soft_prompt = FFN(soft_prompt)
model_input = concat([soft_prompt, x], dim=seq)
return model(model_input)
\`\`\`

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

## 방법: P-Tuning

![이미지](/assets/img/2024-07-14-Parameter-EfficientFine-TuningPEFTforLLMsAComprehensiveIntroduction_3.png)

P-Tuning은 다른 부드러운 프롬팅 방법인 prompt와 prefix tuning과는 다른 방법으로, Liu 등에 의해 소개되었습니다. 일상적인 용어로 말하면 P-Tuning은 LSTM을 사용하여 프롬프트를 인코딩하는 것으로 생각할 수 있습니다.

P-Tuning은 저자들이 발견한 두 가지 문제를 해결하기 위해 시작되었습니다. 첫 번째는 모델에 전달된 단어 임베딩의 이산성입니다. 저자들은 임베딩이 무작위로 초기화되고 확률적 경사 하강을 통해 최적화된다면, 모델이 지역 최소값에 빠질 가능성이 있다고 주장합니다. 두 번째는 단어 임베딩의 연관성입니다. prompt-tuning 및 prefix-tuning의 매개변수화에서는 부드러운 프롬프트가 사실상 서로 독립적이라고 여겨집니다. 저자들은 프롬프트 토큰이 서로 종속되도록 하는 방식을 원했습니다.

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

저자들은 프롬프트가 문맥 x와 대상 y를 가져와서 자체를 템플릿 T로 구성하는 함수라고 제안합니다. 저자들은 "영국의 수도는 [MASK]입니다"라는 예시 시퀀스를 제공합니다. 여기서 프롬프트는 "영국의 수도는 ..."이고, 문맥은 "영국"이며 대상은 [MASK]입니다. 우리는 이 수식을 사용하여 문맥 이전과 대상 이전의 토큰 시퀀스 두 개를 만들 수 있습니다. 이 추가 정보의 표현을 학습하여 연속적인 출력으로 줄이고 언어 모델로 공급할 수 있습니다.

이 방식으로 프롬프트를 임베딩하기 위해, 우리는 작은 LSTM 네트워크와 두 개의 FFN 레이어를 포함한 소형 네트워크를 사용합니다. 프롬프트 토큰, 문맥 이전과 대상 이전 후의 토큰을 전달합니다.

```python
def p_tuning(seq_tokens, prompt_tokens):
    """작성자가 작성한 p-tuning의 의사 코드."""
    h = prompt_embedding(prompt_tokens)
    h = LSTM(h, bidirectional=True)
    h = FFN(h)

    x = seq_embedding(seq_tokens)
    model_input = concat([h, x], dim=seq)

    return model(model_input)
```

## 방법: LLaMA-어댑터

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

![PEFT](/assets/img/2024-07-14-Parameter-EfficientFine-TuningPEFTforLLMsAComprehensiveIntroduction_4.png)

안녕하세요! 오늘은 LLaMA 어댑터에 대해 이야기해보려고 해요. LLaMA 어댑터는 Zhang 등이 소개한 소프트 프롬프팅 기술인데, 이 기술은 람마 모델에 더 효율적인 버전의 프리픽스 학습을 적용해요.

LLaMA 어댑터는 Prefix Tuning과 다소 다른 점이 있어요. 적응 프롬프트라 불리는 것을 도입하는데, 이것은 소프트 프롬프트로서 입력과 트랜스포머 레이어에 추가된 것이에요. 이러한 적응 프롬프트는 N개의 트랜스포머 레이어 중 상위 L개에 삽입돼요.

저자들은 또한 제로 초기화 어텐션을 소개했어요. 이러한 부가 방법을 통해 가중치에 일정한 무작위 초기화가 있는 새로운 매개변수 집합을 도입해요. LM에 이 무작위 노이즈가 추가되면 초기 단계에서 큰 손실 값을 유발할 수 있는 불안정한 미세 조정 경험을 할 수 있어요. 이 문제를 해결하기 위해 저자들은 초기화된 0의 값으로 설정된 게이팅 요소를 소개했어요. 자가 주의 메커니즘이 이 게이팅 요소와 곱해진 결과가 제로 초기화 어텐션이라고 불리고 있어요. 게이팅 값은 훈련 단계를 통해 적응적으로 조정돼 네트워크 매개변수의 부드러운 업데이트를 만들어내요.

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

```python
def transformer_block_llama_adapter(x, soft_prompt, gating_factor):
    """LLaMA-Adapter pseudocode created by Author"""
    residual = x

    adaption_prompt = concat([soft_prompt, x], dim=seq)
    adaption_prompt = self_attention(adaption_prompt) * gating_factor  # zero-init attention

    x = self_attention(x)
    x = adaption_prompt * x
    x = layer_norm(x + residual)
    residual = x
    x = FFN(x)
    x = layer_norm(x + residual)

    return x
```

**Reparameterization-Based Methods**

Reparameterization-Based methods focus on finding low-dimensional representations of the same weight matrices found in the base model. The first connection between fine-tuning and a low-dimensional representation was shown in Hu et al [8]. The authors make a connection between the full parameters of the model and a lower-dimensional representation. Depending on the task, the authors are able to achieve 90% of the results of the fully fine-tuned model with approximately 0.0002% of the trainable parameters.

Method: LoRa

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

![image](/assets/img/2024-07-14-Parameter-EfficientFine-TuningPEFTforLLMsAComprehensiveIntroduction_5.png)

파인튜닝에서 가장 인기 있는 기술 중 하나는 LoRa (Low-Rank Adaptation)라는 재매개변환 기반 방법입니다. LoRa는 최적화로부터의 업데이트를 나타내는 별도의 행렬을 학습하여 가중치 행렬을 업데이트합니다. 이 차이를 나타내는 두 개의 작은 차원 가중치 행렬을 만들어 더 적은 매개변수를 학습할 수 있게 됩니다.

LoRa를 훈련시키기 위해 경사 하강법의 기본 아이디어를 사용합니다. 여기서 일련의 매개변수를 점진적으로 조정하여 목표(손실 함수)에 더 가까워지도록 합니다. LoRa에서는 모든 업데이트를 별도의 행렬로 격리하기로 결정합니다. 이 행렬을 ΔW로 표시하며, 파인튜닝 과정에서 학습한 모든 매개변수 업데이트를 나타냅니다.

W_0를 dxk(행 d 및 열 k) 차원으로 할당하고, 해당 매개변수를 업데이트하여 새로운 목표와 일치시키고자 합니다. 매개변수에 대한 이 업데이트를 ΔW로 나타낼 수 있으며, 이 역시 dxk 차원입니다. 아래의 방정식을 사용하여 업데이트 규칙을 모델링할 수 있습니다.

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

이제 업데이트 규칙을 변경하여 ΔW가 행렬 곱셈 AB로 모델링되도록 합시다. 우리는 행렬 A를 차원이 dxr이고 B를 차원이 rxk인 것으로 할당합니다. 행렬 곱셈에 능통하다면 AB가 W_0와 동일한 차원을 가진다는 것을 알 수 있습니다. 따라서 이러한 행렬의 덧셈은 유효합니다. AB가 DeltaW보다 나은 선택인 이유는 다음과 같습니다: 행렬 A는 dxr을 가지고 있으며 행렬 B는 rxk를 가지고 있습니다. 만약 우리가 r을 매우 작은 수로 설정한다면 (일반적인 값인 r=8), A와 B의 매개변수 수는 ΔW보다 크게 작아집니다. 만약 A와 B의 매개변수만을 학습한다면 d*k-d*r-r\*k만큼 더 적은 매개변수를 학습하게 됩니다. 이를 실제로 적용하면 원래 네트워크의 매개변수의 0.1-0.5%만을 학습할 수 있습니다.

방금 제시한 방법이 LoRa가 작동하는 본질입니다. 추가 학습 단계를 통해 행렬 W를 최적화하는 대신, 매개변수가 훨씬 더 적은 두 개의 새로운 행렬 A와 B를 통해 행렬 ΔW를 수정합니다. 이 결과는 매우 적은 매개변수를 최적화하게 도와줌으로써 학습을 훨씬 효율적으로 만들어줍니다.

일반적으로 우리는 이 업데이트 규칙을 트랜스포머 블록의 셀프 어텐션에서 Key와 Value 행렬에 사용합니다. 또한 업데이트에 주어지는 정보량에 대해 1/r로 설정된 스케일링 요소를 추가합니다. 아래의 의사 코드를 확인해보세요.

```js
def lora_linear(x, W):
    scale = 1 / r  # r은 랭크
    h = x @ W
    h += x @ W_a @ W_b  # W_a, W_b는 W에 따라 결정됩니다
    return scale * h

def self_attention_lora(x):
    """ Lialin et al. [2]의 의사 코드."""

    k = lora_linear(x, W_k)
    q = x @ W_q
    v = lora_linear(x, W_v)
    return softmax(q @ k.T) @ v
```

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

## 선택적 방법

선택적 방법을 사용하면 몇 가지 매개변수를 업데이트하고 다른 매개변수를 업데이트하지 않습니다. 이러한 방식의 문제는 우리가 매개변수의 희소 행렬을 만들었다는 점입니다. 희소 행렬 연산은 현재의 GPU에서 잘 지원되지 않으며 계산상의 어려움을 제공합니다. 희소 행렬이 계산상의 어려움을 초래하는 이유에 대해 더 알고 싶다면 [10]을 확인해보세요.

Selective methods에는 실패한 벡터를 제거하거나 모델 바이어스를 조작하는 기술도 있습니다. 이러한 기술은 모델을 훈련하는 과정에서 추가 복잡성을 초래합니다. 일반적으로 이러한 방법은 다른 연산보다 컴퓨팅 작업이 더 비싸기 때문에 구현이 더 어렵습니다.

방법: AdaLoRa

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

이 방법은 리파라미터화 및 선택적 방법에서 아이디어를 결합한 하이브리드 접근법입니다. Zhang 등 [12]은 LoRa를 연구하여 파라미터 효율적인 파인 튜닝의 성능을 향상시키기 위해 모듈의 중요성에 따라 파라미터 예산을 적응적으로 할당하는 방법에 대해 생각한 AdaLoRa를 개발했습니다. 이것이 의미하는 것은 "성능을 향상시키는 데 중요한 파라미터를 모든 파라미터를 동등하게 다루지 않고 우선시하는 방법은 무엇일까?"입니다.

LoRa에서처럼 두 행렬 A 및 B를 사용하는 대신, AdaLoRa는 벡터 공간의 차원을 세 행렬인 P(왼쪽 특이벡터), Lambda(특이값), Q(오른쪽 특이벡터)로 축소하기 위해 특이값 분해 (SVD)의 근사치를 사용합니다. 이 세 행렬을 사용하여 우리의 벡터 공간 Δ의 근사치를 P _ Lambda _ Q로 재구성할 수 있습니다. SVD를 사용하는 이점은 특이값이이 낮은 차원의 공간에서 벡터의 중요성을 나타낸다는 것입니다. 이 논문의 기여는 가중치를 최적화해야 하는 중요성을 고려하기 위해 SVD 관련 접근법을 사용하는 일부 효율적인 구현을 적용한 것입니다.

LoRa에서 우리는 두 행렬 A 및 B로 delta W를 근사했음을 보았습니다. 여기서 람다가 대각선을 따라 값만 가지고 있기 때문에 람다를 열 벡터로 저장합니다. 행렬 P(d x r), Lambda(r x r) 및 Q(r x k)를 가중치 행렬 W(d x k)의 차원과 일치하도록 선택합니다.

다른 혁신적인 결과는 SVD의 어떤 요소를 제거할 수 있는 중요성 샘플링 기술을 사용하는 아이디어입니다. 본질적으로 기술은 SVD의 각 항목을 나타내는 삼중 세트를 고려하고 이러한 요소들이 낮은 차원의 표현에 얼마나 중요한지를 결정하는 것입니다. 중요성 값을 왼쪽/오른쪽 특이벡터와 관련시키는 함수를 사용하여 이를 구현합니다. 이후 그것들은 기울기 가중치 곱의 단계 사이에서 지수 이동 평균을 사용하는 가중치를 위한 가짜 중요성과 현재 단계와 이전 단계 간에 지수로 평균화된 다른 함수인 불확실성 측정과 결합하는 감수성 함수를 통해 실행됩니다.

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

SVD의 요소를 가지치기할 때, 낮은 차원의 rank(행렬의 r 항목)은 가장 중요하지 않은 트리플릿을 삭제함에 따라 반복적으로 변경됩니다. 이는 훈련 단계별로 rank r을 감소시키는 전역 예산 스케줄러를 통해 수행됩니다. 예산은 대상 예산의 크기의 1.5배로 초기화되고, t번의 선행 단계 후에 대상 예산까지 세제곱 감소합니다.

이 개념적으로는 이해하기 어려운 메소드입니다. 기술적으로 관심이 있다면 논문을 읽어 해당 방법의 내부 작업을 이해하는 것이 좋습니다. 덜 중요한 특이 벡터를 가지치기하는 효율적인 SVD 구현으로 이를 Lora에 적용한 것으로 기억하신다면, 개념적으로 안전합니다.

```python
def adalora_linear(x, W, curr_sv):
    scale = alpha / r  # r은 rank입니다
    h = x @ W

    # p, 람다, q는 W 행렬과 관련됩니다
    # curr_sv는 현재 최적화 중인 특이 벡터를 표시합니다.
    h += x @ p[curr_sv] @ lamda[curr_sv] @ q[curr_sv]
    return scale * h

def self_attention_lora(x):
    """
    저자가 만든 AdaLoRa 의사코드.
    이것은 self_attention 블록의 차이점만 보여줍니다.
    가지치기 기술에 대한 코드는 포함되어 있지 않습니다.
    """
    k = adalora_linear(x, W_k)
    q = x @ W_q
    v = adalora_linear(x, W_v)

    return softmax(q @ k.T) @ v
```

## 메소드 비교

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

한 곳에서 모든 방법을 비교하기 위해 아래 표를 만들어서 추가적인 매개변수 개수(네트워크에 추가되는 모든 매개변수), 방법 유형 및 방법에 대한 비공식적 요약을 보여주었습니다. 비공식적 요약은 이 방법을 처음 듣는 대학생에게 한 문장으로 설명할 때 제가 사용할 내용입니다.

![image](/assets/img/2024-07-14-Parameter-EfficientFine-TuningPEFTforLLMsAComprehensiveIntroduction_6.png)

# 유일한 개념 가이드인가요?

나는 이것이 유일한 개념 가이드라고 주장하는데, PEFT 기법의 기초를 이해할 수 있기 때문입니다. 알아차리셨을 것처럼, 모든 기법이 다른 아이디어를 확장했습니다. 이 소개를 통해 충분한 기초를 이해하게 되므로 나중에 연구 논문을 스스로 살펴볼 수 있습니다. 그러나 개념을 이해하기 위해 다른 가이드가 필요하다면, 기사 댓글에 공유해주십시오. 그렇게 되면 다른 독자들도 이러한 자료를 찾을 수 있습니다!

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

# 시작해보세요!

이 개념적인 리뷰를 마치고 나면, 여러분은 여러분의 모델을 학습시키기 위해 이러한 방법들을 실험해볼 수 있는 좋은 지점에 있게 될 거에요. Hugging Face에서 제공하는 훌륭한 구현 가이드들이 많이 있습니다. 덜 손댈 수 있는 방법을 원하신다면 Google의 Vertex AI 모델을 사용하거나 OpenAI fine tuning을 진행할 수도 있어요.

이 글을 읽어 주셔서 감사합니다! 추가적인 질문이나 애매한 점이 있으시면 댓글을 남겨 주세요. 제가 빠르게 답변드리겠습니다. 이와 같은 기사를 더 보고 싶으시다면, 제 Medium과 LinkedIn을 팔로우해주세요.

이 글에서 기술적인 오류를 발견하셨다면, 즉시 알려주세요! 제가 발행하는 정보가 가능한 정확하도록 노력하고 있지만, 완벽한 사람은 없으니까요.

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

참고 자료:

[1] Nicholas Carlini, Daphne Ippolito, Matthew Jagielski, Katherine Lee, Florian Tramer, & Chiyuan Zhang. (2023). Quantifying Memorization Across Neural Language Models.

[2] Vladislav Lialin, Vĳeta Deshpande, & Anna Rumshisky. (2023). Scaling Down to Scale Up: A Guide to Parameter-Efficient Fine-Tuning.

[3] Ashish Vaswani, Noam Shazeer, Niki Parmar, Jakob Uszkoreit, Llion Jones, Aidan N. Gomez, Lukasz Kaiser, & Illia Polosukhin (2017). Attention Is All You Need. CoRR, abs/1706.03762.

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

[4] Neil Houlsby, Andrei Giurgiu, Stanislaw Jastrzebski, Bruna Morrone, Quentin de Laroussilhe, Andrea Gesmundo, Mona Attariyan, & Sylvain Gelly (2019). Parameter-Efficient Transfer Learning for NLP. CoRR, abs/1902.00751.

[5] Haokun Liu, Derek Tam, Mohammed Muqeeth, Jay Mohta, Tenghao Huang, Mohit Bansal, & Colin Raffel. (2022). Few-Shot Parameter-Efficient Fine-Tuning is Better and Cheaper than In-Context Learning.

[6] Xiao Liu, Yanan Zheng, Zhengxiao Du, Ming Ding, Yujie Qian, Zhilin Yang, & Jie Tang (2021). GPT Understands, Too. CoRR, abs/2103.10385.

[7] Renrui Zhang, Jiaming Han, Chris Liu, Peng Gao, Aojun Zhou, Xiangfei Hu, Shilin Yan, Pan Lu, Hongsheng Li, & Yu Qiao. (2023). LLaMA-Adapter: Efficient Fine-tuning of Language Models with Zero-init Attention.

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

[8] Armen Aghajanyan, Luke Zettlemoyer, & Sonal Gupta (2020). 내재적 차원성이 언어 모델 파인튜닝의 효과를 설명합니다. CoRR, abs/2012.13255.

[9] Edward J. Hu, Yelong Shen, Phillip Wallis, Zeyuan Allen-Zhu, Yuanzhi Li, Shean Wang, & Weizhu Chen (2021). LoRA: 대형 언어 모델의 저랭크 적응. CoRR, abs/2106.09685.

[10] Trevor Gale, Matei Zaharia, Cliff Young, & Erich Elsen. (2020). 딥러닝을 위한 희소 GPU 커널.

[11] Brian Lester, Rami Al-Rfou, & Noah Constant. (2021). 매개변수 효율적 프롬프트 튜닝의 규모의 힘.

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

**[12]** Qingru Zhang, Minshuo Chen, Alexander Bukharin, Pengcheng He, Yu Cheng, Weizhu Chen, & Tuo Zhao. (2023). Adaptive Budget Allocation for Parameter-Efficient Fine-Tuning.
