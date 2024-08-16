---
title: "Segment-Anything ModelSAM의 디코더 작동 방식은"
description: ""
coverImage: "/assets/img/2024-07-10-HowdoestheSegment-AnythingModelsSAMsdecoderwork_0.png"
date: 2024-07-10 00:11
ogImage: 
  url: /assets/img/2024-07-10-HowdoestheSegment-AnythingModelsSAMsdecoderwork_0.png
tag: Tech
originalTitle: "How does the Segment-Anything Model’s (SAM’s) decoder work?"
link: "https://medium.com/towards-data-science/how-does-the-segment-anything-models-sam-s-decoder-work-0e4ab4732c37"
isUpdated: true
---





2024년 7월 10일 - 세그먼트 물체 모델 (Segment-Anything Model, SAM)의 디코딩 과정에 대한 심층 분석을 진행하겠습니다. 이 논문은 SAM의 디코더에 초점을 맞추고, 그 자가 주의와 상호주의 메커니즘이 어떻게 작동하는지에 대해 다뤄보겠습니다.

이 기사에서는 SAM의 인코더가 아닌 디코더에만 초점을 맞추고 있습니다. SAM의 인코더에 관심이 있는 분들은 다른 기사 "세그먼트 물체 모델 (SAM)의 인코더는 어떻게 작동할까요?"를 참조해주세요.

세그먼트 물체 모델 (SAM)은 2D 대화형 세그멘테이션 모델이며, 안내형 모델입니다. SAM은 이미지를 세그먼트화하기 위해 사용자 입력을 필요로 합니다. 이 입력은 모델에게 어디를 세그먼트해야 하는지 알려줍니다. 모델의 출력은 서로 다른 수준의 세그멘테이션 마스크 집합과 각 마스크에 연관된 확신 점수입니다.

<div class="content-ad"></div>

Segmentation mask은 입력 이미지와 동일한 크기의 2D 이진 배열입니다. 이 2D 배열에서 (x, y) 위치에 있는 항목은 해당 위치 (x, y)의 픽셀이 분할된 영역에 속한다고 모델이 판단하면 값이 1입니다. 그렇지 않으면 항목은 0입니다. 이 신뢰 점수는 모델이 각 분할의 품질에 대한 믿음을 나타내며, 높은 점수는 더 높은 품질을 의미합니다.

SAM의 네트워크 아키텍처는 인코더와 디코더로 구성되어 있습니다:
- 인코더는 이미지와 사용자 프롬프트 입력을 받아 이미지 임베딩, 이미지 위치 임베딩 및 사용자 프롬프트 임베딩을 생성합니다.
- 디코더는 다양한 임베딩을 입력으로 받아 세그멘테이션 마스크와 신뢰 점수를 생성합니다.

이 기사는 SAM의 디코더 작동 방식에 중점을 두고 있습니다. 인코더에 대해 다른 기사를 작성할 예정입니다.

<div class="content-ad"></div>

# SAM의 입력과 출력

SAM은 세그먼트화할 입력 이미지 외에도 사용자 프롬프트를 필요로 합니다. 다음과 같은 종류의 프롬프트를 지원합니다.

## 다양한 종류의 입력 사용자 프롬프트

- 마우스 클릭. 마우스 클릭은 모델에게 클릭된 위치를 생성된 세그먼테이션 마스크에 포함할 것을 알리는 긍정적인 클릭일 수 있습니다. 또한, 클릭된 위치를 피하라는 부정적인 클릭일 수도 있습니다. SAM은 모델에 대해 긍정적 또는 부정적 클릭을 여러 번 받을 수 있습니다.
- 바운딩 박스. 바운딩 박스는 항상 긍정적인 신호로, 생성된 세그먼테이션 마스크가 상자 영역 내에 있어야 한다는 모델에게 알려줍니다. 부정적인 바운딩 박스는 없습니다. SAM은 여러 개의 바운딩 박스를 받을 수 있습니다.
- 밀도 마스크. 밀도 마스크는 입력 이미지와 동일한 크기의 2D 이진 배열 형태로 표현됩니다. 이 2D 배열의 값이 1인 항목은 모델에게 마스크로 예측해야 하는 픽셀을 알려줍니다. 밀도 마스크 프롬프트는 항상 긍정적인 신호입니다. SAM은 하나의 밀도 마스크 프롬프트를 받을 수 있습니다.

<div class="content-ad"></div>

Mouse 클릭 및 바운딩 박스 클릭을 희소 프롬프트라고 부릅니다.

### 다중 수준의 출력 마스크 및 확신 점수

SAM의 디코더는 주어진 프롬프트의 모호성을 처리하기 위해 세 가지 수준의 분할 마스크를 생성합니다.

예를 들어 말쥬 클릭 프롬프트가 주어지면, 다음 그림은 예측된 세 가지 수준의 분할 마스크를 보여줍니다. 이 그림에서 초록색 별표는 마우스 클릭 위치를 나타냅니다. 파란 패치는 예측된 분할 마스크를 나타냅니다.

<div class="content-ad"></div>

각 세분화 수준은 예측된 신뢰 점수인 iou 점수를 갖고 있습니다. "iou"는 "교차 부분을 교집합으로 나누었을 때"를 의미합니다. 이는 세분화 모델의 성능을 측정하는 기본 메트릭스입니다. 여기서 교차 부분을 교집합으로 나누는 개념은 엄밀하게는 사용되지 않으므로, 모델 관점에서의 예측 품질을 대략적으로 이해할 수 있는 점수로 이해해주세요.

![image](/assets/img/2024-07-10-HowdoestheSegment-AnythingModelsSAMsdecoderwork_1.png)

## 예측된 마스크는 세 개 또는 네 개?

SAM 모델은 몇 개의 세분화 마스크 수준을 생성하나요? multimask_output 구성을 사용하여 모델은 3개의 마스크 수준 또는 한 가지 수준만 출력하도록 구성될 수 있습니다. 그러나 기저 모델 아키텍처는 항상 4개 요소 및 4개의 신뢰 점수로 마스크 배열(또는 동등하게 텐서)을 생성함으로써 이 두 경우를 함께 처리합니다. SAM이 단일 마스크를 출력하도록 구성된 경우, SAM은 마스크 배열에서 첫 번째 마스크를 반환하고, 그렇지 않은 경우에는 마스크 배열에서 나머지 세 마스크를 반환합니다. 신뢰 점수도 동일한 방식으로 처리됩니다.

<div class="content-ad"></div>

SAM의 코드에 더 가깝게 유지하기 위해, 이 글에서는 언제나 SAM이 네 개의 마스크를 반환한다는 사실을 설명하겠습니다. 이렇게 함으로써, 내 설명에서의 텐서 모양이 코드에서의 모양과 일치하도록 할 것입니다.

# 코드와 차트에서의 디코딩 절차

이제 SAM의 디코딩 절차가 어떻게 작동하는지 이해하기 위해 코드와 네트워크 아키텍처 차트로 빠져들어볼 것입니다.

가끔은 설명을 더 쉽게 하기 위해 코드를 조정해야 할 때가 있습니다. 예를 들어, 동일한 변수 이름이 서로 다른 줄에서 여러 값을 받는 것은 좋지 않은 프로그래밍 스타일이므로 피해야 합니다. 그러므로 이 글에서 보여드리는 코드 스니펫은 원본 SAM의 구현과 동일하지는 않지만 원본에 가까운 내용입니다.

<div class="content-ad"></div>

The SamPredictor.predict_torch method is a great resource for diving into how the SAM decoder operates. 

![SAM Decoder](/assets/img/2024-07-10-HowdoestheSegment-AnythingModelsSAMsdecoderwork_2.png)

This method is divided into three key steps:

1. Generating embeddings for user prompts, code lines 222 to 226.
2. Predicting low-resolution segmentation masks at various levels and their corresponding confidence levels, code lines 229 to 235.
3. Interpolating low-resolution segmentation masks to match the original image size, code lines 237 to 238.

<div class="content-ad"></div>

## 디코딩 과정에서 중요한 텐서들

다음의 플로우차트는 인코딩과 디코딩 과정 중 핵심 텐서들과 그 모양을 강조합니다. 플로우차트는 신경망 아키텍처를 이해하는 데 많은 도움이 되는 중요한 텐서 모양들을 나열합니다. 이 기사에서 배치 차원은 생략되어 모양 표기를 간략화했습니다.

![](/assets/img/2024-07-10-HowdoestheSegment-AnythingModelsSAMsdecoderwork_3.png)

원본 이미지는 임의의 높이 H와 너비 W를 가진 자연 이미지이므로 해당 텐서는 3채널 RGB를 가지며 모양은 3×H×W입니다.

<div class="content-ad"></div>

네트워크에 도달하기 전에 임의 크기의 입력 원본 이미지는 채널을 유지한 채 고정 크기 1024×1024로 조정됩니다.

**이미지 임베딩**
이미지 임베더 ImageEncoderViT 클래스는 조정된 이미지에 대한 임베딩을 생성합니다. 위 스니펫의 self.features인 이미지 임베딩은 feature_dim × embed_height × embed_width의 형태이며, 이는 256×64×64입니다. 이미지 임베딩은 해상도가 낮으며 64×64뿐입니다. 이 낮은 해상도의 각 픽셀 패치는 256채널을 갖는 특징 벡터로 변환됩니다. 그래서 조정된 자연 이미지 공간에서 16×16 픽셀 패치는 이 64×64 임베딩 공간 내에서 256 길이의 특징 벡터에 영향을 줍니다.

**이미지 위치 임베딩**
PromptEncoder 클래스는 또한 이미지 위치 임베딩을 생성하는데, 이는 256×64×64의 형태로, 이미지 임베딩과 동일한 형태입니다. 왜냐하면 이미지 위치 임베딩은 이미지 임베딩에 요소별로 추가될 것이기 때문입니다.

**밀집 프롬프트 임베딩**
다양한 종류의 사용자 프롬프트가 주어지면, PromptEncoder는 밀집 마스크 임베딩과 희소 임베딩을 생성합니다. 밀집 마스크 임베딩은 이미지 임베딩과 동일한 256×64×64 형태를 갖습니다. 이는 밀집 프롬프트 임베딩도 이미지 임베딩에 요소별로 추가되기 때문입니다.

<div class="content-ad"></div>

"희소 프롬프트 임베딩은 T×256 형태를 가지고 있습니다. 여기서 T는 토큰 수이며 256은 특성 벡터의 길이를 나타냅니다. 하나의 토큰은 희소 프롬프트나 희소 프롬프트 일부를 나타내는 텐서입니다 (바운딩 박스의 경우). 사용자의 희소 프롬프트 수에 따라 T의 값이 결정됩니다:

- 가이던스 클릭은 하나의 토큰을 기여합니다.
- 바운딩 박스는 두 개의 토큰을 기여하는데, 하나는 좌상단을 나타내고 다른 하나는 우하단을 나타냅니다.

인코딩 작동 방식은 어떻게 되는 걸까요?
이 기사에는 SAM의 인코더가 인코딩 절차를 통해 위의 임베딩을 어떻게 생성하는지 설명되어 있지 않습니다. SAM의 인코더의 내부 작업에 대해 다른 기사를 썼습니다. “Segment-Anything Model의 (SAM의) 인코더는 어떻게 작동하나요?”를 참조해주세요. 이 기사를 먼저 읽고 나서 인코더에 대한 것을 보시기를 권장드립니다. 인코더의 결과물인 여러 것들의 임베딩 (예: 이미지, 가이던스 프롬프트)이 디코더가 어떻게 활용하는지를 이해하시면, 인코더의 설계를 보다 감상하는 것이 더 쉬워집니다.

예측된 저해상도 마스크 및 신뢰 점수
이미지 임베딩, 이미지 위치 임베딩, 밀도 마스크 임베딩, 그리고 희소 프롬프트 임베딩을 감안할 때 MaskDecoder 클래스는 4 단계의 저해상도 분할 마스크와 4개의 신뢰 점수를 생성합니다."

<div class="content-ad"></div>

마스크의 형태는 level_of_masks × low_height × low_width인데요, 이는 4×256×256입니다. 확신 점수는 4개의 부동 소수점 숫자로 이루어져 있으며, 형태는 4×1입니다.

# MaskDecoder가 어떻게 작동하는지

SAM이 세그먼트 매스크를 생성하는 방법을 이해하는 가장 좋은 장소는 아래에 나와 있는 MaskDecoder.predict_masks 메소드입니다. 

[MaskDecoder.predict_masks 메소드](/assets/img/2024-07-10-HowdoestheSegment-AnythingModelsSAMsdecoderwork_4.png)을 살펴보세요.

<div class="content-ad"></div>

위 목록에서 src 변수를 132번째 줄부터 src2로 변경하여 132번째 줄 전에 있는 이미지 임베딩 (src로 불림)과 transformer 호출 이후에 출현하는 attended 이미지 임베딩 (src2로 불림)을 쉽게 구별할 수 있게 했습니다.

다음 두 개의 플로우차트를 통해 코드를 이해하는 데 도움이 될 것입니다.

첫 번째 절반 (126번째 줄부터 132번째 줄)은 132번째 줄에 있는 self.transformer 메서드 호출에 대한 인수를 준비하고, 그런 다음 132번째 줄에서 transformer를 호출합니다. Transformer 호출이 대부분의 마법을 부릴 뿐만 아니라 attended 토큰 임베딩과 attended 이미지 임베딩을 생성합니다.

![Flowchart](/assets/img/2024-07-10-HowdoestheSegment-AnythingModelsSAMsdecoderwork_5.png)

<div class="content-ad"></div>

The second part of the process involves adjusting the embeddings of the tokens in focus and the image to create lower resolution segmentation masks and confidence ratings. These modifications are straightforward, involving operations like MLP projections, matrix multiplications, and tensor upsampling.

![Visualization](/assets/img/2024-07-10-HowdoestheSegment-AnythingModelsSAMsdecoderwork_6.png)

### Understanding the input token tensor

The input token tensor combines three components, following a set sequence: the iou_token first, followed by the mask_tokens, and then the sparse_prompt_embeddings tokens. Here, "input" indicates that this tensor serves as an input for the Transformer function call.

<div class="content-ad"></div>

The iou_token (in the blue box)

The input iou_token tensor comes from PyTorch’s Embedding class with a fixed shape of 1×256. This tensor contains trainable parameters and serves as inputs for the transformer to generate the attended iou_token (in the green box). The attended iou_token then produces the confidence scores.

To put it simply, the input iou_token is not the sole information source used to create the attended iou_token. The transformer utilizes attention to blend information from prompt embeddings, image embeddings, and image positional embeddings to obtain the attended iou_token tensor.

It's important to note that the input iou_token is not connected to any user input, such as the raw input image or user prompts. So, what are the trainable parameters in iou_token used for? They ensure that the input and output tensors of the attention transformer share the same shape. These parameters are adjusted during parameter learning via stochastic gradient descent.

You might wonder if the parameters within iou_token have to be trainable. Could we simply set them all to zeros? While that could be a possibility, the creators of the SAM model have chosen to make them trainable, potentially leading to improved model performance. The same rationale applies to mask_tokens, as explained in the following section.

<div class="content-ad"></div>

**블루 박스 안의 mask_tokens**

입력 mask_tokens은 PyTorch의 Embedding 클래스에서 나온 고정된 크기인 4×256을 가지고 있습니다. 각 벡터가 길이 256인 4개의 벡터를 포함하고 있습니다. 이 4개의 벡터는 트랜스포머 호출에 대한 입력으로 작용하여 주목할 mask_tokens(녹색 박스 안)을 생성합니다. 주목된 mask_tokens은 최종적으로 네 가지 수준의 마스크에 대한 분할 마스크 예측 헤드를 생성합니다.

**블루 박스 안의 sparse_prompt_embeddings**

sparse_prompt_embeddings 입력 텐서는 크기가 다양합니다. 그 크기는 T×256이며, 여기서 T는 희소 프롬프트의 수를 나타냅니다. 따라서 더 많은 희소 프롬프트, 예를 들어 더 많은 가이드 클릭 또는 바운딩 박스는 T가 커진다는 것을 의미합니다. sparse_prompt_embeddings는 직접 PromptEncoder에서 왔으며, 간단히 토큰 텐서에 연결(concatenated)됩니다.

<div class="content-ad"></div>

이 세 부분으로 인해 입력 토큰 텐서의 형태는 (5+T)×256입니다. 여기서 T는 희소 프롬프트의 수이며, 따라서 토큰 텐서의 형태가 다양합니다.

iou_token, mask_tokens, 그리고 sparse_prompt_embeddings도 학습 가능한 매개변수를 포함하고 있습니다.

## 밀집 프롬프트 정보를 갖는 src 이미지 임베딩

라인 126~127에서 image_embeddings 텐서는 밀집한 프롬프트 임베딩 텐서와 요소별로 더해져서 src 텐서를 형성합니다. 두 텐서 모두 256×64×64의 형태를 갖고 있으므로, src 텐서도 256×64×64의 형태를 가지며, 낮은 해상도 64×64(높이×너비)에서 이미지 정보를 밀집 마스크 프롬프트 정보와 혼합하여 제공합니다. 라인 126에서의 repeat_interleave 부분은 이해하시는 데 영향을 미치지 않으니 무시해 주세요.

<div class="content-ad"></div>

이미지 임베딩 텐서는 ImageEncoderViT 인코더에서 나오고, dense_prompt_embeddings 텐서는 PromptEncoder에서 나옵니다.

## pos_src 이미지 위치 임베딩

pos_src 텐서는 이미지 위치 인코딩으로, 형태는 256×64×64 입니다. 이것은 PromptEncoder에서 나옵니다.

## transformer 호출

<div class="content-ad"></div>

라인 132에서의 transformer 호출은 위 세 개의 텐서를 입력으로 받아 두 개의 새로운 텐서를 생성합니다. 이 두 텐서는 다음과 같습니다: 위에서 언급된 'hs'라는 이름의 주목 받는 토큰 임베딩 텐서와 'src2'라는 이름의 주목 받는 이미지 임베딩 텐서입니다.

Transformer 호출 내부의 어텐션 메커니즘은 세 입력으로부터 정보를 조합하여 이 두 주목 받는 출력 텐서를 생성합니다:

- 주목 받는 토큰 임베딩 텐서 hs
- 주목 받는 이미지 임베딩 텐서 src2.

주목 받는 토큰 임베딩 'hs'

<div class="content-ad"></div>

Attended tokens embedding 'hs'의 구조는 입력 토큰 임베딩 텐서와 동일한 구조와 모양을 가지고 있습니다. 구조적으로 'hs' 텐서는 iou_token, mask_tokens, sparse_prompt_embeddings 세 가지 부분으로 구성되어 있습니다. 그리고 그 모양 또한 (5+T)×256입니다.

'hs' 텐서에서는 sparse_prompt_embeddings 부분을 무시하고 iou_token 부분과 mask_tokens 부분만을 마스크 생성에 사용합니다:
- iou_token 부분은 신뢰도 점수를 생성하기 위해 사용됩니다.
- mask_tokens 부분은 세그멘테이션 헤드를 생성하기 위해 사용됩니다.

그렇다면 왜 무시되는 sparse_prompt_embeddings 부분이 있을까요? 이는 transformer 작업이 주의력 작업이기 때문에 입력과 출력의 모양이 동일하게 디자인되었기 때문입니다.

<div class="content-ad"></div>

**최종 세분화 마스크 및 신뢰 점수 생성**

이미지 임베딩 텐서와 같은 256×64×64 형태를 갖는 출력된 이미지 임베딩 텐서인 'src2'가 이미지 임베딩 텐서와 같은 형태인 것을 확인합니다.

두 번째 절반의 플로차트에서 세분화 마스크를 생성하고 있습니다.

<div class="content-ad"></div>

To create four segmentation masks, SAM uses the conventional method of multiplying the segmentation head matrix with the attended image embedding approach, which is common in most segmentation models.

The segmentation head is derived from the attended mask_tokens tensor, which has a shape of 4×256. This tensor contains 4 vectors, each consisting of 256 entries. Each vector serves as a segmentation head for a different level of masking.

Next, the attended mask_tokens tensor is transformed into the hyper_in tensor with a shape of 4×32 using the output_hypernetworks_mlps network.

In the second part of the flowchart, the attended image embedding tensor src2 with a shape of 256×64×64 is upscaled to 32×256×256. This means a reduction in feature dimension (from 256 to 32) but an increase in resolution (from 64×64 to 256×256). It is then reshaped into a 32×65536 tensor. This tensor will be multiplied by the segmentation head tensor to generate the segmentation masks.

<div class="content-ad"></div>

이제 4×32 크기의 세분화 헤드 텐서를 32×65536 크기의 이미지 임베딩 텐서와 행렬곱하여 4×65536 크기의 세분화 마스크를 생성합니다. 이 마스크는 다시 4×256×256 크기로 변형됩니다. 이것이 마지막 세분화 마스크입니다 — 각 레벨에서 256×256의 낮은 해상도를 가진 4개의 마스크가 생성됩니다.

이 글의 앞부분에서 우리는 256×256의 저해상도 마스크가 사용자에게 반환되기 전에 입력 이미지의 원래 차원 H×W로 보간된다는 것을 알게 되었습니다.

두 번째 반의 흐름도에서 확신 점수 생성하기

세분화 마스크의 네 가지 레벨에 대한 확신 점수를 생성하기 위해 MLP 네트워크 iou_prediction_head가 1×256 형태의 iou_token을 새로운 1×4 크기의 텐서로 투영합니다. 이 1×4 텐서의 각 항목은 해당 레벨의 세분화 마스크에 대한 확신 점수로 해석되는 부동 소수점 숫자입니다.

<div class="content-ad"></div>

## Trainable Parameters

The second half of the flowcharts reveals the presence of trainable parameters in components that deal with tensor manipulation: `iou_prediction_head`, `output_hypernetwork_mlps`, and `output_upscaling`.

# The Attention Mechanism

Now, let's delve into the workings of the attention mechanism within the transformer. The transformer call is specified in line 132 of `MaskDecoder.predict_mask`.

<div class="content-ad"></div>

![How does the segment-anything models decoder work 7](/assets/img/2024-07-10-HowdoestheSegment-AnythingModelsSAMsdecoderwork_7.png)

이미지에 보이는 것처럼 TwoWayTransformer.forward 메서드를 호출합니다. 몇 가지 변수를 다시 이름짓았습니다. 예를 들어, queries를 queries2로, queries3를 queries3으로 변경했습니다.

![How does the segment-anything models decoder work 8](/assets/img/2024-07-10-HowdoestheSegment-AnythingModelsSAMsdecoderwork_8.png)

이 메서드의 목적은 입력 인수에서 정보를 혼합하는 데 어텐션을 사용하는 것입니다.

<div class="content-ad"></div>

- 이미지 삽입 이미지 삽입 이미지 삽입의 밀도 마스크 인코딩이 이미 추가되어 있습니다(마스크 디코더의 predict_masks의 첫 번째 절반 참조),
- 이미지 위치 임베딩 이미지_pe,
- 희소 프롬프트 임베딩 포인트 임베딩,

호출은 라인 150에서 산출결과로 고밀도 프롬프트 임베딩인 point_embed_attn4와 이미지 임베딩인 image_embed_attn2를 반환합니다.

호출 위치에서 본다면:

![How does the Segment-AnythingModelsSAMsdecoderwork](/assets/img/2024-07-10-HowdoestheSegment-AnythingModelsSAMsdecoderwork_9.png)

<div class="content-ad"></div>

이제 florian이 TwoWayTransformer.forward 메서드의 수정된 버전 코드를 flowchart와 함께 확인해보도록 하겠습니다.

![segmentation flowchart](/assets/img/2024-07-10-HowdoestheSegment-AnythingModelsSAMsdecoderwork_10.png)

<div class="content-ad"></div>

라인 129~130에서는 메소드가 먼저 이미지 임베딩 텐서 image_embedding의 형태를 256×64×64에서 4096×256으로 변경했습니다. 코드 버전에서 라인 192에서 결과를 image_embedding1이라고 부르고, 그 다음에 라인 134에서 keys0 변수가 image_embedding1에 할당됩니다. 이미지 위치 임베딩 텐서 image_pe에 대해서도 동일한 형태 변경을 수행하여 4096×256의 image_pe1 텐서를 얻게 됩니다.

라인 135~137에서는 point_embedding 텐서와 image_embedding1 텐서가 TwoWayAttentionBlock 구성 요소인 self.layers[0]를 통과하여 변경되지 않은 형태의 각각의 주의 집중 버전을 생성합니다:

- shape가 (5+T)×256인 point_embed_attn1
- shape가 4096×256인 image_embed_attn1.

그리고 라인 139~141에서 point_embed_attn1과 image_embed_attn1이 동일한 TwoWayAttentionBlock을 통과하여 또 다른 주의 집중 버전을 생성합니다:

<div class="content-ad"></div>

- 포인트 임베드 _ attn2의 형태는 (5+T)×256입니다.
- 이미지 임베드 _ attn2의 형태는 4096×256입니다.

포인트 임베드 _ attn2 텐서는 최종 포인트 임베드 _ attn4를 생성하도록 추가 조작됩니다.

이미지 임베드 _ attn2 텐서는 변경없이 호출자에게 반환되지만, 포인트 임베드 _ attn2 텐서는 라인 144에서 추가로 관찰되어 포인트 임베드 _ attn4 텐서를 생성한 후 반환됩니다. 우리는 라인 144에서 동일한 TwoWayAttentionBlock을 사용하기 때문에 이러한 추가 조작을 무시할 수 있습니다.

학습 가능한 매개변수는 라인 148의 레이어 정규화에서 관리됩니다. 다른 학습 가능한 매개변수는 TwoWayAttentionBlock에 존재합니다.

<div class="content-ad"></div>

이제 우리는 드디어 self.layers[0]의 관점에서 TwoWayAttentionBlock.forward 메서드에 깊게 들어가볼 때입니다. 아래에는 변수 이름이 의미 있는 코드 버전을 보여드렸어요. 이 메서드는 다음과 같은 입력을 받아들입니다:

- 희소 프롬프트 임베딩 포인트 임베딩, 모양은 (5+T)×256
- 이미지 임베딩 이미지 임베딩, 모양은 4096×256
- 이미지 위치 임베딩 이미지_pe1, 모양은 4096×256.

그리고 새로운 두 개의 텐서를 반환합니다:

<div class="content-ad"></div>

- point_embed7_attn이라는 이름의 sparse prompt embedding이 참석한 embedding이 형성되었습니다. 이는 (5+T)×256의 모양을 가지고 있습니다.
- image_embed2_attn이라는 이름의 이미지 embedding이 참석되었습니다. 이는 4096×256의 모양을 가지고 있습니다.

이 방법은 입력값에 세 가지 attention 작업을 적용합니다:

- 희소한 prompt embedding으로부터 희소 prompt embedding으로의 self attention 작업. 즉, 라인 232에서 point_embedding에서 point_embedding으로 attention을 적용하여 (5+T)×256 모양의 point_embed1_attn 텐서를 생성합니다.
- 라인 239에서 (라인 240에서 잔여 추가 후의) 희소한 prompt embedding으로부터 이미지 embedding에 교차 attention을 적용합니다 (라인 241에서 이미지 positional embedding과 합산한 후) point_embed3_attn 텐서를 생성합니다. 이 텐서의 모양은 (5+T)×256입니다.
- 라인 252에서 이미지 embedding에서 (라인 253에서 이미지 positional embedding과 더해진 후) 희소 prompt embedding으로의 attention 작업을 수행하여 4096×256 모양의 image_embed1_attn 텐서를 생성합니다.

point 2와 point 3으로부터의 두 attentions은 클래스 이름인 TwoWayAttentionBlock을 제시했습니다.

<div class="content-ad"></div>

위 목록에서:

- "point_embed" 접두어를 가진 모든 변수들, 예를 들어 point_embedding1, point_embedding3_attn 등은 (5+T)×256의 형태를 가지고 있습니다.
- "image_embed" 접두어를 가진 모든 변수들, 예를 들어 image_embedding1_attn은 4096×256의 형태를 가지고 있습니다.
- 이미지 위치 임베딩인 image_pe1은 4096×256의 형태를 가지고 있습니다.

훈련 가능한 파라미터
self.norm1와 같은 layernorm 연산, 그리고 self.mlp와 같은 mlp 연산에 있는 훈련 가능한 파라미터들입니다.

<div class="content-ad"></div>

# 주목.앞으로 메커니즘

위에서 소개 된 세 가지 주의 작업은 동일한 주의 메커니즘을 사용합니다. Attention.forward 방법에 구현되어 있습니다. 이제 우리가 탐구해야 할 최종 코드 조각입니다. 

위의 세 가지 주의 때문에 이 주의 메커니즘을 이해하기 위해 Attention.forward에 세 번 다가가야합니다. 따라서 각 버전에는 각 주의 종류에 특화된 의미있는 변수명이 포함되어 있습니다. 즉, 프롬프트 토큰 자기 주의 종류, 토큰에서 이미지로의 종류 및 이미지에서 토큰으로의 종류입니다.

## Attention.forward의 희소 토큰 자기 주의 종류를 위한 함께하기

<div class="content-ad"></div>

![SAMs decoder](/assets/img/2024-07-10-HowdoestheSegment-AnythingModelsSAMsdecoderwork_12.png)

이 코드 버전은 point_embedding에서 point_embedding으로의 셀프 어텐션을 위한 것이므로, shape가 (5+T)×256인 point_embedding 하나만 전달하면 됩니다.

315번 줄은 point_embedding 텐서를 동일한 shape인 (5+T)×256의 q 텐서로 투사하기 위해 선형 레이어를 사용합니다. 여기서 T는 가이드 클릭이나 바운딩 박스의 코너와 같은 희소한 클릭 수입니다. 사용자가 하나 이상의 클릭이나 바운딩 박스를 제공할 수 있기 때문에 T는 변동하는 숫자입니다.

316~317번 줄은 동일한 선형 투사를 수행합니다.

<div class="content-ad"></div>

이 선형 레이어는 입력 크기 T가 다양한 경우를 어떻게 처리합니까?

여기 작동 방식입니다: (5+T) 부분은 선형 레이어에 대한 배치 차원 역할을 합니다. 선형 레이어 자체는 입력 크기 256을 가지고 있습니다. 다시 말해, 이 (5+T) 배치 차원의 각 토큰에 대해, 동일한 선형 레이어, 즉 동일한 집합의 학습 가능한 매개변수가 사용되어 투영을 수행합니다.

코드의 다른 부분은 행렬 곱셈 및 소프트맥스와 같은 선형 대수 연산만 사용합니다. 이러한 선형 대수 연산에는 학습 가능한 매개변수가 포함되지 않으므로 다양한 T는 문제가 되지 않습니다. 마치 PyTorch의 행렬 곱셈 메소드 matmul이 임의 크기의 행렬을 지원하는 것과 같습니다.

334번 줄의 linear 프로젝션 out_proj는 다양한 T를 처리하는 데 배치 차원을 사용하며, q_proj 프로젝션과 동일한 방식으로 작동합니다.

다중 헤드 분할
319번 줄은 다중 헤드 분할을 수행합니다. (5+T)×256 텐서를 8×(5+T)×32 텐서로 변형하여, 첫 번째 차원 8이 헤드 수가 되도록 재구성합니다. 다중 헤드 분할의 목적은 8개의 헤드가 병렬로 처리될 수 있어 더 나은 병렬 계산을 달성하는 것입니다. 단점은 같은 헤드에서만 정보가 어텐션을 사용하여 혼합된다는 것입니다.

<div class="content-ad"></div>

아래 플로우차트는 주요 행렬 연산을 설명합니다. 플로우차트에서 보이는 행렬 그리드는 실제 모양과 일치하지 않습니다. 이는 시연용일 뿐입니다.

![Flowchart](/assets/img/2024-07-10-HowdoestheSegment-AnythingModelsSAMsdecoderwork_13.png)

q2×k3 곱셈

q2 텐서는 point_embedding이고, k3 텐서는 전치된 point_embedding입니다. 그림에서 보듯이 q2의 형태는 (5+T)×32이며, 이는 여덟 개 헤드 중 하나에 대한 것입니다.

<div class="content-ad"></div>

결과적으로 어텐션 텐서는 쌍으로 이루어진 포인트 유사도 행렬로 해석됩니다. 이는 어텐션 행렬의 한 항목이 q2의 한 행과 k3의 한 열의 내적입니다. 내적은 두 벡터 사이의 유사도를 측정하는 것입니다. 여기서의 벡터는 토큰 s (가이던스 클릭, 바운딩 박스 코너, iou 토큰, 마스크 토큰)에 대한 임베딩입니다. 두 토큰 임베딩 간의 내적은 이 두 토큰이 얼마나 유사한지 알려줍니다. 플로차트에서는 이 내적을 보여주기 위해 빨간 벡터를 사용했습니다.

![image](/assets/img/2024-07-10-HowdoestheSegment-AnythingModelsSAMsdecoderwork_14.png)

이 내적은 정규화되지 않은 코사인 유사도와도 같습니다. 여기서 왜 코사인 유사도를 계산하지 않는지 궁금할 수도 있습니다. 여기에서는 그럴 필요가 없습니다. 이들 행렬에 후속으로 적용되는 여러 레이어 정규화 연산이 있기 때문입니다.

<div class="content-ad"></div>

이 곱셈은 페어별 토큰 유사성 행렬을 점 임베딩 행렬과 곱하며, 우리는 이 결과를 self attended point embedding이라고 부릅니다. 그것을 왜 그렇게 부르는지 이해하기 위해 결과 행렬의 항목이 어떻게 계산되는지 다시 살펴 봅시다.

![image](/assets/img/2024-07-10-HowdoestheSegment-AnythingModelsSAMsdecoderwork_15.png)

결과 행렬인 out1의 항목은 특성 차원에서 모든 토큰의 정보의 가중 합입니다 (단일 헤드의 특성 차원은 32입니다). 가중 합의 가중치는 attn 행렬의 한 행에서 나옵니다. 이 가중치는 모든 토큰 쌍 간의 유사성을 설명합니다. 이 가중 합 해석은 주의 매커니즘의 직관과 정확히 일치합니다. 원래 주의 매커니즘에 대해 더 알고 싶다면, 내가 보기에 더 쉽고 직관적인 해석이 있는 원래 주의 메커니즘에 대한 다른 글을 확인해주세요:

가중치 행렬
가중치 행렬은 입력 투영 작업 q_proj, k_proj, v_proj 및 출력 투영 작업 out_proj에 존재합니다.

<div class="content-ad"></div>

## Attention.forward for cross attention from token embedding to image embedding kind

To achieve cross attention from token embedding to image embedding and obtain an attended token embedding, refer to the code snippet below. Keep in mind that this process involves multiple arguments being passed in.

![Cross Attention Code Snippet](/assets/img/2024-07-10-HowdoestheSegment-AnythingModelsSAMsdecoderwork_16.png)

Please note the linear projections from line 348 to 350, which reduce the channel size from 256 to 128. It's essential to differentiate this from token self-attention, where linear projections do not alter the channel size.

<div class="content-ad"></div>

![image](/assets/img/2024-07-10-HowdoestheSegment-AnythingModelsSAMsdecoderwork_17.png)

앞서 설명한 대로 q2×k3 행렬 곱셈을 하면 attn 행렬의 한 항목은 점의 임베딩과 픽셀의 임베딩 간의 내적입니다. 따라서 attn은 점과 픽셀 간의 쌍별 유사도 행렬입니다. attn의 모양은 (5+T)×4096인데, 이는 픽셀의 수인 4096개보다 훨씬 적은 점(5+T개)이 있기 때문에 정방행렬이 아닙니다.

그런 다음 attn×v2 곱셈은 쌍별 유사도 행렬에서 가중치를 사용하여 v2 행렬 또는 이미지 임베딩 행렬의 행들을 합산합니다. 이 행렬 곱셈의 결과는 이미지 임베딩에 주목하는 토큰 임베딩인 out1 행렬입니다.

이상이 image 임베딩에서 토큰 임베딩으로의 교차 주의를 위한 Attention.forward입니다.

<div class="content-ad"></div>

이미지 주소가 Markdown 형식으로 바뀌었습니다.


![이미지 설명](/assets/img/2024-07-10-HowdoestheSegment-AnythingModelsSAMsdecoderwork_18.png)


다시 한 번 언급하자면, 322번에서 324번 라인에서 채널 차원이 256에서 128로 줄었습니다.


![이미지 설명](/assets/img/2024-07-10-HowdoestheSegment-AnythingModelsSAMsdecoderwork_19.png)


메커니즘은 이전에 설명한 다른 교차 어텐션과 동일하므로 여기에 다시 언급하지는 않겠습니다.

<div class="content-ad"></div>

# 결과

이 글은 SAM의 디코더가 코드 조각과 플로우차트를 사용하는 방식에 대해 설명하고 있습니다. 다음 글에서는 SAM의 인코더가 어떻게 작동하는지 다룰 예정입니다.