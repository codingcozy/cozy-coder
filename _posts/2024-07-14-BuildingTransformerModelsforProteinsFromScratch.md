---
title: "기초부터 시작하는 단백질을 위한 트랜스포머 모델 구축 방법"
description: ""
coverImage: "/assets/img/2024-07-14-BuildingTransformerModelsforProteinsFromScratch_0.png"
date: 2024-07-14 01:26
ogImage: 
  url: /assets/img/2024-07-14-BuildingTransformerModelsforProteinsFromScratch_0.png
tag: Tech
originalTitle: "Building Transformer Models for Proteins From Scratch"
link: "https://medium.com/towards-data-science/building-transformer-models-for-proteins-from-scratch-60884eab5cc8"
---


# 소개

지난 글에서 단백질 과학 기초에 대한 이해를 바탕으로, 이제 인공지능/기계학습과 단백질 과학의 흥미로운 교차점을 탐험할 준비가 되었습니다. 이 글은 ChatGPT와 같은 고급 챗봇을 구동하는 기술인 트랜스포머 기반의 언어 모델에 초점을 맞춥니다. 트랜스포머에 대한 깊은 설명에는 많은 공간을 할애하지 않겠습니다. 이 부분에 대해서는 Jay Alammar의 "The Illustrated Transformer"라는 블로그 글을 강력히 추천합니다. 그 글은 자세한 설명과 아름다운 일러스트를 제공합니다. 대신, 우리는 실제로 단백질 분석을 위해 트랜스포머의 실제 구현에 초점을 맞출 것입니다.

구체적으로, 항체 서열의 항원 특이성을 예측하기 위해 기본적인 단백질 트랜스포머 모델을 구축할 것입니다. 이 프로젝트는 트랜스포머의 구현을 향상시키고 단백질 과학 내에서의 잠재적인 응용 분야를 탐색하는 데 도움이 될 것입니다.

"Attention Is All You Need"라는 획기적인 논문에서 소개된 트랜스포머는 인코더 및 디코더 구성요소를 갖는 신경망입니다. BERT (Bidirectional Encoder Representations from Transformers)와 같은 모델은 언어 이해를 위해 인코더를 활용하며 분류와 같은 하위 작업에서 뛰어난 성과를 보입니다. 여기서 우리는 HIV-1 또는 SARS-CoV-2 특이성을 가진 항체를 분류하기 위해 인코더 기반 모델을 구현하고 훈련할 것입니다 (Figure 1).

<div class="content-ad"></div>

![Image](https://miro.medium.com/v2/resize:fit:1400/1*1FmDSkoFMkpCRYrNGJFy-g.gif)

# 코드

이 문서 전반에 걸쳐서, 설명과 명확성을 위한 코드 스니펫을 제공할 것입니다. 완전하고 문서화된 코드 및 스크립트는 이 문서를 위한 GitHub 저장소를 참조해 주세요.

# 데이터

<div class="content-ad"></div>

이 프로젝트에서는 HIV-1 또는 SARS-CoV-2를 특정 대상으로 하는 몇 백 개의 BCR(항체) 데이터 세트를 컴파일했습니다. 이 데이터 세트는 IEDB에서 수집되었으며 크리에이티브 커먼즈 저작권 4.0 국제 라이선스 (CC BY 4.0)에 따라 라이선스가 부여되었습니다. 상업적 이용이 허용됩니다. 목표는 transformer 모델을 훈련하여 항체 서열이 어떤 항원에 결합하는지 예측하는 것입니다.

![BuildingTransformerModelsforProteinsFromScratch](/assets/img/2024-07-14-BuildingTransformerModelsforProteinsFromScratch_0.png)

데이터 세트는 초기에 훈련 세트와 보류된 테스트 세트로 분할되었습니다. 모델 훈련 및 튜닝 중에 훈련 세트는 더 작은 훈련 하위 집합과 검증 하위 집합으로 나눠집니다. 가공된 데이터 세트는 GitHub 저장소의 데이터 디렉토리에서 다운로드할 수 있으며, 전처리 단계는 notebooks/bcr_preprocessing.ipynb 노트북에 자세히 기술되어 있습니다. 우리는 아래와 같이 커스텀 PyTorch Dataset 클래스를 구현하여 모델을 위해 데이터를로드하고 준비할 것입니다:

```python
class BCRDataset(Dataset):

    def __init__(self, df: pd.DataFrame):
        super().__init__()
        self.df = df

    def __len__(self) -> int:
        return len(self.df)

    def __getitem__(self, i) -> tuple[str, int]:
        x = self.df.loc[i, "sequence"]
        y = self.df.loc[i, "label"]
        return x, y 
```

<div class="content-ad"></div>

이 코드는 BCRDataset 클래스를 정의합니다. 이 클래스는 Pandas DataFrame에 저장된 항체 데이터를 다루기 위해 설계된 사용자 정의 데이터셋입니다. 이 클래스에는 데이터프레임을 사용하여 데이터셋을 초기화하는 함수인 init, 데이터프레임 내 전체 샘플(행) 수를 반환하는 함수인 len 및 인덱스로 단일 데이터 샘플을 검색하는 함수인 getitem이 포함되어 있습니다. Getitem 함수는 데이터프레임에서 아미노산 서열 (`sequence`)과 해당 레이블 (`label`)을 추출하고 이를 튜플로 반환하는 역할을 합니다.

학습 중에 이 데이터셋 클래스는 데이터를 효율적으로 가져와 전처리할 때 사용될 것입니다. 그런 다음 데이터셋은 모델에 전달될 것이며, 이는 데이터가 어떻게 배치되고 GPU(사용 가능한 경우)로 로드되는지 관리하는 사용자 정의 DataLoader에 의해 처리될 것입니다.

# Tokenization

트랜스포머는 원시 텍스트나 단백질 서열을 직접 처리하지 않습니다. 대신, 입력은 먼저 토큰으로 분해됩니다. 단백질 분석에서 우리는 단백질 서열을 개별 아미노산으로 토큰화합니다. 그런 다음 각 아미노산은 시작, 끝, 패딩 및 알 수 없는 심볼에 대한 특별 토큰과 함께 고유한 정수 값을 할당받습니다. 이 과정을 통해 단백질 서열이 변환되어 트랜스포머 모델이 이해할 수 있는 숫자 표현으로 번역됩니다.

<div class="content-ad"></div>

여기는 토큰화 과정에 필수적인 코드입니다:

```js
class Tokenizer:

    def __init__(self):
        # 특수 토큰
        vocab = ["<cls>", "<pad>", "<eos>", "<unk>"]
        # 20개의 기본 아미노산
        vocab += list("ACDEFGHIKLMNPQRSTVWY")
        # 매핑
        self.token_to_index = {tok: i for i, tok in enumerate(vocab)}
        self.index_to_token = {i: tok for i, tok in enumerate(vocab)}

    def __call__(
        self, seqs: list[str], padding: bool = True
    ) -> dict[str, list[list[int]]]:
        """
        단백질 서열의 목록을 토큰화하고 어텐션 마스크를 포함한 입력 표현을 생성합니다.
        """

        input_ids = []
        attention_mask = []

        if padding:
            max_len = max(len(seq) for seq in seqs)

        for seq in seqs:
            # 전처리: 공백 제거, 대문자로 변환
            seq = seq.strip().upper()

            # 특수 토큰 추가
            toks = ["<cls>"] + list(seq) + ["<eos>"]

            if padding:
                # '<pad>' 토큰으로 채워 최대 길이에 도달
                toks += ["<pad>"] * (max_len - len(seq))

            # 토큰을 ID로 변환 (알 수 없는 아미노산 처리)
            unk_id = self.token_to_index["<unk>"]
            input_ids.append([self.token_to_index.get(tok, unk_id) for tok in toks])

            # 어텐션 마스크 생성 (실제 토큰은 1, 패딩은 0)
            attention_mask.append([1 if tok != "<pad>" else 0 for tok in toks])

        return {"input_ids": input_ids, "attention_mask": attention_mask}
```

Tokenizer는 `cls`, `eos`, 그리고 단백질 서열 내부의 패딩을 나타내는 `pad`와 같은 특수 토큰을 활용합니다. 또한, 알 수 없는 아미노산에 대해서는 `unk` 토큰을 사용합니다. 초기화 단계(__init__)에서는 아미노산, 특수 토큰, 그리고 각각의 숫자 표현 사이에 매핑을 수립하여 어휘를 구축합니다. 핵심 토큰화 과정은 __call__ 함수 내에서 이루어지며, 여기서는 특수 토큰이 추가되고, 서열이 정수 ID의 목록으로 변환되며, 균일한 입력 길이를 위해 선택적으로 패딩이 적용됩니다.

# 임베딩 및 위치 인코딩


<div class="content-ad"></div>

토큰화 후에는 임베딩 레이어를 사용하여 우리의 아미노산을 나타내는 정수값을 부동 소수점 숫자 벡터로 변환합니다. 임베딩은 고차원 공간 내의 좌표로 생각할 수 있습니다. 훈련 중에 모델은 이 공간 내에서 유사한 아미노산을 가까이 배치하도록 학습합니다.

![Figure 3](/assets/img/2024-07-14-BuildingTransformerModelsforProteinsFromScratch_1.png)

아미노산의 순서는 단백질 서열의 주요 열을 결정합니다. 그러나 트랜스포머 모델은 순차적 위치에 대한 본질적인 이해가 부족합니다. 이를 해결하기 위해 위치 인코딩을 사용합니다. 위치 인코딩은 아미노산 임베딩에 추가되는 특별한 신호로, 모델에게 단백질 서열 내 각 아미노산의 상대적 위치에 대한 정보를 제공합니다. Figure 4에서 보이는 것처럼, 이는 서열 내 각 위치에서 고유한 인코딩 패턴으로 이어집니다.

![Figure 4](/assets/img/2024-07-14-BuildingTransformerModelsforProteinsFromScratch_2.png)

<div class="content-ad"></div>

# 셀프 어텐션

토큰화 후, 우리의 아미노산 정수들은 임베딩 벡터로 변환됩니다. 이러한 임베딩 각각은 선형 변환이 사용되어 쿼리(q), 키(k), 밸류(v) 세 개의 새로운 벡터를 생성하는 데 사용됩니다. 이러한 벡터들은 트랜스포머 내에서 셀프 어텐션을 가능케 하는 핵심 메커니즘인 스케일드 닷-프로덕트 어텐션을 계산하는 데 필수적입니다. 스케일드 닷-프로덕트 어텐션 공식은 다음과 같습니다:

![](/assets/img/2024-07-14-BuildingTransformerModelsforProteinsFromScratch_3.png)

각 토큰의 쿼리 벡터는 시퀀스 내 모든 토큰(자기 자신 포함)의 키 벡터와 닷 프로덕트를 사용하여 비교됩니다. 닷 프로덕트는 유사성을 측정하므로 더 높은 닷 프로덕트는 토큰 간의 더 강력한 관계를 제안합니다. 안정성을 향상시키기 위해 이러한 닷 프로덕트를 임베딩 차원(dk)의 제곱근으로 스케일링합니다. 또한 필요한 경우 특정 토큰을 마스킹할 수도 있습니다.

<div class="content-ad"></div>

닷 프로덕트에 softmax 함수를 적용하여 우리는 어텐션 점수를 얻습니다. 이러한 점수는 각 토큰이 시퀀스의 다른 토큰에 "주의를 기울여야 하는" 정도를 알려줍니다.

마지막으로, 각 토큰에 대해 모든 토큰의 값 벡터(v)를 그들 각자의 어텐션 점수에 따라 가중합하여 새로운 표현을 계산합니다. 이는 시퀀스 내에서 가장 관련성 높은 아미노산이 토큰의 새로운 표현에 더 큰 영향을 미친다는 것을 의미합니다. 코드로 이를 어떻게 구현할 수 있는지 살펴보겠습니다:

```python
def scale_dot_product_attention(
    q: torch.Tensor, k: torch.Tensor, v: torch.Tensor, mask: torch.Tensor = None
) -> tuple[torch.Tensor, torch.Tensor]:
    """
    Transformer 아키텍처에서 핵심 구성 요소인 스케일된 닷 프로덕트 어텐션을 구현합니다.
    """

    dk = q.shape[-1]
    attn_logits = q @ k.transpose(-2, -1) / np.sqrt(dk)

    if mask is not None:
        # 매우 큰 음수 값으로 마스킹 처리
        attn_logits.masked_fill_(mask == 0, -1e9)

    attention = torch.softmax(attn_logits, dim=-1)
    values = attention @ v

    return values, attention
```

과거 RNN 모델과는 달리, 트랜스포머는 행렬 곱셈을 사용하여 모든 토큰에 대한 이러한 어텐션 관계를 병렬로 계산합니다. 이는 GPU의 강력함을 최대한 활용하여 속도와 효율성을 크게 향상시킨다.

<div class="content-ad"></div>

# 멀티헤드 어텐션

셀프 어텐션은 단백질 서열 내 아미노산 간의 관계를 포착하는 강력한 메커니즘을 제공하지만, 멀티헤드 어텐션은 이 개념을 더욱 발전시킵니다. 멀티헤드 어텐션은 병렬로 여러 개의 셀프 어텐션 메커니즘을 사용하여, 각 "헤드"가 입력의 서로 다른 측면에 집중할 수 있도록 합니다. 이를 통해 모델은 단백질 서열 내의 다양한 관계와 미묘한 차이를 포착할 수 있는 능력을 갖게 됩니다. 다수의 헤드로부터의 출력은 결합되어 각 토큰의 풍부한 표현을 이끌어냄으로써 더욱 풍성한 표현이 가능해집니다. 중요한 점은 단일 헤드 셀프 어텐션과 마찬가지로, 트랜스포머는 모델의 계산 효율성을 유지하는 동시에 여러 어텐션 헤드를 동시에 계산하기 위해 행렬 곱셈을 활용합니다. 아래는 이것이 어떻게 구현되는지에 대한 간단한 예시입니다:

```python
클래스 MultiheadAttention(nn.Module):
    # ... (클래스의 다른 부분들)
def forward(
        self, x: torch.Tensor, mask: torch.Tensor = None, return_attention: bool = False
    ) -> torch.Tensor:
        """
        입력에 대해 멀티헤드 어텐션을 수행합니다.
        """
        batch_size, seq_len = x.shape[0], x.shape[1]
        x = self.input(x)
        # 헤드 분할
        x = x.reshape(batch_size, seq_len, self.num_heads, 3 * self.head_dim)
        # 차원 교환
        x = x.permute(0, 2, 1, 3)
        # q, k, v
        q, k, v = x.chunk(3, dim=-1)
        # 4D로 마스크 확장
        if mask is not None:
            mask = self.expand_mask(mask)
        # 스케일링된 닷 프로덕트 어텐션 수행
        # values: (batch_size, num_heads, seq_len, head_dim)
        # attention: (batch_size, num_heads, seq_len, seq_len)
        values, attention = scale_dot_product_attention(q, k, v, mask=mask)
        # 차원 변경
        values = values.permute(
            0, 2, 1, 3
        )
        # 헤드 연결
        values = values.reshape(
            batch_size, seq_len, -1
        )
        # 출력 레이어
        out = self.output(values)
        if return_attention:
            return out, attention
        return out
```

이 코드 조각은 멀티헤드 어텐션 메커니즘의 핵심 기능을 보여줍니다. x = self.input(x) 줄은 입력 시퀀스를 더 높은 차원으로 옮겨, 모델이 쿼리(모델이 집중하는 대상), 키(관련성 점수 매기기에 사용), 값(추출해야 하는 실제 정보)에 대해 별도의 표현을 만들 수 있도록 합니다. 이어지는 몇 줄은 프로젝트된 입력을 여러 헤드(self.num_heads)로 분할하고, 효율적인 멀티헤드 계산을 위해 재구성합니다. 최종적으로, 코드는 트랜스포머 아키텍처 내에서 어텐션 계산을 수행하기 위한 필수 요소인 개별 쿼리, 키, 밸류(q, k, v)를 추출합니다.

<div class="content-ad"></div>

# 인코더 레이어와 인코더

우리는 이제 여러 헤드 어텐션 구현을 기반으로하여 트랜스포머 인코더의 구성 요소인 인코더 레이어를 만들 수 있습니다. 각 인코더 레이어는 단백질 서열 내에서 아미노산 간의 관계를 파악하기 위한 여러 헤드 어텐션 메커니즘을 포함하고, 이어서 추가 처리를 위한 완전히 연결된 피드 포워드 네트워크(FFN) 레이어가 포함됩니다. 또한, 각 레이어 내에서 잔여 연결 및 레이어 정규화 기술이 사용되어 교육 안정성과 성능을 향상시킵니다.

다음은 인코더 레이어를 구현하는 방법입니다:

```python
class EncoderLayer(nn.Module):
    # ... (클래스의 다른 부분들) 

    def forward(self, x: torch.Tensor, mask: torch.Tensor = None) -> torch.Tensor:
        """
        인풋 시퀀스를 인코더 블록을 통해 전달합니다.
        """

        #  여러 헤드 어텐션 레이어
        attn_out = self.attention(x, mask)

        # 잔여 연결
        x = x + self.dropout(attn_out)
        # 레이어 정규화
        x = self.norm1(x)

        # 피드 포워드
        ffn_out = self.ffn(x)
        # 잔여 연결
        x = x + self.dropout(ffn_out)
        # 레이어 정규화
        x = self.norm2(x)

        return x
```

<div class="content-ad"></div>

여러 인코더 레이어를 쌓음으로써 모델은 단백질 서열의 더 복잡하고 계층적인 표현을 학습할 수 있습니다. 인코더의 순방향 함수는 각 쌓인 인코더 레이어를 통해 입력 시퀀스의 통과를 조정합니다. 각 레이어에서 멀티헤드 어텐션과 피드포워드 네트워크는 단백질 서열의 표현을 점진적으로 개선합니다. 최종 인코더 레이어의 출력은 하류 작업에 사용할 준비가 된 매우 유익한 인코딩된 표현입니다.

```python
class Encoder(nn.Module):
    # ... (클래스의 다른 부분)

    def forward(self, x: torch.Tensor, mask: torch.Tensor = None) -> torch.Tensor:
        """
        입력 시퀀스를 모든 인코더 레이어를 통과시킵니다.
        """

        for layer in self.layers:
            x = layer(x, mask)

        return x
```

## 분류 모델

이제 우리가 구축한 트랜스포머 인코더를 사용하여 항체 분류 모델을 완성할 수 있습니다. 모델은 다음과 같이 작동합니다: 먼저 이전 단계와 마찬가지로 아미노산 토큰을 숫자 표현으로 임베딩하고 위치 인코딩 신호를 추가합니다. 모델의 핵심인 인코더는 이러한 시퀀스 임베딩을 처리하고 아미노산 간의 복잡한 관계를 학습합니다. 전체 항체 서열의 단일 표현을 얻기 위해 각 토큰에 대한 인코더의 출력을 평균합니다. 마지막으로 FFN은 이 평균화된 시퀀스 표현을 처리하고 하나의 값(또는 로짓)을 출력하여 항체 클래스를 예측합니다.

<div class="content-ad"></div>

```python
class AntibodyClassifier(nn.Module):
    # ... (Class 정의, __init__ 및 mean_pooling은 간략히 제외) 

    def forward(self, batch: dict[str, torch.Tensor]) -> torch.Tensor:
        """모델을 통한 순방향 전파."""

        input_ids, attention_mask = batch["input_ids"], batch["attention_mask"]

        # 토큰을 벡터로 매핑
        x = self.embedding(input_ids)

        # 위치 인코딩 추가
        x = self.pe(x) 

        # 시퀀스 처리를 위해 Transformer 인코더를 통과
        x = self.encoder(x, attention_mask)

        # 평균 풀링
        x = self.mean_pooling(x, attention_mask)

        # 분류 헤드
        logits = self.classifier(x)

        return logits
```

순방향 함수는 입력 항체 서열을 클래스 예측으로 변환하는 과정을 단계별로 설명합니다(예: HIV-1 또는 SARS-CoV-2). 먼저, 모델은 아미노산 토큰을 임베딩하고 위치 정보를 추가합니다. 그 다음, 모델의 핵심인 Transformer 인코더가 이 시퀀스 표현을 처리하여 단백질 내 복잡한 관계를 학습합니다. 전체 시퀀스를 대표하는 단일 벡터를 얻기 위해 평균 풀링을 적용하며, 이때 주의 마스크도 고려됩니다. 마지막으로, 피드포워드 분류 헤드가 이 풀링된 표현을 사용하여 각 항체 클래스에 대한 로짓(정규화되지 않은 확률)을 생성합니다.

# 모델 훈련

다음 단계는 이전에 준비한 HIV-1 및 SARS-CoV-2 데이터를 사용하여 항체 분류기를 훈련하는 것입니다. 모델을 훈련하려면 프로젝트 저장소의 루트 디렉토리에서 터미널을 사용하여 train.py 스크립트를 실행하십시오. 이 스크립트는 훈련 프로세스를 제어하는 다양한 맞춤 설정 가능한 매개변수를 제공합니다. 예를 들어, 기본 매개변수로 훈련 스크립트를 실행하고 결과를 train01이라는 run ID로 저장하려면 다음 명령을 사용하십시오:

<div class="content-ad"></div>

```bash
python protein_transformer/train.py --run-id train01 --dataset-loc data/bcr_train.parquet
```

이 곳에는 몇 가지 중요한 기본 하이퍼파라미터가 있습니다:

- `embedding_dim = 64`: 토큰 임베딩의 차원.
- `num_layers = 8`: 인코더 레이어의 수.
- `num_heads = 2`: 인코더 내의 어텐션 헤드 수.
- `ffn_dim = 128`: 인코더의 피드포워드 레이어의 차원.
- `dropout = 0.05`: 정규화를 위한 드롭아웃 비율.

훈련 중에는 훈련 손실과 검증 손실을 모니터링하세요. 이 이상적인 경우, 둘 다 에폭마다 감소해야 합니다. 이 손실들을 에폭에 대한 그림으로 나타내면 훈련 과정에 대한 유용한 통찰을 얻을 수 있습니다 (Figure 5).

<div class="content-ad"></div>

`![BuildingTransformerModelsforProteinsFromScratch_4](/assets/img/2024-07-14-BuildingTransformerModelsforProteinsFromScratch_4.png)`

Upon completion, the script stores training results in the `runs/train01` directory by default. This includes model arguments, the best-performing model (based on validation loss), training and validation loss records, along with validation metrics for each epoch. These metrics, which include the following, are saved in the `runs/train01/results.csv` file:

```js
Accuracy: 0.727
AUC score: 0.851
Precision: 0.734
Recall: 0.727
F1-score: 0.725
```

We can potentially improve our model’s performance even further by exploring different hyperparameter combinations. We’ll cover hyperparameter tuning in the next section.

<div class="content-ad"></div>

# 하이퍼파라미터 튜닝

우리의 항체 분류기 성능을 향상시키기 위해 Ray Tune을 사용하여 하이퍼파라미터 튜닝을 탐색할 것입니다. 이를 통해 효율적으로 병렬로 실험을 실행할 수 있습니다. 우리가 조사할 검색 공간은 다음과 같습니다:

- **임베딩 차원**: 16, 32, 64, 또는 128의 값.
- **레이어 수**: 1부터 8까지의 값.
- **헤드 수**: 1, 2, 4, 또는 8의 값.
- **드롭아웃**: 0에서 0.2 사이의 값으로 0.02의 증가량을 가짐.
- **학습률 (learning rate)**: 1e-5부터 1e-3까지의 값 (로그-균일 분포).

기본 매개변수로 튜닝 프로세스를 시작하고 결과를 tune01이라는 실행 ID로 저장하려면 프로젝트 루트 디렉토리에서 tune.py 스크립트를 실행하세요.

<div class="content-ad"></div>


python protein_transformer/tune.py --run-id tune01 --dataset-loc /home/ytian/github/protein-transformer/data/bcr_train.parquet


기본 설정으로는 각각 최대 30번 에폭까지 실행되는 다양한 매개변수 조합을 사용하여 100회의 실행을 수행합니다. Ray Tune은 유망하지 않은 시행에 대해 조기 중지를 사용하여 하이퍼파라미터 공간을 효율적으로 탐색하고 성능이 더 좋은 구성에 리소스를 집중시킵니다. 각 시행의 결과를 추적하며, 완료될 때 가장 성능이 우수한 검증 손실 기준의 모델은 기본 설정에 따라 runs/tune01 디렉토리에 저장됩니다. 또한, 각 시행의 결과를 포함하는 튜닝 로그는 동일한 runs/tune01 디렉토리에 저장되어 쉬운 접근과 분석을 위해 보관됩니다.

# 모델 평가

이제 하이퍼파라미터 튜닝 프로세스에서 가장 성능이 우수한 모델의 성능을 홀드아웃 테스트 데이터셋에서 평가하는 시간입니다. 이 데이터셋은 교육 및 튜닝 중에 별도로 유지되어 평가가 실제 일반화 능력을 반영하도록 합니다.


<div class="content-ad"></div>

예를 들어, tune01 실행에서 최적 모델을 평가하려면 다음 명령을 명령줄에서 실행하십시오:

```bash
python protein_transformer/evaluate.py --run-dir runs/tune01 --dataset-loc /home/ytian/github/protein-transformer/data/bcr_test.parquet
```

작업을 완료하면 evaluate.py 명령에서 제공한 실행 디렉터리 내에 test_metrics.json이라는 파일에 테스트 메트릭을 저장합니다. 다음 예와 같이:

```json
정확도: 0.761
AUC 점수: 0.837
정밀도: 0.761
재현율: 0.761
F1 점수: 0.761
```

<div class="content-ad"></div>

우리 모델의 성능을 깊이 이해하기 위해 두 가지 주요 시각화인 혼동 행렬과 ROC(수신자 조작 특성) 곡선을 살펴보겠습니다. 혼동 행렬은 모델의 예측을 진양성, 진음성, 가양성 및 가음성으로 분해하여 모델이 어디에서 뛰어나며 어디에서 고전할 수 있는지 통찰을 제공합니다. ROC 곡선은 모델이 서로 다른 결정 임계값에서 클래스를 구별하는 능력을 보여줌으로써 곡선 아래 전체 영역(AUC)이 효과를 나타냅니다.

도표 6에서 보듯이 혼동 행렬은 HIV-1 및 SARS-CoV-2 클래스 모두에 대해 균형 잡힌 분포를 보여줍니다. 이는 모델이 대부분의 샘플을 올바르게 분류할 수 있는 능력을 나타냅니다. ROC 곡선은 이를 더 지지하여 클래스 간의 구별 능력을 잘 보여주며(0.84의 AUC 점수), 무작위 모델보다 훌륭한 성능을 나타냅니다.

![그림](/assets/img/2024-07-14-BuildingTransformerModelsforProteinsFromScratch_5.png)

# 요약

<div class="content-ad"></div>

이 블로그 포스트에서는 항체 분류를 위한 트랜스포머 기반 모델 구축의 실제적 측면을 탐구했습니다. 우리는 HIV-1 및 SARS-CoV-2 데이터셋에서 항체 서열 데이터를 수집하고 전처리하는 것부터 시작했습니다. 그 다음으로 이러한 서열을 토큰화하고 숫자 표현(embeddings)으로 변환한 후, 트랜스포머 레이어를 사용하여 인코더 아키텍처를 구축했습니다. 예측을 위해 인코더 위에 분류 머리를 추가했습니다. 그런 다음 모델을 훈련시키고 성능을 향상시키기 위해 Ray Tune을 사용한 하이퍼파라미터 튜닝을 탐험했으며, 마지막으로 정확도, AUC 점수, 정밀도, 재현율 및 F1 점수와 같은 메트릭을 사용하여 최고의 모델을 홀드아웃 테스트 세트에서 평가했습니다.

# 향후 작업

우리가 여기에서 구현한 트랜스포머 모델은 비교적 간단하지만, 단백질 과학 분야에서 이러한 모델의 잠재력을 보여주고 있습니다. 실제로 ESM-2와 같은 여러 트랜스포머 기반 단백질 언어 모델(PLMs)은 수백만 개의 단백질 서열에 대해 사전 훈련되어 다양한 하류 작업(예: 단백질 특성 및 구조 예측)에 적응될 수 있습니다. 우리는 이 흥미로운 영역을 향후 포스트에서 계속 탐구할 것입니다.

# 감사의 말씀

<div class="content-ad"></div>

- IEDB
- UvA Deep Learning
- Made With ML