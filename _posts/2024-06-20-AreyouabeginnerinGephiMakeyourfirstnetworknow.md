---
title: "지피에 초보이신가요 이제 첫 네트워크를 만들어 보세요"
description: ""
coverImage: "/assets/img/2024-06-20-AreyouabeginnerinGephiMakeyourfirstnetworknow_0.png"
date: 2024-06-20 15:06
ogImage:
  url: /assets/img/2024-06-20-AreyouabeginnerinGephiMakeyourfirstnetworknow_0.png
tag: Tech
originalTitle: "Are you a beginner in Gephi? Make your first network now!"
link: "https://medium.com/@vespinozag/are-you-a-beginner-in-gephi-make-your-first-network-now-3768d35c733e"
isUpdated: true
---

2024년 6월 19일, 브로니카 에스피노자 박사

▪트위터 (X) @Verukita1 ▪링크드인: Dra. Verónica Espinoza ▪웹사이트: www.nethabitus.org

![이미지](/assets/img/2024-06-20-AreyouabeginnerinGephiMakeyourfirstnetworknow_0.png)

# 소개

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

# Gephi란?

Gephi는 그래프를 탐색하고 이해하려는 데이터 분석가와 과학자들을 위한 도구입니다. Photoshop™가 그래프 데이터용으로 있는 것처럼, 사용자는 표현과 구조, 모양, 색상을 조작하여 숨겨진 패턴을 찾아냅니다 [1].

이 도구의 목표는 데이터 분석가들이 가설을 세우고 직관적으로 패턴을 발견하거나 데이터 소싱 과정에서 구조의 특이성이나 결함을 격리하는 데 도움을 주는 것입니다. 시각화된 인터페이스와 상호작용을 통한 시각적 사고는 이제 추론을 용이하게 하는 것으로 인정받고 있어, 전통적인 통계의 보완 도구입니다. 이는 시각 분석 분야의 탐구에서 나온 탐색적 데이터 분석 소프트웨어입니다 [1, 2].

# Gephi 인터페이스

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

Gephi의 주요 섹션입니다.

![image1](/assets/img/2024-06-20-AreyouabeginnerinGephiMakeyourfirstnetworknow_1.png)
![image2](/assets/img/2024-06-20-AreyouabeginnerinGephiMakeyourfirstnetworknow_2.png)

😉 Gephi에 대해 더 알고 싶다면, 이 이야기를 읽어보세요: Gephi란 무엇인가요? 유용한 네트워크 분석 도구를 만나보세요.

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

🌐 Gephi를 여기서 다운로드하세요: [https://gephi.org/](https://gephi.org/)

# 이 이야기에서는 무엇을 검토할까요?

본 튜토리얼에서는 5단계로 Gephi 네트워크를 시각화하는 방법을 배우게 될 것입니다. 이 연습에서는 Spotify API를 사용하여 Python 코드로 이전에 생성한 GEXF 파일을 사용할 것입니다. 이 파일은 Spotify에서 관련 아티스트의 네트워크를 나타냅니다.

저도 이 튜토리얼에서 사용할 동일한 GEXF 파일을 공유하므로 여러분도 연습을 따라 할 수 있습니다!

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

👉 이 연습을 위한 GEXF 파일을 다운로드하세요.

![이미지](/assets/img/2024-06-20-AreyouabeginnerinGephiMakeyourfirstnetworknow_3.png)

# 이 튜토리얼에서 다룰 5단계의 시각적 요약.

# 🏁 시작해 봅시다!

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

# 단계 1. 통계 적용: 모듈러리티 실행

당신의 Gephi 도구 / 개요 탭에서:

- 저와 공유한 GEXF 파일을 엽니다. 파일 ➡ 파일 열기.
- 통계 섹션에서: 모듈러리티 적용 ➡ 확인.

여기에서 모듈러리티 알고리즘에 대해 알아보세요: [링크1](링크 주소) 및 [링크2](링크 주소)

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

🎦 아래는 이 단계의 데모입니다. 파일을 열어보세요 — 통계 적용: 모듈성 실행.

![미리보기](https://miro.medium.com/v2/resize:fit:1400/1*NUdHo3fKobSTJ52OYFoSFg.gif)

# 단계 2. 외형 조정.

개요 탭 / 외형 섹션:

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

- 모듈러리티에 따라 노드 색상 지정하기 (경로: 색상 아이콘 ➡ 노드 ➡ 파티션 ➡ 모듈러리티 클래스 선택 ➡ 적용 ▶)

![이미지](/assets/img/2024-06-20-AreyouabeginnerinGephiMakeyourfirstnetworknow_4.png)

- 팔로워 수에 따라 노드 크기 순위 매기기 (경로: 크기 아이콘 ➡ 노드 ➡ 랭킹 ➡ 팔로워 선택 ➡ 최소 크기: 2, 최대 크기: 14 조정 ➡ 적용 ▶)

![이미지](/assets/img/2024-06-20-AreyouabeginnerinGephiMakeyourfirstnetworknow_5.png)

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

- 팔로워별로 글꼴 크기를 조정하세요. (경로: 레이블 크기 아이콘 ➡ 노드 ➡ 랭킹 ➡ 팔로워 선택 ➡ 최소 크기: 0.3, 최대 크기: 0.5 조정 ➡ 적용 ▶)

![이미지](/assets/img/2024-06-20-AreyouabeginnerinGephiMakeyourfirstnetworknow_6.png)

🎦 아래에서는 이 단계의 시연이 제공됩니다 — 외관 조정.

![이미지](https://miro.medium.com/v2/resize:fit:1400/1*-QRqxxCeH3YMy0UPxfbltw.gif)

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

# 단계 3. 레이아웃 적용

개요 탭 / 레이아웃 섹션:

- ForceAtlas 2 적용
- Prevent Overlap 선택
- ▶실행 ➡◼정지

이 논문에서 ForceAtlas 2 레이아웃 알고리즘에 대해 알아보기

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

아래에서는 이 단계인 "레이아웃 적용"의 데모가 제공됩니다.

![이미지](https://miro.medium.com/v2/resize:fit:1400/1*6EgxWEHzuIOfBYMfAGVOrQ.gif)

# 단계 4. 미리보기 설정 조정하기.

미리보기 탭 / 미리보기 설정 섹션:

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

- 프리셋 — 기본값 선택.

![이미지](/assets/img/2024-06-20-AreyouabeginnerinGephiMakeyourfirstnetworknow_7.png)

- 설정 탭➡노드 레이블 ➡ 레이블 표시 선택 ➡ 새로 고침

![이미지](/assets/img/2024-06-20-AreyouabeginnerinGephiMakeyourfirstnetworknow_8.png)

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

- 렌더러 탭 관리하기 ➡ "기본 노드 라벨"을 가장 먼저 위치로 이동 ⬆ ➡ 새로 고침.

![이미지](/assets/img/2024-06-20-AreyouabeginnerinGephiMakeyourfirstnetworknow_9.png)

- 설정 탭 ➡ 엣지 ➡ 가중치 재조정 선택 ➡ 두께 조절 (0.5) ➡ 새로고침.

![이미지](/assets/img/2024-06-20-AreyouabeginnerinGephiMakeyourfirstnetworknow_10.png)

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

🎦 이 단계를 보여주는 동영상입니다 - 미리보기 설정 조정.

![동영상](https://miro.medium.com/v2/resize:fit:1400/1*i70FOt41lKGSLGLUHxw2FQ.gif)

# 단계 5. 네트워크 저장하기.

미리보기 탭

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

- **Export를 클릭하세요 (SVG / PDF / PNG).**

![이미지](/assets/img/2024-06-20-AreyouabeginnerinGephiMakeyourfirstnetworknow_11.png)

- 네트워크 이름을 지정하세요 ➡ 옵션 클릭 ➡ 이미지의 폭과 높이 조정 ➡ 확인

![이미지](/assets/img/2024-06-20-AreyouabeginnerinGephiMakeyourfirstnetworknow_12.png)

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

🎦 아래는 이번 단계의 데모입니다 - 귀하의 네트워크를 저장하세요.

![Demo](https://miro.medium.com/v2/resize:fit:1400/1*rBJ8w588EVdHLNcr5t6Oow.gif)

😉 첫 번째 네트워크가 이제 준비되었습니다!

![Network](/assets/img/2024-06-20-AreyouabeginnerinGephiMakeyourfirstnetworknow_13.png)

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

# 결론

이 튜토리얼에서는 Gephi에서 첫 번째 네트워크를 만드는 방법을 살펴보았습니다. 여기에 표시된 단계는 Gephi가 제공하는 기능에 익숙해지기 위한 예제였습니다.

위에서 언급했듯이, 귀하의 네트워크 파일에 따라이 튜토리얼에서 검토했던 매개변수를 다시 조정해야 할 수도 있습니다. 예를 들어 파일에 있는 다른 속성으로 노드에 색상을 입히는 것을 시도해보세요. 또한 파일에 따라 최소 및 최대 숫자 값을 조정하여 다른 속성에 따라 노드 크기와 레이블을 순위 지정할 수도 있습니다.

귀하의 파일을 업로드하고 검토했던 다양한 매개변수를 조정해보며 실험해 보기를 장려합니다. 또한 다른 레이아웃으로 전환하여 네트워크가 어떻게 보이는지 확인할 수도 있습니다.

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

또한 미리보기 탭에서 파일을 보기에 적합하다고 생각되는 모든 조정 사항을 시도해보세요.

특정한 화면 조정에 따라 매개변수 설정이 달라질 수 있으니 유념해주세요.

당신의 프로젝트에 이 멋진 도구를 활용하기 시작하는 데 도움이 되었기를 바랍니다.

😉이 이야기를 읽어 주셔서 감사합니다.

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

👉 내가 쓴 더 많은 이야기를 찾아보세요

✔ 트위터에서 나를 팔로우해요 (X) @Verukita1

✔ LinkedIn: Dra. Verónica Espinoza

✔ 웹사이트: www.nethabitus.org

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

![image](/assets/img/2024-06-20-AreyouabeginnerinGephiMakeyourfirstnetworknow_14.png)

# Resources

🌐 Gephi website.

💠 Do you want to continue learning more about Gephi? Check out all the stories I’ve written about Gephi here.

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

📕 이야기에서 네트워크 과학에 관한 무료 자료를 찾아보세요: Network Science: Open access resources (books, chapters, articles, tools & more)

📄 다음 논문을 읽어보세요: Fast unfolding of communities in large networks

✅ 깃허브: Modularity Github-Gephi

🗄️ 데이터셋 찾기: Gephi 샘플 데이터셋을 다양한 형식(GEXF, GDF, GML, NET, GraphML, DL, DOT)으로 제공합니다.

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

👨‍💻 Gephi 커뮤니티를 팔로우하세요! Gephi FB 그룹, 트위터, Reddit 등 다양한 소셜 네트워크에서 만나보세요.

![image](/assets/img/2024-06-20-AreyouabeginnerinGephiMakeyourfirstnetworknow_15.png)

참고 자료

[1] Bastian M., Heymann S., Jacomy M. (2009). Gephi: exploring and manipulating networks을 위한 오픈 소스 소프트웨어. 웹로그 및 소셜 미디어 국제 AAAI 컨퍼런스

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

[2] Gephi - 오픈 그래프 시각화 플랫폼 [인터넷]. [2024년 6월 15일에 확인]. 이용 가능한 링크: https://gephi.org/
