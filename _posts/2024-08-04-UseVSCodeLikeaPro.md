---
title: "VS Code를 더 잘 사용하는 방법"
description: ""
coverImage: "/assets/img/2024-08-04-UseVSCodeLikeaPro_0.png"
date: 2024-08-04 18:37
ogImage: 
  url: /assets/img/2024-08-04-UseVSCodeLikeaPro_0.png
tag: Tech
originalTitle: "Use VS Code Like a Pro"
link: "https://medium.com/javascript-in-plain-english/use-vs-code-like-a-pro-53973daa534f"
isUpdated: true
---





<img src="/assets/img/2024-08-04-UseVSCodeLikeaPro_0.png" />

VS Code은 강력하지만 적절한 조정으로 더욱 효과적으로 사용할 수 있어요. 코딩 역량을 높이기 위한 빠른 팁을 확인해보세요:

## 1. 파일 트리 가시성 향상

파일 트리를 더 명확하고 조직적으로 만들어보세요! 구성 파일에 다음 설정을 추가하세요:

<div class="content-ad"></div>

- VS Code을 열어주세요.
- 명령 팔레트를 열어주세요:
  
  - Cmd + Shift + P를 눌러주세요.

3. "Preferences: Open Settings (JSON)"을 입력하고 선택해주세요:
  
  - 명령 팔레트에 settings.json을 입력하고 Preferences: Open Settings (JSON)을 선택해주세요.

<div class="content-ad"></div>


4. settings.json에 다음 라인을 추가하세요:

```js
{
  "workbench.tree.indent": 15,
  "workbench.tree.renderIndentGuides": "always",
  "workbench.colorCustomizations": {
    "tree.indentGuidesStroke": "#05ef3c"
  }
}
```

## 2. 폰트 업그레이드: Fira Code

Fira Code로 변경하세요! 이 폰트는 프로그래밍 리거처들을 갖추고 있어 코드를 더 읽기 쉽고 미적으로 감각적으로 만들어줍니다. 설치하는 방법은 다음과 같습니다:


<div class="content-ad"></div>

- Fira Code 다운로드:

- Fira Code GitHub 페이지로 이동하여 최신 릴리스를 다운로드하세요.

2. 폰트 설치:

- 맥에서: 다운로드한 .ttf 파일을 열고 "폰트 설치"를 클릭합니다.
- 윈도우에서: 다운로드한 .ttf 파일에 마우스 오른쪽 버튼을 클릭하고 "설치"를 선택합니다.

<div class="content-ad"></div>

3. VS Code 설정하기:

- 위에서 설명한 대로 설정.json 파일을 열어주세요.
- 다음 라인을 추가하거나 업데이트해주세요:

```json
{
  "editor.fontFamily": "Fira Code",
  "editor.fontLigatures": true
}
```

<img src="/assets/img/2024-08-04-UseVSCodeLikeaPro_1.png" />

<div class="content-ad"></div>

## 3. 새로운 테마 탐색하기

이 멋진 테마들로 VS Code를 개인화하세요. 확장 보기에서 이 테마들을 검색하기만 하면 됩니다 (Ctrl + Shift + X 또는 Cmd + Shift + X):

- One Dark Pro: 어두운 모드로 눈에 부담 없이 사용할 수 있어요.

![One Dark Pro](/assets/img/2024-08-04-UseVSCodeLikeaPro_2.png)

<div class="content-ad"></div>

- 드라큘라 공식: 선명한 색상과 어두운 배경.

![](/assets/img/2024-08-04-UseVSCodeLikeaPro_3.png)

- GitHub 테마: GitHub 팬들을 위한 완벽한 테마.

![](/assets/img/2024-08-04-UseVSCodeLikeaPro_4.png)

<div class="content-ad"></div>

## 4. 아이콘 강화: Material Icon Theme

마테리얼 아이콘 테마로 아이콘을 더욱 돋보이게 만들어보세요. 확장 보기에서 "Material Icon Theme"을 검색하여 설치하면 모든 파일 및 폴더에 대해 명확하고 인식하기 쉬운 아이콘을 사용할 수 있습니다.

이 팁들을 시도하고 VS Code를 프로 수준의 코딩 환경으로 변신해보세요. 생산성 향상이 조금의 설정만으로 가능합니다!

# 간단하게 설명하기 🚀

<div class="content-ad"></div>

"In Plain English" 커뮤니티에 참여해 주셔서 감사합니다! 떠나시기 전에:

- 작가를 박수 치고 팔로우해주세요 👏️️
- 팔로우하기: X | LinkedIn | YouTube | Discord | 뉴스레터
- 다른 플랫폼 방문하기: CoFeed | Differ
- PlainEnglish.io에서 더 많은 콘텐츠 확인하기