---
title: " Nodejs TypeScript 지원의 진실"
description: ""
coverImage: "/assets/img/2024-07-30-TheTruthAboutNodejsTypescriptSupport_0.png"
date: 2024-07-30 16:51
ogImage: 
  url: /assets/img/2024-07-30-TheTruthAboutNodejsTypescriptSupport_0.png
tag: Tech
originalTitle: " The Truth About Nodejs Typescript Support"
link: "https://medium.com/@tomaszs2/the-truth-about-node-js-typescript-support-8602f690ee69"
---


<img src="/assets/img/2024-07-30-TheTruthAboutNodejsTypescriptSupport_0.png" />

# 아니요, Typescript를 Node.js와 함께 실행하기 위해 기다릴 필요가 없어요

요즘 프로그래밍 소셜 미디어를 둘러보면 Node.js가 Typescript를 지원한다는 뉴스가 돌고 있습니다:

<img src="/assets/img/2024-07-30-TheTruthAboutNodejsTypescriptSupport_1.png" />

<div class="content-ad"></div>

하지만 이 뉴스는 잘못된 정보이며 가짜 뉴스입니다.

우리가 머지 요청에서 읽을 수 있는 것처럼, 실제로 Node.js에 추가된 것이 무엇인지와 Typescript에 대한 진실도 다릅니다.

따라서, 새 실험적 기능은 사실 Typescript를 직접 사용하는 것이 아닙니다. 실제로 하는 일은 소스 코드에서 Typescript를 제거하는 것입니다.

즉, 플래그를 사용하여 Typescript 파일을 실행할 수 있다는 것을 의미합니다. 그러나 이러한 파일은 타입 체크되지 않고 변환되지 않습니다. 그러나 Typescript 타입은 코드에서 제거되어 마치 그곳에 없었던 것처럼 됩니다.

<div class="content-ad"></div>

백엔드와 프론트엔드 팀이 TypeScript로 작성된 공통 코드를 공유할 때 유용합니다. 그러나 백엔드 팀은 TypeScript를 다루기 싫어해서 순수한 Javascript로 작성합니다.

그래서 TypeScript로 작성된 소스 코드는 그대로 유지되고, 백엔드 팀도 계속 사용할 수 있습니다.

즉, TypeScript를 지원하는 것이 아니라 실행 중에 제거하는 방법입니다.

이제 Node.js로 TypeScript를 실행하는 것은 이미 가능했습니다. 따라서 여기서는 아무 변화가 없습니다. TypeScript 변환기를 사용하여 코드를 JavaScript로 변환하고 Node.js로 실행할 수 있습니다. 이를 수행하는 방법에 대한 안내도 있습니다.

<div class="content-ad"></div>

그러나 대부분의 개발자들은 ts-node 패키지를 사용하여 모든 작업을 처리하므로 Typescript를 수동으로 JavaScript로 컴파일할 필요가 없습니다.

보시다시피, 이미 Node.js에서 Typescript를 실행하는 것이 가능합니다. Node.js의 새로운 기능은 Typescript를 실행하는 것이 아니라 코드에서 Typescript를 제거하고 Javascript로 실행하는 것입니다.

![이미지](/assets/img/2024-07-30-TheTruthAboutNodejsTypescriptSupport_2.png)

그렇다면, 정말 TypeScript에 관심이 많으신가요! 저는 TypeScript 카드 게임 'Summon The JSON: TypeScript'을 디자인했습니다. 우연한 일이네요! 지금 주문할 수 있습니다!

<div class="content-ad"></div>

타입스크립트를 좋아하시나요? 소셜 미디어에서 박수, 구독, 좋아요, 그리고 공유해보세요!

Tom Smykowski를 팔로우하는 14,000명 이상의 개발자들과 함께하세요! 매달 $5로 모든 미디엄 기사에 액세스할 수 있고 Tom은 타입스크립트에 대해 더 많은 글을 쓸 수 있을 거에요! 지금 회원이 되어보세요!