---
title: "리뷰  OpenAI Whisper 대규모 약식 감독을 통한 강력한 음성 인식 기술"
description: ""
coverImage: "/assets/img/2024-07-10-ReviewOpenAIWhisperRobustSpeechRecognitionviaLarge-ScaleWeakSupervision_0.png"
date: 2024-07-10 00:21
ogImage:
  url: /assets/img/2024-07-10-ReviewOpenAIWhisperRobustSpeechRecognitionviaLarge-ScaleWeakSupervision_0.png
tag: Tech
originalTitle: "Review — OpenAI Whisper: Robust Speech Recognition via Large-Scale Weak Supervision"
link: "https://medium.com/@sh-tsang/review-openai-whisper-robust-speech-recognition-via-large-scale-weak-supervision-f7b9bb646356"
isUpdated: true
---

## OpenAI Whisper for Speech-to-Text (STT)

![OpenAI Whisper](/assets/img/2024-07-10-ReviewOpenAIWhisperRobustSpeechRecognitionviaLarge-ScaleWeakSupervision_0.png)

Hey there! OpenAI Whisper for Speech-to-Text (STT) has achieved some amazing results. By leveraging 680,000 hours of multilingual and multitask supervision, the models can generalize effectively and even compete with fully supervised models in zero-shot transfer settings without additional fine-tuning.

Curious about the different versions of Whisper? Let's explore them further.

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

**위스퍼(Whisper)**의 여러 버전이 있습니다: 2022년 9월 (원본 시리즈), 2022년 12월 (large-v2) 및 2023년 11월 (large-v3).

**Whisper-v2**

- large-v2 모델은 2.5배 더 많은 epoch에 대해 훈련되었으며, SpecAugment, 확률적 깊이 및 BPE 드롭아웃을 사용하여 정칙화되었습니다. 훈련 절차 이외에도 모델 아키텍처와 크기는 원래 대형 모델과 동일하며, 이제 large-v1로 이름이 변경되었습니다.

**Whisper-v3**

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

- 큰-v3의 Whisper-v3는 이전의 큰 모델과 동일한 구조를 가지고 있지만 다음과 같은 작은 차이가 있습니다:

- 입력은 80대신 128개의 Mel 주파수 바이닐을 사용합니다.
- 칸토니스어용 새로운 언어 토큰;

- 큰-v3 모델은 약하게 레이블이 지정된 오디오 1백만 시간과 큰-v2를 사용하여 수집된 의사 레이블이 지정된 오디오 4백만 시간에서 훈련되었습니다. 이 모델은 이 혼합 데이터세트에서 2.0 에포크 동안 교육되었습니다.

# 개요

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

- 속삭임 (원본 속삭임-v1 논문에서)
- 결과

# 1. 속삭임 (원본 속삭임-v1 논문에서)

![이미지](/assets/img/2024-07-10-ReviewOpenAIWhisperRobustSpeechRecognitionviaLarge-ScaleWeakSupervision_1.png)

## 1.1. 데이터셋

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

- 여기에는 여전히 많은 다른 단계들이 있습니다.

## 1.2. 전처리

- 특징 정규화를 위해 입력은 사전 훈련 데이터셋 전체에 걸쳐 대략적으로 제로 평균을 갖도록 전역적으로 -1과 1 사이로 스케일 조정됩니다.

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

## 1.3. 모델

- 인코더-디코더 트랜스포머가 사용됩니다.
- 인코더는 3의 필터 폭을 갖는 두 개의 합성곱 레이어와 두 번째 합성곱 레이어가 스트라이드 2를 갖는 GELU 활성화로 이 입력 표현을 처리합니다.
- 그런 다음, 정현 파 위치 임베딩이 합성곱 레이어의 출력에 추가됩니다.
- 트랜스포머는 사전 활성화 잔차 블록을 사용하며 최종 레이어 정규화가 인코더 출력에 적용됩니다.
- 디코더는 배운 위치 임베딩과 연결된 입력-출력 토큰 표현을 사용합니다.
- GPT-2에서 사용된 동일한 바이트 수준의 BPE 텍스트 토크나이저가 사용됩니다.

## 1.4. 멀티태스크 형식

- 완전한 기능을 갖춘 음성 인식 시스템은 음성 활동 감지, 화자 분리, 그리고 역 텍스트 정규화와 같은 많은 추가 구성 요소를 포함할 수 있습니다.
- 단일 모델이 전체 음성 처리 파이프라인을 수행하는 것이 선호됩니다.
- 예측의 시작은 `|startoftranscript|` 토큰으로 시작됩니다.
- 먼저, 말하고 있는 언어가 예측되며, 훈련 세트의 각 언어에 대해 고유한 토큰으로 표현됩니다(총 99개). 이러한 언어 타겟은 앞서 설명한 VoxLingua107 모델에서 가져옵니다.
- 오디오 세그먼트에 음성이 없는 경우, 모델은 `|nospeech|` 토큰을 예측하도록 훈련됩니다.
- 다음 토큰은 작업(전사 또는 번역)을 `|transcribe|` 또는 `|translate|` 토큰으로 지정합니다.
- 이후, Whisper는 `|notimestamps|`를 포함하여 타임스탬프를 예측할지 여부를 지정합니다.
- 타임스탬프 예측에는 현재 오디오 세그먼트에 상대적인 시간이 예측되며, 모든 시간이 가장 가까운 20밀리초로 양자화됩니다.
- 이러한 추가 토큰들은 각각의 어휘에 추가됩니다.
- 이 예측들은 캡션 토큰들과 교차되어 있습니다: 시작 시간 토큰은 각 캡션 텍스트의 앞에 예측되고, 종료 시간 토큰은 그 뒤에 예측됩니다.
- 마지막으로, `|endoftranscript|` 토큰이 추가됩니다.
- 위 그림은 예시를 보여줍니다.

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

## 1.5. 훈련 세부 정보

![Training Image](/assets/img/2024-07-10-ReviewOpenAIWhisperRobustSpeechRecognitionviaLarge-ScaleWeakSupervision_2.png)

- FP16을 사용하여 동적 손실 스케일링과 활성화 체크포인팅을 통해 가속기 간 데이터 병렬 처리를 사용합니다.
- 256 세그먼트의 배치 크기를 사용했으며, 모델은 데이터 세트에 대해 2²⁰ 업데이트 동안 훈련되었습니다. 이는 데이터 세트를 2회에서 3회 지나는 범위입니다.
- 어떠한 증강 및 정규화도 사용되지 않았으며, 다양한 데이터에 순전히 의존합니다.

# 2. 결과

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

**2.1. 제로샷 평가**

![이미지1](/assets/img/2024-07-10-ReviewOpenAIWhisperRobustSpeechRecognitionviaLarge-ScaleWeakSupervision_3.png)

![이미지2](/assets/img/2024-07-10-ReviewOpenAIWhisperRobustSpeechRecognitionviaLarge-ScaleWeakSupervision_4.png)

- 제로샷 Whisper 모델은 LibriSpeech clean-test WER 지수가 2.5로, 현대의 감독된 기준선이나 2019년 중반의 최신 기술과 비슷한 수준의 성능을 보입니다.

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

- 모델의 zero-shot 및 out-of-distribution 평가는 특히 인간의 성능과 비교할 때 중요합니다. 기계 학습 시스템의 성능을 과대포장하지 않기 위해 오해를 일으킬 수 있는 비교를 피하기 위함이죠.

## 2.2. 멀티링귀 음성 인식

![image](/assets/img/2024-07-10-ReviewOpenAIWhisperRobustSpeechRecognitionviaLarge-ScaleWeakSupervision_5.png)

![image](/assets/img/2024-07-10-ReviewOpenAIWhisperRobustSpeechRecognitionviaLarge-ScaleWeakSupervision_6.png)

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

## 2.3. 음성 번역

![이미지](/assets/img/2024-07-10-ReviewOpenAIWhisperRobustSpeechRecognitionviaLarge-ScaleWeakSupervision_7.png)

## 2.4. 언어 식별

![이미지](/assets/img/2024-07-10-ReviewOpenAIWhisperRobustSpeechRecognitionviaLarge-ScaleWeakSupervision_8.png)

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

## 2.5. 호함성 추가 소음

![image](/assets/img/2024-07-10-ReviewOpenAIWhisperRobustSpeechRecognitionviaLarge-ScaleWeakSupervision_9.png)

- 모든 모델이 잡음이 더 강해지면 빠르게 성능이 떨어지게 됩니다.

## 2.6. 장문 전사

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

![Image](/assets/img/2024-07-10-ReviewOpenAIWhisperRobustSpeechRecognitionviaLarge-ScaleWeakSupervision_10.png)

- 오디오의 긴 부분을 연이어 30초씩 전사하고, 모델이 예측한 타임스탬프에 따라 창을 이동시키는 전략이 개발되었습니다.
- 오랜 오디오를 신뢰성 있게 전사하기 위해 모델 예측의 반복성과 로그 확률에 기반한 빔 서치(beam search) 및 온도 스케줄링(temperature scheduling)이 필수적입니다.

- 그럼에도 불구하고, 상업용 ASR 시스템 중 일부가 공개 데이터셋 중 일부에서 훈련되었을 가능성이 있으므로, 이러한 결과가 시스템의 상대적인 강건성을 정확하게 반영하지 못할 수 있다는 점을 주의해야 합니다.

## 2.7. 사람의 성능과 비교

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

**2.8. Model Scaling**

![Model Scaling](/assets/img/2024-07-10-ReviewOpenAIWhisperRobustSpeechRecognitionviaLarge-ScaleWeakSupervision_11.png)

**2.9. Dataset Scaling**

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

**마법사의 비밀의 힘을 이용한 강력한 대규모 소음 지도를 통한 음성 인식**

- 라벨이 달린 오디오 시간이 68만 시간에 이르는 Whisper 데이터 세트는 지도 학습 음성 인식을 위해 만들어진 가장 큰 데이터 세트 중 하나입니다.
- 전체 데이터 세트 크기의 0.5%, 1%, 2%, 4%, 8%에 대해 훈련이 이루어졌습니다.

- (다른 실험들도 여전히 진행 중이니, 관심이 있다면 논문을 직접 읽어보세요.)
