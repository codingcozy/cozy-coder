---
title: "부트스트랩 List Groups과 Badges 마스터하기"
description: ""
coverImage: "/assets/img/2024-08-18-MasteringBootstrapListGroupsandBadges_0.png"
date: 2024-08-18 10:19
ogImage: 
  url: /assets/img/2024-08-18-MasteringBootstrapListGroupsandBadges_0.png
tag: Tech
originalTitle: "Mastering Bootstrap List Groups and Badges"
link: "https://medium.com/@tomas-svojanovsky/mastering-bootstrap-list-groups-and-badges-bae703aec17e"
isUpdated: false
---


<img src="/assets/img/2024-08-18-MasteringBootstrapListGroupsandBadges_0.png" />

## 간단한 리스트 그룹

ul 목록 안에 list-group 클래스를 추가합니다. 각 li 항목에는 list-group-item 클래스를 추가합니다. 아래에서 항목 주변에 예쁜 테두리가 추가되는 것을 볼 수 있습니다. 활성 상태를 추가하는 것도 가능합니다.

```js
<div class="container p-3">
  <ul class="list-group">
    <li class="list-group-item">Whiskers</li>
    <li class="list-group-item">Mittens</li>
    <li class="list-group-item active">Shadow</li>
    <li class="list-group-item">Simba</li>
    <li class="list-group-item disabled">Luna</li>
  </ul>
</div>
```

<div class="content-ad"></div>


![이미지](/assets/img/2024-08-18-MasteringBootstrapListGroupsandBadges_1.png)

## 링크

이전 예제와 비슷해 보이지만 여기서는 링크를 사용합니다. 예를 들어, 이것은 메뉴에 유용할 수 있습니다. 또한 링크에 비활성 상태를 설정할 수도 있습니다.

```js
<div class="container p-3">
  <div class="list-group">
      <a class="list-group-item" href="#">Whiskers</a>
      <a class="list-group-item" href="#">Mittens</a>
      <a class="list-group-item active" href="#">Shadow</a>
      <a class="list-group-item" href="#">Simba</a>
      <a class="list-group-item disabled" href="#">Luna</a>
  </div>
</div>
```


<div class="content-ad"></div>


![이미지](/assets/img/2024-08-18-MasteringBootstrapListGroupsandBadges_2.png)

## Flush 스타일

리스트 주변에 테두리를 원치 않는 경우, ul 요소에 list-group-flush 클래스를 추가할 수 있습니다.

```js
<div class="container p-3">
  <ul class="list-group list-group-flush">
    <li class="list-group-item">Whiskers</li>
    <li class="list-group-item">Mittens</li>
    <li class="list-group-item active">Shadow</li>
    <li class="list-group-item">Simba</li>
    <li class="list-group-item disabled">Luna</li>
  </ul>
</div>
``` 


<div class="content-ad"></div>


![Image](/assets/img/2024-08-18-MasteringBootstrapListGroupsandBadges_3.png)

## 순서가 매겨진 목록

직관적으로는 번호를 추가하려면 ul에서 ol로 전환하면 되는 줄 알겠지만, 부트스트랩에서는 그게 안 돼요. 번호 매기기에는 list-group-numbered 클래스를 사용해야 해요.

```js
<div class="container p-3">
  <ol class="list-group list-group-numbered">
    <li class="list-group-item">Whiskers</li>
    <li class="list-group-item">Mittens</li>
    <li class="list-group-item active">Shadow</li>
    <li class="list-group-item">Simba</li>
    <li class="list-group-item disabled">Luna</li>
  </ol>
</div>
```


<div class="content-ad"></div>

<img src="/assets/img/2024-08-18-MasteringBootstrapListGroupsandBadges_4.png" />

## 배지

배지는 중요한 정보를 강조하거나 특정 콘텐츠에 관심을 끌기 위해 사용됩니다. 일반적으로 텍스트 옆에 작고 색상이 지정된 레이블로 표시되며 상태, 개수 또는 분류를 나타내는 데 사용됩니다.

배지는 제목, 버튼, 내비게이션 항목 등 다양한 맥락에서 사용할 수 있습니다.

<div class="content-ad"></div>

```js
<div class="container p-3">
  <h1>예시 제목 <span class="badge bg-primary">새로운</span></h1>
  <h2>예시 제목 <span class="badge bg-secondary">새로운</span></h2>
  <h3>예시 제목 <span class="badge bg-success">새로운</span></h3>
  <h4>예시 제목 <span class="badge bg-danger">새로운</span></h4>
  <h5>예시 제목 <span class="badge bg-warning">새로운</span></h5>
  <h6>예시 제목 <span class="badge bg-info">새로운</span></h6>
</div>
```

<img src="/assets/img/2024-08-18-MasteringBootstrapListGroupsandBadges_5.png" />

## 버튼 안에서

버튼 내부에 뱃지를 사용할 수 있습니다. 이는 더 많은 정보나 관련 컨텍스트를 제공하기 위한 시각적인 표시 역할을 합니다.

<div class="content-ad"></div>


<div class="container p-3">
  <button type="button" class="btn btn-primary">
    Notifications <span class="badge text-bg-dark">New</span>
  </button>
</div>


<img src="/assets/img/2024-08-18-MasteringBootstrapListGroupsandBadges_6.png" />

## Badges in List Group

We can put everything together.

<div class="content-ad"></div>

텍스트, 배지 및 구조화 정렬의 조합은 이 디자인을 기능적이고 미학적으로 매력적으로 만들어 많은 웹 애플리케이션에 적합합니다.

다음과 같은 다양한 맥락에서 사용할 수 있습니다:

- 카테고리 목록: 다양한 카테고리와 해당 항목 수를 표시합니다.
- 작업 관리: 우선순위 또는 마감 기한이 있는 작업을 보여줍니다.
- 통지 패널: 다른 카테고리의 읽지 않은 메시지나 경고를 나타냅니다.

```js
<div class="container p-3">
  <ul class="list-group">
    <li
        class="list-group-item d-flex justify-content-between align-items-start"
        >
      <div class="ms-2 me-auto">
        <div class="fw-bold">전자제품</div>
        이 항목에 대한 내용
      </div>
      <span class="badge bg-primary rounded-pill">20</span>
    </li>
    <li
        class="list-group-item d-flex justify-content-between align-items-start"
        >
      <div class="ms-2 me-auto">
        <div class="fw-bold">패션</div>
        이 항목에 대한 내용
      </div>
      <span class="badge bg-danger rounded-pill">10</span>
    </li>
    <li
        class="list-group-item d-flex justify-content-between align-items-start"
        >
      <div class="ms-2 me-auto">
        <div class="fw-bold">스포츠</div>
        이 항목에 대한 내용
      </div>
      <span class="badge bg-success rounded-pill">30</span>
    </li>
  </ul>
</div>
```

<div class="content-ad"></div>


![Image 1](/assets/img/2024-08-18-MasteringBootstrapListGroupsandBadges_7.png)

![Image 2](https://miro.medium.com/v2/resize:fit:400/0*jx0CFpb6Nxc1jYR2.gif)
