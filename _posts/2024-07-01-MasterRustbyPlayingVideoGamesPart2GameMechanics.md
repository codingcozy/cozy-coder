---
title: "비디오 게임으로 러스트 마스터하기 2부  게임 메커닉 익히기"
description: ""
coverImage: "/assets/img/2024-07-01-MasterRustbyPlayingVideoGamesPart2GameMechanics_0.png"
date: 2024-07-01 17:46
ogImage: 
  url: /assets/img/2024-07-01-MasterRustbyPlayingVideoGamesPart2GameMechanics_0.png
tag: Tech
originalTitle: "Master Rust by Playing Video Games (Part 2) — Game Mechanics"
link: "https://medium.com/@jonte-osterberg/master-rust-by-playing-video-games-part-2-game-mechanics-110173e9986a"
---


1부를 아직 안 읽으셨나요? 확인해보세요!

자, 이제 우리는 Rust 로봇을 세팅했어요. 그럼 이제는 뭘 해야 할까요?

목표는 최고의 로봇 킬러를 만드는 것입니다. 순위 시스템에서 다른 플레이어들의 로봇과 경쟁할 겁니다 — 더 많은 경기에서 승리하면 순위가 올라가요. 이게 다예요! 덤으로 매월 대회가 열려 최종 우승자를 가립니다. 👑

![이미지](/assets/img/2024-07-01-MasterRustbyPlayingVideoGamesPart2GameMechanics_0.png)

<div class="content-ad"></div>

멋져요! 이제 우리가 만들고 있는 로봇을 이해해야 합니다. 로봇은 빌딩 화면에서 선택할 4개의 구성 요소가 있습니다. 현재 우리 로봇은 4개의 라이플을 가지고 있습니다. 이러한 구성 요소는 아래 이미지에 표시된 대로 숫자로 식별됩니다. 여기서 0은 앞, 1은 위, 2는 왼쪽, 3은 아래를 나타냅니다. 이는 머리의 방향에 기반합니다. 나중에 머리를 회전시킬 것입니다.

![로봇 구성요소](/assets/img/2024-07-01-MasterRustbyPlayingVideoGamesPart2GameMechanics_1.png)

구성 요소 외에도 모듈이 있습니다. 이러한 모듈은 특별한 기능을 수행하여 로봇이 주변을 관찰하고 독특한 능력을 실행할 수 있도록 합니다. 실제 로봇을 프로그래밍해본 적이 있다면 센서가 세계를 인식하는 데 중요한 역할을 한다는 것을 이해할 것입니다. 이 게임은 모듈을 통해 비슷한 개념을 적용합니다. 레이더, 레이저 및 힘-필드 모듈로 시작할 것입니다요.

![로봇 모듈](/assets/img/2024-07-01-MasterRustbyPlayingVideoGamesPart2GameMechanics_2.png)

<div class="content-ad"></div>

그럼, 첫 번째 강력한 머신을 만드는 것으로 들어가 봅시다.

이 머신의 계획은, 우리의 위치를 기반으로 적이 어디에 있는지 감지하기 위해 레이더 모듈을 사용할 것입니다. 그런 다음 우리는 적에게 모든 구성 요소(소총)를 조준하여 사용할 것입니다. 그리고 계속 발포하도록 반복할 것입니다!

나는 코드를 모두 넣어둘 것인데, 한 번 살펴봐 주겠니? 러스트에 완전히 처음이신 분들은 조금 무서울 수 있습니다. 하지만 우리는 모든 것을 살펴보고 기사의 끝에는 더 잘 이해하실 수 있기를 희망합니다!

그래서 여기 커다란 무서운 코드 덤프 입니다:

<div class="content-ad"></div>

```rust
#![allow(unused_must_use)]
use rbot;

// 코드의 진입점입니다. lib.rs 파일에 main 함수가 필요합니다. 사용자 정의 코드와 상호작용하는 게임의 인터페이스로 작동합니다. 새 파일, 모듈을 생성하고 온라인 패키지를 사용하십시오. 코드는 WebAssembly로 컴파일되어야 합니다.
//
// 문서는 다음에서 확인할 수 있습니다: https://docs.rs/rbot/latest/rbot/
pub fn main() {
    loop {
        // 레이더를 사용 가능할 때까지 기다립니다. 쿨다운이 있으므로 항상 호출할 수 없습니다. await_module을 사용하여 준비되었음을 확인합니다.
        rbot::modules::await_module(rbot::modules::Module::Radar);

        // 적 위치를 알아내기 위해 레이더 모듈을 사용합니다.
        let radar_msg = rbot::modules::radar().unwrap();

        // 적의 위치에 대한 각도를 x와 y를 각도로 변환하여 구합니다.
        let angle = rbot::conversions::xy_to_angle(radar_msg.x, radar_msg.y);

        for i in 0..4 {
            // 특정 구성 요소를 적 방향으로 조준합니다.
            rbot::await_aim(i, angle, 0.5);

            // 구성 요소가 사용할 준비가 될 때까지 기다립니다.
            rbot::await_component(i);

            // 특정 구성 요소를 사용합니다.
            rbot::use_component(i, false);
        }
    }
}
```

<img src="https://miro.medium.com/v2/resize:fit:1152/1*hDUUgh1Dy2iWkOXtpkviBQ.gif" />

아아아아!? 무서워!! 하지만 걱정 마세요, 모두 함께 해결해 나갈 거에요. 한 걸음씩.
 
메인: 먼저, 우리의 main 함수를 살펴봅시다.

<div class="content-ad"></div>

```rust
pub fn main() {
  
}
```

이게 뭐지? 우리 프로그램의 진입점이야. 똑똑한 사람이 이렇게 해야 한다고 결정하고, 우리는 단순히 받아들일 뿐이야. 대부분의 프로그래밍 언어에서 비슷한 구조를 찾을 수 있어. 오늘은 여기서 우리 모든 코드를 작성할 거야.

Loop: 다음 코드 블록은 loop야.

```rust
loop {

}
```

<div class="content-ad"></div>

루프는 코드를 여러 번 반복해서 실행하는 데 정말 좋은 도구에요. 계속해서 반복해서 반복해서 반복해서 반복해서 반복해서 반복해서 반복해서… 이렇게 이해하시죠. '...' 안에 있는 내용이 계속해서 반복되죠. 멋지죠! 로봇이 끊임없이 작업을 실행하도록 원할 때 매우 유용한 기능이에요.

로봇 명령어: 이제 로봇 명령어에 대해 이야기해볼게요! 여기서 우리는 로봇이 원하는 일을 하도록 지정할 수 있어요. 이 코드 블록에서 레이다 모듈이 활성화될 준비가 될 때까지 기다리도록 로봇에게 지시하는 거에요. 이걸 안하면 레이다를 호출했을 때 실패하고 대신 에러를 반환할 수도 있어요 (에러에 대해서는 나중에 더 설명할게요).

```js
rbot::modules::await_module(rbot::modules::Module::Radar);
```

로봇 명령어: 이제 레이다를 안전하게 사용하고 로봇으로부터 센서의 읽기 값이 돌아올 수 있어요. 이 메시지에는 적 로봇의 위치에 대한 정보가 포함되어 있어요. 이 정보를 통해 그 적을 조준하고 파괴할 수 있어요! 😈

<div class="content-ad"></div>

```js
let radar_msg = rbot::modules::radar().unwrap();
```

로봇 명령: 이 메시지로부터 적에 대한 x와 y 좌표를 상대적으로 얻습니다. 이 정보를 적이 있는 각도로 변환할 수 있습니다. 각도와 좌표에 대해 아무것도 모른다면 일단 받아들이세요. 우리는 앞으로 수학적인 내용에 더 깊이 들어갈 것입니다! 이것은 로봇공학을 할 때 이해해야 하는 매우 중요한 개념으로, 여기서 배우고자 하는 것입니다!

```js
let angle = rbot::conversions::xy_to_angle(radar_msg.x, radar_msg.y);
```

For-loop: Loop와 비슷하게, for-loop는 코드를 여러 번 반복해서 호출하는 데 사용됩니다. Loop와 for-loop의 차이점은 우리가 블록을 몇 번 실행할지를 지정할 수 있다는 것입니다. 이 경우에는 블록을 4번 실행할 것입니다. 또한 몇 번째 반복 중에 있는지를 나타내는 변수 i를 가지고 있을 것입니다. 이렇게 하면 모든 컴포넌트(조준하고 발사하는 등)에 대해 동일한 절차를 수행할 수 있습니다!

<div class="content-ad"></div>

로봇 명령: 이 명령은 로봇이 지정된 각도를 향하도록 하는 것을 명령합니다. 로봇이 각도를 향할 때까지 코드는 실행을 멈추며, 허용 오차는 0.5도입니다. 한 번 로봇이 각도를 향했을 때, 코드는 계속 실행됩니다. 우리는 조준한 후에만 발포하길 원하기 때문에 정말 좋은 방법이죠! for 루프에서 i에 주목해주세요! 이전에 기사에서 설명한 대로 '0,1,2,3' 구성요소를 사용 중입니다.

```js
rbot::await_aim(i, angle, 0.5);
```

로봇 명령: 이제 거의 적을 공격할 준비가 된 상태입니다. 그러나 무기가 준비된 상태인지 확인해야 합니다. 무기에 총알이 없는 상태에서 쏘려고 한다면 어색할 테니까요? 그래서 이 명령은 로봇에게 코드 실행을 계속하기 전에 구성 요소가 준비될 때까지 기다리라고 지시하는 것입니다!

```js
rbot::await_component(i);
```

<div class="content-ad"></div>

로봇 명령: 발사! 이제 우리는 소총으로 쏠 수 있어요! 우리 총알이 적에 맞아 가는 걸 지켜보고 승리의 달콤함을 누려보세요! 참고로, "false"는 로봇에게 소총을 계속 발사하지 말라고 말해요. 우리는 한 번만 쏘기를 원해요!

```js
rbot::use_component(i, false);
```

준비를 마친 우리 로봇은 이제 투기장에 들어가 다른 로봇과 싸울 준비가 돼 있어요!

친절하게, 로봇을 컴파일하는 것을 기억해 주세요!!! 코드를 업데이트하더라도 아무것도 바뀌지 않는다고 생각하신다면, 아마 컴파일을 잊은 것일 겁니다.

<div class="content-ad"></div>

서버에 코드를 업로드하려면 뒤로 가기 버튼을 누르고 실행 버튼을 눌러주세요! 당신의 로봇은 랭킹에 따라 실력에 맞는 로봇과 대결하게 될 것입니다. 이기면 이기는 만큼 로봇의 랭킹이 올라갑니다.

![로봇 대결](https://miro.medium.com/v2/resize:fit:1152/1*YYOoeiUS00hHvJd9-twJUg.gif)

당신의 로봇이 승리할 수 있기를 바랍니다!

이 기사는 많이 진전되었죠. 제가 알고 있어요! 우리는 일어날 수 있는 최소한의 장벽을 넘어야 했을 뿐입니다. 걱정하지 마세요, 앞으로 우리는 천천히 속도를 줄여 뒤로 한 걸음 물러서며 천천히 전진할 겁니다!

<div class="content-ad"></div>

다음 기사를 기대해주세요! 거기에서는 Rust, 프로그래밍 및 로봇 공학 세계를 더 깊이 파헤쳐볼 예정이에요!

코딩 즐겁게 하세요!

참고: 이 게임은 초기 액세스 중이며 개발 중에 있습니다. 이해하기 어려운 부분이 있다면, https://botbeats.net에서 찾을 수 있는 디스코드 채널로 이동해보세요. 개발자들은 여러분의 모든 질문에 즐겁게 답변해주고 버그를 가능한 한 모두 해결하려 할 거에요!