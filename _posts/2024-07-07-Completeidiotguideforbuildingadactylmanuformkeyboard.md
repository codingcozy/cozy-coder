---
title: "초심자를 위한 다크틸 매뉴폼 키보드 제작 가이드"
description: ""
coverImage: "/assets/img/2024-07-07-Completeidiotguideforbuildingadactylmanuformkeyboard_0.png"
date: 2024-07-07 02:52
ogImage: 
  url: /assets/img/2024-07-07-Completeidiotguideforbuildingadactylmanuformkeyboard_0.png
tag: Tech
originalTitle: "Complete idiot guide for building a dactyl manuform keyboard."
link: "https://medium.com/swlh/complete-idiot-guide-for-building-a-dactyl-manuform-keyboard-53454845b065"
isUpdated: true
---





![2024-07-07-Completeidiotguideforbuildingadactylmanuformkeyboard_0.png](/assets/img/2024-07-07-Completeidiotguideforbuildingadactylmanuformkeyboard_0.png)

## 이 가이드는 다음과 같은 경험이 없는 분들을 위해 만들어졌어요:

- 3D 프린팅 및 3D 파일 편집
- 커스텀 키보드 제작
- 솔더링 기술
- QMK 프로그래밍 또는 일반적인 프로그래밍

이 빌드 로그에서는 제가 처음부터 dactyl manuform mini 5x6를 만드는 방법을 보여드릴 예정이에요. 전문가가 아니라서 이 문제에 있어서 전적으로 온라인 튜토리얼에 의존했어요. 때때로 어려운 시간을 극복하는 데 도움이 되었죠.

<div class="content-ad"></div>

이 안내서는 제가 제 키보드를 만들 때 겪고 싶었던 경험들을 상세히 설명하는 것이기 때문에 좀 지루할 수 있어요.

그럼 본론으로 들어가볼까요? 이 프로젝트에 얼마나 돈을 썼는지 파악해봅시다. 도구 제외한 비용을 보면:

- 3D 프린팅 케이스 x2 (27 달러)
- 레이저 컷 하단 판 x2 (3.5 달러)
- 키 스위치 x80 (13 달러)
- 다이오드 x100 (0.5 달러)
- 15 cm 24 AWG 와이어 x100 (0.8 달러)
- 아두이노 프로 마이크로 x4 (10 달러)
- 마이크로 USB에서 타입 C로 연결되는 케이블 x3 (3 달러)
- 3.5mm 잭 커넥터 x4 (1 달러)
- M3 셀프탭핑 인서트 x10 (1 달러)

도구를 포함하면 (저는 이 도구들이 전혀 없어서 키보드 부품보다 도구에 더 많이 소비했어요)

<div class="content-ad"></div>

- 납
- 납은 다리
- 집게
- 와이어 스트리퍼
- 납 장치

![이미지](/assets/img/2024-07-07-Completeidiotguideforbuildingadactylmanuformkeyboard_1.png)

색인

- 원하는 키보드 찾기
- 당신에게 맞는 Dactyl 찾기
- 키보드 케이스 3D 프린팅
- 기타 물건 구매
- 당신의 Dactyl Manuform 만들기
- QMK 소프트웨어로 키보드 테스트하기

<div class="content-ad"></div>

## 1. 원하는 키보드 찾기

온라인에서는 많은 커뮤니티 지원을 받는 다양한 맞춤형 키보드가 있습니다. 그리고 수많은 제작일지가 있어요. 하지만 이것들은 제게는 조금 모자란 듯합니다. 에르고독스나 아트레우스, 그리고 몇 가지 60% 키보드 등 여러 종류가 있기 때문에, 자신의 필요에 맞는 키보드를 찾고 그 레이아웃을 배우기 위해 시간을 투자할 준비가 필요합니다. 분할 키보드는 일반적인 풀 사이즈나 TKL 키보드와는 매우 다르므로, 단축키를 많이 사용하는 경우 학습에 시간이 걸릴 수 있습니다. 에르고독스나 아트레우스와 같은 레이아웃을 시도해보고 싶다면, 시장에는 이미 매우 맞춤화된 제품이 있습니다. 제가 추천하는 건 키보디오 모델01인데, 디자인 면에서 이쁘고 번거로움 없이 사용할 수 있는 키보드이고 (두 번째 버전을 곧 출시할 예정인 듯 합니다).

![이미지](/assets/img/2024-07-07-Completeidiotguideforbuildingadactylmanuformkeyboard_2.png)

자, 그만 허풍 떼고 다크틸 키보드를 만들어 봅시다. 프로젝트를 시작하기 전에 더 많은 조사를 하면 프로젝트 도중에 받는 고통이 줄어듭니다.

<div class="content-ad"></div>

## 2. 당신에게 알맞은 다크틸 키보드 찾기

다크틸 키보드에는 두 가지 주요 유형이 있습니다. 일반 다크틸 키보드와 두 번째는 manuform 버전입니다. 주된 차이점은 엄지 클러스터의 위치인데, 일반 다크틸은 엄지를 올린 위치에 두어야 하지만 manuform은 엄지를 아래쪽으로 움직일 수 있습니다. 이는 주로 개인의 선호도에 따라 결정됩니다. 전자는 일반 버전과 유사한 엄지 클러스터가 있는 에르고독스 디자인으로 나쁜 경험을 했기 때문에 나는 manuform을 선택했습니다.

<img src="/assets/img/2024-07-07-Completeidiotguideforbuildingadactylmanuformkeyboard_3.png" />

이후에는 키보드에 있는 키의 수를 정해야 합니다. 이 키보드에는 4x5, 4x6, 5x6, 5x7 등 다양한 퍼뮤테이션이 존재하므로 만족스러운 것을 찾을 때까지 인터넷을 둘러보세요. 나는 beekeeb의 다크틸 manufirm mini를 선택했습니다. 그는 STL 파일과 일부 고수준 빌드 로그를 제공합니다. 그의 디자인은 키보드의 높이와 키의 수를 줄였습니다 (전에 시도했지만 성취하지 못한 부분입니다).

<div class="content-ad"></div>

## 3. 키보드 케이스의 3D 프린팅

여러분, 이 부분은 매우 간단합니다. 만약 3D 프린터를 소유하고 있다면, 이 부분에서 추가로 말씀 드릴 것이 없습니다. 모두 자유롭게 진행하세요. 만약 소유하고 있지 않다면, 두 가지 옵션이 있습니다. 3D 프린터를 구입하고 래빗 홀에 빠져들거나 다른 사람에게 프린팅을 의뢰하세요. Reddit와 beekeeb의 상점에서 프린팅 서비스를 제공하며, 아시아에서는 Taobao로 불리는 aliexpress에 접근하여 더 경제적인 방법으로 프린팅할 수 있습니다.

프린팅에 사용하는 재료는 정말 중요하지 않습니다. 키캡이 설치된 후에는 케이스를 거의 만지지 않을 것이기 때문입니다. 하지만 유연성을 제공하는 나일론 소재를 추천드립니다.

리셋 버튼을 위한 여유 공간을 남겨두는 것을 기억하세요. 대부분의 빌드에서는 이를 위한 공간이 예약되어 있지 않습니다.

<div class="content-ad"></div>

<img src="/assets/img/2024-07-07-Completeidiotguideforbuildingadactylmanuformkeyboard_4.png" />

참고: 이 빌드에서 디자이너는 3.5 커넥터와 마이크로 USB 출구 홀을 위한 특정 슬롯을 사용했습니다. 구입하는 액세서리가 정확한 치수에 맞지 않을 수 있으므로 인쇄하기 전에 커넥터의 크기를 측정하는 것이 권장됩니다. 또는 핫 글루를 사용하거나 납기로 녹여 접착시킬 수도 있습니다. 내가 알기로, RJ9 커넥터를 사용하여 다칠 스위치를 조립하는 일반적인 빌드의 경우, 여기서 발생하는 문제가 없습니다.

## 4. 다른 것들 구매하기

a. 키 스위치 x80 | 키보드를 직접 제작할 수 있는 주요 장점 중 하나는 선호하는 키 스위치를 선택할 수 있다는 것입니다. 다양한 종류가 있어서 어떤 것을 선택해도 크게 틀릴 일이 없습니다. 그러나 이 케이스는 대부분 체리 또는 체리와 유사한 키 스위치용으로 설계되어있어, kaih 또는 다른 제품이 적합하지 않을 수 있습니다. 케이스에 꼭 맞게 하려면 핫 글루가 필요합니다. 저는 자신이 실패할 경우 몇 펜스를 절약해 줄 것 같은 가터론 블루로 선택했습니다. (이에 대해 비난하지 마세요.)

<div class="content-ad"></div>

b. 키캡 x 1 풀 세트 | 키캡을 구매할 때 OEM DSA SA 등과 같은 다양한 종류가 있음을 기억해 주세요. 키캡의 균일한 높이 스타일을 권장합니다. 동일한 키보드 롤 내에서 높이 차가 발생하지 않도록 합니다. 적합한 키캡 스타일을 찾는 것은 쉽지 않습니다. 특정 유닛 크기의 특수 키가 필요하기 때문입니다. 귀찮음을 피하고 싶다면 올바른 스타일과 유닛 크기를 찾는 데 머리아픈다면 빈 키캡을 구매할 수도 있습니다.

![이미지](/assets/img/2024-07-07-Completeidiotguideforbuildingadactylmanuformkeyboard_5.png)

c. 와이어, 와이어 스트리퍼 및 면도칼 x1 | 처음 할 때 좋은 와이어 선택이 특히 중요합니다. 두꺼운 와이어는 더 쉽게 솔더링 및 벗길 수 있으며 프로세스에서 덜 머리아플 것입니다. 저는 24 AWG 와이어를 쓴 다중 비织체(WAG는 와이어의 너비를 나타내며, 숫자가 작을수록 케이블이 두꺼워집니다)가었지만 22AWG를 선택하면 더 좋았을 거라고 생각합니다. 와이어 스트리퍼는 와이어 작업 시 매우 유용합니다. 예리한 면도칼이 손에 있으면 좋습니다.

d. 다이오드 x80 | 자명한 사실이며, 단순히 올바른 1N4148을 구입하고, 후에 개별로 접는 시간을 절약할 수 있도록 이러한 방식으로 발송된 제품을 찾는 것이 좋습니다.

<div class="content-ad"></div>

e. 아두이노 프로 마이크로 x4 | 다양한 종류의 아두이노가 많이 있습니다. 좋은 평가와 합리적인 가격의 제품을 찾아보세요. 만약 납땜에 익숙하지 않다면, 다리가 이미 설치된 제품을 찾아보세요. 저는 혹시 모를 일을 대비하여 2개 더 구입했지만, 보통 조금 편집증이 있어서 그냥 사두었습니다.

![이미지](/assets/img/2024-07-07-Completeidiotguideforbuildingadactylmanuformkeyboard_6.png)

f. 3.5 밀리미터 잭 / TRS / TRRS 커넥터 x4 | 키보드의 양 쪽을 연결하기 위해 3.5 밀리미터 잭 케이블을 사용합니다. 그러나 사용 중인 3.5 밀리미터 잭의 종류를 확인하세요. 댁틸 키보드는 적어도 3개의 데이터 핀 연결이 필요하기 때문에 TRS 또는 TRRS만 사용 가능합니다.

![이미지](/assets/img/2024-07-07-Completeidiotguideforbuildingadactylmanuformkeyboard_7.png)

<div class="content-ad"></div>

g. 작은 리셋 버튼 x3 | 키보드 안에 설치된 Arduino pro micro를 재설정해야하는 경우, 각 쪽에 버튼이 필요합니다. 그렇지 않으면 제 처럼 두 개의 노출된 점퍼 와이어로 수치심에 살 것입니다. 버튼 도착까지 기다리며요.

h. 자체탭 스레드 인서트 145M3 x15 | 인서트의 올바른 길이를 구입하는지 확인해주세요. Taobao를 통해 구매하시는 분들을 위해 중국어 이름은 自攻螺套自攻丝套입니다.

i. 아두이노 점퍼 케이블 (선택 사항이지만 강력히 권장됨) | 아두이노와 함께 사용하는 점퍼 케이블은 덜 납땜하면서도 견고한 연결을 제공합니다. 또한, 키 스위치의 하단에 쉽게 접근하여 테스트하거나 납땜 목적으로 사용할 수 있습니다.

j. 타입 C 커넥터 (선택 사항) x3 | 내 dactyl manuform 키보드 디자인에서는 사용자가 직접 Arduino 보드에 꽂지 않으므로, 마이크로 USB에서 타입 C 연장 케이블을 사용하거나 동일한 결과를 얻기 위해 타입 C 브레이크아웃 보드를 구입할 수 있습니다.

<div class="content-ad"></div>

k. 납땜 도구 x1 | 납땜기와 납은 프로젝트의 성격상 필수품입니다. 납땜기를 잘 다루지 못해도 걱정하지 마세요. 이 프로젝트를 마치면 납땜기를 잘 다룰 수 있을 거예요. 공간이 좁은 곳에서 납땜을 하기 때문에 작은 지름의 납이 유용할 수 있지만 주로 개인의 선호에 따라 다릅니다.

l. 기타 도구

- 크로스(+자)드라이버
- 뾰족한/평평한 집게
- 전기 테이프
- 핫 글루

이제 당신이 닥틸 매뉴폼 키보드를 만들기 위해 필요한 모든 것을 갖추었어요. 이제 다른 사람들의 작품에 열중하며 2-3 주를 기다리세요.

<div class="content-ad"></div>

## 5. 다크틸 매누폼 키보드 만들기

제작 과정은 실제로 익히면 꽤 쉽습니다. 기본적으로 키보드는 그리드 형식의 행렬에 의해 연결되며, 각 키 스위치는 다이오드가 있는 수평선과 수직선으로 연결되어야 합니다. 이 다이어그램은 저가 구성한 디자인보다 한 가지 더 많은 엄지 키가 있습니다. 배선을 조정해야 하는데요, 보통 그리드의 하단 키를 건너뛰고 바로 엄지 그리드로 가면 됩니다.

![다크틸 매누폼 키보드 구성도](/assets/img/2024-07-07-Completeidiotguideforbuildingadactylmanuformkeyboard_8.png)

참고: 키보드 제작을 왼손용 부분으로 시작해서 제작을 완료하면 테스트를 시작할 수 있습니다. 제가 한 것처럼 오른손 부분부터 시작하면 QMK 소프트웨어로 양 손을 완료할 때까지 제대로 키보드를 테스트할 수 없습니다.

<div class="content-ad"></div>


![이미지](/assets/img/2024-07-07-Completeidiotguideforbuildingadactylmanuformkeyboard_9.png)

보통 사람들은 수평(다이오드가 달린)부터 시작한 뒤 수직으로 이어갑니다. 다이오드를 90도로 구부러 다이오드의 시작점에서 다른 한쪽을 스위치 자체보다 약간 긴 길이로 잘라내세요.

![이미지](/assets/img/2024-07-07-Completeidiotguideforbuildingadactylmanuformkeyboard_10.png)

키보드 배선에 사용한 방법은 스트립 케이블 방식입니다. 15cm 선을 구매해, 각 스위치의 돌출부 사이의 거리를 표시하고 선의 작은 길이(8mm-1cm)를 벗겨내세요. 이 과정을 모든 돌출부에 반복하세요. 그런 다음 선을 스위치의 돌출부 주위로 감도록 하세요. 선 자체가 스위치 주위로 감겨 있으면 실드링을 하지 않아도 움직이지 않도록 합니다. 이렇게 하면 실드링 작업이 더 쉬워집니다.


<div class="content-ad"></div>


![image](/assets/img/2024-07-07-Completeidiotguideforbuildingadactylmanuformkeyboard_11.png)

NOTE: 다이오드는 한 방향으로만 전류가 통과됩니다. 만약 다이오드에 직접 납땜을 하는 경우, 연결 지점은 항상 다이오드의 끝 부분에 있어야 합니다.

![image](/assets/img/2024-07-07-Completeidiotguideforbuildingadactylmanuformkeyboard_12.png)

![image](/assets/img/2024-07-07-Completeidiotguideforbuildingadactylmanuformkeyboard_13.png)


<div class="content-ad"></div>

수평 매트릭스를 완성한 후에는 자신에게 박수를 치세요. 당신은 빌드의 가장 어려운 부분을 완수했습니다.

기억해야 할 중요한 점은 수평과 수직이 절대로 서로 닿아서는 안 된다는 것입니다. 주변에 전기 테이프가 있으면 매트릭스의 하단 층에 적용하여 의도하지 않은 입력으로 매트릭스를 실수로 단락시키지 않도록 해야 합니다.

일부 빌드에서는 RJ9을 커넥터로 사용하는 경우도 있지만, 저는 3.5 잭 커넥터를 사용했습니다. 구매한 커넥터와 케이블이 동일한 유형이라는 것을 확인해야 합니다. TRRS 커넥터와 TRS 케이블의 연결은 TRRS 커넥터와 TRRS 케이블의 연결보다 약간 다르게 배선되어야 합니다.

![이미지](/assets/img/2024-07-07-Completeidiotguideforbuildingadactylmanuformkeyboard_14.png)

<div class="content-ad"></div>

다음은 키보드의 납땝 및 조립 과정을 나보다 더 잘 보여주는 비디오입니다.

![Link to the soldering and build process video](/assets/img/2024-07-07-Completeidiotguideforbuildingadactylmanuformkeyboard_15.png)

키보드를 재설정하려면 RST 및 GND에 연결된 리셋 버튼을 연결해야 합니다. 이 버튼을 두 번 누르면 Arduino가 8초 동안 재설정되어 QMK를 플래시할 수 있습니다.

![Link to reset button connection](/assets/img/2024-07-07-Completeidiotguideforbuildingadactylmanuformkeyboard_16.png)

<div class="content-ad"></div>

위의 내용을 친근하게 번역해 드리겠습니다.

장착을 하실 때 커넥터를 열심히 고무로 붙이는 걸 잊지 마세요. 더 많이 붙이는 게 더 좋습니다.

## 6. QMK 소프트웨어로 키보드 테스트하기

제 생각에는 이 부분이 실제로 가장 까다로운 부분인데요, 왜냐하면 저는 QMK를 시도해보지 않았기 때문이고 대부분의 빌드 로그는 주로 키보드의 물리적 구성에 초점을 맞춥니다.

QMK 툴킷에서 지원하는 dactyl 키보드 레이아웃의 원본 디자인을 사용하고 있다면 큰 문제가 없을 겁니다. 하지만 제처럼 약간 다른 dactyl 레이아웃을 사용하고 있는 경우, 약간 다른 절차를 거쳐야 할 것입니다.

<div class="content-ad"></div>

만약 QMK 툴킷에 키맵이 없더라도, 가장 유사한 키보드를 찾아서 먼저 테스트해 보세요. 분명히 성취감과 커다란 행복을 느낄 수 있을 거예요.

QMK 사용 방법 (키보드가 지원되는 경우)

여기 QMK 사용 방법에 대한 많은 문서가 있어요.

일반 레이아웃에 대한 요약은 다음과 같습니다:

<div class="content-ad"></div>

- QMK 구성기로 이동하세요.
- 키보드 이름을 선택하세요.
- 원하는 키맵을 변경하세요.
- 다음 단계를 위해 .hex 파일을 생성하세요.
- QMK 툴킷을 다운로드하고 설치하세요.
- 툴킷에서 준비된 .hex 파일을 열어주세요.
- "자동 플래시"를 클릭하세요.
- Arduino Pro Micro를 재설정하세요.
- 끝났습니다.

키보드의 경우 QMK 사용 방법이 표시되지 않을 때

일반 레이아웃을 사용하지 않는 경우, 직접 레이아웃을 컴파일해야 합니다. 컴퓨터 환경에 QMK를 설정하는 방법은 이 가이드를 따라주세요. "QMK 환경 설정하기"와 "첫 번째 펌웨어 빌드하기"를 따라주세요.

QMK를 설치한 후, 로컬 머신에 QMK를 다운로드하고 자신만의 키맵과 키보드를 편집할 수 있습니다.

<div class="content-ad"></div>

다음 위치로 이동하세요: /handwired/dactyl_manuform/5x6에서 5x6.h를 수정하고 기존 keymap을 수정하세요. 필요 없는 키는 KC_NO를 추가하거나 매트릭스에 더 많은 키를 등록하세요.

![이미지](/assets/img/2024-07-07-Completeidiotguideforbuildingadactylmanuformkeyboard_17.png)

5x6.h 파일을 수정한 후 keymap.c 파일도 그에 맞게 수정하세요.

![이미지](/assets/img/2024-07-07-Completeidiotguideforbuildingadactylmanuformkeyboard_18.png)

<div class="content-ad"></div>

키맵을 헥스 파일로 컴파일하세요.

헥스 파일을 더블 클릭하여 QMK 툴킷을 시작하고, "자동 플래시"를 클릭하는 것을 기억해주세요. 그리고 키보드의 리셋 버튼을 더블 클릭하면 텍스트가 노란색으로 표시되어야 합니다.

![이미지](/assets/img/2024-07-07-Completeidiotguideforbuildingadactylmanuformkeyboard_19.png)

이제 당신의 Dactyl Manuform 키보드를 완전히 제어할 수 있습니다. 계속 사용해서 연습해보세요!

<div class="content-ad"></div>

이 긴 가이드 끝까지 읽은 분들에게 축하드립니다. 이것은 나의 키보드 만들기 경험을 전하려는 시도이며, 아마 누군가에게 도움이 되기를 바랍니다.