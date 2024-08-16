---
title: "KMPKotlin Multiplatform 개발자를 위한 Google 지원 라이브러리 8선"
description: ""
coverImage: "/assets/img/2024-07-29-8GooglesupportedlibraryforKMP_0.png"
date: 2024-07-29 13:47
ogImage: 
  url: /assets/img/2024-07-29-8GooglesupportedlibraryforKMP_0.png
tag: Tech
originalTitle: "8 Google supported library for KMP"
link: "https://medium.com/@niranjanky14/8-google-supported-library-for-kmp-ea520888db3e"
---


구글이 KMP(코틀린 멀티플랫폼)를 지원하는 라이브러리를 명시하지 않으셨지만, 안정적이거나 실험적인 KMP 지원을 갖춘 일부 주요 Jetpack 라이브러리를 강조해 드릴 수 있습니다. 아래는 안정적인 KMP 지원을 갖는 라이브러리들입니다:

# 안정적인 KMP 지원

![Google Supported Library for KMP](/assets/img/2024-07-29-8GooglesupportedlibraryforKMP_0.png)

```kotlin
val commonMain by getting {
    dependencies {
        implementation("androidx.annotation:annotation:1.7.0-alpha02")       

        implementation("androidx.collection:collection:1.3.0-alpha04")

        // 사용자 지정 직렬화를 지원하는 하위 수준 API
        implementation("androidx.datastore:datastore-core-okio:1.1.0-alpha03")
        // 기본 유형의 값 저장을 위한 상위 수준 API 
        implementation("androidx.datastore:datastore-preferences-core:1.1.0-alpha03")
   }
}
```

<div class="content-ad"></div>

두 라이브러리에 대한 미리보기 API 참조 문서를 확인하여 사용 가능한 API에 대해 더 자세히 알아볼 수 있어요.