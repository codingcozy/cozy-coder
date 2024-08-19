---
title: "Angular 18에서 NgRx 상태 관리하는 방법"
description: ""
coverImage: "/assets/img/2024-08-19-NavigatingAngular18NgRxStateManagementDemystified_0.png"
date: 2024-08-19 02:23
ogImage: 
  url: /assets/img/2024-08-19-NavigatingAngular18NgRxStateManagementDemystified_0.png
tag: Tech
originalTitle: "Navigating Angular 18 NgRx State Management Demystified"
link: "https://medium.com/@trippytom28/navigating-angular-18-ngrx-state-management-demystified-e61e5ac38274"
isUpdated: true
updatedAt: 1724034707743
---



![Navigating Angular NgRx State Management Demystified](/assets/img/2024-08-19-NavigatingAngular18NgRxStateManagementDemystified_0.png)

애플리케이션에서 효과적인 상태 관리는 일관성과 예측 가능성을 유지하는 데 필수적입니다. 애플리케이션이 확장되면 여러 컴포넌트 간의 데이터 처리가 점점 복잡해집니다. NgRx와 같은 상태 관리 라이브러리는 애플리케이션 상태를 중앙 집중화하고 조직화함으로써 중요한 역할을 합니다. 이를 통해 데이터 흐름을 관리하고 부작용을 처리하며 원활한 상태 전환을 촉진합니다. 개발자가 진실의 단일 출처를 준수함으로써 버그를 방지하고 유지 보수 가능성을 향상시켜 더 나은 사용자 경험을 만들 수 있습니다.

# NgRx 이해하기

NgRx는 Angular 애플리케이션에서 상태 관리와 부작용 처리를 위해 설계된 라이브러리 세트입니다. React의 잘 알려진 상태 관리 도구인 Redux에서 영감을 받았으며 Angular의 반응형 프로그래밍 모델에 비슷한 방법론을 적용합니다.


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

여기에는 주요 구성 요소에 대한 세부 정보가 나와 있어요:

- 스토어: 스토어는 애플리케이션 전체 상태를 보유하는 곳이에요. 단일하고 변경할 수 없는 상태 트리입니다. 상태 변경은 액션과 리듀서를 통해 이뤄져요.
- 액션: 액션은 애플리케이션에서 스토어로 정보를 전달하는 데이터 페이로드로, 이벤트나 사용자 상호작용을 나타냅니다.
- 리듀서: 리듀서는 상태가 액션에 응답하여 어떻게 변경되는지를 결정하는 함수들이에요. 현재 상태와 액션을 받아 새로운 상태를 반환합니다.
- 셀렉터: 셀렉터는 스토어에서 특정 상태 조각을 추출하는 데 사용되는 함수들이에요. 특정 상태 일부를 검색하고 결과를 캐싱하여 성능을 최적화하는 데 도움을 줍니다.
- 이펙트: 이펙트는 비동기 작업(예: HTTP 요청)과 같은 부작용을 처리해요. 액션을 감시하고 작업을 수행하며 결과에 따라 새로운 액션을 발행합니다.
- 엔티티: NgRx 엔티티는 스토어에서 엔티티 컬렉션(예: 객체 배열)을 관리하는 것을 간소화하는 유틸리티 라이브러리로, CRUD 작업과 데이터 정규화를 지원합니다.

NgRx는 RxJS를 활용하여 상태 관리에 대한 반응적인 접근을 제공해요. 개발자들이 비동기 데이터와 상태 업데이트를 효율적으로 처리하기 위해 Observables을 사용할 수 있도록 도와줘요. 이 프레임워크는 Angular 애플리케이션 내 복잡한 상태 상호작용과 부작용을 처리하며 유지 보수 및 명확성을 향상시켜 줍니다.

![사진](/assets/img/2024-08-19-NavigatingAngular18NgRxStateManagementDemystified_1.png)

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

# NgRx 구현하기

간단히 말해서, 저는 사용자가 하드코딩된 영화에 평점을 매길 수 있는 영화 리뷰 시스템을 구현했고, 각 영화의 평점 상태는 NgRx Store를 이용해 관리하고 있습니다. 제 GitHub 저장소를 참고해주세요.

## NgRx 설치하기

프로젝트 폴더에서 다음 명령어를 실행하세요.

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

```js
npm i @ngrx/store @ngrx/store-devtools
```

## NgRx 설정

위 명령을 실행한 후, 세 개의 파일 movie.action.ts, movie.selector.ts, movie.reducer.ts를 포함하는 store 폴더를 정의해야 합니다. 저는 상태를 컴포넌트별로 관리하고 있지만, 전역적으로 폴더와 그 내용을 정의할 수도 있습니다.

<img src="/assets/img/2024-08-19-NavigatingAngular18NgRxStateManagementDemystified_2.png" />


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

## 영화 모델

```js
export interface Movie {
  id: number;
  name: string;
  imageUrl: string;
  rating: any;
  length: string;
  lang: string;
}
```

## 작업 생성

이 파일에서는 작업의 모든 동작을 나타내는 함수를 정의하여 작업 중인 데이터의 상태를 변경하는 데 책임이 있습니다. 다음 코드를 고려해보세요:

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

```js
import { createAction, props } from '@ngrx/store';

// 영화 평가 액션
export const rateMovies = createAction(
  '[Movie] Rate Movies',
  props<{ id: number; rating: number }>()
);
```

@ngrx/store 모듈의 createAction 함수는 액션 설명을 첫 번째 매개변수로 사용하고 처리할 데이터를 두 번째 매개변수로 사용한다는 것을 알 수 있습니다. "영화 상태"에는 영화 ID에 따라 영화를 평가하는 액션이 있을 것입니다.

## Reducers 생성

이 파일에서는 영화 목록의 초기 상태를 정의하고 각 액션을 처리하기 위한 로직을 구현한 후 상태를 수정합니다. 자세한 내용은 다음 코드를 참조해주세요:


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

```js
import { createReducer, on } from '@ngrx/store';
import { rateMovies } from './movie.action';
import { Movie } from '../components/Models/Movie.model';

// 영화 상태
export interface MovieState {
  movies: Movie[];
}

// 초기 상태
export const initialState = {
  movies: [
    {
      id: 1,
      name: '배트맨',
      imageUrl:
        'https://i.pinimg.com/originals/d7/25/a9/d725a9b969c231f6964fb431156f4c87.jpg',
      rating: [],
      lang: '영어',
      length: '2시간 32분',
    },
    {
      id: 2,
      name: '데드풀',
      imageUrl:
        'https://data1.ibtimes.co.in/en/full/598988/deadpool-one-most-successful-superhero-movies-all-time.jpg',
      rating: [],
      lang: '영어',
      length: '3시간 1분',
    },
    {
      id: 3,
      name: '스카페이스',
      imageUrl:
        'https://posterspy.com/wp-content/uploads/2019/11/scarface_preview.jpg',
      rating: [],
      lang: '영어',
      length: '2시간 10분',
    },
    {
      id: 4,
      name: '대부',
      imageUrl:
        'https://m.media-amazon.com/images/M/MV5BM2MyNjYxNmUtYTAwNi00MTYxLWJmNWYtYzZlODY3ZTk3OTFlXkEyXkFqcGdeQXVyNzkwMjQ5NzM@._V1_.jpg',
      rating: [],
      lang: '영어',
      length: '3시간 10분',
    },
    {
      id: 5,
      name: '덤 앤 더머',
      imageUrl:
        'https://image.tmdb.org/t/p/original/A5HCmQrRbj2FECfjNldoKyX1Qbg.jpg',
      rating: [],
      lang: '영어',
      length: '2시간',
    },
  ],
};

// 영화 리듀서
export const movieReducer = createReducer(
  initialState,
  on(rateMovies, (state: MovieState, { id, rating }) => {
    // id에 따라 영화의 평점을 업데이트
    const updatedMovies = state.movies.map((movie) => {
      if (movie.id === id) {
        return { ...movie, rating: [...movie.rating, rating] };
      } else {
        return movie;
      }
    });
    return { ...state, movies: updatedMovies };
  })
);
```

이 코드는 NgRx를 사용하여 영화 상태를 관리하는 리듀서 함수를 정의합니다. createReducer로 생성된 movieReducer 함수는 영화에 대한 평가가 0인 영화 목록을 포함한 초기 상태로 시작합니다. rateMovies 액션이 디스패치되면, 리듀서는 해당 id를 가진 영화의 평점 배열을 새로운 평점을 추가하여 업데이트합니다. 그런 다음 업데이트된 영화 목록이 새로운 상태로 반환됩니다.

## 셀렉터 생성

이제 스토리지에서 데이터를 검색하려면 셀렉터를 만들어야 합니다. 다음 코드를 참조하십시오:


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

```js
import { createSelector } from '@ngrx/store';

// 영화 선택
export const selectMovies = createSelector(
  (state: any) => state.movies,
  (movies) => movies.movies
);
```

해당 코드는 createSelector를 사용하여 selectMovies라는 셀렉터를 정의하며, 애플리케이션 전체 상태에서 영화 데이터를 추출합니다. 이후 이 데이터 슬라이스에서 movies 속성을 검색하여 실제 영화 목록 또는 데이터에 액세스할 수 있게 합니다.

app.config.ts에 provideStore에 reducer를 추가하는 것을 잊지 마세요.

<img src="/assets/img/2024-08-19-NavigatingAngular18NgRxStateManagementDemystified_3.png" />

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

# 우리의 논리를 사용하기

데이터에 대한 변경 사항을 듣고 UI에 업데이트해야 합니다.

## 셀렉터 관찰하기

기본적으로 UI에서 데이터를 가져오고 렌더링하기 위해 observables와 async 파이프를 사용하고 있습니다. angular/core에서 inject를 사용하고 ngrx/store에서 Store를 ProviderToken으로 전달할 것입니다.

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

<img src="/assets/img/2024-08-19-NavigatingAngular18NgRxStateManagementDemystified_4.png" />

## 평점 업데이트

이 MoviesService 클래스는 싱글톤 서비스로 제공되며, Angular의 의존성 주입을 사용하여 NgRx 저장소에 액세스합니다. rateMovieById라는 메서드를 사용하여 지정된 id와 평점을 가진 영화의 평점을 업데이트하는 rateMovies 액션을 저장소에 전송합니다.

<img src="/assets/img/2024-08-19-NavigatingAngular18NgRxStateManagementDemystified_5.png" />

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

## UI에 데이터 렌더링하기

템플릿에서 프로퍼티를 사용하고 TypeScript에서 Observable로 도착하는 데이터를 관리하기 위해 async 파이프를 함께 사용하고 있음을 유의해 주세요.

![image](/assets/img/2024-08-19-NavigatingAngular18NgRxStateManagementDemystified_6.png)

# 결론

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

NgRx 상태 관리는 구조화되고 예측 가능한 방식으로 복잡한 응용 프로그램 상태를 처리하는 데 사용됩니다. 단일하고 변경할 수 없는 상태 트리를 활용하고 액션, 리듀서 및 셀렉터를 사용하여 NgRx는 상태 업데이트와 검색을 간소화합니다. 비동기 작업에 대한 RxJS의 활용은 효율적이고 반응적인 데이터 처리를 보장합니다. 전반적으로, NgRx는 Angular 애플리케이션에서 유지 관리성을 향상시키고 성능을 향상시키며 상태 관리 프로세스를 보다 투명하고 관리할 수 있게 만듭니다. GitHub Repo에서 자세히 알아볼 수 있습니다. 라이브 배포 링크.

# 소통해요

LinkedIn, GitHub, Twitter.