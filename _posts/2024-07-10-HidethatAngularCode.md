---
title: "Angular 코드를 숨기는 7가지 방법"
description: ""
coverImage: "/assets/img/2024-07-10-HidethatAngularCode_0.png"
date: 2024-07-10 00:45
ogImage: 
  url: /assets/img/2024-07-10-HidethatAngularCode_0.png
tag: Tech
originalTitle: "Hide that Angular Code!"
link: "https://medium.com/javascript-in-plain-english/hide-that-angular-code-512f5236d0dc"
isUpdated: true
---





타인이 당신의 코드를 훔치지 못하도록 하고 싶나요? 함께 살펴봐요!

![이미지](/assets/img/2024-07-10-HidethatAngularCode_0.png)

## 개요

프론트엔드 개발자라면 브라우저 인스펙터의 콘솔 탭에서 작성한 모든 코드를 쉽게 볼 수 있다는 것을 아실 겁니다(컴파일된 것과 원시 코드 모두). 이는 코드를 테스트하고 디버깅할 때 필요한 일이지만, 악의적인 사용자에게 이 모든 것을 보여주고 싶지는 않죠! 우리의 코드는 사유적이니까요! 우리 것이니까요! 다른 사람들이 훔치는 것을 어떻게 막을까요?

<div class="content-ad"></div>

이 기사는 코드를 보호해야 하는 이유와 다양한 세부 사항을 모두 설명하는 것이 아니며, 누군가가 코드를 도용하거나 역공학을 하는 방법에 대한 모든 가능성에 대해 다루는 것도 아닙니다. 이 문서는 코드를 보호하는 데 도움이 되는 기본적인 방법을 소개하는 것에 더 중점을 둡니다.

# 용어

## 최소화

최소화는 가능한 한 조밀하게 만들기 위한 화려한 용어입니다. 이를 통해 브라우저로 코드를 더 빨리 로드하기 위해 더 적은 코드를 전송할 수 있을 뿐만 아니라 코드를 읽기 어렵게 만드는 부수적인 효과도 있습니다. 이는 변수 이름을 단일 문자로 변환하거나 코드를 읽기 쉽게 만드는 용도로만 사용되는 공백 및 주석을 제거하는 작업 등을 포함합니다.

<div class="content-ad"></div>

## 숨김처리