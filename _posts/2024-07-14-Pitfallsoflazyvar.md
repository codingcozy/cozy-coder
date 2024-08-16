---
title: "lazy var 사용 시 피해야 할 4가지 실수"
description: ""
coverImage: "/assets/img/2024-07-14-Pitfallsoflazyvar_0.png"
date: 2024-07-14 00:25
ogImage: 
  url: /assets/img/2024-07-14-Pitfallsoflazyvar_0.png
tag: Tech
originalTitle: "Pitfalls of `lazy var`"
link: "https://medium.com/swiftblade/pitfalls-of-lazy-var-f6970ddcb047"
isUpdated: true
---





![Pitfallsoflazyvar](/assets/img/2024-07-14-Pitfallsoflazyvar_0.png)

안녕하세요! 오늘은 'lazy var'에 대해 이야기해보려고 해요.

'lazy var'는 필요할 때만 복잡한 객체를 생성해 성능을 향상시킬 수 있어요. 하지만 여러 이유로 과도하게 사용하는 건 피하는 게 좋다고 생각해요.

- 'lazy var'의 초기화 순서를 추론하기 어렵게 만들어 디버깅을 어렵게 할 수 있어요.
- 서로를 참조하는 'lazy var'가 2개 있거나 순환을 만들 수 있다면 데드락이 발생할 수 있어요.
- 이중 초기화 문제로 인해 발견하기 어렵고 심각한 버그를 일으킬 수 있어요. 'lazy var'가 'init()'이 완료되지 않은 상태에서 여러 번 접근되는 경우 발생해요.
- 원자성을 가지지 않아요. 여러 스레드에서 초기화되지 않은 'lazy var'에 접근하면 프로세스가 충돌할 수 있어요.
- 'lazy var'는 값 타입과 잘 작동하지 않아요. 왜냐하면 'lazy var'는 암묵적으로 변이(mutating)하기 때문이에요.

따라서, 속성이 초기화하는 데 시간이 많이 걸리지 않고 어차피 언젠가는 사용된다면 'lazy var'로 만들지 말아야 해요.
'self'를 delegate로 설정해야 하는 경우, 대부분 'lazy var'로 만들곤 해요. 하지만 이는 init()에서도 순서적으로 올바르게 처리할 수 있다는 점을 명심해요.
init()에서 올바른 순서로 만들어주는 데 시간을 투자해보세요. 'lazy var'을 절대 사용하지 말아야하는 것은 아니지만, 사용하기 전에 모든 트레이드오프를 고려해보세요.