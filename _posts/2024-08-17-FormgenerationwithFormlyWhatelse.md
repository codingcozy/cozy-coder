---
title: "Formly를 사용해서 폼 쉽게 만드는 방법"
description: ""
coverImage: "/assets/img/2024-08-17-FormgenerationwithFormlyWhatelse_0.png"
date: 2024-08-17 00:54
ogImage: 
  url: /assets/img/2024-08-17-FormgenerationwithFormlyWhatelse_0.png
tag: Tech
originalTitle: "Form generation with Formly What else"
link: "https://medium.com/itnext/form-generation-with-formly-what-else-01f0694f0e86"
isUpdated: true
updatedAt: 1723863651578
---


우리는 양식이 필요해요, 만들어야 해요.

문제를 구글로 찾아보니:

완벽해요!

![이미지](/assets/img/2024-08-17-FormgenerationwithFormlyWhatelse_0.png)

<div class="content-ad"></div>

# 체크리스트

오픈 소스 라이브러리를 도입하기 전에 고려해야 할 항목들에 대한 체크리스트를 작성했습니다. Formly가 어떻게 되는지 살펴보겠습니다.

## 유지보수 되고 있는가?

최근 그들과 작은 문제를 열었고, 네, 유지보수가 되고 있습니다. 몇 일 안에 수정되었습니다. ✅

<div class="content-ad"></div>

## 문제를 해결하나요?

네, Angular 자체에서 동적 형식을 지원하지만, JSON 스키마로부터 동적으로 형식을 생성하고 필드를 필요에 따라 추가하는 것은 많은 수고가 들어갑니다.✅

## 우리 문제를 해결할까요?

만약 우리가 자체 디자인 프레임워크를 갖고 있지 않고 Formly에서 기본적으로 지원하는 프레임워크 중 하나를 사용하며, 일반적인 형식을 동적으로 생성하는 것이 우리에게 흔한 사용 사례라면, 확실히 💯

<div class="content-ad"></div>

프로젝트의 요구 사항을 항상 신중히 고려해야 합니다. 작은 프로젝트의 경우에는 거의 매우 쉬운 결정입니다. 예제를 사용하면 문제없이 이용할 수 있습니다.

큰 프로젝트의 경우에는 이러한 타사 솔루션을 사용하기 위해 많은 지원 코드를 작성해야 한다는 것을 곧 알게 될 것입니다. 데이터 변환기, 사용자 정의 필드, 동일한 작동 방식을 하는데 다른 모습을 가진 필드, 특수 케이스를 위한 Formly 구성 생성기 등이 필요할 수 있습니다.

# 무엇이 더 있을까요?

- Angular은 양식 생성을 지원하므로 고려할 가치가 있습니다.
- 일부 프레임워크는 양식 생성에 대한 변형을 지원합니다. 보다 다양하지만 덜 복잡할 수도 있습니다. 예를 들어, Fundamental은 이와 같은 하나의 프레임워크입니다.
- 다른 것들은 마찬가지로 조금 다른 라이브러리일 수 있습니다.