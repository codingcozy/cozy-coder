---
title: "뉴욕 타임즈 크로스워드를 위한 필기 인식 실험방법"
description: ""
coverImage: "/assets/img/2024-07-10-ExperimentingwithHandwritingRecognitionforTheNewYorkTimesCrossword_0.png"
date: 2024-07-10 01:25
ogImage: 
  url: /assets/img/2024-07-10-ExperimentingwithHandwritingRecognitionforTheNewYorkTimesCrossword_0.png
tag: Tech
originalTitle: "Experimenting with Handwriting Recognition for The New York Times Crossword"
link: "https://medium.com/timesopen/experimenting-with-handwriting-recognition-for-new-york-times-crossword-a78e08fec08f"
---


### 손글씨 인식 실험: 뉴욕 타임스 크로스워드

작성자: 샤픽 콰라이쉬

2023년 MakerWeek에서, 뉴욕 타임스가 진행한 연례 해커톤에서 iOS 및 안드로이드 모바일 엔지니어들이 각 플랫폼의 앱에서 손글씨로 쓸 수 있는 능력을 탐구했습니다.

<div class="content-ad"></div>

안녕하세요! 저는 안드로이드 엔지니어로서 실험에 참여한 경험을 공유해 드리는 것에 흥분하고 있어요. 안드로이드 플랫폼에 온 디바이스 기계 학습을 구현하는 과정에서 느낀 특별한 경험을 뉴욕 타임스 크로스워드에 적용해 보았어요.

참고: 이 탐구는 아직 출시되지 않은 미래의 기능을 위한 것이에요.

## 초기 설정 및 요구 사항

![이미지](/assets/img/2024-07-10-ExperimentingwithHandwritingRecognitionforTheNewYorkTimesCrossword_1.png)

<div class="content-ad"></div>

뉴욕 타임스 십자매 게임은 앱 내에 사용자 정의 소프트웨어 키보드를 탑재하고 있어요. 사용자가 키보드에서 글자를 입력하면 해당 네모칸에 그 글자가 표시돼요.

글씨를 쓸 수 있도록 하는 것이 중요했죠. 먼저 미니 퍼즐과 데일리 퍼즐의 각 네모칸을 가져와 ‘스케치박스’라는 사용자 지정 컴포넌트로 변환했어요. 이 컴포넌트는 사용자가 화면에 필기나 손으로 쓸 때 발생하는 각각의 획을 캡쳐하며, 터치 및 드래그 이벤트를 감지해 그려진 글자의 획을 표시하도록 특별히 설계됐어요.

스케치박스가 캔버스에서 그려진 글자 픽셀들을 캡쳐한 후, 그 데이터를 선택한 기계 학습 알고리즘에 보낼 수 있게 되었어요.

<div class="content-ad"></div>

![Experimenting with Handwriting Recognition for The New York Times Crossword](/assets/img/2024-07-10-ExperimentingwithHandwritingRecognitionforTheNewYorkTimesCrossword_2.png)

우리가 손글씨 감지로 넘어가기 전에, 미묘하지만 중요한 점을 알아야 합니다.

사용자가 스케치박스에 쓸 때, 그들은 일반적으로 손가락이나 스타일러스를 캔버스에서 들어 올리는데, 특히 'K', 'A', 'H'와 같은 글자를 완성할 때 더 그렇습니다. 이는 사용자가 언제 쓰기를 완료했는지, 각 획 사이에서 결정해야 했다는 것을 의미합니다. 예를 들어, 사용자가 'K'의 수직선을 입력한 경우, 쓰기 도구를 캔버스에서 들어 올릴 때 이게 정확히 어떤 글자인지 언제까지 기다려야 하는지 결정해야 합니다. 예를 들어, 사용자가 'K'의 수직선을 입력한 경우, 쓰기 도구를 캔버스에서 들어 올릴 때 이것은 'I'로 해석될 수도 있습니다.

그렇다면 우리는 얼마나 오래 획 사이에 기다려야 할까요?

<div class="content-ad"></div>

우리의 초기 구현에서는 뮤텍스와 유사한 입력 잠금 시스템의 개념을 도입했습니다. 각 획 간에는 특정 조건에 따라 500에서 1000 밀리초 사이의 값을 실험했습니다. 스타일러스를 잠금 해제하기 전에 너무 오랫동안 기다리고 싶지 않았는데, 그렇지 않으면 사용자 입력 경험이 저하되고 끊김이 생길 수 있었기 때문입니다.

글쓰기 메커니즘을 디자인하는 과정에서 고려해야 할 많은 복잡성 중 하나이며, 앞으로 개선 사항으로 열릴 것임을 염두에 두어야 하는 사항입니다.

## 데이터 준비, 조건화 및 정규화

이미지를 텍스트로 변환하기 전에, 다양한 기기로부터의 글자 입력을 고려해야 했습니다. 이들은 화면 크기와 해상도가 다른 다양한 기기로부터 입력됐습니다.

<div class="content-ad"></div>

필수 전처리 단계 중 하나는 알고리즘이 정확한 학습을 위해 필요한 데이터를 가장 간단한 형태로 가공하는 것입니다. 이미지 데이터의 경우, 비필수적인 잡음과 "사소한 기하학"을 제거해야 합니다. 우리는 글자 데이터를 축소 및 이진화하고, 그리고 128x128 크기의 원시 입력 글자를 훨씬 작고 효율적이며 간소화된 28x28 이미지로 변환했습니다.

![image](/assets/img/2024-07-10-ExperimentingwithHandwritingRecognitionforTheNewYorkTimesCrossword_3.png)

이렇게 처리한 후에 우리는 마침내 래스터화된 캔버스 이미지 데이터를 우리의 크로스워드 앱이 이해하는 실제 문자로 어떻게 번역하는지에 대해 논의할 수 있었습니다.

필기 인식은 광학 문자 인식(OCR) 내의 클래식한 기계 학습 과제입니다. 특히 1998년 Yann LeCun 박사의 LeNet-5 아키텍처를 통해 상당한 발전을 이루었는데, 이는 수정된 미국 국립표준기술연구소(MNIST) 데이터셋에서 숫자 인식을 크게 향상시켰습니다. MNIST 데이터셋에는 1부터 9까지의 수많은 변종이 포함되어 있으며, 이는 숫자 인식의 표준 데이터베이스입니다.

<div class="content-ad"></div>

마치 저희가 구축하려고 했던 시스템은 이런 고수준 아키텍처 다이어그램처럼 보입니다. 

![Diagram 1](/assets/img/2024-07-10-ExperimentingwithHandwritingRecognitionforTheNewYorkTimesCrossword_4.png)

저희가 선택한 머신 러닝 알고리즘은 문자 데이터를 인식 클러스터로 가장 잘 분리하는 것이 목표였습니다. 아래의 이상적인 상황처럼요. 이렇게 하면 예를 들어 사용자가 'A' 대신 'C'를 입력하려고 하는지 쉽게 판별할 수 있습니다. 저희는 Deep Convolutional Neural Network 아키텍처의 사용으로 최종적으로 결정하기 전에 여러 다른 옵션들을 탐색해보았습니다. 자세한 내용은 생략하겠습니다. 그러나 해당 아키텍처는 잘 해내는 것으로 입증되었습니다.

<div class="content-ad"></div>

## 깊은 합성곱 신경망 구축하기

깊은 CNN(합성곱 신경망)은 현대 이미지 기반 기계 학습 시스템의 필수 요소입니다. 이미지 데이터의 섹션을 조사하는 특수한 종류의 신경망으로, 학습 메커니즘을 사용하여 중요한 특징을 지능적으로 찾아내어 이미지를 식별하고 분류하는 데 도움이 되는 핵심적인 기능을 찾습니다.

![ExperimentingwithHandwritingRecognitionforTheNewYorkTimesCrossword_7](/assets/img/2024-07-10-ExperimentingwithHandwritingRecognitionforTheNewYorkTimesCrossword_7.png)

<div class="content-ad"></div>

위의 예시에서 신경망은 새의 입력 이미지를 받아들이고, 새에 해당하는 다양한 질적 요소에 대한 구조를 감지합니다. 이미지의 일부분을 감지할 수 있습니다. 즉, 깃털, 눈, 부리의 가장자리, 내부 부리 기하학 등과 같은 구조물을 감지할 수 있습니다. 이를 처음부터 알고리즘을 설계하는 데 어려움을 겪을 수 있는 사람이 메꿀 수 있는 특징들을 감지할 수 있습니다.

가장 중요한 특징을 사용하여, 새를 보고 어떤 종류의 새인지 판단할 수 있습니다. 네트워크가 적절하게 훈련되면, 새가 어느 방향으로 날고 있는지나 바라보고 있는지 등의 것들도 결정할 수 있습니다. 단서 앱에 입력된 사용자의 문자에도 동일한 원리를 적용합니다.

기본 CNN은 다음 기본 레이어들의 조합입니다. 이러한 구조를 적절한 매개변수와 혼합함으로써 우리가 감지하지 못하는 이미지 요소가 거의 없습니다:

- 합성곱 레이어는 모델이 특징을 자동으로 추출할 수 있게 합니다
- 맥스 풀링 레이어는 Conv 레이어에서 얻은 특징을 좁히는 역할을 합니다
- ReLU 레이어는 비선형성을 도입하고 복잡한 패턴 식별이 가능하게 함
- 드롭아웃 레이어는 네트워크가 훈련 데이터에 과적합되는 것을 줄입니다

<div class="content-ad"></div>

**TensorFlow Lite 및 모델 가져오기**

모델을 구축하는 것 외에도, 장치로 모델을 전달하는 방법을 찾아야 했습니다. 저희는 Python으로 컴파일된 머신 러닝 모델을 안드로이드 또는 iOS 장치에 설치하는데 사용되는 모바일 프레임워크인 TensorFlow Lite를 선택했습니다.

![image](/assets/img/2024-07-10-ExperimentingwithHandwritingRecognitionforTheNewYorkTimesCrossword_9.png)

<div class="content-ad"></div>

![Image](/assets/img/2024-07-10-ExperimentingwithHandwritingRecognitionforTheNewYorkTimesCrossword_10.png)

한 번 우리가 모델에 만족했을 때, 우리는 .tflite 파일로 컴파일하고 응용 프로그램에 넣어서 십자말풍경에서 오는 글씨 쓰기 이벤트를 듣기 위한 샌트를 만들었습니다. 우리는 최종 결과에 만족할 때까지 여러 가지 모델과 다양한 구성을 반복하여 향상시켰고, 그것은 모바일 응용 프로그램에 적합한 100K 정도의 훈련 파일로 변했습니다.

## 숫자 인식

전체 글자 인식 문제에 접근하기 전에, 우리는 간단하게 시작하고 근본적인 문제인 숫자 인식 문제에 대처하기로 결정했습니다. 아래의 핵심 코드 섹션에서 우리가 구현한 기본 CNN 설정을 제공합니다:

<div class="content-ad"></div>

하이 장소! 

오늘은 첫 번째 디지트 기반 CNN 모델의 실패에 대해 이야기하려고 해요. 아쉽게도, MNIST에서 얻은 다양한 훈련 데이터에도 불구하고, 우리의 인식 결과는 좋지 않았어요. 😔

<div class="content-ad"></div>

우리는 훈련 데이터가 "너무 완벽하다"는 것을 확인했습니다. 모든 숫자가 서로의 작은 변형들이었고, 그 중 대부분이 상자의 중앙에 있었습니다.

이것은 사람들이 세로로 데이터를 입력하는 방식이 아닙니다. 사람들은 서로 다른 필기 스타일을 가지고 있으며, 문자를 비스듬하게 눕히거나 상자 중앙이 아닌 곳에 배치하는 방법이 다릅니다.

이 문제를 해결하기 위해, 우리는 데이터 증강이라 불리는 잘 알려진 기계 학습 기술을 사용해야 했습니다.

데이터 증강은 훈련 데이터의 비중심화 및 왜곡된 버전을 자동으로 생성하여 수동 조정 및 왜곡이 필요 없게 합니다. 이를 통해 초기 데이터 세트의 여러 변형, 특히 문자의 크게 중앙이 아닌 버전을 만들 수 있습니다.

<div class="content-ad"></div>

이미지 tag를 Markdown 형식으로 변경해 드리겠습니다.


![](https://www.example.com/assets/img/2024-07-10-ExperimentingwithHandwritingRecognitionforTheNewYorkTimesCrossword_13.png)


데이터 증가 기술을 적용하여, 오프-센터 미세 이동, 회전 및 스케일링과 같이 대폭 변형이라고도하는 이러한 방법으로, 저희의 데이터셋을 수천 개에서 100만 개 이상의 샘플로 확장했어요.

## 숫자에 대한 데이터 증가된 숫자 모델의 성공

![](https://www.example.com/assets/img/2024-07-10-ExperimentingwithHandwritingRecognitionforTheNewYorkTimesCrossword_14.png)


<div class="content-ad"></div>

안녕하세요! 오늘은 풀로 Letter Recognition으로 넘어갈 차례에요. 이미 숫자 인식 기술이 크게 향상되어진 것을 보실 수 있어요. 우리 작업에서의 중요한 발전단계랍니다!

## 풀로 Letter Recognition으로 나아가기

![Handwriting Recognition for The New York Times Crossword](/assets/img/2024-07-10-ExperimentingwithHandwritingRecognitionforTheNewYorkTimesCrossword_15.png)

이제 숫자도 해결했으니, 다음 단계는 알파벳을 해결하는 거예요. 소문자, 대문자, 그리고 그들의 모든 변형들까지 포함이에요. 오직 열 개의 숫자가 아니라, 이제는 26개의 소문자, 26개의 대문자와 열 개의 숫자까지 처리하는데요. 총 62개의 문자가 필요합니다.

<div class="content-ad"></div>

우리는 모델을 더 잘 학습시키기 위해, 음절과 숫자뿐만 아니라 구두점까지 포함한 MNIST 세트의 확장 버전인 EMNIST 데이터셋(데이터 출처: http://arxiv.org/abs/1702.05373)을 사용할 수 있습니다.

그러나 향상된 데이터셋을 사용하더라도, 숫자 특화 모델만으로는 우리의 요구를 충족시키기에는 부족합니다. 이는 직관적으로 놀랍지 않은 일입니다. 왜냐하면 숫자 인식 모델은 강력할지 몰라도, 문자를 살펴보면 도입된 문자 구조의 크게 다양해진 변화에 대해 충분히 "지능적"이지 않습니다.

## 하이퍼 파라미터 최적화를 통한 모델의 강화

우리의 모델의 성능을 향상시키기 위해, 네트워크에 훨씬 더 깊이를 추가했습니다. 또한, 무작위 하위 데이터 집합을 사용하여 다양한 훈련을 진행하기 위해 Stratified K-Fold 교차 검증을 사용했습니다.

<div class="content-ad"></div>

우리가 층을 최적으로 설계했는지 확인하기 위해 무작위 매개변수 검색과 향상된 통계 검정을 적용했습니다. 이를 통해 모델의 최적 하이퍼파라미터를 찾을 수 있었습니다. 이전의 매개변수를 추측하는 전략과 비교했을 때 이는 향상된 것으로 입증되었습니다.

테스트 단계에서 보강된 EMNIST 데이터셋에서 약 91%의 평균 검증 정확도를 달성했습니다. 이는 우리 모델이 잘 작동할 것이라는 믿음을 가지게 했습니다.

마침내: 성공!

![ExperimentingwithHandwritingRecognitionforTheNewYorkTimesCrossword](/assets/img/2024-07-10-ExperimentingwithHandwritingRecognitionforTheNewYorkTimesCrossword_16.png)

<div class="content-ad"></div>

긴 여정을 거쳐 ML 모델 빌딩의 풍경을 탐험한 끝에 우리는 작동하는 크로스워드 모델에 도달했고, 이는 흥미로웠습니다. 우리는 뛌륭한 결과를 얻었지만, 부분적 글자 처리나 간격이 불규칙한 글자와 같은 완전한 크로스워드 경험을 위해 더 많은 작업이 필요합니다.

## 결론

안드로이드 크로스워드 앱에 필기 인식을 구현하는 것은 실험적인 문맥에서 흥미진진한 모험이었습니다. 필기 인식 외에도 "스크립블-지우기" 감지와 같은 대화형 기능, 앱 내 자가 교육 메커니즘 가능성, 그리고 게임 앱에서의 온-디바이스 ML이 열어주는 다양한 가능성이 있습니다.

타임스에서 엔지니어로 일하는 것이 얼마나 독특하고 가치 있는지의 핵심 부분은 기존 제품에 새로운 기술과 확대 전략 실험을 할 수 있는 기회입니다. 우리는 언젠가 이를 통해 현재 사용자들에게 게임 경험을 증폭시켜 주고 새 구독자를 유치하는 기능으로 만들기를 희망합니다.

<div class="content-ad"></div>

Shafik Quoraishee님은 뉴욕타임즈에서 게임 팀의 시니어 안드로이드 엔지니어로 일하고 계십니다. 그는 열정적인 머신 러닝/AI 애호가입니다. 업무 외에는 기타 연주, 글쓰기, 그리고 독특한 실험을 하는 것을 즐기고 계십니다.