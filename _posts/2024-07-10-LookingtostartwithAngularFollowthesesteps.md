---
title: "Angular 시작하기 따라야 할 단계들"
description: ""
coverImage: "/assets/img/2024-07-10-LookingtostartwithAngularFollowthesesteps_0.png"
date: 2024-07-10 00:36
ogImage: 
  url: /assets/img/2024-07-10-LookingtostartwithAngularFollowthesesteps_0.png
tag: Tech
originalTitle: "Looking to start with Angular? Follow these steps."
link: "https://medium.com/angular-with-abhinav/looking-to-start-with-angular-follow-these-steps-6ee131781842"
isUpdated: true
---





![2024-07-10-LookingtostartwithAngularFollowthesesteps_0.png](/assets/img/2024-07-10-LookingtostartwithAngularFollowthesesteps_0.png)

앵귤러를 사용한 웹 응용 프로그램 작성에 대한 기사를 진지하게 생각해 왔습니다. 시작하기 위해, 첫 번째 글로 필수 사항을 개괄적으로 소개하고 이후 글에서 다룰 주요 주제를 제공할 계획입니다. 

프런트엔드 개발을 시작할 때, 인터넷이 작동하는 방식, 브라우저가 웹 페이지를 렌더링하는 방법, 클라이언트-서버 통신이 발생하는 방법 등을 이해하는 것이 매우 중요합니다. 이러한 기본 지식을 보유하면 애플리케이션이 어떻게 동작하고 행동하는지 더 잘 이해할 수 있습니다. 또한 여기서 참고할 수 있는 유익한 글이 포함된 이 로드맵을 살펴볼 수도 있습니다.

그럼, 앵귤러 개발에 뛰어들기 전에 배워야 할 필수 주제들을 살펴보도록 하겠습니다.

<div class="content-ad"></div>

## HTML에서 시작하기

HTML은 Hypertext Markup Language의 약자로, 애플리케이션의 기본 구조를 제공합니다. 이 파일에는 페이지에 필요한 모든 요소 - 텍스트, 이미지, 버튼, 폼 등 - 이 다른 곳에 통합되기 전에 들어 있습니다. HTML을 배우기 위한 수많은 기사와 비디오 자습서가 온라인에서 제공됩니다. 하나의 추천 자료는 특히 유용하다고 생각되는 MDN의 HTML 문서입니다.

## CSS로 스타일링하기

HTML 구조를 완성한 후, CSS를 사용하여 시각적으로 매력적인 디자인을 만듭니다. CSS는 텍스트와 배경에 색상을 추가하는 것부터 다양한 화면 크기에 대해 반응하는 HTML 페이지를 보장하는 작업까지 다룹니다. 이 측면은 애플리케이션이 세련되고 매력적으로 보이도록 하는 데 중요합니다. 마찬가지로 MDN의 CSS 문서를 참조하세요.

<div class="content-ad"></div>

## Typescript를 사용하여 반응형으로 만들기

Typescript는 JavaScript의 상위 집합으로, 정적 형식 검사와 컴파일 시간 오류 식별을 특징으로 하며, JavaScript가 런타임에서만 감지할 수 있는 많은 프로그래밍 오류를 잡을 수 있습니다. Typescript는 사용자 입력을 처리하고 작업을 수행하며 클라이언트-서버 통신을 용이하게 하는 방식으로 응용 프로그램에 반응성을 제공합니다. 이에 대해 더 알아보려면 공식 Typescript 문서를 참조해보세요.

기본 개념을 다룬 후, 이제 능숙해지기 위해 집중해야 할 주요 Angular 주제들을 탐구해보겠습니다.

## Angular 개념 학습부터 시작하기

<div class="content-ad"></div>

Angular은 HTML과 TypeScript를 사용하여 단일 페이지 클라이언트 애플리케이션을 구축하기 위해 설계된 플랫폼 및 프레임워크입니다. TypeScript로 작성된 Angular는 핵심 및 선택적 기능을 제공하여 응용 프로그램에 가져올 수 있는 TypeScript 라이브러리 모음을 제공합니다.

Angular 애플리케이션의 기본 구성 요소는 Angular 컴포넌트입니다. 모든 Angular 애플리케이션에는 적어도 하나의 컴포넌트가 있으며, 이 컴포넌트에는 응용 프로그램의 로직이 포함되어 해당 HTML 파일과 연결됩니다.

더 자세히 이해하려면 아래 목록에 나열된 주제들을 공부해보세요. 또한 각 주제를 설명하는 일련의 기사를 게시할 예정이며, 실용적인 사용법을 보여주는 샘플 코드 스니펫으로 설명할 것입니다.

- 컴포넌트
- 템플릿
- 디렉티브
- 데이터 바인딩
- 서비스
- 의존성 주입
- 라우팅

<div class="content-ad"></div>

이러한 주제는 Angular 프레임워크의 개요를 제공하고 소규모 애플리케이션을 구축하는 데 도움이 될 것입니다.

## 컴포넌트 라이프사이클 이해하기

각 컴포넌트 인스턴스는 Angular이 컴포넌트 클래스를 인스턴스화하고 렌더링할 때 시작되는 라이프사이클을 가지고 있습니다. 라이프사이클은 변경 감지로 이어지며 컴포넌트의 뷰와 인스턴스를 필요에 따라 업데이트하는 데 도움이 됩니다.

아래에 나열된 라이프사이클 이벤트를 자세히 살펴보세요.

<div class="content-ad"></div>

- ngOnChanges()
- ngOnInit()
- ngDoCheck()
- ngAfterContentInit()
- ngAfterContentChecked()
- ngAfterViewInit()
- ngAfterViewChecked()
- ngOnDestroy()

## 컴포넌트 상호 작용하는 방법 배우기

Angular에서 컴포넌트간 데이터를 공유해야 할 때가 많습니다. 이를 지원하는 여러 기능이 있습니다:

- Input() 및 Output() 데코레이터: 자식 및 부모 컴포넌트간 통신에 이러한 데코레이터를 사용하세요.
- 모델 Inputs: Angular v18에서 소개된(아직 개발자 미리보기 상태) 모델 Inputs은 다른 컴포넌트로 값 변경을 전파할 수 있습니다.
- 상태 관리 라이브러리: 애플리케이션 전체에 상태를 전파하기 위해 NgRx Store와 같은 상태 관리 라이브러리를 사용할 수 있습니다. 저는 NgRx Store에 대한 경험이 있습니다. 이전에 NgRx Store에 관한 글을 썼는데, 여기서 찾아볼 수 있습니다.

<div class="content-ad"></div>

# RxJs 사용하기

RxJS (JavaScript용 반응형 확장 라이브러리)는 옵저버블을 사용하여 반응형 프로그래밍을 하는 데 사용되는 라이브러리로, 비동기 또는 콜백 기반 코드의 구성을 단순화합니다. 데이터 스트림과 변경 전파에 관심이 있습니다.

이 라이브러리는 또한 옵저버블을 생성하고 작업하는 데 유용한 함수를 제공합니다. 이를 사용하여 다음과 같은 작업을 할 수 있습니다:

- 기존 비동기 작업을 옵저버블로 변환
- 스트림의 값들을 반복
- 값을 다른 형식으로 매핑
- 스트림 필터링
- 여러 스트림을 조합

<div class="content-ad"></div>

RxJS는 주로 서비스에 비동기 호출을 할 때 사용되며, 받은 데이터를 처리하여 뷰나 컴포넌트 인스턴스를 업데이트해야 할 때 주로 활용됩니다.

더 탐구할 개념들이 많지만, 이 기본 사항들은 Angular 개발을 시작하는 데 도움이 될 것입니다. 만약 빠트린 중요한 내용이 있다면, 자유롭게 댓글을 남겨주세요.

이 기사를 읽는 데 즐거웠고 도움이 되었다면, 제게 동기부여가 됩니다. 제가 여러분을 위해 더 많은 유익한 기사를 가져오도록 하기 위해 기사를 팔로우하고 격려해주세요.

또한 LinkedIn에서 저와 연락을 맺고, 다음에 어떤 주제로 글을 써야 할지 제안해주세요.