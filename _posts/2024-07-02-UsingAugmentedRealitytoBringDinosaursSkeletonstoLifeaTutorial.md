---
title: "증강 현실로 공룡 골격을 생생하게 재현하는 방법  튜토리얼"
description: ""
coverImage: "/assets/img/2024-07-02-UsingAugmentedRealitytoBringDinosaursSkeletonstoLifeaTutorial_0.png"
date: 2024-07-02 22:49
ogImage: 
  url: /assets/img/2024-07-02-UsingAugmentedRealitytoBringDinosaursSkeletonstoLifeaTutorial_0.png
tag: Tech
originalTitle: "Using Augmented Reality to Bring Dinosaurs Skeletons to Life — a Tutorial"
link: "https://medium.com/better-programming/using-augmented-reality-to-bring-dinosaurs-skeletons-to-life-a-tutorial-db57c85e51d3"
isUpdated: true
---





![AR-powered museum](/assets/img/2024-07-02-UsingAugmentedRealitytoBringDinosaursSkeletonstoLifeaTutorial_0.png)

안녕하세요. 저는 주간에 산업용 증강 현실 기술을 다루는 프랑스 엔지니어인 플로랑 제르맹입니다. 밤에는 공룡의 뼈구조를 출력하는 취미를 가지고 있어요.

제가 최근에 고민 중인 아이디어는 주간 직업과 3D 프린팅 취미를 결합하여 딸들과 제가 즐길 수 있는 작은 AR(증강 현실) 박물관을 만드는 것입니다.

기본 아이디어는 매우 간단해요: 골격을 3D 프린트하여 조립하고 도색; 컴퓨터 비전 기술을 사용하여 공간에서 인식하고 추적; 뼈로 된 동물의 3D 디자인을 겹쳐 넣는 것입니다. 아래 이미지를 통해 보실 수 있어요:

<div class="content-ad"></div>

이 간단한 사용 설명서에서는이 프로젝트를 누구나 재현할 수 있도록 과정을 자세히 설명하고 있습니다.

# 골격 구축

이 단계는 매우 간단하지만 현실적으로 그리고 3D 프린트임을 숨기려면 가장 시간이 많이 소요되는 단계일 것입니다.

디자인을 선택하세요. 선택한 디자인을 원하는 프린터로 출력하세요. 레진을 사용하는 경우 인쇄물을 완전히 경화시키고 개인 보호구를 착용하세요. 골격을 도색하고 조립하세요(그 순서로 할 필요는 없습니다).

<div class="content-ad"></div>

![2024-07-02-UsingAugmentedRealitytoBringDinosaursSkeletonstoLifeaTutorial_1.png](/assets/img/2024-07-02-UsingAugmentedRealitytoBringDinosaursSkeletonstoLifeaTutorial_1.png)

![2024-07-02-UsingAugmentedRealitytoBringDinosaursSkeletonstoLifeaTutorial_2.png)

조립이 견고하고 안전하게 확인하세요: 3D 프린팅된 골격은 깨지기 쉽기 때문에 작은 낙하도 파손된 조각을 야기할 수 있습니다. 이는 쉽게 수리할 수 있지만, 재조립은 당신을 다시 원점으로 되돌릴 수 있습니다. 나중에도 프로세스 중간에 원본 조립과 약간의 차이가 생겨 계속 조립해야 하는 상황이 되어 버릴 수 있습니다 (이런 일이 있었었거든요…).

현재 이 정도로 갖추어져 있어야 합니다:

<div class="content-ad"></div>


![Image](/assets/img/2024-07-02-UsingAugmentedRealitytoBringDinosaursSkeletonstoLifeaTutorial_3.png)

If you don’t feel like designing your own skeleton, head over to Cults3D or other 3D print databases. I was personally more than happy with designs from Inhuman_Species and Think3DPrint. It’s also possible to source 3D-printed skeletons and skulls directly, as I did with the Microraptor Gui skull you’ll see below. The rest of the body still needs to be printed, but I uploaded the design for free.

## 3D scan your print

The next step requires you to 3D scan your 3D printed skeleton because the computer vision tool we’ll use to recognize, track, and augment the dinosaur needs a 3D model as input.


<div class="content-ad"></div>

"그러면 그냥 3D 프린팅 STL 파일을 사용하면 안 되냐고 묻는 소리가 들립니다. 그렇죠. 하지만, 인쇄된 조립품과 원본 STL 파일이 모든 면에서 완벽하게 1 대 1이어야만 가능합니다. 제 여러 시도 중 어디에서도 그런 경우를 발견하지 못했어요. 팔이 조금 더 들어간 각도로, 꼬리가 정확히 같은 각도에 위치해 있지 않았고, 두상이 더 앞으로 기울어져 있었죠... 그렇게 보이지 않을 수 있지만, 다음 단계에서 AR 오버레이가 물리적 프린트와 완벽하게 일치할 때 중요한 것입니다.

![Image](/assets/img/2024-07-02-UsingAugmentedRealitytoBringDinosaursSkeletonstoLifeaTutorial_4.png)

3D 스캐닝이 필요할 때 여러 옵션이 있어요. 좋은 소식은 상세한 스캔이 필수적이지 않다는 것입니다. 그냥 치수와 세부사항에서 충분히 정확해야 합니다. 스캔하려는 스켈레톤/물체의 크기와 사용할 스캐닝 기술에 따라 달라지죠. 일반적인 규칙은 3D 스캔이 가장 거칠어도 뼈가 빠지지 않는 한 괜찮다는 점입니다.

(예를 들어, 1:1 삼지창곤 영겁이)는 아마도 iPhone LIDAR로 충분히 정확하게 스캔할 수 있습니다. 그런 면에서 LIDAR 스캔에서 3D 모델을 출력할 수 있는 앱 (Polycam, Metascan 등)이 작동할 수 있어요. 그러나 충분히 큰 스켈레톤을 구한다거나 인쇄하는 것이 이 경우의 진정한 도전입니다…"

<div class="content-ad"></div>

작은 골과 이빨이 있는 것들, 길이가 50cm에서 1.5m(2-5 피트)인 경우에는 충분한 세부 사항을 스캔하기 위해 최소한의 포토그래메트리를 수행해야 할 것입니다. 이를 위한 내 기본 애플리케이션은 Polycam과 그의 포토 모드입니다. 무료는 아니지만 정기적으로 물건을 스캔해야 하는 경우 가격을 충분히 가치 있습니다. 사용하기 쉽고 사진을 올바르게 촬영하면 대부분의 경우 충분한 품질의 출력물을 제공합니다. Metascan이나 Qlone과 같은 많은 다른 애플리케이션들도 이제 유사한 포토그래메트리 파이프라인을 앱으로 제공합니다.

세부 사항(손가락 부족이나 꼬리 끝 부분)에 대해 만족스럽지 않은 경우 다음 옵션은 Epic Games의 Reality Capture입니다. 이것은 좀 더 수동적인 작업이지만 재구성 품질에 대한 완전한 제어를 얻으며 따라서 더 많은 세부 사항을 얻을 수 있습니다. 그러나 이를 하나의 워크플로에 적어도 하나의 추가 단계로 추가한다는 점을 염두에 두어야 합니다. 왜냐하면 여러 백만 다각형 모델의 출력물을 단순화해야 할 것입니다. Reality Capture 대안으로는 Agisoft의 Metashape나 오픈 소스 Meshroom이 있습니다.

다음은 아주 작은 Microraptor Gui와 큰 1.40m Velociraptor Mongoliensis의 충분히 좋은 스캔 예시입니다:

![이미지](/assets/img/2024-07-02-UsingAugmentedRealitytoBringDinosaursSkeletonstoLifeaTutorial_5.png)

<div class="content-ad"></div>

이 3D 스캔된 모델은 3가지 목적으로 사용될 것입니다:

- 컴퓨터 비전 알고리즘에 입력으로 사용될 것으로, 이를 카메라 피드에서 인식하고 우주에서 위치를 추적할 수 있도록 훈련시킬 것입니다.
- 나중에 좋은 애니메이션 셰이더 및/또는 가려짐 효과를 위해 사용될 오버레이입니다.
- 완성된 공룡의 조각/자세를 안내하는 것입니다.

다음 단계에 있어서 이 마지막 포인트는 중요합니다. 왜냐하면 우리는 이 스켈레톤을 보완할 다른 3D 모델이 필요하기 때문입니다: 완성된 공룡.

# 스켈레톤을 완성하기

<div class="content-ad"></div>

당신의 뼈대 위에 고기, 깃털, 그리고 각질을 씌우는 것에 관한 것입니다. 아마도 몇 가지 조각 작업이 필요할 것입니다. 만약 당신이 인쇄된 스켈레톤과 일치하는 3D 모델에 접근할 수 있는 경우를 제외하고요. 심지어 이 경우에도, 그들은 아마도 인쇄본과 정확히 동일한 자세로 일치하지 않을 것이며, 크기나 일반 해부학적 오차에 문제가 있을 수 있습니다. 그래서 완벽하게 인쇄본과 일치하도록 모델을 재조정하고 포즈를 디자인하기 위해 어쩔 수 없이 3D 작업을 해야한다고 가정해주세요. 그래서 이 섹션의 나머지 부분에서는 처음부터 시작한다고 가정합니다.

첫 번째 옵션은 "조각" 트랙입니다: 당신은 스컬팅 소프트웨어(Zbrush, Blender, Ipad의 Nomad Sculpt 등)에 3D 스캔본을 가져와서 직접적으로 모델을 설계합니다. 이는 애니메이션에 관심이 없고 조각, 자세의 세부사항에 초점을 맞추고 추가 단계를 최소화하고 싶다면 흥미로운 접근 방식입니다. 이는 보통 시각적으로 가장 좋은 결과를 제공합니다. 왜냐하면 당신은 실제로 주변의 피부 움푹 젖은 팔꿈치 관절 주위의 피부 행동이나, 예를 들어 입을 벌린 쪽 주위의 주름과 같은 자세에 관련된 세부사항에 집중할 수 있기 때문입니다. 다만 상기해두세요, 당신은 폰이나 홀로렌즈가 런타임에서 10백만 이상의 폴리곤 모델을 처리하지 못할 수 있기 때문에, 모든 이러한 세부사항을 저 폴리모델에 넣어야 할 필요가 있습니다.

![이미지](/assets/img/2024-07-02-UsingAugmentedRealitytoBringDinosaursSkeletonstoLifeaTutorial_6.png)

![이미지](/assets/img/2024-07-02-UsingAugmentedRealitytoBringDinosaursSkeletonstoLifeaTutorial_7.png)

<div class="content-ad"></div>

두 번째 옵션은 "리깅" 트랙입니다. 이를 위해 공룡을 "T" 자세로 조각내고, 리깅하고, 3D 스캔과 일치하도록 자세를 맞추어야 합니다. 이 방법은 추가 단계가 필요하며, 스켈레톤을 리깅하고 정점 가중치를 그리는 방법에 대한 지식이 필요합니다. 하지만 이렇게 함으로써 모델을 애니메이션화할 수 있고, 따라서 스켈레톤에서 "뛰어오르" 수 있어 경험에 추가적인 차원을 더할 수 있습니다. 또한 다른 실시간 응용 프로그램에서도 이 AR 경험을 넘어 모델을 재사용할 수 있다는 것을 의미합니다.

![이미지](/assets/img/2024-07-02-UsingAugmentedRealitytoBringDinosaursSkeletonstoLifeaTutorial_8.png)

어떤 트랙을 선택하더라도, 당신은 공룡의 3D 질감이 적용된 모델을 얻게 되며, 3D 프린팅된 스캔과 일치시킬 수 있습니다. 이제 이러한 모든 요소를 AR 경험으로 결합할 때입니다.

# AR 경험 만들기

<div class="content-ad"></div>

이 부분이 아마도 처음 링크를 클릭한 이유라고 생각되어 더 많은 내용을 자세히 설명하겠습니다. 하지만 Unity에 대해 어느 정도의 기본 지식이 있고, 패키지를 추가하는 방법, 에셋 스토어 자산을 가져오는 방법, 모바일 장치에 앱을 배포하는 방법을 알고 있다고 가정합니다.

면책 조항으로, 저는 AR SDK Vuforia Engine을 소유한 PTC Inc에서 일하고 있습니다. 이 SDK를 사용하여 AR 경험을 구축하고, 매우 익숙하며 3D 모델에 대한 최고 수준의 추적을 제공합니다. 이를 위해 다른 SDK를 사용하여 이를 구현할 수도 있지만, 사용하신다면 귀하의 경험에 대해 매우 흥미롭게 생각할 것입니다!

소프트웨어 측면에서 필요한 것은 다음과 같습니다:

- Unity 2021.3.11f1
- 최신 Vuforia Engine SDK
- Vuforia Model Target Generator
- 기본 Vuforia Engine 라이센스
- Vuforia VFX 라이브러리 자산
- 고급 Dissolve 자산 (선택 사항)

<div class="content-ad"></div>

## 프로젝트 설정하기

첫 번째 단계는 Vuforia Engine을 위한 프로젝트를 설정하는 것입니다. 이를 위해 Hub에서 새로운 3D 프로젝트를 생성해야 합니다. 지금은 기본 설정을 유지하고 URP나 AR 템플릿을 사용하지 않는 3D Core 템플릿을 선택해 보겠습니다.

![이미지](/assets/img/2024-07-02-UsingAugmentedRealitytoBringDinosaursSkeletonstoLifeaTutorial_9.png)

프로젝트가 로드되면 dev.vuforia.com에서 다운로드한 Vuforia Engine Unity Package를 넣어주세요.

<div class="content-ad"></div>

<img src="/assets/img/2024-07-02-UsingAugmentedRealitytoBringDinosaursSkeletonstoLifeaTutorial_10.png" />

모든 것을 가져오고 Unity가 컴파일하도록하십시오. 이제 계층에서 마우스 오른쪽 버튼을 클릭하면 Vuforia Engine 맥락 메뉴가 표시됩니다.

<img src="/assets/img/2024-07-02-UsingAugmentedRealitytoBringDinosaursSkeletonstoLifeaTutorial_11.png" />

이제 Vuforia 설정 창에서 라이센스를 입력해야합니다. 이를 위해 먼저 Vuforia 개발자 동의서에 동의해야합니다: 도움말/Vuforia Engine/개발자 동의서 표시로 이동하여 수락하십시오. 그런 다음, dev.vuforia.com에서 Basic 라이센스를 생성해야합니다. 이미 완료되지 않은 경우 복사하여 App 라이센스 키 필드에 붙여넣으십시오. 이제 프로젝트에서 Vuforia Engine를 사용할 준비가 되었습니다.

<div class="content-ad"></div>

![2024-07-02-UsingAugmentedRealitytoBringDinosaursSkeletonstoLifeaTutorial_12](/assets/img/2024-07-02-UsingAugmentedRealitytoBringDinosaursSkeletonstoLifeaTutorial_12.png)

## 배포 테스트 및 Sequence Recording 앱 만들기

이 단계에서는 최소한의 앱을 배포해보는 것을 권장합니다. 유용하게 만들기 위해 Sequence Recording 앱을 만들어 보겠습니다. 이를 통해 Unity에서 재생해볼 수 있는 비디오를 녹화할 수 있으며, 앱을 디바이스에서 테스트하는 것과 동일한 방식으로 개발 속도를 상당히 높일 수 있습니다. 이는 비디오뿐만 아니라 디바이스의 공간 위치도 녹화합니다. Unity의 재생 모드에서 사용되어, 무한히 이동하며 앱의 모든 측면을 테스트할 수 있게 해줍니다.

이를 위해, 계층 구조에서 마우스 오른쪽 버튼을 클릭하고 Vuforia Engine 메뉴에서 "Sequence Recorder"를 선택하세요. 메인 카메라를 삭제한 후 다시 마우스 오른쪽 버튼을 클릭하고 Vuforia Engine 메뉴에서 "AR 카메라"를 선택하세요.

<div class="content-ad"></div>

<img src="/assets/img/2024-07-02-UsingAugmentedRealitytoBringDinosaursSkeletonstoLifeaTutorial_13.png" />

그러면 이제 애플리케이션을 빌드하고 배포할 수 있어요. 배포가 성공적이라면, 카메라 피드와 녹화 버튼이 있는 앱이 만들어질 거에요. 여기에서 시퀀스 녹화를 사용하는 방법과 그 의미를 배울 수 있어요: https://youtu.be/RFU7y9YQSK4. 몇 개의 동영상을 녹화하는 동안 3D 프린팅된 스켈레톤 주위를 움직여 보세요. 나중에 이러한 동영상을 사용할 거에요.

## Vuforia VFX 라이브러리 가져오기

- Unity 에셋 스토어에 방문하여 Vuforia VFX 라이브러리를 자산에 추가하세요.
- Unity 프로젝트에서 패키지 매니저를 열어주세요.
- 드롭다운 메뉴에서 내 자산을 선택하고 애셋 패키지를 Unity 프로젝트로 다운로드 및 가져오세요.

<div class="content-ad"></div>

<img src="/assets/img/2024-07-02-UsingAugmentedRealitytoBringDinosaursSkeletonstoLifeaTutorial_14.png" />

이 에셋 중의 데모 씬 중 하나를 사용하여 고대 포식자의 해골을 증강시키는 기본적인 AR 체험을 만들어 보겠습니다! Sample Ressources 폴더에 있는 LightWave 씬을 열어 주세요:

<img src="/assets/img/2024-07-02-UsingAugmentedRealitytoBringDinosaursSkeletonstoLifeaTutorial_15.png" />

당신의 씬은 이렇게 보일 것입니다. 하얀 펌프 모델이 추가된 모습입니다:

<div class="content-ad"></div>

![image](/assets/img/2024-07-02-UsingAugmentedRealitytoBringDinosaursSkeletonstoLifeaTutorial_16.png)

자, 이제 재생 버튼을 눌러보세요. 3D 프린터로 출력된 펌프를 즉시 파란 빛 파장으로 스캔하는 동영상이 나타날 겁니다.

![image](/assets/img/2024-7-02-UsingAugmentedRealitytoBringDinosaursSkeletonstoLifeaTutorial_17.png)

여기에서 무슨 일이 일어나고 있는지 설명해드릴게요:

<div class="content-ad"></div>

- 이 장면에는 유니티가 사용할 수 있는 녹화물(펌프의 비디오)이 포함되어 있습니다. 이 비디오 피드와 녹화된 기기의 위치 데이터를 보포리아(Vuforia)에 공급하여 AR 장치로 대상을 보는 것을 시뮬레이션할 수 있습니다.
- 장면에는 또한 펌프의 고급 모델 타겟이 포함되어 있어 인식 및 추적할 수 있습니다.
- LightWave VFX는 펌프 모델이 감지되고 추적되는 즉시 재생되어 사용자에게 인식 및 추적에 대한 신호를 제공하는 셰이더 애니메이션입니다.

이 장면을 기기에 배포하고 이 펌프의 3D 프린트를 소유한다면 기기에서 이 효과를 볼 수 있습니다. 우리는 이제 이것을 펌프 대신 3D 스캔된 공룡 스켈레톤을 감지하도록 수정할 것입니다.

## 자체 Model Target 생성

우리는 어떤 각도에서도 공간에 골격을 인식하고 추적하고 싶습니다. 이를 위해 이전에 획득한 3D 스캔을 Model Target으로 변환해야 합니다.

<div class="content-ad"></div>

Vuforia 개발자 포털에서 Model Target Generator 도구를 다운로드하고 설치해야 합니다. 첫 번째 실행 시 Vuforia 계정으로 인증해야 합니다. 그런 다음 "Create New Model Target" 버튼을 클릭하세요.

![이미지](/assets/img/2024-07-02-UsingAugmentedRealitytoBringDinosaursSkeletonstoLifeaTutorial_18.png)

3D 스캔을 선택하고 이름을 지어 모델 타겟을 만들어 주세요. 다음 화면에는 3D 모델과 해당 방향 및 크기가 표시됩니다. 3D 스캔된 텍스처가 적용되어 있어야 하며, 기대보다 어둡게 나올 수 있습니다. 크기가 적당하고 골격과 일치하는지 확인해주세요. 물리적 버전과 비교했을 때 너무 크거나 작은 객체는 나중에 문제를 일으킬 수 있습니다.

![이미지](/assets/img/2024-07-02-UsingAugmentedRealitytoBringDinosaursSkeletonstoLifeaTutorial_19.png)

<div class="content-ad"></div>

만약 방향과 크기가 잘 맞는다면, 앞으로 진행하세요. 색상 및 복잡도 단계는 이 프로젝트에 중요하지 않으므로 그냥 확인을 클릭하세요. "추적 최적화"에 대해 묻힐 때 "기본값"을 선택하고 확인하세요.

![이미지](/assets/img/2024-07-02-UsingAugmentedRealitytoBringDinosaursSkeletonstoLifeaTutorial_20.png)

마지막 단계에서 "고급 보기 생성"을 선택하고 "제한된 각도 범위 및 대상 범위"를 선택하세요. 일반적으로 360 돔이나 풀 360이 적합하지만, 화강암의 미세 구조는 대상 범위에 수동 조정이 필요할 수 있습니다.

![이미지](/assets/img/2024-07-02-UsingAugmentedRealitytoBringDinosaursSkeletonstoLifeaTutorial_21.png)

<div class="content-ad"></div>


![Step 1](/assets/img/2024-07-02-UsingAugmentedRealitytoBringDinosaursSkeletonstoLifeaTutorial_22.png)

Click “Next Step”. In the new view, click the blue box (the Target Extent) and adjust it to encompass only the rib cage and bassin. It’s not always necessary (on larger, more opaque models it doesn’t need to be adjusted) but help in this case with the skeleton’s fine structures.

![Step 2](/assets/img/2024-07-02-UsingAugmentedRealitytoBringDinosaursSkeletonstoLifeaTutorial_23.png)

![Step 3](/assets/img/2024-07-02-UsingAugmentedRealitytoBringDinosaursSkeletonstoLifeaTutorial_24.png)


<div class="content-ad"></div>

마지막으로, 모델의 가시성에 기반하여 인식 범위를 조정하고 "사용자 정의 뷰 생성"을 클릭하세요.

![image](/assets/img/2024-07-02-UsingAugmentedRealitytoBringDinosaursSkeletonstoLifeaTutorial_25.png)

![image](/assets/img/2024-07-02-UsingAugmentedRealitytoBringDinosaursSkeletonstoLifeaTutorial_26.png)

모델 타겟이 거의 완료되었습니다. "고급 모델 타겟 생성"을 클릭하여 데이터베이스 생성을 완료하고 새로 생성된 데이터베이스 앞에 있는 "훈련"을 클릭하여 훈련을 시작해주세요.

<div class="content-ad"></div>


![Image 1](/assets/img/2024-07-02-UsingAugmentedRealitytoBringDinosaursSkeletonstoLifeaTutorial_27.png)

![Image 2](/assets/img/2024-07-02-UsingAugmentedRealitytoBringDinosaursSkeletonstoLifeaTutorial_28.png)

![Image 3](/assets/img/2024-07-02-UsingAugmentedRealitytoBringDinosaursSkeletonstoLifeaTutorial_29.png)

After a couple of hours, the “Train” button switches to “Export”, allowing you to download a set of files, with one of them being a Unity Package. Simply drag and drop this package into your Unity project to add your Model Target. Now you're ready to recognize and track your skeleton in the next session.


<div class="content-ad"></div>

PS: 만약 Model Target에 대해 더 배우고, 작동 방식, 최선의 방법 및 해야 할 일과 하지 말아야 할 것들을 알고 싶다면, Vuforia 라이브러리 기사를 확인해보세요.

## 샘플 씬을 구성하여 공룡 뼈대를 인식하도록 설정하기

Model Target 유니티 패키지를 Unity 프로젝트에 가져온 후, 첫 번째 단계는 ModelTarget 게임 오브젝트에서 데이터 세트를 변경하는 것입니다:

![이미지](/assets/img/2024-07-02-UsingAugmentedRealitytoBringDinosaursSkeletonstoLifeaTutorial_30.png)

<div class="content-ad"></div>

그러면 "대상 표현 추가"를 클릭하세요. 골격의 3D 스캔이 이제 펌프 옆에 나타날 것입니다. 계층 구조에서 해당하는 게임 오브젝트도 볼 수 있습니다:

![image](/assets/img/2024-07-02-UsingAugmentedRealitytoBringDinosaursSkeletonstoLifeaTutorial_31.png)

다음으로, VFX Lightwave 스캔 게임 오브젝트의 "TargetObject"를 벨로시랩터 대상 표현으로 교체하세요:

![image](/assets/img/2024-07-02-UsingAugmentedRealitytoBringDinosaursSkeletonstoLifeaTutorial_32.png)

<div class="content-ad"></div>

이제 "VSYPump_360" GameObject를 제거할 수 있습니다. 이제 스켈레톤을 인식하고 그 뼈들과 함께 멋진 "스캐닝" 효과를 볼 수 있어야 합니다. 바로 테스트하려면 "ARCamera" 컴포넌트를 선택하고 "Reload Scene With Recording"이라는 스크립트에서 컴포넌트 오른쪽 클릭/제거를 하세요.

![이미지](/assets/img/2024-07-02-UsingAugmentedRealitytoBringDinosaursSkeletonstoLifeaTutorial_33.png)

그런 다음, 이전에 첫 번째 배포한 애플리케이션으로 캡처한 비디오 녹화 파일을 가져와야 합니다. Ctrl+Shift+V를 클릭하여 Vuforia 구성 패널을 열고, 하단에서 Play Mode Type을 "Recording"으로 변경하고 연속계의 위치를 찾아 누르세요(https://youtu.be/RFU7y9YQSK4). 재생을 누르세요. 모든 것이 잘 되었다면, 이렇게 보일 것입니다:

이제 증강 현실에서 스켈레톤을 인식하고 추적할 수 있습니다! 원한다면 앱을 배포하여 실시간으로 테스트할 수 있습니다. VFX_Lightwave_Scan 효과의 "루프" 매개변수를 선택하여 계속 실행되도록 하세요.

<div class="content-ad"></div>

추적된 해골이 준비되었으니 이제 그것을 당신이 만든 3D 조각된 살살한 덮개로 보강해야 합니다.

## 해골에 살살한 모델을 겹치기

가상의 살아 있는 공룡으로 덮인 해골을 보려면 Unity로 3D 모델을 가져와서 Model Target GameObject에 부모로 설정해야 합니다.

만약 3D 스캔 위에 조각했다면, 그들은 상자에서 완벽하게 정렬되어야 합니다.

<div class="content-ad"></div>

만약 그렇지 않으면, 이 단계에서 모델을 이동하거나 회전시켜 스켈레톤과 정렬할 수도 있어요.

![image](/assets/img/2024-07-02-UsingAugmentedRealitytoBringDinosaursSkeletonstoLifeaTutorial_34.png)

마지막 단계는 스캔 효과가 완료될 때까지 증강(살)이 나타나지 않도록 하는 것입니다. 이러한 문제를 해결하는 다양한 방법이 있지만, 여기 간단하고 코드 없이 접근하는 방법입니다: VFX_Lightwave_Scan Gameobject에서 "On Reached End()" 옆에 있는 "+" 버튼을 클릭하세요.

![image](/assets/img/2024-07-02-UsingAugmentedRealitytoBringDinosaursSkeletonstoLifeaTutorial_35.png)

<div class="content-ad"></div>

첫 번째 필드에 Target Representation을 드래그하고 두 번째 필드에 Augmentation을 드래그해주세요.

<img src="/assets/img/2024-07-02-UsingAugmentedRealitytoBringDinosaursSkeletonstoLifeaTutorial_36.png" />

첫 번째 요소는 스캔 효과가 끝나면 "Off"로 바꿀 것입니다. 그리고 두 번째 요소는 동시에 "On"으로 바꿀 것입니다.

<img src="/assets/img/2024-07-02-UsingAugmentedRealitytoBringDinosaursSkeletonstoLifeaTutorial_37.png" />

<div class="content-ad"></div>

마지막으로, 체크 상자를 해제하여 기본적으로 증강 기능을 비활성화하세요.

녹화를 다시 재생하여 이벤트의 순서를 테스트해 봅시다. 이렇게 보일 것입니다:

<div class="content-ad"></div>

여기 오셨네요. 3D 프린트된 뼈대에 대한 AR 경험입니다. 또는 인식하고 추적하고 증강하고 싶은 다른 것이 있다면요.

당연히 이것은 101번째 증강입니다 — 여기서는 개선할 여지가 많습니다. 재료, 조명, 전환, 상호 작용 등에 대해요. 여기 몇 가지 지시사항과 아이디어가 있습니다:

- 대상이 움직이지 않고 주변 빛이 일정하면/안정적이라면, 자식으로 빛 근원을 추가하여 보다 현실적으로 증강을 조명할 수 있습니다. 가상과 실제 빛 근원을 조정하여 정렬합니다.
- 여러 뼈대나 대상을 순차적으로 추적하고 싶다면 Model Target 데이터베이스를 만들 수 있습니다. 씬에 더 많은 Model Target을 추가하고 반복 프로세스를 실행하고 각각의 데이터베이스에서 올바른 대상을 선택합니다. 더 많은 정보는 여기에서 확인하세요.
- 증강물이 너무 갑자기 나타난다면, 재료의 불투명도를 애니메이션화하거나 Advanced Dissolve 애셋과 같은 해체 쉐이더를 사용하여 출현을 완화할 수 있습니다. 이 애셋에 제공된 재료에는 설정 가능한 컷아웃 효과가 있어 가장자리, 컷아웃의 모양, 매핑 등을 제어할 수 있습니다. 파라미터(_AdvancedDissolveCutoutStandardClip)를 1에서 0으로 애니메이션화하면 증강이 점진적으로 나타나는 것처럼 보여 뼈대 주변에 성장하는 것처럼 만들어줍니다.

<img src="/assets/img/2024-07-02-UsingAugmentedRealitytoBringDinosaursSkeletonstoLifeaTutorial_40.png" />

<div class="content-ad"></div>

- Vuforia VFX 라이브러리에는 많은 효과가 포함되어 있습니다 (출현 효과, 히트맵, 엑스레이...), 이를 활용하여 쉽게 AR 경험을 업그레이드할 수 있습니다. 정보와 상호작용의 층을 추가하여 다양한 경험을 만들어보세요.
- 이미지와 모델 대상을 동시에 추적할 수 있어 흥미로운 결합 가능성을 제공합니다.
- 만약 Magic Leap 2 또는 Hololens 2에 액세스할 수 있다면, 이 모든 것이 작동할 수도 있습니다!

제가 제시한 내용이 여러분이 자신만의 증강 현상장을 구현하는 데 조금이나마 도움이 되었기를 바랍니다. 결과물을 보내주시면 정말 감사하게 생각하겠습니다!