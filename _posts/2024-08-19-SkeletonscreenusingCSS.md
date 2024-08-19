---
title: "CSS로 스켈레톤 화면 만드는 방법"
description: ""
coverImage: "/assets/img/2024-08-19-SkeletonscreenusingCSS_0.png"
date: 2024-08-19 01:46
ogImage: 
  url: /assets/img/2024-08-19-SkeletonscreenusingCSS_0.png
tag: Tech
originalTitle: "Skeleton screen using CSS"
link: "https://medium.com/@frontendinterviewquestions/skeleton-screen-using-css-2eb424ca4702"
isUpdated: true
updatedAt: 1724034762791
---


<img src="/assets/img/2024-08-19-SkeletonscreenusingCSS_0.png" />

더 많은 질문과 답변을 보려면 Frontend Interview Questions 웹 사이트를 방문해주세요.

# 스켈레톤 화면이란 무엇인가요?

스켈레톤 화면은 웹 페이지에 로드될 내용의 구조를 시뮬레이트하는 임시 레이아웃입니다. 일반적으로 텍스트, 이미지, 버튼 등의 요소가 나타날 위치를 나타내는 회색 또는 연한 색상의 블록이 특징입니다. 스켈레톤 화면의 사용은 사용자가 인식하는 로딩 시간을 줄이고 전반적인 사용자 경험을 향상시킴으로써 사용자 참여를 유지하는 데 도움이 됩니다.

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

# 왜 스켈레톤 화면을 사용해야 하나요?

- 성능 인식 향상: 스켈레톤 화면을 통해 사용자에게 미리 보여주어 로딩 과정이 빠르게 느껴질 수 있습니다.
- 당혹감 감소: 시각적인 구조를 제공하여 사용자가 콘텐츠가 로딩될 때 혼란스럽거나 좌절하는 경험을 줄일 수 있습니다.
- 향상된 사용자 경험: 스켈레톤 화면은 일관된 느낌과 느낌을 유지하는 데 도움이 되어 로딩에서 콘텐츠 표시로의 전환을 더 부드럽게 만들어줍니다.

# 스켈레톤 화면 생성을 위한 기본 CSS

스켈레톤 화면 생성에는 CSS를 사용하여 콘텐츠 레이아웃을 모방하는 플레이스홀더 요소에 스타일을 지정하는 것이 포함됩니다. 중요한 것은 애니메이션을 사용하여 스켈레톤 화면이 로딩되는 것처럼 보이도록하는 것입니다.

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

## 기본 스켈레톤 화면 예제

간단한 사용자 프로필 카드용 스켈레톤 화면을 만들어 보겠습니다. 이 카드에는 프로필 사진, 사용자 이름 및 간단한 설명이 포함될 것입니다.

HTML 구조:

```js
<!DOCTYPE html>
<html lang="en">
<head>
    <meta charset="UTF-8">
    <meta name="viewport" content="width=device-width, initial-scale=1.0">
    <title>스켈레톤 화면 예제</title>
    <link rel="stylesheet" href="styles.css">
</head>
<body>
    <div class="skeleton-profile-card">
        <div class="skeleton-avatar"></div>
        <div class="skeleton-text name"></div>
        <div class="skeleton-text description"></div>
    </div>
</body>
</html>
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

CSS 스타일링:

```js
/* 스켈레톤 화면을 위한 기본 스타일 */
.skeleton-profile-card {
    display: flex;
    align-items: center;
    width: 300px;
    padding: 20px;
    border: 1px solid #ddd;
    border-radius: 8px;
    background-color: #f9f9f9;
}
.skeleton-avatar {
    width: 60px;
    height: 60px;
    border-radius: 50%;
    background-color: #e0e0e0;
    margin-right: 20px;
    animation: pulse 1.5s infinite ease-in-out;
}
.skeleton-text {
    background-color: #e0e0e0;
    border-radius: 4px;
    margin-bottom: 10px;
}
.skeleton-text.name {
    width: 150px;
    height: 20px;
    animation: pulse 1.5s infinite ease-in-out;
}
.skeleton-text.description {
    width: 200px;
    height: 14px;
    animation: pulse 1.5s infinite ease-in-out;
}
/* 반짝임 효과를 위한 애니메이션 */
@keyframes pulse {
    0% {
        background-color: #e0e0e0;
    }
    50% {
        background-color: #f5f5f5;
    }
    100% {
        background-color: #e0e0e0;
    }
}
```

# CSS 설명

- 스켈레톤 컨테이너 (skeleton-profile-card): 스켈레톤 화면을 위한 컨테이너의 스타일을 지정합니다. 항목을 정렬하기 위해 flexbox를 사용하고 배경색과 테두리를 설정합니다.
- 아바타 (skeleton-avatar): 프로필 사진 영역을 나타냅니다. 원형 모양이며 배경색과 로딩을 시뮬레이션하기 위한 애니메이션을 가지고 있습니다.
- 텍스트 (skeleton-text): 이름과 설명을 위한 텍스트 자리 표시자를 나타냅니다. 기대되는 내용과 일치하도록 배경색과 크기를 지정합니다.
- Pulse 애니메이션: Pulse 애니메이션은 빛나는 효과를 생성하여 콘텐츠 로딩을 시각적으로 강화합니다. 배경색이 두 가지 쉐이드로 전환하여 활동을 시뮬레이트합니다.

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

# 고급 스켈레톤 화면 예제

제품 목록이나 글 미리보기와 같이 복잡한 레이아웃의 경우, 추가적인 CSS 속성을 사용하여 콘텐츠의 구조와 일치하는 스켈레톤 화면을 만들 수 있습니다.

HTML 구조:

```js
<div class="skeleton-product-list">
    <div class="skeleton-product">
        <div class="skeleton-image"></div>
        <div class="skeleton-text title"></div>
        <div class="skeleton-text price"></div>
    </div>
    <!-- 추가 제품에 대해 반복합니다 -->
</div>
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

CSS 스타일링:

```js
.skeleton-product-list {
    display: flex;
    flex-direction: column;
    gap: 20px;
}
.skeleton-product {
    display: flex;
    align-items: center;
    gap: 15px;
    padding: 15px;
    border: 1px solid #ddd;
    border-radius: 8px;
    background-color: #f9f9f9;
}
.skeleton-image {
    width: 80px;
    height: 80px;
    background-color: #e0e0e0;
    border-radius: 4px;
    animation: pulse 1.5s infinite ease-in-out;
}
.skeleton-text.title {
    width: 120px;
    height: 18px;
    background-color: #e0e0e0;
    border-radius: 4px;
    animation: pulse 1.5s infinite ease-in-out;
}
.skeleton-text.price {
    width: 60px;
    height: 14px;
    background-color: #e0e0e0;
    border-radius: 4px;
    animation: pulse 1.5s infinite ease-in-out;
}
```

# 요약

스켈레톤 화면은 콘텐츠가 로딩되는 동안 시각적 구조를 제공하여 사용자 경험을 향상시킵니다. 이러한 자리 표시자를 만들기 위해 CSS를 활용하면, 사용자들에게 콘텐츠 로딩 시 부드럽고 매력적인 경험을 제공할 수 있습니다. 제공된 예시는 기본적인 및 고급 사용 사례를 보여주며, 다양한 요소와 레이아웃에 스켈레톤 화면을 적용하는 방법을 설명하고 있습니다.

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

면책 조항: 일부 제휴 링크가 포함되어 있습니다. 별도 비용은 없습니다. 지원해 주셔서 감사합니다!

여기서 구입하세요: - Angular에 대한 실용적이고 포괄적인 안내서