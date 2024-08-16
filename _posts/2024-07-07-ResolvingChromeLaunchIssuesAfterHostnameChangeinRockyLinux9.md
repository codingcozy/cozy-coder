---
title: "Rocky Linux 9에서 호스트네임 변경 후 Chrome 실행 문제 해결 방법"
description: ""
coverImage: "/assets/img/2024-07-07-ResolvingChromeLaunchIssuesAfterHostnameChangeinRockyLinux9_0.png"
date: 2024-07-07 22:32
ogImage: 
  url: /assets/img/2024-07-07-ResolvingChromeLaunchIssuesAfterHostnameChangeinRockyLinux9_0.png
tag: Tech
originalTitle: "Resolving Chrome Launch Issues After Hostname Change in Rocky Linux 9"
link: "https://medium.com/@equus3144/resolving-chrome-launch-issues-after-hostname-change-in-rocky-linux-9-3805fd707d3f"
---


이 문서는 Rocky Linux 9 시스템에서 호스트 이름을 수정한 후 Google Chrome을 실행하려고 할 때 발생하는 특정 문제에 대해 다룹니다. 이 문제는 Chrome이 시작하지 못하는 상황으로 나타나며, 기존의 Chrome 프로세스와 충돌이 발생할 수 있다는 오류 메시지와 함께 나타납니다.

![Resolving Chrome Launch Issues After Hostname Change in Rocky Linux 9](/assets/img/2024-07-07-ResolvingChromeLaunchIssuesAfterHostnameChangeinRockyLinux9_0.png)

# 문제 설명

Rocky Linux 9에서 호스트 이름을 변경한 후 Google Chrome을 실행하려고 시도했지만 실패했습니다. `google-chrome`을 사용하여 명령줄에서 Chrome을 시작하려고 하면 다음과 같은 오류 메시지가 표시됩니다:

<div class="content-ad"></div>

```js
[6128:6128:0706/205226.592952:ERROR:process_singleton_posix.cc(353)] 해당 프로필은 다른 컴퓨터(localhost.localdomain)의 다른 Google Chrome 프로세스(4177)에서 사용 중인 것으로 보입니다. Chrome은 프로필을 잠근 상태로 유지하여 손상되지 않도록 합니다. 만약 다른 프로세스가 이 프로필을 사용하고 있지 않다고 확신한다면, 프로필을 잠금 해제하고 Chrome을 다시 시작할 수 있습니다.
[6128:6128:0706/205226.593044:ERROR:message_box_dialog.cc(144)] UI 스레드 메시지 루프 외부에서 대화 상자를 표시할 수 없음: Google Chrome - 해당 프로필은 다른 컴퓨터(localhost.localdomain)의 다른 Google Chrome 프로세스(4177)에서 사용 중인 것으로 보입니다. Chrome은 프로필을 잠근 상태로 유지하여 손상되지 않도록 합니다. 만약 다른 프로세스가 이 프로필을 사용하고 있지 않다고 확신한다면, 프로필을 잠금 해제하고 Chrome을 다시 시작할 수 있습니다.
[6142:6142:0100/000000.602832:ERROR:zygote_linux.cc(673)] write: Broken pipe (32)
```

이 오류는 Chrome이 이미 다른 시스템에서 실행 중인 것으로 간주하여 새 인스턴스를 실행하지 못한다고 합니다.

# 원인 분석

조사 결과, 이 문제는 Chrome의 알려진 버그와 관련이 있다는 것이 확인되었습니다. 해당 버그 보고서는 다음 URL에서 확인할 수 있습니다:  

<div class="content-ad"></div>

- https://issues.chromium.org/issues/41103620
- https://askubuntu.com/questions/476918/google-chrome-wont-start-after-changing-hostname

문제는 호스트 이름 변경으로 인해 발생하는 것으로 보이며, Chrome이 시스템의 식별을 잘못 이해하고 사용자 프로필을 잘못 잠근 것으로 판단됩니다.

# 해결 방법

이 문제를 해결하기 위해 다음 명령을 터미널에서 실행할 수 있습니다:

<div class="content-ad"></div>

```js
rm -rf ~/.config/google-chrome/Singleton*
```

이 명령은 Chrome 구성 디렉터리에서 Singleton 파일을 삭제합니다. 이 파일들은 Chrome의 단일 인스턴스만이 실행되도록 하는 역할을 합니다. 이 파일들을 삭제함으로써 Chrome은 이를 다시 생성하도록 강제되어 실행 중인 인스턴스에 대한 이해를 재설정합니다.

# 결론

Rocky Linux 9 시스템에서 호스트 이름을 변경할 때 발생할 수 있는 구체적인 문제를 설명했습니다. 이로 인해 Google Chrome을 실행할 수 없는 문제가 발생합니다. 해당 문제의 원인을 이해하고 제공된 해결책을 적용함으로써 사용자는 이 문제를 빠르게 해결하고 Chrome 브라우저를 정상적으로 사용할 수 있게 됩니다.

<div class="content-ad"></div>

시스템 구성을 수정한 후 비슷한 문제를 겪는 사용자들은 이 잠재적인 해결책을 고려해 보는 것이 좋습니다. 언제나 시스템과 애플리케이션을 최신 버전으로 업데이트하는 것이 버그 수정과 개선 사항을 활용하는 데 도움이 됩니다.