---
title: "Transformer 모델 파인 튜닝 및 Segment Anything 사용법 알아보기"
description: ""
coverImage: "/assets/img/2024-07-06-LearnTransformerFine-TuningandSegmentAnything_0.png"
date: 2024-07-06 11:33
ogImage:
  url: /assets/img/2024-07-06-LearnTransformerFine-TuningandSegmentAnything_0.png
tag: Tech
originalTitle: "Learn Transformer Fine-Tuning and Segment Anything"
link: "https://medium.com/towards-data-science/learn-transformer-fine-tuning-and-segment-anything-481c6c4ac802"
isUpdated: true
---

## Train Meta’s Segment Anything Model (SAM) to segment high fidelity masks for any domain

타로 전문가님, 여러 강력한 공개 기초 모델들의 출시와 세밀 조정 기술의 발전으로, 머신 러닝과 인공 지능의 새로운 패러다임이 탄생했습니다. 이 혁명의 중심에는 트랜스포머 모델이 있습니다.

과거에는 고정확도의 도메인 특화 모델이 핵심적으로 중요했지만, 이제 기초 모델 패러다임은 학생이나 독립적인 연구원들이도 고액 지급 기업을 제외한 대다수의 자본으로 금일의 최신 전용 모델과 견주어질 수 있는 결과를 달성할 수 있게 해줍니다.

![Image](/assets/img/2024-07-06-LearnTransformerFine-TuningandSegmentAnything_0.png)

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

이 글은 Meta의 Segment Anything Model (SAM)을 리모트 센싱 작업 중 하천 픽셀 세분화에 적용한 내용을 다룹니다. 만약 코드를 바로 확인해보고 싶다면, 해당 프로젝트의 소스 파일은 GitHub에 있고 데이터는 HuggingFace에서 확인할 수 있습니다. 하지만 전문을 먼저 읽는 것이 좋습니다.

# 프로젝트 요구사항

첫 번째 단계는 적합한 데이터셋을 찾거나 생성하는 것입니다. 기존 문헌에 따르면, SAM의 좋은 파인튜닝 데이터셋은 적어도 200~800개의 이미지를 가지고 있어야 합니다. 딥 러닝 발전의 지난 10년간의 중요한 교훈은 데이터가 많을수록 항상 좋다는 것이므로, 더 큰 파인튜닝 데이터셋을 선택하는 것이 잘못되지 않습니다. 그러나 기본 모델의 목표는 비교적 작은 데이터셋도 강력한 성능에 충분하도록 하는 것입니다.

또한, HuggingFace 계정이 필요하며, 여기에서 계정을 생성할 수 있습니다. HuggingFace를 사용하여 우리는 언제든지 어떤 기기에서도 데이터셋을 쉽게 저장하고 가져올 수 있어서 협업과 재현성을 쉽게 유지할 수 있습니다.

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

마지막 요구 조건은 학습 워크플로우를 실행할 수 있는 GPU가 장착된 장치입니다. Nvidia T4 GPU는 Google Colab을 통해 무료로 이용할 수 있으며, 1000개의 이미지에 대해 50회 에포크로 sam-vit-huge 모델 체크포인트를 학습하는 데 충분한 성능을 갖추고 있습니다. 이를 12시간 이내에 완료할 수 있습니다.

호스팅된 런타임의 사용 제한으로 인한 진행 상실을 피하려면 Google 드라이브를 연결하고 각 모델 체크포인트를 거기에 저장할 수 있습니다. 또는 제한을 완전히 우회하려면 GCP 가상 머신을 배포하고 연결할 수 있습니다. GCP를 처음 사용하는 경우 무료 $300 달러 크레딧을 받을 자격이 있으며, 이는 모델을 최소 한 번은 열두 번 이상 학습하는 데 충분합니다.

# SAM 이해

학습을 시작하기 전에 SAM의 구조를 이해해야 합니다. 모델에는 세 가지 구성 요소가 포함되어 있습니다: 이미지 인코더(최소한으로 수정된 마스크된 자동 인코더), 다양한 프롬프트 유형을 처리할 수 있는 유연한 프롬프트 인코더 및 빠르고 가벼운 마스크 디코더가 있습니다. 이 설계의 한 동기는 이미지 임베딩이 한 번만 계산되면 되고 마스크 디코더가 CPU에서 약 50ms에 실행될 수 있기 때문에 엣지 장치(예: 브라우저)에서 신속한 실시간 세그멘테이션이 가능하도록 하는 것입니다.

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

이론적으로, 이미지 인코더는 이미 최적의 이미지 임베딩 방법을 익혔으며, 모양, 가장자리 및 기타 일반적인 시각적 특성을 식별했습니다. 마찬가지로, 프롬프트 인코더는 이미 최적으로 프롬프트를 인코딩할 수 있습니다. 마스크 디코더는 모델 아키텍처의 일부로, 이미지 및 프롬프트 임베딩을 가져와 실제로 이미지 및 프롬프트 임베딩에 작용하여 마스크를 생성하는 역할을 합니다.

따라서 하나의 접근 방법은 훈련 중에 이미지 및 프롬프트 인코더와 관련된 모델 매개변수를 동결하고 마스크 디코더 가중치만 업데이트하는 것입니다. 이 방법은 지도 및 비지도 하향식 작업을 모두 수행할 수 있는 이점이 있습니다. 왜냐하면 제어점 및 경계 상자 프롬프트는 모두 자동화되어 있으며 사람들이 사용할 수 있기 때문입니다.

![image 1](/assets/img/2024-07-06-LearnTransformerFine-TuningandSegmentAnything_2.png)

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

대체적인 접근 방식은 프롬프트 인코더를 오버로드하는 것이며, 이미지 인코더를 고정시키고 마스크 디코더를 사용하지 않는 것입니다. 예를 들어, AutoSAM 아키텍처는 이미지를 기반으로 프롬프트 임베딩을 생성하기 위해 Harmonic Dense Net에 기반한 네트워크를 사용합니다. 이 강좌에서는 이미지와 프롬프트 인코더를 고정시키고 마스크 디코더만 학습하는 첫 번째 접근 방식을 다룰 것입니다. 그러나 이 대안적인 방법에 대한 코드는 AutoSAM GitHub 및 논문에서 찾을 수 있습니다.

## 프롬프트 구성하기

다음 단계는 모델이 추론 시 어떤 종류의 프롬프트를 받을지 결정하고, 이러한 유형의 프롬프트를 훈련 시 제공할 수 있도록 하는 것입니다. 실제로 나는 자연어 처리의 예측 불안정성 및 불일치성을 감안하여 본격적인 컴퓨터 비전 파이프라인에 텍스트 프롬프트의 사용을 권장하지 않습니다. 이로 인해 포인트와 바운딩 박스가 남게 되며, 특정 데이터셋의 성격에 따라 선택은 마침내 당신의 비전적인 특정 데이터셋의 성격에 달려 있습니다. 그러나 문헌에서는 바운딩 박스가 일관적으로 컨트롤 포인트보다 더 우월함을 발견했습니다.

이러한 이유는 완전히 명확하지 않지만 다음 요인 중 하나 또는 이들의 조합일 수 있습니다:

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

- 추론 시(실제 마스크가 알려지지 않은 상황) 좋은 제어 점을 선택하는 것은 바운딩 박스보다 어려울 수 있어요.
- 가능한 점 프롬프트 공간은 가능한 바운딩 박스 프롬프트 공간보다 몇 단계 크기가 크기 때문에 더 철저하게 훈련되지 않았어요.
- SAM 작성자들은 모델의 제로샷 및 퓨샷 능력(인간 프롬프트 상호작용 단계로 계산)에 중점을 두었기 때문에 미리 훈련된 모델은 바운딩 박스에 더 중점을 둔 것으로 보입니다.

그럼에도 불구하고, 강 세분화는 사실 점 프롬프트가 바운딩 박스를 약간 능가하는 드문 경우 중 하나에 속합니다(매우 유리한 도메인에서도 차이는 미미합니다). 강의 이미지에서 물체는 이미지의 한쪽 끝부터 다른 쪽 끝까지 이어지기 때문에 포괄적인 바운딩 박스는 거의 항상 이미지의 대부분을 덮을 것입니다. 따라서 같은 강의 매우 다른 부분에 대한 포괄적인 바운딩 박스는 이론적으로 매우 유사할 수 있으며, 그 결과 바운딩 박스는 모델에 점 제어보다 훨씬 적은 정보를 제공하므로 성능이 떨어지게 될 수 있습니다.

![이미지](/assets/img/2024-07-06-LearnTransformerFine-TuningandSegmentAnything_3.png)

위 그림에서 볼 수 있듯이, 두 강 부분의 실제 세분화 마스크가 완전히 다르지만, 그들의 바운딩 박스는 거의 동일하지만 점 프롬프트가 다르다고 할 수 있어요.

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

다른 중요한 요소는 추론 시 입력 프롬프트를 얼마나 쉽게 생성할 수 있는지입니다. 인간이 개입되는 것을 기대하는 경우, 바운딩 박스와 제어 점 모두 추론 시 상대적으로 쉽게 얻을 수 있습니다. 그러나 완전히 자동화된 파이프라인을 갖기를 원하는 경우, 이 질문에 대답하는 것이 좀 더 복잡해질 수 있습니다.

제어 점이나 바운딩 박스를 사용하든, 일반적으로 프롬프트를 생성하는 것은 주로 흥미로운 객체의 대략적인 마스크를 추정하는 것으로 시작합니다. 바운딩 박스는 대략적인 마스크를 감싸는 최소한의 상자일 수 있지만, 제어 점은 대략적인 마스크에서 샘플링해야 합니다. 이는 추정된 마스크를 사용해야 하는 경우, 제어 점은 객체의 윤곽에 더 가깝게 일치해야 하므로 바운딩 박스가 실존하는 경우 보다 획득하기 쉽다는 것을 의미합니다.

강 분할의 경우, RGB와 NIR에 모두 접근할 수 있다면, 분광 지수 임계값화 방법을 사용하여 대략적인 마스크를 얻을 수 있습니다. RGB만 접근하는 경우, 이미지를 HSV로 변환하고 특정 색조, 채도 및 값 범위 내의 모든 픽셀을 임계값화할 수 있습니다. 그런 다음, 일정 크기 임계값 이하의 연결된 구성 요소를 제거하고, skimage.morphology에서 침식을 사용하여 대형 파란 덩어리 중앙으로 이동한 픽셀만 마스크에 유지되도록 할 수 있습니다.

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

# 모델 훈련

우리 모델을 훈련하기 위해서는 각 훈련 에포크마다 반복할 수 있는 모든 훈련 데이터를 포함한 데이터 로더가 필요합니다. HuggingFace에서 데이터셋을 불러올 때, datasets.Dataset 클래스의 형식으로 받게 됩니다. 데이터셋이 비공개인 경우, 먼저 HuggingFace CLI를 설치하고 !huggingface-cli login을 사용하여 로그인해야 합니다.

```python
from datasets import load_dataset, load_from_disk, Dataset

hf_dataset_name = "stodoran/elwha-segmentation-v1"
training_data = load_dataset(hf_dataset_name, split="train")
validation_data = load_dataset(hf_dataset_name, split="validation")
```

이후 우리는 이미지와 레이블 뿐만 아니라 프롬프트도 반환하는 사용자 정의 데이터셋 클래스를 작성해야 합니다. 아래 구현은 제어 지점과 바운딩 박스 프롬프트 모두를 처리할 수 있습니다. 이를 초기화하기 위해서 HuggingFace datasets.Dataset 인스턴스와 SAM 프로세서 인스턴스가 필요합니다.

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
from torch.utils.data import Dataset

class PromptType:
    CONTROL_POINTS = "pts"
    BOUNDING_BOX = "bbox"

class SAMDataset(Dataset):
    def __init__(
        self,
        dataset,
        processor,
        prompt_type = PromptType.CONTROL_POINTS,
        num_positive = 3,
        num_negative = 0,
        erode = True,
        multi_mask = "mean",
        perturbation = 10,
        image_size = (1024, 1024),
        mask_size = (256, 256),
    ):
        # Assign all values to self
        ...

    def __len__(self):
        return len(self.dataset)

    def __getitem__(self, idx):
        datapoint = self.dataset[idx]
        input_image = cv2.resize(np.array(datapoint["image"]), self.image_size)
        ground_truth_mask = cv2.resize(np.array(datapoint["label"]), self.mask_size)

        if self.prompt_type == PromptType.CONTROL_POINTS:
            inputs = self._getitem_ctrlpts(input_image, ground_truth_mask)
        elif self.prompt_type == PromptType.BOUNDING_BOX:
            inputs = self._getitem_bbox(input_image, ground_truth_mask)

        inputs["ground_truth_mask"] = ground_truth_mask
        return inputs
```

We also have to define the `SAMDataset._getitem_ctrlpts` and `SAMDataset._getitem_bbox` functions. Although, if you only plan to use one prompt type, you can refactor the code to directly handle that type in `SAMDataset.__getitem__` and remove the helper function.

```python
class SAMDataset(Dataset):
    ...

    def _getitem_ctrlpts(self, input_image, ground_truth_mask):
        # Get control points prompt. See the GitHub for the source of this function, or replace with your own point selection algorithm.
        input_points, input_labels = generate_input_points(
            num_positive=self.num_positive,
            num_negative=self.num_negative,
            mask=ground_truth_mask,
            dynamic_distance=True,
            erode=self.erode,
        )
        input_points = input_points.astype(float).tolist()
        input_labels = input_labels.tolist()
        input_labels = [[x] for x in input_labels]

        # Prepare the image and prompt for the model.
        inputs = self.processor(
            input_image,
            input_points=input_points,
            input_labels=input_labels,
            return_tensors="pt"
        )

        # Remove the batch dimension which the processor adds by default.
        inputs = {k: v.squeeze(0) for k, v in inputs.items()}
        inputs["input_labels"] = inputs["input_labels"].squeeze(1)

        return inputs

    def _getitem_bbox(self, input_image, ground_truth_mask):
        # Get the bounding box prompt.
        bbox = get_input_bbox(ground_truth_mask, perturbation=self.perturbation)

        # Prepare the image and prompt for the model.
        inputs = self.processor(input_image, input_boxes=[[bbox]], return_tensors="pt")
        inputs = {k: v.squeeze(0) for k, v in inputs.items()}  # Remove the batch dimension which the processor adds by default.

        return inputs
```

Putting it all together, we can create a function which creates and returns a PyTorch dataloader given either split of the HuggingFace dataset. Writing functions that return dataloaders rather than just executing cells with the same code is not only good practice for writing flexible and maintainable code but is also necessary if you plan to use HuggingFace Accelerate to run distributed training.

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

이후에는 모델을 불러오고 이미지 및 프롬프트 인코더를 동결한 후 원하는 반복 횟수만큼 훈련하는 것으로 간단히 진행됩니다.

```js
model = SamModel.from_pretrained(f"facebook/sam-vit-{model_size}")
optimizer = AdamW(model.mask_decoder.parameters(), lr=learning_rate, weight_decay=weight_decay)

# 디코더만 훈련합니다.
for name, param in model.named_parameters():
    if name.startswith("vision_encoder") or name.startswith("prompt_encoder"):
        param.requires_grad_(False)
```

아래는 훈련 루프 코드의 기본적인 개요입니다. forward_pass, calculate_loss, evaluate_model, save_model_checkpoint 함수는 간략함을 위해 제외되었지만 GitHub에서 구현을 확인할 수 있습니다. forward pass 코드는 프롬프트 타입에 따라 약간 다를 수 있으며, 손실 계산은 프롬프트 타입에 따라 특별한 경우가 필요합니다. 포인트 프롬프트를 사용할 때는 SAM이 각 입력 포인트에 대해 예측된 마스크를 반환하므로 예측된 마스크를 평균화하거나 SAM의 예측된 IoU 점수에 따라 최상의 예측된 마스크를 선택해야 합니다.

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

train_losses = []
validation_losses = []
epoch_loop = tqdm(total=num_epochs, position=epoch, leave=False)
batch_loop = tqdm(total=len(train_dataloader), position=0, leave=True)

while epoch < num_epochs:
epoch_losses = []

    batch_loop.n = 0  # Loop Reset
    for idx, batch in enumerate(train_dataloader):
        # Forward Pass
        batch = {k: v.to(accelerator.device) for k, v in batch.items()}
        outputs = forward_pass(model, batch, prompt_type)

        # Compute Loss
        ground_truth_masks = batch["ground_truth_mask"].float()
        train_loss = calculate_loss(outputs, ground_truth_masks, prompt_type, loss_fn, multi_mask="best")
        epoch_losses.append(train_loss)

        # Backward Pass & Optimizer Step
        optimizer.zero_grad()
        accelerator.backward(train_loss)
        optimizer.step()
        lr_scheduler.step()

        batch_loop.set_description(f"Train Loss: {train_loss.item():.4f}")
        batch_loop.update(1)

    validation_loss = evaluate_model(model, validation_dataloader, accelerator.device, loss_fn)
    train_losses.append(torch.mean(torch.Tensor(epoch_losses)))
    validation_losses.append(validation_loss)

    if validation_loss < best_loss:
        save_model_checkpoint(
            accelerator,
            best_checkpoint_path,
            model,
            optimizer,
            lr_scheduler,
            epoch,
            train_history,
            validation_loss,
            train_losses,
            validation_losses,
            loss_config,
            model_descriptor=model_descriptor,
        )
        best_loss = validation_loss

    epoch_loop.set_description(f"Best Loss: {best_loss:.4f}")
    epoch_loop.update(1)
    epoch += 1

## 튜닝 결과

엘과강 프로젝트를 위해, 1천 개 이상의 세분화 마스크 데이터셋을 사용하여 GCP 인스턴스를 활용해 12시간 이내에 "sam-vit-base" 모델을 학습시킨 결과 가장 좋았습니다.

기준인 SAM과 비교하여 세밀한 조정으로 성능이 대폭 향상되었고, 중간 마스크의 정확성은 사용 불가에서 매우 정확한 수준으로 개선되었습니다.

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

한 가지 주목해야 할 중요한 사실은 1,000개의 강 이미지로 이루어진 학습 데이터셋이 불완전했다는 점입니다. 분할 레이블은 올바르게 분류된 픽셀의 양이 상당히 다양했습니다. 그 결과, 위에 표시된 메트릭은 225개의 강 이미지로 이루어진 보류 중인 픽셀 완벽 데이터셋을 기반으로 계산되었습니다.

흥미로운 현상은 모델이 불완전한 학습 데이터로부터 일반화하는 방법을 배웠다는 것입니다. 학습 예제에 명백한 분류 오류가 포함된 데이터포인트를 평가할 때, 모델의 예측이 오류를 피하려고 한다는 것을 관찰할 수 있습니다. 학습 샘플을 보여주는 상단 행의 이미지에서는 강 주변까지 완전히 채우지 못하는 마스크를 포함한 반면, 모델 예측을 보여주는 하단 행은 강 경계를 더 정교하게 분할합니다.

**결론**

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

축하해요! 이 정도까지 오셨군요. Meta의 Segment Anything Model을 완벽하게 세밀하게 조정하기 위해 필요한 모든 것을 배우셨어요!

단계별 시각 작업에 대한 구현과는 달리 세세한 조정 작업 흐름은 다를 수 있지만, 이 튜토리얼에서 제시된 지식은 세분화 프로젝트 뿐만 아니라 미래의 딥러닝 프로젝트와 더불어 계속 전이될 거예요.

머신 러닝의 세계를 계속 탐험하고, 호기심을 가지세요! 그리고 늘 코딩하는 즐거움을 누리세요!

# 부록

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

이 예제에서 사용된 데이터셋은 워싱턴 대학교의 GeoSMART 연구실이 작성한 Elwha V1 데이터셋입니다. 이 데이터셋은 고도 조정된 대형 비전 트랜스포머의 지리적 분할 작업에 대한 연구 프로젝트를 위해 사용되었습니다. 이 기사의 튜토리얼은 줄어들고 접근하기 쉬운 형태로 다가오는 논문을 대표합니다. Elwha V1 데이터셋은 SAM 체크포인트를 사용하여 고도 조정된 모델 예측으로 구성되어 있으며, Buscombe 등이 발표한 레이블이 지정된 오토이미지들의 하위 집합을 사용하였습니다. Zenodo에서 발표된 것입니다.
