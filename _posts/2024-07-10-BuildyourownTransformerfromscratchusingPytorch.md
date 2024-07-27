---
title: "Pytorch로 Transformer 직접 만드는 방법"
description: ""
coverImage: "/assets/img/2024-07-10-BuildyourownTransformerfromscratchusingPytorch_0.png"
date: 2024-07-10 00:15
ogImage: 
  url: /assets/img/2024-07-10-BuildyourownTransformerfromscratchusingPytorch_0.png
tag: Tech
originalTitle: "Build your own Transformer from scratch using Pytorch"
link: "https://medium.com/towards-data-science/build-your-own-transformer-from-scratch-using-pytorch-84c850470dcb"
---


## 파이토치를 사용하여 단계별로 Transformer 모델 구축하기

![Transformer Image](/assets/img/2024-07-10-BuildyourownTransformerfromscratchusingPytorch_0.png)

안녕하세요! 이번 튜토리얼에서는 PyTorch를 사용하여 기본 Transformer 모델을 처음부터 만들어보려 합니다. Transformer 모델은 Vaswani 등이 발표한 논문 "Attention is All You Need"에서 소개된 딥 러닝 아키텍처로, 기계 번역 및 텍스트 요약과 같은 시퀀스 대 시퀀스 작업에 적합합니다. 이 모델은 self-attention 메커니즘을 기반으로 하며, GPT 및 BERT와 같은 최신 자연어 처리 모델의 기반으로 사용되고 있습니다.

Transformer 모델에 대해 자세히 알아보시려면 이 두 가지 기사를 방문해보세요:

<div class="content-ad"></div>

## 1. ‘주의 집중’과 ‘트랜스포머’에 대한 모든 것을 알아보세요 — 깊은 이해 — 파트 1

## 2. ‘주의 집중’과 ‘트랜스포머’에 대한 모든 것을 알아보세요 — 깊은 이해 — 파트 2

저희의 트랜스포머 모델을 구축하기 위해 다음 단계를 따를 것입니다:

- 필요한 라이브러리와 모듈 가져오기
- 기본 구성 요소 정의: 다중 헤드 어텐션, 위치별 피드포워드 네트워크, 위치 인코딩
- 인코더 및 디코더 레이어 구축
- 인코더와 디코더 레이어를 결합하여 완전한 트랜스포머 모델 생성
- 샘플 데이터 준비
- 모델 훈련

<div class="content-ad"></div>

자, 그럼 필요한 라이브러리와 모듈들을 불러오는 것부터 시작해봅시다.

```js
import torch
import torch.nn as nn
import torch.optim as optim
import torch.utils.data as data
import math
import copy
```

이제 Transformer 모델의 기본 구성 요소를 정의해 보겠습니다.

## Multi-Head Attention

<div class="content-ad"></div>

![Image](/assets/img/2024-07-10-BuildyourownTransformerfromscratchusingPytorch_1.png)

멀티헤드 어텐션 메커니즘은 시퀀스 내 각 위치 쌍 간의 어텐션을 계산합니다. 이는 입력 시퀀스의 다양한 측면을 포착하는 여러 "어텐션 헤드"로 구성됩니다.

```python
class MultiHeadAttention(nn.Module):
    def __init__(self, d_model, num_heads):
        super(MultiHeadAttention, self).__init__()
        assert d_model % num_heads == 0, "d_model은 num_heads로 나눠야 합니다."
        
        self.d_model = d_model
        self.num_heads = num_heads
        self.d_k = d_model // num_heads
        
        self.W_q = nn.Linear(d_model, d_model)
        self.W_k = nn.Linear(d_model, d_model)
        self.W_v = nn.Linear(d_model, d_model)
        self.W_o = nn.Linear(d_model, d_model)
        
    def scaled_dot_product_attention(self, Q, K, V, mask=None):
        attn_scores = torch.matmul(Q, K.transpose(-2, -1)) / math.sqrt(self.d_k)
        if mask is not None:
            attn_scores = attn_scores.masked_fill(mask == 0, -1e9)
        attn_probs = torch.softmax(attn_scores, dim=-1)
        output = torch.matmul(attn_probs, V)
        return output
        
    def split_heads(self, x):
        batch_size, seq_length, d_model = x.size()
        return x.view(batch_size, seq_length, self.num_heads, self.d_k).transpose(1, 2)
        
    def combine_heads(self, x):
        batch_size, _, seq_length, d_k = x.size()
        return x.transpose(1, 2).contiguous().view(batch_size, seq_length, self.d_model)
        
    def forward(self, Q, K, V, mask=None):
        Q = self.split_heads(self.W_q(Q))
        K = self.split_heads(self.W_k(K))
        V = self.split_heads(self.W_v(V))
        
        attn_output = self.scaled_dot_product_attention(Q, K, V, mask)
        output = self.W_o(self.combine_heads(attn_output))
        return output
```

멀티헤드 어텐션 코드는 입력 매개변수와 선형 변환 레이어로 모듈을 초기화합니다. 어텐션 점수를 계산하고 입력 텐서를 여러 헤드로 다시 모양화하며 모든 헤드에서 어텐션 출력을 결합합니다. forward 메서드는 모델이 입력 시퀀스의 다양한 측면에 초점을 맞출 수 있는 멀티헤드 셀프 어텐션을 계산합니다.

<div class="content-ad"></div>

# 위치별 피드포워드 네트워크

```js
class PositionWiseFeedForward(nn.Module):
    def __init__(self, d_model, d_ff):
        super(PositionWiseFeedForward, self).__init__()
        self.fc1 = nn.Linear(d_model, d_ff)
        self.fc2 = nn.Linear(d_ff, d_model)
        self.relu = nn.ReLU()

    def forward(self, x):
        return self.fc2(self.relu(self.fc1(x)))
```

PositionWiseFeedForward 클래스는 PyTorch의 nn.Module을 확장하며 위치별 피드포워드 네트워크를 구현합니다. 이 클래스는 두 개의 선형 변환 레이어와 ReLU 활성화 함수를 초기화합니다. forward 메서드는 이러한 변환과 활성화 함수를 연속적으로 적용하여 출력을 계산합니다. 이 과정을 통해 모델은 예측을 수행하는 동안 입력 요소의 위치를 고려할 수 있습니다.

# 위치 인코딩

<div class="content-ad"></div>

Positional Encoding은 입력 시퀀스의 각 토큰의 위치 정보를 주입하는 데 사용됩니다. 이는 서로 다른 주파수의 사인과 코사인 함수를 사용하여 위치 인코딩을 생성합니다.


class PositionalEncoding(nn.Module):
    def __init__(self, d_model, max_seq_length):
        super(PositionalEncoding, self).__init__()
        
        pe = torch.zeros(max_seq_length, d_model)
        position = torch.arange(0, max_seq_length, dtype=torch.float).unsqueeze(1)
        div_term = torch.exp(torch.arange(0, d_model, 2).float() * -(math.log(10000.0) / d_model))
        
        pe[:, 0::2] = torch.sin(position * div_term)
        pe[:, 1::2] = torch.cos(position * div_term)
        
        self.register_buffer('pe', pe.unsqueeze(0))
        
    def forward(self, x):
        return x + self.pe[:, :x.size(1))


PositionalEncoding 클래스는 d_model 및 max_seq_length와 같은 입력 매개변수로 초기화되며 위치 인코딩 값을 저장하는 텐서를 생성합니다. 해당 클래스는 스케일링 요소 div_term을 기반으로 짝수 및 홀수 인덱스에 대해 사인 및 코사인 값을 계산합니다. forward 메서드는 저장된 위치 인코딩 값을 입력 텐서에 더함으로써 위치 정보를 캡처하여 입력 시퀀스의 위치 정보를 모델이 파악할 수 있도록 합니다.

이제 Encoder 및 Decoder 레이어를 구축해보겠습니다.

<div class="content-ad"></div>

# 인코더 레이어

![2024-07-10-BuildyourownTransformerfromscratchusingPytorch_2](/assets/img/2024-07-10-BuildyourownTransformerfromscratchusingPytorch_2.png)

인코더 레이어는 멀티헤드 어텐션 레이어, 포지션 와이즈 피드포워드 레이어, 두 개의 레이어 정규화 레이어로 구성됩니다.

```python
class EncoderLayer(nn.Module):
    def __init__(self, d_model, num_heads, d_ff, dropout):
        super(EncoderLayer, self).__init__()
        self.self_attn = MultiHeadAttention(d_model, num_heads)
        self.feed_forward = PositionWiseFeedForward(d_model, d_ff)
        self.norm1 = nn.LayerNorm(d_model)
        self.norm2 = nn.LayerNorm(d_model)
        self.dropout = nn.Dropout(dropout)
        
    def forward(self, x, mask):
        attn_output = self.self_attn(x, x, x, mask)
        x = self.norm1(x + self.dropout(attn_output))
        ff_output = self.feed_forward(x)
        x = self.norm2(x + self.dropout(ff_output))
        return x
```

<div class="content-ad"></div>

The EncoderLayer class is initialized with various components such as a MultiHeadAttention module, a PositionWiseFeedForward module, two layer normalization modules, and a dropout layer. During the forward pass, it calculates the encoder layer output by utilizing self-attention, adding the attention output to the input tensor, and then normalizing the result. After that, it computes the position-wise feed-forward output, combines it with the normalized self-attention output, and normalizes the final result before returning the processed tensor.

## Decoder Layer

![Decoder Layer](/assets/img/2024-07-10-BuildyourownTransformerfromscratchusingPytorch_3.png)

A Decoder layer comprises two Multi-Head Attention layers, a Position-wise Feed-Forward layer, and three Layer Normalization layers.

<div class="content-ad"></div>

```python
class DecoderLayer(nn.Module):
    def __init__(self, d_model, num_heads, d_ff, dropout):
        super(DecoderLayer, self).__init__()
        self.self_attn = MultiHeadAttention(d_model, num_heads)
        self.cross_attn = MultiHeadAttention(d_model, num_heads)
        self.feed_forward = PositionWiseFeedForward(d_model, d_ff)
        self.norm1 = nn.LayerNorm(d_model)
        self.norm2 = nn.LayerNorm(d_model)
        self.norm3 = nn.LayerNorm(d_model)
        self.dropout = nn.Dropout(dropout)

    def forward(self, x, enc_output, src_mask, tgt_mask):
        attn_output = self.self_attn(x, x, x, tgt_mask)
        x = self.norm1(x + self.dropout(attn_output))
        attn_output = self.cross_attn(x, enc_output, enc_output, src_mask)
        x = self.norm2(x + self.dropout(attn_output))
        ff_output = self.feed_forward(x)
        x = self.norm3(x + self.dropout(ff_output))
        return x
```

이 DecoderLayer는 MultiHeadAttention 모듈을 사용하여 마스킹된 자기 주의와 교차 주의를 위한 구성요소, PositionWiseFeedForward 모듈, 세 개의 레이어 정규화 모듈 및 드롭아웃 레이어와 같은 입력 매개변수 및 구성요소로 초기화됩니다.

forward 메서드는 다음 단계를 수행하여 디코더 레이어 출력을 계산합니다:
- 마스킹된 자기 주의 출력을 계산하고 입력 텐서에 추가한 후 드롭아웃 및 레이어 정규화를 적용합니다.
- 디코더 및 인코더 출력 사이의 교차 주의 출력을 계산하고 정규화된 마스킹된 자기 주의 출력에 추가한 후 드롭아웃 및 레이어 정규화를 적용합니다.
- 위치별 피드포워드 출력을 계산하고 이를 정규화된 교차 주의 출력에 결합한 후 드롭아웃 및 레이어 정규화를 적용합니다. 
- 처리된 텐서를 반환합니다.


<div class="content-ad"></div>

이러한 작업은 디코더가 입력 및 인코더 출력을 기반으로 대상 시퀀스를 생성할 수 있도록 합니다.

이제 인코더와 디코더 레이어를 결합하여 완전한 트랜스포머 모델을 만들어 봅시다.

## 트랜스포머 모델

[Build your own Transformer from scratch using Pytorch](/assets/img/2024-07-10-BuildyourownTransformerfromscratchusingPytorch_4.png)

<div class="content-ad"></div>

다음은 Transformer 클래스입니다. 이 클래스는 이전에 정의된 모듈을 결합하여 완전한 Transformer 모델을 만듭니다. 초기화 중에 Transformer 모듈은 입력 매개변수를 설정하고 원본 및 대상 시퀀스에 대한 임베딩 레이어, 위치 인코딩 모듈, 쌓인 레이어를 만들기 위한 EncoderLayer 및 DecoderLayer 모듈, 디코더 출력을 투사하기 위한 선형 레이어, 그리고 드롭아웃 레이어를 초기화합니다.

generate_mask 메서드는 원본 및 대상 시퀀스에 대한 이진 마스크를 생성하여 패딩 토큰을 무시하고 디코더가 미래 토큰에 주의를 기울이지 않도록 합니다. forward 메서드는 Transformer 모델의 출력을 계산하는 다음 단계를 통해 수행됩니다:

<div class="content-ad"></div>

- `generate_mask` 메서드를 사용하여 소스 및 타겟 마스크를 생성합니다.
- 소스 및 타겟 임베딩을 계산하고 위치 인코딩 및 드롭아웃을 적용합니다.
- 소스 시퀀스를 인코더 레이어를 통해 처리하여 enc_output 텐서를 업데이트합니다.
- 타겟 시퀀스를 마스크 및 enc_output을 사용하여 디코더 레이어를 통해 처리하고 dec_output 텐서를 업데이트합니다.
- 디코더 출력에 선형 프로젝션 레이어를 적용하여 출력 로짓을 얻습니다.

이러한 단계들을 통해 Transformer 모델은 입력 시퀀스를 처리하고 구성 요소의 결합 기능에 기초하여 출력 시퀀스를 생성할 수 있습니다.

# 샘플 데이터 준비

이 예시에서는 시연을 위해 장난감 데이터셋을 생성합니다. 실제로는 더 큰 데이터셋을 사용하고 텍스트를 전처리하고 소스 및 타겟 언어에 대한 어휘 매핑을 생성할 것입니다.

<div class="content-ad"></div>

```js
**모델 훈련**

이제 샘플 데이터를 사용하여 모델을 훈련시킬 것입니다. 실제로는 더 큰 데이터셋을 사용하고 이를 훈련 및 검증 세트로 나누게 될 것입니다.



<div class="content-ad"></div>

위의 텍스트를 친절하게 번역해 드리겠습니다.

**Pytorch에서 처음부터 간단한 Transformer를 만드는 방법을 사용할 수 있습니다. 대형 언어 모델은 모두 트레이닝에 Transformer 인코더 또는 디코더 블록을 사용합니다. 그러므로 모든 것을 시작한 네트워크를 이해하는 것은 굉장히 중요합니다. 이 기사가 LLM에 심취하려는 분들에게 도움이 되기를 바랍니다.**

# 참고 자료

# Attention is all you need

A. Vaswani, N. Shazeer, N. Parmar, J. Uszkoreit, L. Jones, A. Gomez, '. Kaiser 및 I. Polosukhin. Advances in Neural Information Processing Systems , 페이지 5998–6008. (2017)