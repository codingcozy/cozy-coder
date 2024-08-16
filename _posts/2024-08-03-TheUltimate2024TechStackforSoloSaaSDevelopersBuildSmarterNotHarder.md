---
title: "솔로 SaaS를 스마트하고 더 쉽게 구축하는 방법"
description: ""
coverImage: "/assets/img/2024-08-03-TheUltimate2024TechStackforSoloSaaSDevelopersBuildSmarterNotHarder_0.png"
date: 2024-08-03 18:40
ogImage: 
  url: /assets/img/2024-08-03-TheUltimate2024TechStackforSoloSaaSDevelopersBuildSmarterNotHarder_0.png
tag: Tech
originalTitle: "The Ultimate 2024 Tech Stack for Solo SaaS Developers Build Smarter, Not Harder"
link: "https://medium.com/@ixartz/the-ultimate-2024-tech-stack-for-solo-saas-developers-build-smarter-not-harder-011d08292bd1"
isUpdated: true
---





SaaS를 혼자 개발자로 만드는 것은 도전적인 과제입니다. 여러 역할을 맡고 다양한 기술에 능숙해야 하며, 기술 스택에 대한 전략적인 결정이 필요합니다. 이는 프런트엔드와 백엔드 모두에 익숙한 풀스택 개발자여야 한다는 것을 의미합니다.

올바른 기술 스택을 선택하는 것은 최고의 개발자 경험을 위해 중요합니다. 이 글에서는 SaaS를 구축하기 위한 Next.js 스택을 공유하고, 스택의 다양한 부분을 자세히 살펴보겠습니다. 또한 저의 신뢰할 수 있는 도구들도 함께 소개할 것입니다. 최종 결과물은 여기 있습니다:

![이미지](/assets/img/2024-08-03-TheUltimate2024TechStackforSoloSaaSDevelopersBuildSmarterNotHarder_0.png)

실시간 데모를 확인해보세요.

<div class="content-ad"></div>

이 게시물이 여러분의 SaaS 여정에 영감을 주고 도움이 되기를 바랍니다.

# Next.js, 스택의 중추

독립 개발자로써 여러분은 쉽게 풀스택 애플리케이션을 구축할 수 있는 프레임워크가 필요합니다. Next.js는 현대적인 애플리케이션을 효율적으로 구축할 수 있도록 도와주는 React 프레임워크로, SaaS를 구축하기에 우수한 선택입니다.

![이미지](/assets/img/2024-08-03-TheUltimate2024TechStackforSoloSaaSDevelopersBuildSmarterNotHarder_1.png)

<div class="content-ad"></div>

Next.js를 사용하여 사용자 대시보드와 마케팅 사이트를 만들고 있어요. 프론트엔드는 React로 작성되었고, 백엔드는 Next.js Route Handlers를 사용하고 있어요. Route Handlers는 RESTful API를 생성해서 React 컴포넌트 및 모바일 애플리케이션과 같은 다른 클라이언트에서 사용할 수 있어요.

마케팅 사이트와 대시보드 모두에 동일한 프레임워크를 사용하면 구성 요소와 스타일을 SaaS의 모든 부분에 재사용할 수 있어요. 이렇게 하면 디자인이 더 일관성 있고 개발이 더 효율적으로 이루어집니다. 마찬가지로, 프론트엔드와 백엔드 모두가 Next.js에 의존하도록 함으로써 두 영역 간에 코드를 공유하는 것이 매우 쉽습니다.

# TypeScript로 한 가지 언어만 사용해요

<img src="/assets/img/2024-08-03-TheUltimate2024TechStackforSoloSaaSDevelopersBuildSmarterNotHarder_2.png" />

<div class="content-ad"></div>

최대한 생산성을 높이기 위해 저는 TypeScript라는 언어만 사용합니다. Next.js와 결합하여 TypeScript를 사용하면 프론트엔드와 백엔드 코드를 하나의 프레임워크와 언어로 작성할 수 있어 개발 프로세스를 간소화하고 문맥 전환이 줄어듭니다.

# Shadcn UI와 Tailwind CSS

![이미지](/assets/img/2024-08-03-TheUltimate2024TechStackforSoloSaaSDevelopersBuildSmarterNotHarder_3.png)

![이미지](/assets/img/2024-08-03-TheUltimate2024TechStackforSoloSaaSDevelopersBuildSmarterNotHarder_4.png)

<div class="content-ad"></div>

UI로는 Shadcn UI를 선택했어요. Shadcn UI는 Radix UI 위에 구축된 컴포넌트 모음으로, React 컴포넌트를 스타일링하지 않고 제공해요. Shadcn UI는 Tailwind CSS를 사용하여 Radix UI 컴포넌트를 스타일링하여 SaaS에 아름다운 UI를 제공해요. 더 좋은 소식은, 이 컴포넌트를 마케팅 사이트와 사용자 대시보드 사이에서 원활하게 공유할 수 있다는 거예요.

# 인증

인증은 모든 SaaS의 중요한 부분이에요. 저는 Clerk를 사용하여 인증을 처리해요. Clerk는 이메일/비밀번호 및 소셜 로그인과 같은 포괄적인 기능을 제공해요. 이러한 기본적인 기능들은 많은 오픈 소스 라이브러리에서 사용할 수 있어요.

그러나 여러분의 애플리케이션이 더 고급 기능을 필요로 하는 경우, Clerk는 탁월한 선택이에요. Clerk는 다중 요소 인증, 사용자 표시 권한 부여, 다중 세션 지원(한 사용자가 여러 계정에 연결할 수 있음), 일회용 이메일 차단, 브루트포스 방어, 봇 방어 등을 처리할 수 있어요.

<div class="content-ad"></div>

점원은 사용자 정의가 가능한 React에서 완벽한 UI를 제공하여 인증을 제공합니다. 이것은 귀하의 브랜드에 맞추어 사용자 지정할 수 있습니다. 인증을 처음부터 개발하는 수고와 시간을 절약할 수 있습니다. Clerk가 제공하는 내장 UI 중 일부에는 등록, 로그인, 비밀번호 재설정, 비밀번호 초기화 및 사용자 프로필이 포함되어 있습니다.

![이미지](/assets/img/2024-08-03-TheUltimate2024TechStackforSoloSaaSDevelopersBuildSmarterNotHarder_5.png)

# 다중 테넌시 및 팀 관리

강력한 SaaS는 팀 또는 조직 내에서의 협업을 지원해야 합니다. Clerk는 포괄적인 다중 테넌시 및 팀 관리 시스템을 제공하며, 팀을 관리하고 사용자를 초대하는 완벽한 UI를 포함합니다. 이는 Clerk가 초대 이메일을 보내거나 사용자가 팀 간에 신속하게 전환할 수 있도록 하는 것을 포함하여 백엔드 로직이나 팀 관리를 위한 UI를 구현할 필요가 없음을 의미합니다.

<div class="content-ad"></div>


![Role and Permission Image](/assets/img/2024-08-03-TheUltimate2024TechStackforSoloSaaSDevelopersBuildSmarterNotHarder_6.png)

# Role and Permission

다중테넌시를 사용할 때는 역할과 권한을 관리하는 것이 중요합니다. Clerk를 사용하면 사용자가 역할을 할당할 수 있는 사용자 정의 역할과 권한을 생성할 수 있습니다. 예를 들어, 관리자는 팀 내에서 모든 권한을 갖지만 읽기 전용 역할은 리소스를 보기만 할 수 있습니다. 이를 통해 적절한 접근 및 보안이 보장됩니다.

![Another Image](/assets/img/2024-08-03-TheUltimate2024TechStackforSoloSaaSDevelopersBuildSmarterNotHarder_7.png)


<div class="content-ad"></div>

# 데이터베이스

나는 Drizzle ORM을 데이터베이스 관리에 사용합니다. 이는 타입 안전하며 TypeScript와 완벽하게 통합되기 때문입니다. Drizzle을 사용하면 모델과 관계를 직접 TypeScript에서 정의할 수 있어 외부 스키마 파일이 필요하지 않습니다. 이는 다른 구문을 배울 필요가 없다는 뜻입니다.

![이미지](/assets/img/2024-08-03-TheUltimate2024TechStackforSoloSaaSDevelopersBuildSmarterNotHarder_8.png)

또한 Drizzle는 마이그레이션 프로세스를 간소화하는 CLI 도구인 Drizzle Kit을 제공합니다. Drizzle Kit을 사용하면 데이터베이스 스키마를 쉽게 업데이트할 수 있는 마이그레이션 폴더를 생성할 수 있습니다.

<div class="content-ad"></div>

또한, 데이터베이스를 관리하는 시각적 인터페이스로 Drizzle Studio를 사용할 수 있습니다. Drizzle Studio를 통해 데이터베이스 스키마를 확인하고 쿼리를 실행하며 데이터를 살펴볼 수 있습니다.

![이미지](/assets/img/2024-08-03-TheUltimate2024TechStackforSoloSaaSDevelopersBuildSmarterNotHarder_9.png)

# Stripe

![이미지](/assets/img/2024-08-03-TheUltimate2024TechStackforSoloSaaSDevelopersBuildSmarterNotHarder_10.png)

<div class="content-ad"></div>

Stripe는 결제 및 구독을 원활하게 처리합니다. Stripe SDK를 사용하면 Next.js 애플리케이션에 결제 처리를 쉽게 통합할 수 있습니다. Stripe는 사용자를 리디렉션할 수 있는 결제 페이지를 제공합니다. 이 페이지는 사용자가 구독하려는 요금제를 상기시키는 것뿐만 아니라 매달 또는 매년 청구되는 금액도 보여줍니다. 마지막으로 사용자는 선택한 요금제에 가입하기 위해 신용카드 정보를 입력할 수 있습니다.

![이미지](/assets/img/2024-08-03-TheUltimate2024TechStackforSoloSaaSDevelopersBuildSmarterNotHarder_11.png)

구독이 완료되면 Stripe는 사용자가 가입했다는 신호를 포함하는 웹훅 이벤트를 내 REST API 엔드포인트로 전송합니다. 이를 통해 내 데이터베이스에서 사용자의 구독 상태를 업데이트할 수 있습니다.

Stripe는 사용자가 구독을 관리할 수 있는 자체 서비스 포털을 제공합니다. 이 포털에서 사용자는 요금제를 변경하거나 결제 방법을 업데이트하고 구독을 취소하며 송장을 볼 수 있습니다.

<div class="content-ad"></div>

<img src="/assets/img/2024-08-03-TheUltimate2024TechStackforSoloSaaSDevelopersBuildSmarterNotHarder_12.png" />

# 국제화 (i18n)

<img src="/assets/img/2024-08-03-TheUltimate2024TechStackforSoloSaaSDevelopersBuildSmarterNotHarder_13.png" />

글로벌 시장에 다가가기 위해 저는 Next.js에서 여러 언어를 지원하기 위해 Next-Intl 라이브러리를 사용합니다. Next-Intl은 타입 안전한 번역을 제공하여 올바른 번역 키가 사용되었는지 확인합니다. 이를 통해 누락된 또는 잘못된 번역으로 인한 런타임 오류를 방지합니다.

<div class="content-ad"></div>


![이미지](/assets/img/2024-08-03-TheUltimate2024TechStackforSoloSaaSDevelopersBuildSmarterNotHarder_14.png)

번역 경험을 더 효율적으로 만들기 위해 제가 사용하는 Crowdin은 GitHub과 완벽하게 통합되는 로컬라이제이션 플랫폼입니다. Crowdin을 활용하면 번역을 협업적으로 관리할 수 있어 원하는 언어로 애플리케이션이 제공될 수 있습니다.

![이미지](/assets/img/2024-08-03-TheUltimate2024TechStackforSoloSaaSDevelopersBuildSmarterNotHarder_15.png)

# 양식 관리


<div class="content-ad"></div>

<img src="/assets/img/2024-08-03-TheUltimate2024TechStackforSoloSaaSDevelopersBuildSmarterNotHarder_16.png" />

React-Hook-Form과 Zod을 결합하여 양식 관리 및 유효성 검사를 합니다. React-Hook-Form은 React에서 양식 처리를 간소화하며, Zod는 데이터 유효성을 보장합니다. Zod 스키마는 프론트엔드와 백엔드 간에 데이터 유효성을 확실히 하기 위해 쉽게 공유할 수 있습니다.

# 테스트

SaaS 빌더로서, 응용 프로그램이 예상대로 작동하는지 확인하는 것이 중요합니다. 응용 프로그램을 테스트할 팀이 없으므로 자동화된 테스트에 의존해야 합니다. 이렇게 함으로써 새로운 기능이 추가될 때 응용 프로그램에 재귀적인 문제가 발생하지 않음을 확신할 수 있습니다.

<div class="content-ad"></div>


![이미지](/assets/img/2024-08-03-TheUltimate2024TechStackforSoloSaaSDevelopersBuildSmarterNotHarder_17.png)

유닛 테스트에는 Vitest와 React Testing Library를 사용합니다. Vitest는 TypeScript와 ESM을 기본으로 지원하는 테스트 러너로, Jest의 현대적인 대안을 제공합니다. Vitest의 또 다른 장점은 공식 VSCode 익스텐션과 Vitest UI로, Vitest를 더욱 편리하게 사용할 수 있습니다. 또한, React Testing Library는 React 컴포넌트와 상호 작용하기 위한 유틸리티를 제공합니다.

![이미지](/assets/img/2024-08-03-TheUltimate2024TechStackforSoloSaaSDevelopersBuildSmarterNotHarder_18.png)

최종 사용자 테스트(E2E) 및 통합 테스트에는 Playwright를 사용합니다. Playwright는 브라우저 상호작용을 자동화할 수 있는 강력한 도구로, 애플리케이션의 모든 기능을 테스트하는 데 이상적입니다. Playwright를 사용하면 다양한 브라우저에서 사용자 상호작용을 시뮬레이션하여 앱이 일관되게 작동하는지 확인할 수 있습니다. 또한, Playwright는 Next.js Route Handlers를 테스트하는 데 우수하며, HTTP 요청을 쉽게 보내고 응답을 확인할 수 있습니다.


<div class="content-ad"></div>

# GitHub Actions

GitHub Actions은 지속적 통합(CI)을 위해 사용하는 강력한 도구입니다. 이 도구를 사용하여 코드를 주요 브랜치로 병합하기 전에 테스트 및 확인 프로세스를 자동화할 수 있습니다.

![이미지](/assets/img/2024-08-03-TheUltimate2024TechStackforSoloSaaSDevelopersBuildSmarterNotHarder_19.png)

새로운 커밋을 푸시하거나 풀 리퀘스트를 생성할 때마다 나의 GitHub Actions는 미리 정의한 워크플로우를 자동으로 실행합니다. 이 워크플로우는 Vitest로 단위 테스트를 실행하고, Playwright로 엔드 투 엔드 테스트를 실행하며, 린트 및 코드 서식 맞춤 검사를 수행합니다. 문제가 발생하면 GitHub Actions가 알려주어 잘못된 코드를 병합하는 것을 방지합니다.

<div class="content-ad"></div>

내 코드는 지속적으로 테스트되고 검증되어 안전망을 제공하여 새로운 기능을 구축하는 데 집중할 수 있게 도와줍니다. 특히 솔로 개발자로서 여러 역할을 수행하고 응용 프로그램의 모든 측면을 수동으로 테스트할 시간이 제한적인 상황에서 특히 필요합니다.

# 로깅

<img src="/assets/img/2024-08-03-TheUltimate2024TechStackforSoloSaaSDevelopersBuildSmarterNotHarder_20.png" />

저는 Node.js용 빠르고 가벼운 로깅 라이브러리인 Pino를 사용합니다. Pino는 메시지를 기록하고 구조화된 로깅을 지원하여 로그를 쉽게 검색하고 분석할 수 있는 간단한 API를 제공합니다. 제품에서는 로깅을 더 나아가 Better Stack으로 로그를 전송하여 실시간 로그 모니터링, 경보 및 시각화를 제공하는 강력한 로깅 플랫폼을 활용합니다. Pino를 Better Stack과 통합함으로써 모든 로그 데이터가 효율적으로 캡처되고 저장되며 접근 가능하게 되어 실시간 환경에서 문제를 신속하게 식별하고 해결할 수 있도록 지원합니다.

<div class="content-ad"></div>

# 에러 모니터링

에러 모니터링을 위해 Sentry를 사용하고 있어요. Sentry는 에러와 예외를 캡처하여 자세한 보고서를 제공해줘요. 이 보고서에는 스택 추적(stack traces), 사용자 컨텍스트(user context) 및 기타 관련 정보가 포함되어 있어 문제를 식별하기 쉽게 해줘요.

![이미지](/assets/img/2024-08-03-TheUltimate2024TechStackforSoloSaaSDevelopersBuildSmarterNotHarder_21.png)

로컬 개발 시에는 Spotlight를 사용하여 Sentry 이벤트를 캡처하여, 제품 환경을 압도하지 않고 Sentry의 텔레메트리를 활용하고 있어요.

<div class="content-ad"></div>

<img src="/assets/img/2024-08-03-TheUltimate2024TechStackforSoloSaaSDevelopersBuildSmarterNotHarder_22.png" />

# 환경 변수

<img src="/assets/img/2024-08-03-TheUltimate2024TechStackforSoloSaaSDevelopersBuildSmarterNotHarder_23.png" />

T3 Env은 Zod를 사용하여 환경 변수를 유효성 검사하고 변환하는 라이브러리입니다. 이를 통해 모든 환경 변수가 올바르게 정의되고 유효성이 검증됩니다.

<div class="content-ad"></div>

# 린터 및 코드 포매터

깨끗한 코드베이스를 유지하는 것이 중요합니다. 저는 ESLint와 Prettier를 사용하여 린팅 및 코드 포매팅을 합니다. ESLint는 최고의 관행을 강제하고 잠재적인 오류를 잡아내어 코드 품질을 보장하며, Prettier는 일관된 코딩 스타일을 강제함으로써 코드베이스를 더 읽기 쉽고 유지보수하기 쉽게 만듭니다.

![이미지](/assets/img/2024-08-03-TheUltimate2024TechStackforSoloSaaSDevelopersBuildSmarterNotHarder_24.png)

저는 ESLint의 기본 구성으로 Airbnb 스타일 가이드를 추천합니다. 그것은 가장 인기 있는 JavaScript 스타일 가이드 중 하나입니다. 게다가, Playwright 테스트가 최상의 관행을 따르도록 보장하기 위해 eslint-plugin-playwright를 사용하고, Tailwind CSS에 대한 최상의 관행을 강제하기 위해 eslint-plugin-tailwind를 사용합니다.

<div class="content-ad"></div>

# VSCode

<img src="/assets/img/2024-08-03-TheUltimate2024TechStackforSoloSaaSDevelopersBuildSmarterNotHarder_25.png" />

비주얼 스튜디오 코드(VSCode)는 제가 선호하는 코드 편집기이며 풍부한 확장 기능 생태계를 갖추고 있습니다. 제 기술 스택과 잘 작동하는 몇 가지 확장 기능을 소개합니다:

- vscode-eslint, ESLint를 VS Code에 통합
- vscode-tailwindcss, Tailwind CSS에 대한 IntelliSense 및 구문 강조 기능 제공
- vscode-github-actions, GitHub Actions 워크플로우를 VSCode에서 직접 관리
- i18n-ally, 국제화를 지원하며 다국어 작업을 보다 쉽게 만드는 번역 키 관리를 제공합니다

<div class="content-ad"></div>

# 결론

요약하면, 혼자 개발자로 SaaS를 구축하는 것은 상당히 어려울 수 있습니다. 그러나 적절한 기술 스택을 선택하면 이 과정을 훨씬 쉽게 만들 수 있어 사용자에게 가치를 전달하는 데 집중할 수 있습니다. 이 글에서 소개된 Next.js, TypeScript, Shadcn UI with Tailwind CSS, Clerk, Drizzle ORM, Stripe 및 기타 도구들의 조합은 SaaS 제품을 구축하기 위한 확장 가능한 환경을 제공합니다.

이 도구들은 개발 프로세스를 단순화할 뿐만 아니라 애플리케이션이 안전하고 효율적이며 사용자 친화적인지도를 보장합니다. 이들은 인증, 다중 테넌시, 결제 처리부터 데이터베이스 관리, 테스트, 지속적 통합까지 모든 것을 처리하여 비즈니스 로직 및 사용자 경험에 집중하는 데 도움을 줍니다.

최종 결과물을 확인하고 싶다면 라이브 데모에서 찾을 수 있습니다.

<div class="content-ad"></div>

저는 Next.js SaaS 보일러플레이트를 만들었어요. 이 보일러플레이트는 이 글에서 소개한 기술 스택을 사용하여 여러분의 SaaS 제품을 구축하기 위한 포괄적인 시작점이에요.

![이미지](/assets/img/2024-08-03-TheUltimate2024TechStackforSoloSaaSDevelopersBuildSmarterNotHarder_26.png)

혼자 개발자로서 성공하기 위한 비결은 올바른 도구와 기술을 활용하는 것이에요. 이 기술 스택은 제 개인적인 선택으로, 제 경험과 요구 사항을 기반으로 선택했어요. 여러분의 프로젝트에 따라 필요한 도구가 다를 수 있지만, 핵심은 변하지 않아요: 여러분이 생산적일 수 있는 도구를 선택하세요.

이 글이 여러분의 SaaS 여정에 대한 통찰력과 영감을 줬으면 좋겠어요. 즐거운 코딩 되세요!