---
title: "데이터를 라우트에서 전달하는 방법 Angular 간단 가이드"
description: ""
coverImage: "/issue-truck.github.io/assets/no-image.jpg"
date: 2024-07-07 03:00
ogImage: 
  url: /issue-truck.github.io/assets/no-image.jpg
tag: Tech
originalTitle: "Passing Data in Routes in Angular: A Simple Guide"
link: "https://medium.com/@paul-chesa/passing-data-in-routes-in-angular-a-simple-guide-e24b814e7076"
isUpdated: true
---





<img src="https://miro.medium.com/v2/resize:fit:920/1*oiByvR5dsoSmuPxYidV3sw.gif" />

보물찾기 게임을 준비 중이라고 상상해봐요. 각 참가자가 어디로 가야 하고 무엇을 찾아야 하는지 알 수 있도록 다양한 종류의 지침을 보내야 해요. 비슷하게, Angular에서 앱의 다른 부분(컴포넌트)으로 이동할 때 데이터를 전달해야 할 때가 있어요. 이를 위한 다양한 방법을 알아봐요:

# 1. 라우트 매개변수 사용:

라우트 매개변수는 특정 주소를 알려준다고 생각해보세요. 친구에게 숨겨진 보물을 찾도록 하려면, "123 보물 거리"로 가라고 말하면 되죠.

<div class="content-ad"></div>


![이미지](https://miro.medium.com/v2/resize:fit:808/1*9jWPjDR-FOs-rk7KnqHcGg.gif)

# 사용 방법:

매개변수를 사용하여 경로 정의하기:

```js
const routes: Routes = [
  { path: 'treasure/:id', component: TreasureComponent }
];
```

<div class="content-ad"></div>

매개변수를 사용한 라우트로 이동하기:

```js
this.router.navigate(['/treasure', treasureId]);
```

컴포넌트에서 매개변수에 접근하기:

```js
import { ActivatedRoute } from '@angular/router';

export class TreasureComponent implements OnInit {
  treasureId: string;

  constructor(private route: ActivatedRoute) { }

  ngOnInit(): void {
    this.treasureId = this.route.snapshot.paramMap.get('id');
  }
}
```

<div class="content-ad"></div>

라우트 매개변수는 매우 성능이 우수합니다. URL의 일부로 직접 제공되기 때문에 Angular의 라우터가 URL을 효율적으로 파싱하고 매개변수를 추출할 수 있습니다. 이 프로세스는 간단하며 추가 오버헤드가 거의 없기 때문에 라우트 매개변수는 라우트에서 데이터를 전달하는 가장 성능이 우수한 방법입니다.

# 2. 쿼리 파라미터 사용하기:

쿼리 파라미터는 주소에 추가 설명을 추가하는 것과 같습니다. 친구에게 "123 Treasure Street?clue=map"로 가라고 말한다면, 그들은 지도를 가져와야 한다는 의미입니다.

![이미지](https://miro.medium.com/v2/resize:fit:1276/1*GLm2vLF8qNspa2x0sxrT1Q.gif)


<div class="content-ad"></div>

# 사용 방법:

경로 정의하기:

```js
const routes: Routes = [
  { path: 'treasure', component: TreasureComponent }
];
```

```js
this.router.navigate(['/treasure'], { queryParams: { clue: 'map' } });
```

<div class="content-ad"></div>

컴포넌트에서 쿼리 매개변수에 액세스하기:

```js
import { ActivatedRoute } from '@angular/router';

export class TreasureComponent implements OnInit {
  clue: string;

  constructor(private route: ActivatedRoute) { }

  ngOnInit(): void {
    this.route.queryParams.subscribe(params => {
      this.clue = params['clue'];
    });
  }
}
```

쿼리 매개변수는 라우팅 매개변수와 유사하게 높은 성능을 제공합니다. URL의 일부이며 Angular 라우터에서 직접 구문 분석됩니다. 쿼리 문자열을 추가로 구문 분석해야 하므로 라우팅 매개변수보다 약간 오버헤드가 발생하지만 매우 효율적입니다.

# 3. 라우트 데이터 사용하기:

<div class="content-ad"></div>

경로 데이터는 닫힌 봉투 안에 안내 내용이 들어있는 친구에게 비유할 수 있어요. 그들은 해당 위치에서 봉투를 열 때까지 내용을 알 수 없어요.

![이미지](https://miro.medium.com/v2/resize:fit:996/1*CLnQurwuTxos53iz66Y7yw.gif)

## 사용 방법:

데이터와 함께 경로를 정의하세요:

<div class="content-ad"></div>

```js
const routes: Routes = [
  { path: 'treasure', component: TreasureComponent, data: { clue: 'map' } }
];
```

컴포넌트에서 데이터에 접근하기:

```js
import { ActivatedRoute } from '@angular/router';

export class TreasureComponent implements OnInit {
  clue: string;

  constructor(private route: ActivatedRoute) { }

  ngOnInit(): void {
    this.route.data.subscribe(data => {
      this.clue = data['clue'];
    });
  }
}
```

쿼리 매개변수 또한 높은 성능을 제공하며, 라우트 매개변수와 유사합니다. URL의 일부이며 Angular의 라우터에서 직접 구문 분석됩니다. 쿼리 문자열을 추가 구문 분석해야 하기 때문에 오버헤드는 라우트 매개변수보다 약간 높지만 아주 효율적입니다.

<div class="content-ad"></div>

보물 찾기처럼, Angular 라우트에서 데이터를 전달하는 방법은 당신의 필요에 따라 다양하게 할 수 있어요:

- 라우트 매개변수: 구체적인 주소.
- 쿼리 매개변수: 부가 설명이 있는 주소.
- 라우트 데이터: 안내가 담긴 봉투.

![이미지](https://miro.medium.com/v2/resize:fit:716/1*X0gMujOmExf74AQcVu9wHQ.gif)