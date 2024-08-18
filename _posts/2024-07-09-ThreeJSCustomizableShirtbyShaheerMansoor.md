---
title: "Shaheer Mansoor의 ThreeJS로 커스터마이즈할 수 있는 셔츠 만들기"
description: ""
coverImage: "/assets/img/2024-07-09-ThreeJSCustomizableShirtbyShaheerMansoor_0.png"
date: 2024-07-09 23:03
ogImage:
  url: /assets/img/2024-07-09-ThreeJSCustomizableShirtbyShaheerMansoor_0.png
tag: Tech
originalTitle: "ThreeJS Customizable Shirt by Shaheer Mansoor"
link: "https://medium.com/@shaheermansoor321/threejs-customizable-shirt-by-shaheer-mansoor-f179ea28d177"
isUpdated: true
---

![이미지](/assets/img/2024-07-09-ThreeJSCustomizableShirtbyShaheerMansoor_0.png)

제목: ThreeJS Customizable Shirt

작성자: Shaheer Mansoor

주제: 3D 웹 세계

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

언어: 영어

실시간 미리보기: 라이브 웹사이트 방문

소스 코드: 깃허브 저장소

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

저는 WebGL 기술을 활용한 작은 흥미로운 프로젝트를 만들기로 마음먹고 앉아서 이에 대해 고민했어요. ThreeJS와 ReactJS를 이용해 3D 맞춤형 셔츠를 만들기로 결정했죠. Blender를 사용해 셔츠의 3D 모델을 만들고 ThreeJS로 가져왔어요.

# 기술 스택

- Vite를 사용한 ReactJS
- @react-three/drei — @react-three/fiber를 위한 완전히 기능이 구현된 준비된 추상화 도구.
- @react-three/fiber — three.js를 위한 React 렌더러입니다.
- framer-motion — Framer에서 만든 React용 오픈소스 모션 라이브러리.
- maath — 유용한 수학 도우미, 랜덤 생성기, 여러 가지를 모은 것입니다. 주로 three.js와 함께 사용하도록 설계되었습니다.
- react-color — 컬러 휠 피커용입니다.
- three — ThreeJS를 위한 three.
- valtio — 상태 관리를 위한 도구입니다.

# 컬러 휠

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

![Color Wheel](/assets/img/2024-07-09-ThreeJSCustomizableShirtbyShaheerMansoor_1.png)

Color Wheel — Allows users to change color of the shirt by a color wheel.

# Logo / Texture

![Logo / Texture](/assets/img/2024-07-09-ThreeJSCustomizableShirtbyShaheerMansoor_2.png)

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

파일 업로드 - 사용자는 이미지 파일을 업로드한 후 셔츠의 로고 또는 전체 텍스쳐로 이미지를 설정할 수 있습니다.

## 로고 / 텍스쳐 토글

![로고 이미지](/assets/img/2024-07-09-ThreeJSCustomizableShirtbyShaheerMansoor_3.png)

![텍스쳐 이미지](/assets/img/2024-07-09-ThreeJSCustomizableShirtbyShaheerMansoor_4.png)

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

로고와 질감 사이를 토글할 수 있는 간단한 토글 버튼입니다.

# .png 셔츠 다운로드

![셔츠 이미지](/assets/img/2024-07-09-ThreeJSCustomizableShirtbyShaheerMansoor_5.png)

원하는 대로 셔츠를 커스터마이징한 후, 다운로드 버튼을 클릭하여 셔츠의 png 파일을 저장할 수 있습니다.

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

# 향후 업데이트

셔츠 디자인을 저장하고 로컬 저장소에 저장하는 저장 기능을 추가할 예정입니다. 또한 셔츠 디자인을 생성하기 위한 AI 채팅 프롬프트도 추가할 예정입니다.

읽어 주셔서 정말 감사합니다 ❤ 재미있게 즐겨보세요!
