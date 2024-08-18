---
title: "GoRouter로 Flutter 내비게이션 간편하게 구현하는 방법"
description: ""
coverImage: "/assets/img/2024-08-17-SimplifyingNavigationinFlutterwithGoRouter_0.png"
date: 2024-08-17 00:20
ogImage:
  url: /assets/img/2024-08-17-SimplifyingNavigationinFlutterwithGoRouter_0.png
tag: Tech
originalTitle: "Simplifying Navigation in Flutter with GoRouter"
link: "https://medium.com/@syedabdulbasit7/simplifying-navigation-in-flutter-with-gorouter-67bd3151ac43"
isUpdated: true
updatedAt: 1723863789469
---

# 플러터 앱에서 GoRouter를 구현하는 포괄적인 안내서

![이미지](/assets/img/2024-08-17-SimplifyingNavigationinFlutterwithGoRouter_0.png)

# 플러터에서의 네비게이션

"플러터에서는 화면과 페이지를 route(경로)라고 부릅니다."

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

Navigator 클래스는 스택 방식을 사용하여 일련의 하위 위젯을 관리하며, 앱에서 다른 화면 간에 탐색이 가능하게 합니다.

# Navigator 2.0: GoRouter

GoRouter를 사용하려면 MaterialApp에 추가해야 합니다:

```js
class MyApp extends StatelessWidget {
  @override
  Widget build(BuildContext context) {
    return MaterialApp.router(
      routerConfig: _router,
    );
  }
}
```

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

다음으로 GoRouter 생성자를 가진 클래스를 생성하세요:

```js
import 'package:go_router/go_router.dart';

// GoRouter 설정
final _router = GoRouter(
  routes: [
    GoRoute(
      path: '/',
      builder: (context, state) => SplashScreen(),
    ),
    GoRoute(
      path: '/home-screen',
      builder: (context, state) => HomeScreen(),
    ),
  ],
);
```

# 주의할 점

- 네비게이터 메서드 대체: 기존 경로에서 context.pushReplacementNamed를 사용 중이라면 context.go()로 대체하십시오.
  사용법: context.go(`/home-screen`);
- 스택 관리: context.go()를 사용하여 splash에서 home으로 이동하면 스플래시 화면이 스택에 새 화면을 푸시하는 대신 홈 화면으로 대체됩니다.

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

# GoRouter을 사용한 중첩 경로

중첩된 경로를 사용하려면 context.go() 및 context.goNamed()를 사용하세요:

```js
import 'package:go_router/go_router.dart';

import '../screens/bottom_navigation/bottom_navigation.dart';
import '../screens/on_boarding_screen/on_boarding_screen.dart';
import '../screens/phone/phone_screen.dart';
import '../screens/skip.dart';
import '../screens/verification_code.dart';

final router = GoRouter(
  initialLocation: OnBoardingScreen.routeName,
  routes: [
    GoRoute(
      path: OnBoardingScreen.routePath,
      name: OnBoardingScreen.routeName,
      builder: (context, state) => const OnBoardingScreen(),
      routes: [
        GoRoute(
          path: PhoneScreen.routePath,
          name: PhoneScreen.routeName,
          builder: (context, state) => PhoneScreen(
            extra: state.extra as String?,
          ),
          routes: [
            GoRoute(
              path: VerificationScreen.routePath,
              name: VerificationScreen.routeName,
              builder: (context, state) => const VerificationScreen(),
            ),
          ],
        ),
        GoRoute(
          path: SkipScreen.routePath,
          name: SkipScreen.routeName,
          builder: (context, state) => const SkipScreen(),
        ),
      ],
    ),
    GoRoute(
      path: BottomNavigationScreen.routePath,
      name: BottomNavigationScreen.routeName,
      builder: (context, state) => const BottomNavigationScreen(),
    ),
  ],
);
```

# 경로 및 이름 설정

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

아래와 같이 RoutePath와 RouteName을 앞에 슬래시가 없는 상태로 초기화해주세요. GoRouter가 이를 처리합니다:

```js
static const routeName = 'phone-screen';
static const routePath = routeName;
```

# context.go()와 context.goNamed() 사용법

- OnBoardingScreen에서 PhoneScreen으로 이동:
  context.go(`/$'PhoneScreen.routePath'`);
  context.goNamed(PhoneScreen.routePath);
- PhoneScreen에서 VerificationScreen으로 이동:
  context.go(`/$'PhoneScreen.routePath'/$'VerificationScreen.routePath'`);
  context.goNamed(VerificationScreen.routePath);
- PhoneScreen에서 SkipScreen으로 이동 (동일한 레벨):
  context.go(`/$'SkipScreen.routePath'`);
  context.goNamed(SkipScreen.routePath);

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

# 매개변수

GoRouter에서는 라우트 간에 데이터를 전달하는 데 경로 및 쿼리 매개변수를 모두 사용할 수 있습니다. 이 접근 방식은 관련 데이터를 필터링하거나 다음 화면으로 필요한 정보를 전달하는 데 특히 유용합니다.

경로 매개변수는 라우트 경로의 일부이며 매개변수 이름 뒤에 콜론을 사용하여 정의할 수 있습니다. 예를 들어:

```js
GoRoute(
  path: '${PhoneScreen.routePath}/:phoneId',
  name: PhoneScreen.routeName,
  builder: (context, state) => PhoneScreen(
    extra: state.extra as String?,
    phoneId: state.pathParameters['phoneId'],
    phoneName: state.uri.queryParameters['phoneName'],
  ),
);
```

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

이 경로에서는 :phoneId가 경로 매개변수로 사용됩니다. 이 경로로 이동할 때 phoneId에 대한 값을 제공해야 합니다.

쿼리 매개변수는 물음표 뒤에 URL에 추가되며 추가 데이터를 전달하는 데 사용할 수 있습니다:

```js
GoRoute(
  path: PhoneScreen.routePath,
  name: PhoneScreen.routeName,
  builder: (context, state) => PhoneScreen(
    extra: state.extra as String?,
    phoneName: state.uri.queryParameters['phoneName'],
  ),
);
```

이 경로로 이동할 때 URL에 쿼리 매개변수를 포함할 수 있습니다.

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

예시 사용법:

PhoneScreen으로 이동하고 경로 및 쿼리 매개변수를 모두 전달하려면 이렇게 할 수 있어요.

```js
context.goNamed(
  PhoneScreen.routePath,
  extra: '0334-2064807',
  pathParameters: {'phoneId': 'UFONE'},
  queryParameters: {'phoneName': 'iPhone'},
);
```

여기서 phoneId는 경로 매개변수이고, phoneName은 쿼리 매개변수입니다. 이 접근 방식을 통해 다음 화면으로 자세한 정보를 전달하여 앱의 탐색을 더 동적이고 유연하게 만들 수 있어요.

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

# 동적 라우팅 구성

루팅 구성을 사용하여 런타임에 라우트를 추가하세요:

```js
RoutingConfig _generateRoutingConfig() {
  return RoutingConfig(
    routes: <RouteBase>[
      GoRoute(
        path: OnBoardingScreen.routePath,
        name: OnBoardingScreen.routeName,
        builder: (context, state) => const OnBoardingScreen(),
        routes: [
          GoRoute(
            path: '${PhoneScreen.routePath}/:phoneId',
            name: PhoneScreen.routeName,
            builder: (context, state) => PhoneScreen(
              extra: state.extra as String?,
              phoneId: state.pathParameters['phoneId'],
            ),
            routes: [
              GoRoute(
                path: VerificationScreen.routePath,
                name: VerificationScreen.routeName,
                builder: (context, state) => const VerificationScreen(),
              ),
            ],
          ),
          GoRoute(
            path: SkipScreen.routePath,
            name: SkipScreen.routeName,
            builder: (context, state) => const SkipScreen(),
          ),
        ],
      ),
      if (isNewRouteAdded)
        GoRoute(
          path: BottomNavigationScreen.routePath,
          name: BottomNavigationScreen.routeName,
          builder: (context, state) => const BottomNavigationScreen(),
        ),
    ],
  );
}

bool isNewRouteAdded = false;

final ValueNotifier<RoutingConfig> myRoutingConfig = ValueNotifier<RoutingConfig>(
  _generateRoutingConfig(),
);

final GoRouter router = GoRouter.routingConfig(routingConfig: myRoutingConfig);

void addNewRoute() {
  isNewRouteAdded = true;
  myRoutingConfig.value = _generateRoutingConfig();
}
```

# 쉘 라우트를 사용한 중첩 내비게이션

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

추가 Navigator를 추가하려면 ShellRoute를 사용하고 위젯을 반환하는 빌더를 제공하세요:

```js
final GlobalKey<NavigatorState> _rootNavigatorKey = GlobalKey<NavigatorState>(debugLabel: 'root');
final GlobalKey<NavigatorState> _shellNavigatorKey = GlobalKey<NavigatorState>(debugLabel: 'shell');

final routers = GoRouter(
  navigatorKey: _rootNavigatorKey,
  initialLocation: OnBoardingScreen.routePath,
  routes: [
    GoRoute(
      path: OnBoardingScreen.routePath,
      name: OnBoardingScreen.routeName,
      builder: (context, state) => const OnBoardingScreen(),
      routes: [
        GoRoute(
          path: '${PhoneScreen.routePath}/:phoneId',
          name: PhoneScreen.routeName,
          builder: (context, state) => PhoneScreen(
            extra: state.extra as String?,
            phoneId: state.pathParameters['phoneId'],
            phoneName: state.uri.queryParameters['phoneName'],
          ),
          routes: [
            GoRoute(
              path: VerificationScreen.routePath,
              name: VerificationScreen.routeName,
              builder: (context, state) => const VerificationScreen(),
            ),
          ],
        ),
        GoRoute(
          path: SkipScreen.routePath,
          name: SkipScreen.routeName,
          builder: (context, state) => const SkipScreen(),
        ),
      ],
    ),
    ShellRoute(
      navigatorKey: _shellNavigatorKey,
      builder: (BuildContext context, GoRouterState state, Widget child) {
        return BottomNavigationScreen(child: child);
      },
      routes: <RouteBase>[
        GoRoute(
          path: HomeScreen.routePath,
          name: HomeScreen.routeName,
          builder: (context, state) => const HomeScreen(),
          routes: [
            GoRoute(
              path: DetailsScreen.routePath,
              builder: (context, state) => const DetailsScreen(label: 'Home'),
            ),
          ],
        ),
        GoRoute(
          path: ProfileScreen.routePath,
          name: ProfileScreen.routeName,
          builder: (context, state) => const ProfileScreen(),
          routes: [
            GoRoute(
              path: DetailsScreen.routePath,
              builder: (context, state) => const DetailsScreen(label: 'Profile'),
            ),
          ],
        ),
        GoRoute(
          path: SettingsScreen.routePath,
          name: SettingsScreen.routeName,
          builder: (context, state) => const SettingsScreen(),
          routes: [
            GoRoute(
              path: DetailsScreen.routePath,
              builder: (context, state) => const DetailsScreen(label: 'Settings'),
            ),
          ],
        ),
      ],
    ),
  ],
);
```

# 예제 UI 코드

```js
import 'package:flutter/material.dart';
import 'package:go_router/go_router.dart';
import 'package:navigation_for_practice/screens/details.dart';

class HomeScreen extends StatelessWidget {
  static const routeName = 'home-screen';
  static const routePath = '/$routeName';

  const HomeScreen({super.key});

  @override
  Widget build(BuildContext context) {
    return Scaffold(
      appBar: AppBar(
        title: const Text('Home'),
      ),
      body: Center(
        child: ElevatedButton(
          child: Text(
            'Home Detail',
            style: Theme.of(context).textTheme.headlineMedium,
          ),
          onPressed: () => GoRouter.of(context).go("$routePath/${DetailsScreen.routeName}"),
        ),
      ),
    );
  }
}

class DetailsScreen extends StatelessWidget {
  static const routeName = 'detail-screen';
  static const routePath = routeName;

  const DetailsScreen({
    required this.label,
    super.key,
  });

  final String label;

  @override
  Widget build(BuildContext context) {
    return Scaffold(
      appBar: AppBar(
        title: const Text('Details Screen'),
      ),
      body: Center(
        child: Text(
          'Details for $label',
          style: Theme.of(context).textTheme.headlineMedium,
        ),
      ),
    );
  }
}
```

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

Flutter에서 GoRouter를 사용하면 응용 프로그램 내에서의 탐색을 효과적으로 관리할 수 있습니다. 기본 및 중첩된 경로를 설정하는 방법, context.go() 및 context.goNamed()을 사용하여 탐색을 처리하는 방법, 그리고 경로 및 쿼리 매개변수를 효율적으로 활용하는 방법을 이해함으로써, 더욱 효율적이고 사용자 친화적인 탐색 경험을 제공할 수 있습니다. 또한 동적으로 경로를 추가하고 ShellRoute를 통한 중첩 탐색을 사용할 수 있는 기능은 응용 프로그램의 탐색 시스템의 유연성과 성능을 높일 수 있습니다. 이러한 기술을 채택하면 견고하고 확장 가능한 Flutter 응용 프로그램을 구축할 수 있습니다.

여기 프로젝트 예제가 있어요 👇🏿👇🏿

기사에 최대 50번까지 박수를 할 수 있다는 것을 알고 계셨나요? 한 번 시도해보세요!
