---
title: "Compose Multiplatform Android, iOS 배너 캐로셀 위젯 사용 방법 "
description: ""
coverImage: "/assets/img/2024-07-09-ComposeMultiplatformAndroidiOSBannerCarouselWidgets_0.png"
date: 2024-07-09 10:40
ogImage: 
  url: /assets/img/2024-07-09-ComposeMultiplatformAndroidiOSBannerCarouselWidgets_0.png
tag: Tech
originalTitle: "Compose Multiplatform (Android , iOS): Banner Carousel Widgets 🎠"
link: "https://medium.com/@meytataliti/compose-multiplatform-android-ios-banner-carousel-widgets-b3fb3b0b2d4f"
isUpdated: true
---





안녕하세요! 지금부터 간단하게 알려드릴게요. 이미 많이들 알고 계실 수도 있지만, 그래도 함께 공유해보겠어요~

## 뱃지

배너에는 인터넷에서 가져온 이미지를 사용했어요. 높이는 고정 200 dp로 설정하고 ContentScale.Crop을 사용해 초과 부분을 자릅니다. 둥근 모서리를 약 8 dp로 만들어 보기 좋게 디자인했어요.

<div class="content-ad"></div>

죄송해요, 만약 풀 이미지 경로를 만드려면 `coompose` 부분은 `compose`로 수정해야합니다.   

로컬 및 리모트 이미지를 로드하기 위해 Coil을 사용하고 있군요. Coil을 사용하기 위해서는 아래 종속성을 추가해야 합니다.

```kotlin
commonMain.dependencies {
  implementation("io.coil-kt.coil3:coil-compose:3.0.0-alpha06")
  implementation("io.coil-kt.coil3:coil-network-ktor:3.0.0-alpha06")
}
```

하지만 이것만으로 충분하지 않아요. Ktor 종속성도 추가해야 합니다.

<div class="content-ad"></div>


[korean]
{
  카프 = "2.3.12"
}

[라이브러리]
ktor-client-core = { module = "io.ktor:ktor-client-core", version.ref = "ktor" }
ktor-client-okhttp = { module = "io.ktor:ktor-client-okhttp", version.ref = "ktor" }
ktor-client-darwin = { module = "io.ktor:ktor-client-darwin", version.ref = "ktor" }



iosMain.dependencies {
  구현(libs.ktor.client.darwin)
}
androidMain.dependencies {
  구현(libs.ktor.client.okhttp)
}


작업이 완료되면 안드로이드 앱을 위해 AndroidManifest.xml에 인터넷 권한을 추가하는 것을 잊지 마세요.

## 카루셀


<div class="content-ad"></div>

HorizontalPager를 사용하여 배너와 페이지 인디케이터인 Circle Boxes 세트를 보유합니다.

```kotlin
data class BannerModel(
    val imageUrl: String,
    val contentDescription: String
)

@Composable
fun BannerCarouselWidget(
    banners: List<BannerModel>,
    modifier: Modifier = Modifier
) {
    val pagerState = rememberPagerState(pageCount = {
        banners.size
    })

    Box(
        contentAlignment = Alignment.BottomCenter,
        modifier = modifier
    ) {
        HorizontalPager(
            state = pagerState,
            contentPadding = PaddingValues(horizontal = 16.dp),
            pageSpacing = 8.dp,
            verticalAlignment = Alignment.Top,
        ) { page ->
            BannerWidget(
                imageUrl = banners[page].imageUrl,
                contentDescription = banners[page].contentDescription
            )
        }
        Row(
            Modifier
                .fillMaxWidth()
                .padding(bottom = 8.dp),
            horizontalArrangement = Arrangement.Center
        ) {
            repeat(pagerState.pageCount) { iteration ->
                val color = if (pagerState.currentPage == iteration) Color.DarkGray else Color.LightGray
                Box(
                    modifier = Modifier
                        .padding(2.dp)
                        .clip(CircleShape)
                        .background(color)
                        .size(8.dp)
                )
            }
        }
    }
}
```

여기까지입니다.

알아두면 좋을 추가적인 사항들이 있습니다.

<div class="content-ad"></div>

![Image 1](/assets/img/2024-07-09-ComposeMultiplatformAndroidiOSBannerCarouselWidgets_1.png)

![Image 2](/assets/img/2024-07-09-ComposeMultiplatformAndroidiOSBannerCarouselWidgets_2.png)

![Image 3](/assets/img/2024-07-09-ComposeMultiplatformAndroidiOSBannerCarouselWidgets_3.png)

첫 번째 Compose Multiplatform 프로젝트에서 오류를 만난다면 주저하지 마십시오. 함께 해결책을 찾을 수 있습니다. 😃

<div class="content-ad"></div>

감사하고 즐거운 하루 보내세요!