---
title: "오늘 웹 앱을 만들 때 내 기술 스택"
description: ""
coverImage: "/assets/img/2024-08-19-MyTechStackForBuildingWebAppsToday_0.png"
date: 2024-08-19 02:22
ogImage: 
  url: /assets/img/2024-08-19-MyTechStackForBuildingWebAppsToday_0.png
tag: Tech
originalTitle: "My Tech Stack For Building Web Apps Today"
link: "https://medium.com/javascript-in-plain-english/my-tech-stack-for-building-web-apps-today-43398556bb4d"
isUpdated: false
---


매주 새로운 도구와 기술들이 출시됩니다. 예를 들어, 인공 지능(AI) 분야는 빠르게 성장하고 있어요. 거의 매일 새로운 도구나 모델이 출시되고 있죠. JavaScript 생태계도 마찬가지로, 새로운 라이브러리와 프레임워크가 자주 출시되고 있습니다.

계속 속도에 맞춰가기 어려운 느낌이 들죠. 그래서 나는 신뢰할 수 있는 견고한 스택이 필요하다고 생각해요. 오늘날 웹 앱을 구축하는 데 사용할 수 있는 스택이죠.

여기 내가 오늘날 웹 앱을 구축하는 데 사용하는 검증된 기술 스택이 있어요.

눌러서 스택 각 섹션의 도구를 확인해보세요 — 아직 익숙하지 않다면 해당 링크가 추가되어 있어요.

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

# 🌐 프론트엔드

내 프론트엔드에는 React를 사용하는 것을 좋아해요. 지난 몇 년 동안 React를 사용해왔는데, 웹 앱을 구축하는 데 훌륭한 도구라고 생각해요. React 앱을 쉽게 구축하기 위해 Next.js와 함께 사용하고 싶어해요.

가끔 Next.js가 다소 독단적이라는 점이 마음에 들지 않아 많은 규칙을 따르지는 않아요. 하지만 React 앱을 쉽게 만드는 데 도움이 되는 점은 좋아해요. Next.js의 앱 라우터와 이미지 컴포넌트를 사용해요.

아직은 서버 액션을 사용하지는 않아요. 여전히 새로운 것 같아서 더 많이 시도해보지 않아 추천할만한 기회가 없었어요. 그래도 API 통합에 대한 선택은 React Query와 Axios를 쌍으로 사용해요. 둘은 잘 동작하고 React Query가 데이터를 캐시하고 관리하기 쉬운 점이 마음에 들어요.

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

스타일링을 할 때 Tailwind CSS를 사용하는 것을 좋아해요. 최고는 아니지만 디자인을 쉽게 만들 수 있는 점이 마음에 들어요. 조금 번잡스러울 수는 있지만, 사용하기 쉬운 장점 때문에 괜찮다고 생각해요. shadcn/ui와 함께 사용하면, 미니멀한 디자인을 좋아한다면, 컴포넌트를 쉽게 구축할 수 있어 좋아요.

요약하면, 저의 프론트엔드 스택은 다음과 같아요:

- React & Next.js
- React Query & Axios
- Tailwind CSS & shadcn/ui

# ⚒️ 백엔드

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

저는 백엔드로 Node.js를 선호합니다. React와 마찬가지로 지난 몇 년 동안 Node.js를 사용해왔는데, 웹 앱을 만드는 데 좋은 도구라고 생각합니다. 프론트 엔드를 위해 필요한 엔드포인트를 만들기 위해 Nest.js와 함께 사용하고 싶습니다.

Nest.js의 체계적인 구조와 데코레이터를 사용하여 API를 쉽게 빌드할 수 있는 점을 좋아합니다. 때로는 어디서 나온 코드인지 모르는 코드 마법이나 발생할 수 있는 순환 종속성 문제가 있을 수 있지만, 작동 방식에 대한 이해와 문서를 읽으면 이러한 문제를 해결할 수 있습니다.

모든 웹 앱에는 데이터베이스가 필요하기 때문에 PostgreSQL(Supabase)과 TypeORM을 사용하는 것을 좋아합니다. TypeORM은 훌륭한 문서를 갖추고 있으며, 내가 사용해본 ORM 중에서 가장 신뢰할 수 있는 것 중 하나입니다. 이미 꽤 오랫동안 사용되어 왔으며 Nest.js와의 훌륭한 통합을 제공합니다.

요약하면, 제 백엔드 스택은 다음과 같습니다:

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

- Node.js 및 Nest.js
- PostgreSQL (Supabase) 및 TypeORM

# 🏗️ 인프라/배포

배포, 호스팅 또는 심지어 SSL 설정에 대해, 나는 Coolify를 사용하는 것을 좋아합니다. 최근에 발견했고, 웹 앱을 설정하는 데 많은 도움이 되었습니다. 이전에는 Vercel이나 GCP의 Cloud Run을 사용했지만 이러한 서버리스 솔루션을 피했습니다. 전용 프로비저닝 서버를 사용하는 것이 여전히 더 나은 것으로 생각하기 때문입니다. 호스팅 필요 시 Digital Ocean에서 VPS를 구입합니다.

다행히도 Coolify는 쉽게 설정할 수 있고, 자체 호스트 옵션이 있으며, 훌륭한 문서가 있습니다. 웹 앱을 배포하고 SSL을 설정하는 데 더 쉽게 도와주는 점이 마음에 듭니다.

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

클라우드플레어를 DNS 및 CDN으로 사용하고 있어요. 도메인도 그곳에서 구입했는데, 도메인 관리가 너무 간편해서 좋아해요.

그리고 깃허브를 사용해서 CI/CD에 Coolify를 연결했어요. 그리고 배포 알림을 위해 디스코드와 연결했어요.

요약하면, 제 배포 스택은 다음과 같아요:

- Coolify (DigitalOcean)
- 클라우드플레어
- 깃허브

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

# 📧 이메일

이메일은 때로는 필요 없다고 생각할 수 있지만 실제로 필요한 것 중 하나에요. 이메일을 보내기 위해 사용할 수 있는 다양한 서비스들이 있어요. 저는 이메일을 보내기 위해 Resend를 사용하는 것을 좋아해요. Resend는 이메일을 보내는 것을 더 쉽게 만들어주는 간단한 서비스에요.

Resend는 이메일을 보내는 데 사용하고, 이메일 템플릿을 만들기 위해 @react-emails를 사용해요. 이전에 신뢰할 수 있는 이메일 템플릿을 어떻게 만들어야 할지 몰랐었지만, @react-emails를 사용하니 훨씬 쉽게 만들 수 있어요.

Resend는 Supabase와 함께 사용해도 좋아요, 만약 그들의 서비스를 사용하는 것을 좋아한다면요.

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

요약하자면, 제 이메일 스택은 다음과 같습니다:

- Resend
- @react-emails

# 🔑 인증

요즘에는 인증이 필요할 때마다 일반적으로 Supabase를 사용합니다. 백엔드에서 엔드포인트를 생성하고 그들의 API를 호출하여 인증 서비스를 사용합니다. 프론트엔드 SDK를 사용할 수도 있지만, 저는 백엔드에서 설정하는 것을 선호합니다.

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

포스트그레스와 Supabase를 사용하기 때문에 필요하다면 나만의 인증 서비스를 만들 수 있어요. 이 스택을 사용하면 그렇게 유연하게 작업할 수 있죠. 그리고 무엇보다 중요한 건, 사용자 데이터가 제 범위 안에 있고 어떤 제3자 서비스에도 없어요. 

요약하면, 제 인증 스택은:

- Supabase

# 📦 Storage

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

이미지 저장을 위해 이미지킷을 사용하는 것을 좋아해요. 모바일 앱과 웹 앱의 이미지 저장에 이미 꽤 오랫동안 사용 중이고 정말 좋았어요. 이미지를 저장하기 쉽게 만들어주고 필요할 때 쉽게 조작할 수 있어서 좋아해요.

일반 저장을 위해 내가 선택한 것은 아직 없어요. 클라우드플레어 R2, AWS S3 또는 심지어 슈파베이스 스토리지를 사용할 수 있을 것 같아요. 하지만 아직 시도해보지 못했어요. 파일이나 일반 저장이 필요한 웹 앱은 아직 없어요.

요약하면, 내 저장 스택은 다음과 같아요:
- 이미지킷 (이미지용)
- 클라우드플레어 R2, AWS S3 또는 슈파베이스 스토리지 (일반 저장용)

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

# 💰 비용

이 설정으로는 보통 $20~$30 사이에서 움직이는데, 이는 내가 선호하는 DigitalOcean의 VPS 비용이기 때문이다. 조금 더 싸게 구할 수도 있겠지만, 내 요구에 대해서는 그 옵션이 괜찮아 보인다. Hetzner에서 DigitalOcean보다 더 저렴하게 구입할 수도 있다고 생각해.

전체적으로, 다른 많은 서비스들도 너무 후한 무료 티어를 제공하고 있어 — 그래서 단순히 어떤 것을 테스트해보고 싶다면, 그것들이 도움이 될 거야!

# ⚡️ 초사이어인 모드로 갈 건가요?

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

신뢰할 수 있는 웹 스택을 구축하려면 적절한 도구 뿐만 아니라 원활한 프로세스를 보장하기 위한 신뢰할 수 있는 하드웨어도 필요합니다. 모든 것을 구동할 수 있는 기계를 갖고 있으면 차이를 만들 수 있습니다.

![My Tech Stack](/assets/img/2024-08-19-MyTechStackForBuildingWebAppsToday_0.png)

여기에 Gigatech Gadgets이 도입됩니다. 새 노트북, 데스크탑 또는 서버를 찾고 있다면, 그들은 여러분을 위한 적절한 하드웨어를 보유하고 있습니다.

Gigatech Gadgets 웹 사이트를 방문하여 그들이 제공하는 것을 확인해 보세요!

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

# 🚀 결론

이 스택은 저에게 효과가 있었고 웹 앱을 빠르게 구축하는 데 도움이 되었습니다. 신뢰할 수 있는 견고한 스택을 갖는 것이 중요하다고 생각합니다. 항상 최신 도구나 기술을 사용하는 것이 아니라, 그냥 일을 잘할 수 있는 올바른 도구를 사용하는 것이 중요합니다.

저는 다른 도구들도 알고 있지만, 대부분의 경우 이 스택이 필요한 웹 앱을 구축하는 데 충분합니다. 이 도구들이 함께 사용될 수 있고 서로 잘 작동하는 점도 마음에 듭니다.

완벽하지는 않지만, 아이디어를 실행하고 웹 앱을 빠르게 구축하는 데 제게 도움이 되고 있습니다.

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

이 스택이 당신이 웹 앱을 빠르게 구축하는 데 도움이 되기를 바랍니다. 혹은 더 좋은 방법으로, 이 스택을 당신이 의지할 수 있는 스택의 시작점으로 만들어 보세요. 그게 훨씬 좋을 거에요!

언제든지 궁금한 점이나 제안사항이 있으시면 아래 댓글을 남겨주세요. 당신의 소중한 의견을 듣고 싶어요.

읽어주셔서 감사합니다. 이 글이 여러분의 여정에 도움이 되기를 바라요! ❤️

# 간단히 이야기해보자 🚀

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

In Plain English 커뮤니티에 참여해 주셔서 감사합니다! 떠나시기 전에:

- 필자를 클랩하고 팔로우해 주세요 👏
- 팔로우하기: X | LinkedIn | YouTube | Discord | 뉴스레터
- 다른 플랫폼 방문하기: CoFeed | Differ
- PlainEnglish.io에서 더 많은 콘텐츠 만나보세요