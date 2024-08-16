---
title: "PyTorch로 GPT-2 구축하기 2부"
description: ""
coverImage: "/assets/img/2024-07-13-BuildingGPT-2withPyTorchPart2_0.png"
date: 2024-07-13 23:18
ogImage: 
  url: /assets/img/2024-07-13-BuildingGPT-2withPyTorchPart2_0.png
tag: Tech
originalTitle: "Building GPT-2 with PyTorch (Part 2)"
link: "https://medium.com/towards-artificial-intelligence/heres-how-you-can-build-and-train-gpt-2-from-scratch-using-pytorch-part-2-9b41d15baf62"
isUpdated: true
---





이것은 GPT-2를 처음부터 만드는 프로젝트의 두 번째 부분입니다. 아직 첫 번째 부분을 읽지 않으셨다면, 계속하기 전에 언어 모델의 기본 사항에 익숙해지는 것을 권장합니다.

- GPT-2 구축 및 훈련 (1부)

최종 로스:

![이미지](/assets/img/2024-07-13-BuildingGPT-2withPyTorchPart2_0.png)

<div class="content-ad"></div>

# 4. GPT-2 아키텍처 구현하기

이번 섹션에서는 GPT-2의 부분들을 하나씩 추가하고 각 단계에서 모델의 성능을 훈련하고 평가해볼 것입니다. 다음은 진행 방식입니다:

a. 위치 인코딩 + 완전 연결층 (NN)

b. (가리킨) 셀프 어텐션 + 정규화

<div class="content-ad"></div>

c. (가려진) 멀티 헤드 어텐션

d. 다중 GPT 디코더 블록

e. 토크나이저 개선

f. 최종 GPT-2 훈련

<div class="content-ad"></div>

이전 부분을 기억해 보겠습니다. 모델은 아래와 같습니다:

![이미지](/assets/img/2024-07-13-BuildingGPT-2withPyTorchPart2_1.png)

코드:

```python
import torch.nn as nn
import torch.nn.functional as F

# 임베딩 크기 정의에 사용됨
d_model = vocab_size

class GPT(nn.Module):
    def __init__(self, vocab_size, d_model):
        super().__init__()
        self.wte = nn.Embedding(vocab_size, d_model)  # 단어 토큰 임베딩
    
    def forward(self, inputs, targets=None):
        logits = self.wte(inputs)  # 차원 -> 배치 크기, 시퀀스 길이, d_model
        loss = None
        if targets is not None:
            batch_size, sequence_length, d_model = logits.shape
            # 배치 내 모든 토큰 임베딩에 대한 손실 계산
            logits = logits.view(batch_size * sequence_length, d_model)
            targets = targets.view(batch_size * sequence_length)
            loss = F.cross_entropy(logits, targets)
        return logits, loss
    
    def generate(self, inputs, max_new_tokens):
        # 이곳에 모델 출력을 초기 입력 시퀀스와 함께 저장할 것입니다
        # 모델과 간섭하지 않도록 복사본을 생성합니다
        for _ in range(max_new_tokens):
            # 손실 계산을 위해 교육 중에만 대상을 전달합니다
            logits, _ = self(inputs)
            # 모든 배치에 대해, 마지막으로 예측한 시퀀스의 임베딩을 가져옵니다
            logits = logits[:, -1, :]
            probs = F.softmax(logits, dim=1)
            # 입력 확률에 기반하여 가능성 있는 토큰을 가져옵니다
            idx_next = torch.multinomial(probs, num_samples=1)
            
            inputs = torch.cat([inputs, idx_next], dim=1)
        # 초기 입력 + 모델 출력이 모두 포함된 inputs를 최종 출력으로 사용할 수 있습니다
        return [decode(out.tolist()) for out in inputs]

m = GPT(vocab_size=vocab_size, d_model=d_model).to(device)
```

<div class="content-ad"></div>

## a. 위치 부호 인코딩 (PE) + 신경망 (NN) 추가

이제 모델에 위치 부호 인코딩을 추가해 봅시다:

![image](/assets/img/2024-07-13-BuildingGPT-2withPyTorchPart2_2.png)

출력 임베딩(우리의 경우 입력 임베딩인 wte)에 위치 부호 인코딩을 추가하고 나서 더 나아가는 신경망으로 전달됩니다. PE가 무엇인지 이해하려면 토큰 임베딩을 상기해 봅시다. 토큰 임베딩은 우리 어휘에서 각 문자에 대해 d_model 차원의 벡터를 저장합니다. 이는 문자의 다른 특성을 훈련 시 어떻게 나타났는지와 어디에 나타났는지에 기반하여 나타냅니다.

<div class="content-ad"></div>

이와 유사하게 위치 인코딩은 `context_length`에서 각 문자의 순서/위치 신호를 저장합니다. 코사인 및 사인 함수를 사용하여 한 번만 계산되며 훈련이 필요하지 않습니다. 이는 시퀀스의 각 문자의 위치 벡터가 교육 세트의 모든 데이터에 대해 동일할 것을 의미합니다.

따라서 두 가지를 함께 추가하면 모델이 더 잘 학습하도록 도와주는 속성 + 시퀀스 내 문자의 위치를 얻게 됩니다.

모델에 PE(Positional Encoding)를 추가하는 방법은 다음과 같습니다:

```python
# PE 클래스 정의
class PositionalEncoding(nn.Module):
    def __init__(self, context_length, d_model) -> None:
        super().__init__()
        # positional encodings을 저장할 (context_length, d_model) 모양의 행렬 생성
        pe = torch.zeros(context_length, d_model)
        
        # (context_length, 1) 모양의 위치 [0, 1, 2, ..., context_length-1]을 가진 벡터 생성
        position = torch.arange(0, context_length, dtype=torch.float).unsqueeze(1)
        
        # 차원에 따라 기본 자 기준 벡터 생성
        div_term = torch.exp(torch.arange(0, d_model, 2).float() * (-math.log(10000.0) / d_model))
        
        # 삼각함수를 사용하여 positional encodings 계산
        pe[:, 0::2] = torch.sin(position * div_term)
        pe[:, 1::2] = torch.cos(position * div_term)
        
        pe = pe.unsqueeze(0)  # 모양: (1, context_length, d_model)
        
        # pe를 버퍼로 등록하여 파라미터가 아닌 모듈 상태의 일부로 취급
        self.register_buffer('pe', pe)

    def forward(self, x: torch.Tensor) -> torch.Tensor:
        # 입력 임베딩에 positional encodings 추가
        return x + self.pe[:,:x.size(1), :] 

class GPT(nn.Module):
    def __init__(self, vocab_size, d_model):
        ...
       # positional encodings 초기화
       self.wpe = PositionalEncoding(context_length, d_model)
    
    def forward(self, inputs, targets = None):
        logits = self.wte(inputs)
        # 로짓을 PE에 전달
        logits = self.wpe(logits)
        ...
        return logits, loss
```

<div class="content-ad"></div>

Now, if you try to train the model and generate a sequence, you might encounter an error similar to the one below:


![Error Message](/assets/img/2024-07-13-BuildingGPT-2withPyTorchPart2_3.png)


This error indicates that we attempted to generate 1000 tokens one by one, passing the previous *n* tokens to the model to predict the next token. However, with the addition of the PositionalEmbedding layer, it now only accepts tokens of a size less than or equal to the context_length, which is set at 256 in our case.

Let's adjust our generate function to accommodate the context_length:


<div class="content-ad"></div>

```python
def generate(self, inputs, max_new_tokens):
   # 이 저장소는 초기 입력 시퀀스와 함께 모델 출력을 저장할 것입니다.
   # 모델과 간섭하지 않도록 복사본을 만듭니다.
   output = inputs.clone()
   for _ in range(max_new_tokens):
       current_seq_length = inputs.size(1)
       # context_length를 초과하면 입력을 자릅니다.
       if current_seq_length > context_length:
           inputs = inputs[:, -context_length:]
       ...
       output = torch.cat([output, idx_next], dim=1)
   return [decode(out.tolist()) for out in output]
```

모델을 이미 훈련시키고 향상을 관찰할 수 있지만, 그 전에 한 단계 더 추가해 보겠습니다. 현재 문자의 다른 표현을 얻고 모델에 피드하고 있는 방식을 상기해보세요. 만약 이 정보를 결합하고 더 복잡한 임베딩 표현을 학습하는 데 추가 네트워크가 있다면 얼마나 유익할까요?

이것이 Fully Connected Networks입니다. PyTorch의 Linear Layer를 모델에 추가해 보겠습니다:

![2024-07-13-BuildingGPT-2withPyTorchPart2_4](/assets/img/2024-07-13-BuildingGPT-2withPyTorchPart2_4.png)


<div class="content-ad"></div>

코드:

```js
class GPT(nn.Module):
    def __init__(self, vocab_size, d_model):
        ...
        self.fcn = nn.Sequential(
            nn.Linear(d_model, 4 * d_model),
            nn.GELU(),
            nn.Linear(4 * d_model, d_model)
        )

    def forward(self, inputs, targets = None):
        ...
        logits = self.fcn(logits)
        ...
        return logits, loss
```

그게 다야! 정말 간단하죠!! 이제 모델을 훈련하고 성능을 평가해볼게요. 이번 실행에서는 에포크를 5000으로, 학습률을 1e-3으로 설정했어요.

![이미지](/assets/img/2024-07-13-BuildingGPT-2withPyTorchPart2_5.png)

<div class="content-ad"></div>

여기 사진은 b. (가려진) 셀프 어텐션 + 정규화의 과정을 나타냅니다. 이 부분도 열심히 공부하시는군요! 계속해서 함께 알아보도록 하죠.

\![이미지](/assets/img/2024-07-13-BuildingGPT-2withPyTorchPart2_6.png)

🔥🔥🔥🔥🔥🔥🔥🔥🔥🔥🔥🔥🔥🔥🔥🔥🔥🔥🔥🔥🔥🔥🔥🔥🔥🔥

<div class="content-ad"></div>

트랜스포머 모델에서 가장 흥미로운 부분은 Self-Attention입니다. 이 개념을 더 잘 이해하기 위해 Jay Alammar의 시각 자료를 참조하세요. 간단히 말해서, Self-Attention은 현재 및 이전 n 토큰이 주어졌을 때 모델이 다음 토큰에 더 많은 관심을 기울어야 하는지를 정의합니다.

이를 위해 각 문자(우리의 경우)의 임베딩에 점수를 할당하고 이를 쿼리, 키, 밸류를 사용하여 다양한 맥락에 따라 결합합니다.

이론이 충분하다면, 코딩으로 넘어가 봅시다:

다음은 모델에 Self-Attention을 추가하는 방법입니다:

<div class="content-ad"></div>

```python
class SelfAttention(nn.Module):
    def __init__(self, d_model: int):
        super().__init__()
        
        self.query = nn.Linear(d_model, d_model)
        self.key = nn.Linear(d_model, d_model)
        self.value = nn.Linear(d_model, d_model)
        self.fc_out = nn.Linear(d_model, d_model)
        self.dropout = nn.Dropout(0.2)
    
    def forward(self, inputs: torch.Tensor):
        B, seq_length, d_model = inputs.shape
        
        # Project the input embeddings into Q, K, and V
        Q = self.query(inputs)
        K = self.key(inputs)
        V = self.value(inputs)
        
        # Compute attention scores
        attention_scores = torch.matmul(Q, K.transpose(-2, -1))
        
        # Apply mask to prevent attention to future tokens
        mask = torch.triu(torch.ones(seq_length, seq_length), diagonal=1).bool().to(inputs.device)
        attention_scores = attention_scores.masked_fill(mask, float('-inf'))
        
        attention_weights = torch.softmax(attention_scores, dim=-1)
        # Compute the weighted sum of the values
        attention_output = torch.matmul(attention_weights, V)

        # Apply the final linear transformation
        out = self.fc_out(attention_output)
        
        return out

class GPT(nn.Module):
    def __init__(self, vocab_size, d_model):
        ...
        self.att = SelfAttention(d_model)

    def forward(self, inputs, targets = None):
        ...
        logits = self.att(logits)
        logits = self.fcn(logits)
        ...
        return logits, loss
```

Just like that, we have our code ready. Let's embark on the training journey and witness the results:

![GPT-2 model](/assets/img/2024-07-13-BuildingGPT-2withPyTorchPart2_7.png)

😍Oh, wow! Do you feel the same way as I do? It seems like the model is grasping the essence of word representations and their harmonious arrangement in a piece of music, doesn't it? Truly remarkable! Imagine the magic when this layer evolves into Multi-Head.

<div class="content-ad"></div>

정규화(Normalization)

함께 모델을 훈련 중이라면 주목할 점 중 하나는 손실이 매우 빠르게 감소하는 동안 모델이 데이터에 과적합되기 시작한다는 것입니다. 이는 모델이 우리가 가진 제한된 훈련 데이터에 비해 너무 커지는 경우에 일어날 수 있습니다.

이를 완화하기 위해 LayerNorm과 Dropout 레이어를 추가하여 학습을 균형있게 맞추어 보겠습니다.

![image](/assets/img/2024-07-13-BuildingGPT-2withPyTorchPart2_8.png)

<div class="content-ad"></div>

코드:

```python
class GPT(nn.Module):
    def __init__(self, vocab_size, d_model):
        ...
        self.ln1 = nn.LayerNorm(d_model)
        self.ln2 = nn.LayerNorm(d_model)
        self.dropout = nn.Dropout(0.2)

    def forward(self, inputs, targets=None):
        ...
        logits = self.wte(inputs)
        logits = self.wpe(logits)
        att_logits = self.att(logits)
        adn_logits = self.ln1(logits + att_logits)
        logits = self.dropout(adn_logits)
        logits = self.fcn(logits)
        logits = self.ln2(logits + adn_logits)
        ...
        return logits, loss

    ...
```

데이터셋에 오버피팅 없이 모델을 더 오랫동안 학습할 수 있도록 도와줍니다.

빠른 변화

<div class="content-ad"></div>

이제 그 작업을 마쳤으니, d_model=vocab_size로 설정한 점을 기억해주세요. 왜냐하면 우리는 단 하나의 레이어인 Embedding 레이어만 가졌기 때문이죠.

자, 이제 Linear를 사용하여 적절한 매핑 레이어를 갖고 있기 때문에 우리는 원하는 숫자로 Embedding 크기를 변경하고 문자의 더 많은 표현을 학습할 수 있습니다. 512로 만들어보죠.

```python
# 임베딩 크기를 정의하는 데 사용됨
d_model = 512 

# embedding 차원(d_model)을 vocab_size로 변환하는 linear 레이어를 추가하는 것을 잊지 마세요
class GPT(nn.Module):
    def __init__(self, vocab_size, d_model):
        ...
        self.linear1 = nn.Linear(d_model, vocab_size)

    def forward(self, inputs, targets = None):
        ...
        logits = self.ln2(logits + adn_logits)
        logits = self.linear1(logits)
        ...
        return logits, loss
    
    ...
```

딱 이것만 바꾸면, 우리의 GPT-2 Transformer 디코더 아키텍처의 전체 모델을 완성했습니다.

<div class="content-ad"></div>

아직 끝나지 않았어요. 모델을 계속 향상시켜봅시다..

## c. (가리킨) 다중 헤드 어텐션

이미 셀프-어텐션 메커니즘의 강력함과 모델이 텍스트 내의 문맥적 관계를 일반화하는 능력을 알고 있을 수도 있어요. 하지만 만약에 모델이 텍스트 내에서 단어나 문자들이 어떻게 서로 연결되어 있고 그들의 시간적 사용 등과 같은 다양한 언어적 특성을 이해할 수 있는 방법이 있다면 어떨까요? 자음과 모음 사이의 차이, 그리고 어떤 상황에서 어떻게 사용해야 하는지를 학습하는 모델을 상상해보세요.

<div class="content-ad"></div>

흥미로운 내용이죠? Self-Attention에서 d_model에 대한 전체적인 시퀀스 문맥을 나타내는 동안, 이제 d_model을 여러 개의 헤드로 나눌 수 있습니다. 각 헤드는 Query, Key 및 Value를 위한 별도의 표현을 갖고 있어 모델이 시퀀스 내에서 여러 문맥적 뉘앙스를 학습할 수 있게 됩니다. 다중 헤드를 통합하여 우리의 어텐션 레이어를 강화해 봅시다.

![이미지](/assets/img/2024-07-13-BuildingGPT-2withPyTorchPart2_10.png)

코드:

```python
n_heads = 4  # self-attention 헤드의 수. d_model로 나눌 수 있어야 합니다

class MultiHeadAttention(nn.Module):
    def __init__(self, d_model: int, n_heads: int):
        super().__init__()
        
        self.n_heads = n_heads
        self.head_dim = d_model // n_heads

        assert (n_heads * self.head_dim == d_model)

        self.query = nn.Linear(d_model, d_model)
        self.key = nn.Linear(d_model, d_model)
        self.value = nn.Linear(d_model, d_model)
        self.fc_out = nn.Linear(d_model, d_model)
        self.dropout = nn.Dropout(0.2)

    def forward(self, inputs: torch.Tensor):
        B, seq_length, d_model = inputs.shape
        
        # 입력 임베딩을 Q, K 및 V로 프로젝션
        Q = self.query(inputs).view(B, seq_length, self.n_heads, self.head_dim).permute(0, 2, 1, 3)
        K = self.key(inputs).view(B, seq_length, self.n_heads, self.head_dim).permute(0, 2, 1, 3)
        V = self.value(inputs).view(B, seq_length, self.n_heads, self.head_dim).permute(0, 2, 1, 3)
        
        # 어텐션 스코어 계산
        attention_scores = torch.matmul(Q, K.transpose(-2, -1)) / math.sqrt(self.head_dim)
        
        # 미래 토큰에 대한 어텐션 방지를 위한 마스크 적용
        mask = torch.triu(torch.ones(seq_length, seq_length), diagonal=1).bool().to(inputs.device)
        attention_scores = attention_scores.masked_fill(mask, float('-inf'))
        
        attention_weights = torch.softmax(attention_scores, dim=-1)
        # 값들의 가중합 계산
        attention_output = torch.matmul(self.dropout(attention_weights), V)

        # 헤드들을 연결하고 원래 형태로 되돌림
        attention_output = attention_output.permute(0, 2, 1, 3).contiguous()
        attention_output = attention_output.view(B, seq_length, d_model)

        # 최종 선형 변환 적용
        out = self.fc_out(attention_output)
        
        return out

class GPT(nn.Module):
    def __init__(self, vocab_size, d_model, n_heads):
        super().__init__()
        ...
        # selfattention 레이어를 multiheadattention으로 대체
        self.att = MultiHeadAttention(d_model, n_heads)
        ...

m = GPT(vocab_size=vocab_size, d_model=d_model, n_heads=n_heads).to(device)
```

<div class="content-ad"></div>

이제, 편안히 앉아 계속해서 모델을 훈련시키고 마법을 보세요...

![BuildingGPT-2withPyTorchPart2_11.png](/assets/img/2024-07-13-BuildingGPT-2withPyTorchPart2_11.png)

이제 모델의 성능과 출력에서 상당한 향상을 볼 수 있어야 합니다. 이 모든 것은 Multi-Head attention 덕분입니다. 머리 크기를 조절하여 모델이 더 나은 표현을 배우는지 확인해 볼 수 있습니다.

## d. GPT 디코더 블록들

<div class="content-ad"></div>

마음에 드셨으면 좋겠네요!🔮

프로젝트를 통해 제시된 모델 다이어그램을 주의 깊게 살펴보시면, 직사각형 블록 안에 몇 개의 레이어가 추가된 것을 알 수 있습니다. 이것들을 '디코더 블록'이라고 합니다. 하지만 여러 개의 선형 네트워크 레이어를 추가할 수 있는 것처럼, 이 그룹 모델의 여러 블록도 추가할 수 있습니다. 어떻게 할지 알아보겠습니다:

먼저, 우리의 어텐션, 레이어 노름 및 피드 포워드 네트워크를 GPTBlock이라는 별도 모듈로 꺼내보겠습니다.
  
```python
class GPTBlock(nn.Module):
    def __init__(self, d_model, n_heads):
        super().__init__()
        self.att = MultiHeadAttention(d_model, n_heads)
        self.ln1 = nn.LayerNorm(d_model)
        self.ln2 = nn.LayerNorm(d_model)
        self.dropout = nn.Dropout(0.2)
        self.fcn = nn.Sequential(
            nn.Linear(d_model, 4 * d_model),
            nn.GELU(),
            nn.Linear(4 * d_model, d_model)
        )

    def forward(self, logits):
        att_logits = self.att(logits)
        adn_logits = self.ln1(logits + att_logits)
        logits = self.dropout(adn_logits)
        logits = self.fcn(logits)
        logits = self.ln2(logits + adn_logits)
        return logits
```

<div class="content-ad"></div>

이제 여기서 내부의 모든 레이어들을 블록으로 대체하는 GPT 클래스를 수정해봅시다. 추가로 생성자 매개변수 n_layer를 사용하여 디코더 블록/레이어의 개수를 정의합니다.

```python
n_layers = 2 # GPT 블록/레이어 수

class GPT(nn.Module):
    def __init__(self, vocab_size, d_model, n_heads, n_layers):
        super().__init__()
        self.wte = nn.Embedding(vocab_size, d_model) # 단어 토큰 임베딩
        self.wpe = PositionalEncoding(context_length, d_model) # 단어 위치 인코딩
        self.blocks = nn.ModuleList([GPTBlock(d_model, n_heads) for _ in range(n_layers)])
        self.linear1 = nn.Linear(d_model, vocab_size)

    def forward(self, inputs, targets=None):
        logits = self.wte(inputs) # 차원 -> 배치 크기, 시퀀스 길이, d_model
        logits = self.wpe(logits)
        for block in self.blocks:
            logits = block(logits)
        logits = self.linear1(logits)
        ...
        return logits, loss
    ...

m = GPT(vocab_size=vocab_size, d_model=d_model, n_heads=n_heads, n_layers=n_layers).to(device)
```

## e. 토크나이저 개선

이제 코드에서 한 가지 더 수정을 해야합니다. 토크나이저입니다. 네, 모델을 유용한 정보를 많이 얻을 수 있는데도 불구하고 많은 토큰으로 모델의 부하를 증가시키고 있는 문자 레벨 토크나이저를 개선해보겠습니다. GPT 토크나이저를 위한 공식 파이썬 라이브러리인 tiktoken 라이브러리를 사용하여 토크나이저를 개선해봅시다.

<div class="content-ad"></div>

이 라이브러리는 Byte Pair Encoding(BPE) 알고리즘을 사용합니다. 이 알고리즘은 토크나이저를 훈련할 때 단어나 단어의 여러 부분이 얼마나 자주 나타나는지를 기반으로 단어를 병합합니다.

설치:

```js
pip install tiktoken
```

코드:

<div class="content-ad"></div>

```python
import tiktoken

tokenizer = tiktoken.get_encoding('gpt2')
vocab_size = tokenizer.n_vocab
```

![Image](/assets/img/2024-07-13-BuildingGPT-2withPyTorchPart2_13.png)

We have now expanded our vocabulary size to 50257, allowing the model to encounter a wide range of words and sequences.

Next, we will encode our data using the updated tokenizer. Let's adjust our data initialization as follows:


<div class="content-ad"></div>

```python
import torch

# 시스템에 따라 CPU 또는 GPU 사용
device = "cpu"
if torch.cuda.is_available():
    device = "cuda"

data_dir = "data.txt"
text = open(data_dir, 'r').read() # 모든 데이터를 단순 문자열로 로드


# 텍스트 데이터를 토큰화된 텐서로 변환
data = torch.tensor(tokenizer.encode(text), dtype=torch.long, device=device)
```

그런 다음, 이전 인코딩(encode) 및 디코딩(decode) 기능 호출을 tokenizer.encode() 및 tokenizer.decode()로 변경하세요. 이 조정으로 새로운 토큰화 도구와 호환되도록 할 수 있습니다.

## f. 최종 GPT-2 훈련

🥳 마침내 프로젝트의 끝에 다다랐고 새로운 학습 경험을 얻었습니다. 몇 가지 조정을 해서 모델이 더 빨리 더 잘 훈련되도록 할 수 있습니다. 그러면 모든 준비가 끝납니다.


<div class="content-ad"></div>

以下是您请求的更改：

```js
context_length = 512  # 각 배치에서 처리되는 토큰 수
d_model = 512
n_layers = 1  # GPT 블록/레이어 수

class GPT(nn.Module):
    def __init__(self, vocab_size, d_model, n_heads, n_layers):
        super().__init__()
        self.wte = nn.Embedding(vocab_size, d_model)  # 단어 토큰 임베딩
        self.wpe = PositionalEncoding(context_length, d_model)  # 단어 위치 인코딩
        self.blocks = nn.ModuleList([GPTBlock(d_model, n_heads) for _ in range(n_layers)])
        self.linear1 = nn.Linear(d_model, vocab_size)
        
        # 매개변수 공유
        self.wte.weight = self.linear1.weight
```

GPT-2에서 매개변수 공유에 대해 더 자세히 알아보려면 여기를 확인하십시오.

현재 모델 구조를 시각화하려면 모델 변수 자체를 출력해주세요:

<div class="content-ad"></div>

그리고 이렇게 해서 우리만의 29M GPT-2 모델을 만들었습니다. 이건 우리의 사용 목적에 충분할 거에요.

모델을 훈련하기 전에 torch.compile을 사용해 모델을 컴파일해야 해요. 이렇게 하면 모델 내에서 발생하는 거의 모든 행렬 곱셈과 다른 작업들이 미리 매핑되어요. 간단히 말해서, 모델은 각 연산을 한 줄씩 또는 레이어별로 진행하는 대신 모든 작업을 합쳐 최종 단계를 직접 계산할 수 있어요.

<div class="content-ad"></div>

제가 학습률과 훈련 루프를 아래와 같이 수정했습니다:

```js
lr = 1e-3
optim = torch.optim.AdamW(m.parameters(), lr=lr, weight_decay=0.1)
scheduler = torch.optim.lr_scheduler.CosineAnnealingLR(optim, T_max=3000, eta_min=lr*0.1)
```

```js
epochs = 3500
eval_steps = 100  # n 스텝마다 평가 수행

# 손실값 저장
train_loss = {}

# 모델 훈련
for e in range(epochs):
    xb, yb = train_loader.get_batch()

    logits, loss = m(xb, yb)
    optim.zero_grad(set_to_none=True)
    loss.backward()

    # 그래디언트 클리핑
    torch.nn.utils.clip_grad_norm_(m.parameters(), max_norm=1)

    optim.step()
    scheduler.step()
    train_loss[e] = loss.item()

    if e % eval_steps == 0 or e == epochs - 1:
        m.eval()
        with torch.no_grad():
            xvb, yvb = eval_loader.get_batch()
            _, e_loss = m(xvb, yvb)

        print(f"Epoch: {e+ep}\ttrain_loss: {loss:.4f}\teval_loss: {e_loss:.4f}")

        m.train()  # 훈련 모드로 변경
```

모델 크기가 매우 커져서 이제 GoogleColab을 사용하여 모델을 훈련하고 있습니다. 이 링크를 클릭하여 Colab에서 열어볼 수 있습니다.

<div class="content-ad"></div>

After training for approximately 3500 epochs, here is the training loss curve I obtained:

![Training Loss Curve](/assets/img/2024-07-13-BuildingGPT-2withPyTorchPart2_15.png)

And here is a snippet of a song generated by the model 🎶🎶:

![Song Output](/assets/img/2024-07-13-BuildingGPT-2withPyTorchPart2_16.png)

<div class="content-ad"></div>

그곳에 있었습니다. 사용자 데이터로 사용자 정의 GPT-2 모델을 구축하고 훈련하는 단계별 안내서입니다. 데이터 요구 사항에 따라 하이퍼파라미터 및 레이어를 수정해 보세요.

이 프로젝트는 여기까지입니다. 하지만 여기서 멈추지 않을 거예요. 모델 성능 향상과 훈련에 관한 몇 가지 새 글을 준비 중이에요. 계속 기대해 주세요. 즐거운 학습되길 :).