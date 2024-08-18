---
title: "Windows 터미널을 더욱 향상시켜보세요 Oh My Posh와 함께하는 스텝바이스텝 설치 가이드 VSCode 설정 포함"
description: ""
coverImage: "/assets/img/2024-06-20-ImproveYourWindowsTerminalwithOhMyPoshAStep-by-StepInstallationGuidevscodesetupincluded_0.png"
date: 2024-06-20 14:51
ogImage:
  url: /assets/img/2024-06-20-ImproveYourWindowsTerminalwithOhMyPoshAStep-by-StepInstallationGuidevscodesetupincluded_0.png
tag: Tech
originalTitle: "Improve Your Windows Terminal with Oh My Posh: A Step-by-Step Installation Guide(vscode setup included)"
link: "https://medium.com/@pooya-ghorbani-hafez/improve-your-windows-terminal-with-oh-my-posh-a-step-by-s-ep-installation-guide-vscode-setup-92252a279477"
isUpdated: true
---

<img src="/assets/img/2024-06-20-ImproveYourWindowsTerminalwithOhMyPoshAStep-by-StepInstallationGuidevscodesetupincluded_0.png" />

터미널에서 상당한 시간을 보내는 경우, 효율적이고 미려한 명령줄 인터페이스의 가치를 아실 것입니다. Oh My Posh는 터미널 프롬프트를 시각적으로 멋지고 매우 기능적인 작업 공간으로 변환해주는 강력한 도구입니다. 개발자, 시스템 관리자 또는 터미널 애호가이든, Oh My Posh를 통해 터미널 경험을 다음 수준으로 끌어올릴 수 있습니다.

## Oh My Posh란?

Oh My Posh는 프롬프트 문자열을 조정할 수 있는 모든 셸에 대한 사용자 정의 프롬프트 엔진입니다.

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

## Oh My Posh를 사용해야 하는 이유

- 사용자 정의: 다양한 테마 중에서 선택하거나 작업 흐름과 미적 취향에 맞게 자신만의 테마를 만들 수 있습니다.
- 생산성: 추가 명령어를 입력하지 않고도 필수 정보에 빠르게 액세스할 수 있습니다.
- 크로스 플랫폼: Windows, macOS 및 Linux에서 원활하게 작동합니다.
- 커뮤니티 지원: 다양한 테마와 기능을 기여하는 활기찬 커뮤니티가 있습니다.

## 설치 가이드

터미널을 개선하고 싶나요? Oh My Posh를 설치하기 위해 이 간단한 단계를 따르세요:

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

**단계 1:** PowerShell 및 Microsoft 터미널을 설치합니다 (이미 설치되어 있지 않은 경우).

**단계 2:** Oh My Posh 설치:

Oh My Posh를 설치하려면 Microsoft Store에서 설치하거나 PowerShell을 열고 다음 명령을 실행합니다:

```js
winget install JanDeDobbeleer.OhMyPosh -s winget
```

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

단계 3: 터미널 설정 구성

설치 후 Oh My Posh를 사용하도록 터미널을 구성해야 합니다.

- 네르드 폰트 설치: https://www.nerdfonts.com/
- 터미널/편집기를 설치된 폰트를 사용하도록 구성하십시오:
- 터미널에서 설정으로 이동하여 기본 섹션에서 font-face를 찾은 다음 설치한 네르드 폰트 중 하나로 설정하십시오:

![이미지](/assets/img/2024-06-20-ImproveYourWindowsTerminalwithOhMyPoshAStep-by-StepInstallationGuidevscodesetupincluded_1.png)

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

- VS Code를 사용하려면 settings.json 파일로 이동해서 다음 코드를 추가하세요:

```js
"terminal.integrated.fontFamily": "Hack Nerd Font", // Hack Nerd Font은 내가 설치한 글꼴의 이름입니다
```

- 쉘에서 Oh My Posh를 사용하도록 구성하세요:
- PowerShell 프로필 스크립트를 편집하세요

```js
notepad $PROFILE
```

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

위 명령어에서 오류가 발생하면 먼저 프로필을 만들어야 합니다:

```js
New-Item -Path $PROFILE -Type File -Force
```

그런 다음 다음 줄을 추가하십시오:

```js
oh-my-posh init pwsh | Invoke-Expression
```

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

한 번 추가하면 변경 사항이 적용되려면 프로필을 다시로드하세요:

```js
. $PROFILE
```

위의 명령어를 입력한 후 이와 같은 오류가 발생하면:

![이미지](/assets/img/2024-06-20-ImproveYourWindowsTerminalwithOhMyPoshAStep-by-StepInstallationGuidevscodesetupincluded_2.png)

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

새로운 PowerShell 세션을 관리자 권한으로 열어서 다음 코드를 추가해야 해요(자세한 내용은 이 링크에서 확인할 수 있어요):

```shell
Set-ExecutionPolicy -ExecutionPolicy RemoteSigned -Scope CurrentUser
```

그런 다음 실행하세요:

```shell
. $PROFILE
```

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

만약 이와 같은 오류가 vscode에서 발생한다면:

![Error](/assets/img/2024-06-20-ImproveYourWindowsTerminalwithOhMyPoshAStep-by-StepInstallationGuidevscodesetupincluded_3.png)

터미널을 관리자 모드로 실행하여 다음 명령어를 실행해주세요:

```js
Install-Module PsReadLine -Force
```

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

만약 vscode에서 이런 식으로 나쁜 인코딩이 발생한다면:

<img src="/assets/img/2024-06-20-ImproveYourWindowsTerminalwithOhMyPoshAStep-by-StepInstallationGuidevscodesetupincluded_4.png" />

다음 코드를 vscode 설정 파일인 setting.json에 추가해보세요:

```js
"terminal.integrated.profiles.windows": {
    "PowerShell": {
        "shell_integration": true,
            "source": "PowerShell",
            "icon": "terminal-powershell",
            "args": [
                "-NoExit",
                "-Command",
                "oh-my-posh init pwsh | Invoke-Expression"
            ]
        }
  }
```

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

**단계 4: 테마 선택**

여기에서 테마를 찾아 설정할 수 있어요. 이것은 oh-my-posh 문서입니다.

저는 이 테마를 사용하고 이렇게 보여요:

![이미지](/assets/img/2024-06-20-ImproveYourWindowsTerminalwithOhMyPoshAStep-by-StepInstallationGuidevscodesetupincluded_5.png)

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

이 튜토리얼을 즐기시길 바라며, 만약 오류를 만나게 된다면 도움이 되어 문제를 해결하는 데 도움이 되기를 바랍니다 😍.
