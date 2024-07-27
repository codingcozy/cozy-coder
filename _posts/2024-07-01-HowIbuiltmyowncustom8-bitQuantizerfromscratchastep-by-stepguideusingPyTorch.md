---
title: "PyTorch로 나만의 8비트 양자화기를 처음부터 만드는 방법 단계별 가이드"
description: ""
coverImage: "/assets/img/2024-07-01-HowIbuiltmyowncustom8-bitQuantizerfromscratchastep-by-stepguideusingPyTorch_0.png"
date: 2024-07-01 17:48
ogImage: 
  url: /assets/img/2024-07-01-HowIbuiltmyowncustom8-bitQuantizerfromscratchastep-by-stepguideusingPyTorch_0.png
tag: Tech
originalTitle: "How I built my own custom 8-bit Quantizer from scratch: a step-by-step guide using PyTorch"
link: "https://medium.com/towards-artificial-intelligence/how-i-built-my-own-custom-8-bit-quantizer-from-scratch-a-step-by-step-guide-using-pytorch-a913cd12e85d"
---


8-bit 커스텀 양자화기를 PyTorch와 quantize facebook/opt-350m을 사용해 처음부터 만드는 단계별 접근법을 소개해 드릴게요. 

![How I built my own custom 8-bit Quantizer from scratch: a step-by-step guide using PyTorch](/assets/img/2024-07-01-HowIbuiltmyowncustom8-bitQuantizerfromscratchastep-by-stepguideusingPyTorch_0.png)

BitsAndBytes, AWQ, 그리고 GGUF와 같은 인기 있는 양자화기가 실제로 어떻게 작동하는지 궁금하신가요? 저의 답변은 왜 우리가 직접 8비트 양자화기를 처음부터 만들어보고 직접 확인해보지 않을까? 

이제 우리 함께 만들기를 시작해봐요.

<div class="content-ad"></div>

이번 포스트에서는 우리가 좋아하는 PyTorch를 사용하여 처음부터 8비트 커스텀 양자화기를 만들어볼 것입니다. 더 흥미로워 보이도록 MYQ 8비트(My Quantizer)라고 이름 붙여볼게요. 우리의 목표를 달성하기 위한 단계별 공격 계획은 아래와 같아요.

- 단계 1: MYQ 8비트 양자화기 클래스와 관련 함수를 만들겠습니다.
- 단계 2: Hugging Face에서 모델을 가져올 거에요. 상대적으로 작은 모델인 facebook/opt-350m을 선택할 거예요. 이 모델은 양자화를 수행하고 결과를 확인하는 데 더 빨라서 좋을 거예요.
- 단계 3: MYQ 8비트 양자화기를 사용하여 기본 모델에 양자화를 수행할 거에요. 그러고 나서 새로 양자화된 모델 크기를 확인하고 생성된 출력을 관찰하기 위해 추론을 수행할 거예요.
- 단계 4: 이것은 보너스 단계에요. 해당 포스트의 끝에 4비트 양자화기의 전체 소스 코드를 공유하고 이 4비트 양자화기를 구축하기 위해 사용한 기술을 설명할 거에요. 해당 코드를 사용하여 사용 사례에 맞게 더 발전시킬 수 있어요.

단계 1: MYQ 8비트 양자화기 클래스 만들기

깊은 신경망 내부에서 양자화 알고리즘이 어떤 방식으로 작동하는지를 이해하기 위해 아래 다이어그램을 살펴봅시다.

<div class="content-ad"></div>

![How I built my own custom 8-bit Quantizer from scratch: a step-by-step guide using PyTorch](/assets/img/2024-07-01-HowIbuiltmyowncustom8-bitQuantizerfromscratchastep-by-stepguideusingPyTorch_1.png)

If you take a look at the image above, I can sum up the entire process in a simple line: "Replace the linear layer from the base model with the quantized layer in the quantized model." It's pretty straightforward. Let me walk you through the process with bullet points first, and then we can write the code together.

- First, we'll create a class called QuantizedLinearLayer that mirrors all the characteristics of the base model Linear Layer. This step is crucial because we will later replace the base model's Linear Layer with our QuantizedLinearLayer.
- The QuantizedLinearLayer class should be initialized with in_features, out_features, and bias, similar to the linear layer in the base model.
- Next, we'll define a forward function that functions like the activation in a deep neural network. It utilizes PyTorch's built-in linear function (torch.nn.functional.linear) to replicate the linear function in the original base model.
- Lastly, we'll define a critical function called quantize within QuantizedLinearLayer. This function is responsible for converting weights in fp16 from the base model to int-8. To keep it simple, we will only quantize the weight parameters and apply the Symmetric linear quantization method to create our quantizer.

Now, we're set to write the code to create the QuantizedLinearLayer. I've added comments to each line of code to clarify the process. I hope this makes understanding the code easier for you.

<div class="content-ad"></div>


# 위 코드를 실행하기 전에 이 두 라이브러리를 설치해주세요
# !pip install transformers
# !pip install -U "huggingface_hub[cli]"  #허깅페이스 인증을 위해 필요

# 먼저, 필요한 모든 라이브러리를 import 해주세요.
import torch
import torch.nn as nn
import torch.nn.functional as F
from transformers import AutoTokenizer, AutoModelForCausalLM, pipeline

# 우리는 huggingface로부터 basemodel-facebook/opt-350m을 사용할 것이므로, 먼저 인증해야 합니다. huggingface에서 토큰을 만들어주세요.

!huggingface-cli login --token hf_THkbLhyIHHmluGkwwnzpXOvR########## 

# QuantizedLinearLayer 클래스 정의
class QuantizedLinearLayer(nn.Module):
    # 우리의 목표는 기본 모델에서 선형 레이어를 교체하는 것이므로, in_features, out_features, bias=True, dtype=torch.float32 등과 같은 매개변수를 사용해야 합니다. dtype는 편향의 유형입니다.
    def __init__(self, in_features, out_features, bias=True, dtype=torch.float32):
        super().__init__()

        # weight는 (-128, 127) 범위 내에서 무작위로 초기화됩니다.
        self.register_buffer("weight", torch.randint(-128, 127, (out_features, in_features)).to(torch.int8))

        # scale은 출력과 데이터 유형이 동일한 차원을 갖습니다. 선형 레이어의 출력에 곱해질 것입니다.
        self.register_buffer("scale", torch.randn((out_features), dtype=dtype))

        # bias는 선택적 매개변수이므로, none이 아닌 경우에만 추가됩니다.
        if bias:
            self.register_buffer("bias", torch.randn((1, out_features), dtype=dtype))
        else:
            self.bias = None

    def quantize(self, weight):
        # weight를 복제하고 fp32로 변환합니다. 두 유형 모두 fp32이어야 하므로 scale을 계산하기 위함입니다.
        weight_f32 = weight.clone().to(torch.float32)

        # int-8 양자화 범위의 최솟값과 최댓값을 계산합니다. qmin=-128, qmax=127
        Qmin = torch.iinfo(torch.int8).min
        Qmax = torch.iinfo(torch.int8).max

        # 채널 당 스케일을 계산합니다.
        scale = weight_f32.abs().max(dim=-1).values / 127
        scale = scale.to(weight.dtype)

        # 주어진 weight 텐서에 대한 양자화된 weight 값을 반환합니다.
        quantized_weight = torch.clamp(torch.round(weight / scale.unsqueeze(1)), Qmin, Qmax).to(torch.int8)

        self.weight = quantized_weight
        self.scale = scale
    
    def forward(self, input):
        output = F.linear(input, self.weight.to(input.dtype)) * self.scale
        if self.bias is not None:
            output = output + self.bias
        return output


이제 QuantizedLinearLayer 클래스를 정의했으므로, 기본 모델 LinearLayer 클래스를 QuantizedLinearLayer 클래스로 교체하는 함수를 만들겠습니다. 아래를 살펴보세요.


def replace_linearlayer(base_model, quantizer_class, exclude_list, quantized=True):

    for name, child in base_model.named_children():
        if isinstance(child, nn.Linear) and not any([x == name for x in exclude_list]):
            old_bias = child.bias
            old_weight = child.weight
            in_features = child.in_features
            out_features = child.out_features

            quantizer_layer = quantizer_class(in_features, out_features, old_bias is not None, old_weight.dtype)

            setattr(base_model, name, quantizer_layer)

            if quantized:
                getattr(base_model, name).quantize(old_weight)

            if old_bias is not None:
                getattr(base_model, name).bias = old_bias

        else:
            replace_linearlayer(child, quantizer_class, exclude_list, quantized=quantized)


Step 2: 이제 quantizer를 만들었으므로, hugging face에서 basemodel(facebook/opt-350m)을 가져와봅시다. 허깅페이스 계정을 가지고 있고, 자체 auth 토큰을 사용해주세요. 이 과정은 간단하며 무료입니다.


<div class="content-ad"></div>


# 모델을 원래의 fp32 데이터 유형이 아닌 bfloat16 으로 다운로드할 것이라는 점을 유의해 주세요.
# 이렇게 함으로써 베이스 모델의 크기를 줄이고 나중에 양자화하는 데 시간을 단축할 수 있습니다.

tokenizer = AutoTokenizer.from_pretrained("facebook/opt-350m")
model = AutoModelForCausalLM.from_pretrained("facebook/opt-350m", torch_dtype=torch.bfloat16)

print("facebook/opt-350m: 양자화하기 전의 베이스 모델 구조")
print("-"*50)
print(model)


![Link](/assets/img/2024-07-01-HowIbuiltmyowncustom8-bitQuantizerfromscratchastep-by-stepguideusingPyTorch_2.png)

상기 베이스 모델 구조에서 파란 점선 상자의 모든 선형 레이어는 양자화된 레이어로 대체되고, 초록 점선 상자의 레이어는 제외될 것입니다. LLM 모델은 서로 연결된 많은 트랜스포머 레이어로 구성되어 있으며, 각 레이어의 출력 레이어는 다음 레이어의 입력 역할을 합니다. 따라서 가장 바깥쪽 레이어를 양자화하는 것은 모델의 전반적인 정확도에 영향을 줄 수 있습니다. 그러므로 더 나은 접근 방식은 이를 제외하는 것입니다.

이 베이스 모델을 양자화하기 전 크기를 확인해 봅시다. 크기는 0.66 GB (662 MB) 입니다.


<div class="content-ad"></div>


# 양자화하기 전에 이 기본 모델의 사이즈를 확인해봅시다
model_memory_size_before_quantization = model.get_memory_footprint()
print(f"양자화하기 전 총 메모리 사이즈(GB 단위): {model_memory_size_before_quantization / 1e+9}")


![How I built my own custom 8-bit Quantizer from scratch: A step-by-step guide using PyTorch](/assets/img/2024-07-01-HowIbuiltmyowncustom8-bitQuantizerfromscratchastep-by-stepguideusingPyTorch_3.png)

페이스북의 opt-350m 기본 모델로 추론을 수행해봅시다.

```python
# 페이스북의 opt-350m 기본 모델로 추론 수행하기
pipe = pipeline("text-generation", model=model, tokenizer=tokenizer)
pipe("말레이시아는 아름다운 나라이며, ", max_new_tokens=50)
```

<div class="content-ad"></div>

### Step 3

Now, let's run the `replace_linearlayer` function. This step is crucial as it serves two key purposes:

1. It utilizes the `quantizer` class to quantize all weights in the linear layer, excluding those specified in the exclude list.
2. It subsequently replaces all linear layers with the quantized layer, except those specified in the exclude list.

Let's go ahead and implement the code that executes the `replace_linearlayer` function.

<div class="content-ad"></div>


# 모델: base_model, QuantizedLinearLayer: 단계 1에서 만든 양자화된 레이어, ["lm_head"]: 제외 목록
# quantized=True: quantized 값을 False로 설정하면 양자화기는 선형 레이어를 양자화된 레이어로만 대체하지만 가중치를 양자화하지는 않습니다.
# 만약 양자화된 모델을 huggingface나 다른 클라우드 제공업체에 저장하려면 이 옵션이 필요합니다.
# 나중에 어떤 사용자도 이 양자화된 모델을 다운로드하여 기본 모델 구조를 생성하고 모델을 불러올 수 있습니다.

replace_linearlayer(model, QuantizedLinearLayer, ["lm_head"], quantized=True)
print("facebook/opt-350m: 양자화된 모델 아키텍처")
print("-"*50)
print(model)


![Link to the image](/assets/img/2024-07-01-HowIbuiltmyowncustom8-bitQuantizerfromscratchastep-by-stepguideusingPyTorch_5.png)

위 양자화된 모델 아키텍처에서 파란색 상자 내 모든 선형 레이어가 QuantizedLinearLayer로 대체되었음을 볼 수 있고, 녹색 점선 상자의 레이어는 변경되지 않았습니다.

양자화된 모델의 크기를 확인해 봅시다. 크기는 0.35 GB (359 MB)입니다. 이는 기본 모델 크기의 54% 작습니다. 이것은 정말 대단한 성과입니다.


<div class="content-ad"></div>

```python
모델_양자화_후_메모리_크기 = model.get_memory_footprint()
print(f"양자화 후 총 메모리 크기 (GB 단위): {모델_양자화_후_메모리_크기 / 1e+9}")
```

![링크 텍스트](/assets/img/2024-07-01-HowIbuiltmyowncustom8-bitQuantizerfromscratchastep-by-stepguideusingPyTorch_6.png)

이제 이 양자화된 모델을 사용하여 추론을 수행해보겠습니다.

```python
pipe = pipeline("text-generation", model=model, tokenizer=tokenizer)
pipe("말레이시아는 아름다운 나라이고 ", max_new_tokens=50)
```

<div class="content-ad"></div>

![How I built my own custom 8-bit Quantizer from scratch: a step-by-step guide using PyTorch](/assets/img/2024-07-01-HowIbuiltmyowncustom8-bitQuantizerfromscratchastep-by-stepguideusingPyTorch_7.png)

놀랍게도 양자화된 모델에서의 추론은 동일한 정확도를 제공합니다. 정말 인상적죠. 여러분도 한 번 직접 해보시기 바랍니다.

**단계 4: MYQ 4비트 양자화**

4비트 양자화를 구축하기 위해서는 앞에서 수행한 모든 작업 외에도 새로운 기술을 구현해야 합니다. 이 기술은 weight-packing과 weight-unpacking입니다. 현재 PyTorch는 4비트나 2비트, 인트-8보다 작은 양자화를 지원하지 않습니다. 따라서 우리는 목표를 달성하기 위해 weight-packing 기술을 사용해야 합니다.

<div class="content-ad"></div>

**웨이트 패킹:** 만일 우리가 4비트 인코딩된 weight 파라미터 값을 int8 데이터 타입으로 저장한다면, 메모리 풋프린트는 실제 8비트 인코딩된 텐서와 동일할 것입니다. 따라서, 4비트 인코딩된 값이 4비트 메모리 공간을 할당하는 방법을 찾아야 합니다. 만일 4비트로 양자화하면, 전체 양자화 모델의 메모리 풋프린트는 8비트로 양자화된 모델보다 거의 절반 크기가 작아집니다. 따라서 웨이트 패킹 기술을 사용하면 이를 달성할 수 있습니다. 웨이트 패킹 기술에서는, 여러 4비트 인코딩된 값을 8비트 텐서에 추가할 수 있을 때까지 넣는 방식을 사용합니다. 이렇게 하면 4비트 인코딩된 값은 4비트 공간만 할당하고, 나머지 공간은 다른 4비트 인코딩된 값에 의해 활용됩니다.

**웨이트 언패킹:** 모델은 추론 중에만 부동 소수점 값을 처리합니다. 따라서, 각 패킹된 weight 텐서를 가져와서 개별 4비트 인코딩된 값으로 분리하는 웨이트 언패킹 기술을 사용해야 합니다. 각 4비트 인코딩된 값은 8비트 메모리 공간을 할당받고, 이후에 이들을 추론을 위해 fp32로 변환할 수 있습니다.

4비트 양자화기의 소스 코드도 아래에 공유 드리겠습니다. FYI, 4비트 양자화 과정은 꽤 오랜 시간이 걸렸습니다. 코드를 사용하고 테스트하고 변경하실 자유가 있고, 테스트 후에 여러분의 경험을 공유하셔도 좋습니다.

여기까지입니다! 우리는 처음부터 직접 사용자 정의 8비트 양자화기를 성공적으로 만들었습니다.**

<div class="content-ad"></div>

# 나의 마무리 글

- 만일 8비트 양자화기를 생성하며 코딩해왔다면, 이제는 인공지능 분야에 새롭게 등장하는 양자화기들의 핵심 알고리즘을 이해할 수 있을 것입니다. 그리고 훨씬 적은 노력으로 다른 상황에서 그것들을 손쉽게 활용할 수 있을 겁니다.
- 앞에서 수 억 개의 파라미터를 가진 LLM 모델을 양자화할 때의 주요한 과제 중 하나는 처리 및 메모리 측면에서 더 많은 자원이 필요하다는 것입니다. 그러나 10억 개 미만의 작은 모델인 경우, Kaggle Notebook을 사용해보는 것을 권하고 싶습니다. 이제 Kaggle Notebook은 최대 30GB RAM(GPU T4 x 2 가속기)을 제공하기 때문에 정말 대단한 것입니다.

곧 또 다른 이야기가 나올 테니 조금만 기다려 주세요. 읽어주셔서 정말 감사합니다!

- [MYQ 8비트 양자화기 Google Colab 노트북 링크](https://your-link-here)
- [MYQ 4비트 양자화기 Google Colab 노트북 링크](https://your-link-here)

<div class="content-ad"></div>

참고 자료

- Hugging face 블로그: [양자화를 INT8로 수행하는 실용적 단계](https://huggingface.co/docs/optimum/en/concept_guides/quantization#pratical-steps-to-follow-to-quantize-a-model-to-int8)
- Deeplearning.ai: 심층적인 양자화
- Hugging face 블로그: [hf-bitsandbytes 통합](https://huggingface.co/blog/hf-bitsandbytes-integration)