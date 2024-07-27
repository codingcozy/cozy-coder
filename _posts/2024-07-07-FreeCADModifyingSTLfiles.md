---
title: "FreeCAD로 STL 파일 수정하는 방법"
description: ""
coverImage: "/assets/img/2024-07-07-FreeCADModifyingSTLfiles_0.png"
date: 2024-07-07 13:22
ogImage: 
  url: /assets/img/2024-07-07-FreeCADModifyingSTLfiles_0.png
tag: Tech
originalTitle: "FreeCAD Modifying STL files"
link: "https://medium.com/@robofoundry/freecad-modifying-stl-files-35730ab38dee"
---


저는 프로젝트를 위해 약간의 변화를 주려고 사용 가능한 STL 파일 중 일부를 수정해야 했던 일 중 하나였어요. FreeCAD를 처음 사용할 때 어떻게 해야 하는지에 대해 조사를 좀 해야 했어요. 이 주제에 대한 정말 좋은 유튜브 영상들이 있지만, 빠른 단계를 제시하는 자료를 찾을 수 없었죠. 이렇게 문서를 제공해 봄으로써 누군가에게 도움이 될 수 있기를 바라요.

최근에는 FreeCAD에서 Raspberry Pi 카메라를 최신 로봇의 앞면에 부착할 간단한 홀딩 플레이트를 만들었어요. 이를 사용하여 어떻게 기존의 STL 파일을 FreeCAD에서 수정할 수 있는지 보여주는 테스트로 활용할 거예요. 이 프로세스는 간단하지만 특정한 순서대로 몇 가지 단계를 실행해야 해요.

1. FreeCAD에서 기존의 STL 파일을 가져옵니다.

![이미지](/assets/img/2024-07-07-FreeCADModifyingSTLfiles_0.png)

<div class="content-ad"></div>

여기가 당신이 가져온 STL 파일의 모습입니다.

![STL 파일](/assets/img/2024-07-07-FreeCADModifyingSTLfiles_1.png)

다음은 따를 단계들을 간략히 설명해 드리겠습니다. 아래에는 단계별 스크린샷을 제공할 거에요.

- Part → 메시에서 형상 생성, 해상도는 0.01로 설정
- 원본 STL/메시 삭제
- 부품 선택한 다음 Part → 복수로 변환
- 이전 형상 삭제
- Part → 생성된 사본 → 정제된 형상
- 새로운 정제된 형상을 선택. Part Design → 바디 생성. 이렇게 하면 기본 기능도 추가됩니다.
- 여기까지 하면 새 스케치를 만들어 모델을 보통처럼 작업할 수 있어요.

<div class="content-ad"></div>

Step 1 - 메쉬에서 형태 만들기

![메쉬에서 형태 만들기](/assets/img/2024-07-07-FreeCADModifyingSTLfiles_2.png)

형태를 만들기 위한 설정을 제공합니다 [기본값은 0.10이며, 더 나은 정확도를 얻기 위해 0.01로 변경합니다]

![형태 만들기 설정](/assets/img/2024-07-07-FreeCADModifyingSTLfiles_3.png)

<div class="content-ad"></div>

여기 당신이 단조된 모양이 어떻게 보일지에 대한 정보입니다

![image](/assets/img/2024-07-07-FreeCADModifyingSTLfiles_4.png)

단계 2 - 프로젝트에서 원래 STL/메쉬 삭제

단계 3 - 부품 선택 후 Part → Solid로 변환

<div class="content-ad"></div>


![Step 4 — delete the previous shape](/assets/img/2024-07-07-FreeCADModifyingSTLfiles_5.png)

Step 5 — select the part and Part → Create a copy → Refine shape

![](/assets/img/2024-07-07-FreeCADModifyingSTLfiles_6.png)


<div class="content-ad"></div>

## 단계 6 — 새로운 수정된 모양 선택하기. Part Design → 바디 생성 . 이 과정을 통해 베이스 기능도 추가됩니다.

![image](/assets/img/2024-07-07-FreeCADModifyingSTLfiles_7.png)

왼쪽 패널에는 우리가 모델을 수정하기 위해 새로운 스케치를 추가할 베이스 기능이 추가됩니다.

![image](/assets/img/2024-07-07-FreeCADModifyingSTLfiles_8.png)

<div class="content-ad"></div>

7단계 - 스케치를 작성하고 모델에 새로운 기능을 추가하세요.

기존 모델에 무작위 구멍을 추가해보겠습니다. 이것은 예시일 뿐이지만, 이를 통해 새롭게 생성된 본문에 원본 FreeCAD 모델처럼 스케치와 파트 디자인 워크벤치 내에서 가능한 모든 작업을 적용할 수 있음을 보여줍니다.

먼저 본문을 선택한 후 Basefeature를 선택하고 그 다음에 판의 윗면을 선택하고 스케치 → 스케치 생성을 선택하세요.

<div class="content-ad"></div>

그러면 판 위의 스케치 어디에든 새 원을 추가할 수 있습니다.

![image](/assets/img/2024-07-07-FreeCADModifyingSTLfiles_10.png)

스케치를 닫고 부품 설계 작업대를 사용하여 포켓 도구를 클릭합니다. 깊이를 선택할 수 있고 확인을 클릭하면 원본 판에 새로운 관통 구멍이 보여야 합니다.

![image](/assets/img/2024-07-07-FreeCADModifyingSTLfiles_11.png)

<div class="content-ad"></div>

거의 다 왔어요. 예를 들어 변곡선을 가장자리에 추가하고 싶다면 스케치를 사용하지 않고 Parts Design 작업대에서 바로 Fillet 도구를 사용하여 각 수직 가장자리에 필렛을 추가할 수 있어요. 그러면 최종 결과물은 이렇게 나올 거에요:

![image](/assets/img/2024-07-07-FreeCADModifyingSTLfiles_12.png)

이 단계에서는 FreeCAD에서 파일 → 내보내기 를 사용하거나 3D 프린팅을 위해 고품질의 STL 파일을 얻는 방법을 이전 기사에서 제공한 지침대로 사용하여 STL 파일을 간단히 내보낼 수 있어요. 그런 다음 수정된 모델을 3D 프린트할 수 있어요.

즐겁게 만드세요!!!

<div class="content-ad"></div>

## 참고문헌