---
title: "GUID UUID 기본 키 사용으로 SQL 쿼리 성능을 두 배로 향상시키는 방법"
description: ""
coverImage: "/assets/img/2024-08-17-HowWeDoubledthePerformanceofOurAppWhenUsingGUIDUUIDPrimaryKeysinSQL_0.png"
date: 2024-08-17 00:37
ogImage:
  url: /assets/img/2024-08-17-HowWeDoubledthePerformanceofOurAppWhenUsingGUIDUUIDPrimaryKeysinSQL_0.png
tag: Tech
originalTitle: "How We Doubled the Performance of Our App When Using GUID UUID Primary Keys in SQL"
link: "https://medium.com/@mattbentley_67939/how-we-doubled-the-performance-of-our-app-when-using-guid-uuid-primary-keys-in-sql-f9e43d228e1e"
isUpdated: true
updatedAt: 1723863689328
---

<img src="/assets/img/2024-08-17-HowWeDoubledthePerformanceofOurAppWhenUsingGUIDUUIDPrimaryKeysinSQL_0.png" />

SQL 데이터베이스에서 사용할 최상의 기본 키를 선택하는 것은 종종 균형을 맞추는 작업입니다. 현대적인 분산 시스템을 설계할 때, 대부분의 팀은 일반적으로 성능이 더 좋은 INT 또는 BIGINT Identity 열보다 전 세계적으로 고유한 GUID(UUID)를 선호합니다. 저는 항상 GUID 기본 키를 사용하는 것을 좋아합니다. 이유를 설명하고 GUID를 PK로 사용할 때의 성능 영향을 완화하는 방법을 보여줄 것입니다.

본 문서에서는 GUID를 기본 키로 사용할 때 SQL 테이블을 디자인하는 다양한 접근 방식을 다룰 것입니다. 서로 다른 접근 방식이 성능에 어떠한 영향을 미치는지 확인하기 위해 벤치마크를 살펴볼 것이며, 특정 사례에 도움이 된 몇 가지 추가 성능 최적화도 다룰 것입니다. 제가 .NET, Entity Framework Core 및 SQL Server를 사용하여 다양한 접근 방식을 설명할 것이지만, 사용된 기술은 어떤 언어나 SQL 버전에도 적용될 것입니다.

# 기본 키로 GUID를 사용하는 이유?

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

만약 SQL이 숫자 형식의 기본 키를 사용했을 때 가장 좋은 성능을 발휘한다면, GUID를 사용하는 이유는 무엇일까요..?!

## 애플리케이션 생성

GUID의 장점은 그것이 '글로벌하게 고유하다'는 점에서 나옵니다. 이는 우리가 소비하는 앱이 데이터베이스로부터 응답을 기다리지 않고 자체적으로 PK 값을 안전하게 생성할 수 있다는 것을 의미합니다. 숫자 형식의 PK는 일반적으로 자동으로 증가하는 Identity와 관련이 있습니다. 따라서 앱은 레코드를 데이터베이스에 저장한 후에야 값이 알려지게 됩니다.

저는 도메인 주도 설계를 열렬히 지지하는데, 이는 비즈니스/도메인 로직을 Entity 클래스에 최대한 캡슐화하는 것을 장려합니다. 데이터베이스가 Entity의 가장 중요한 속성, 즉 ID를 생성하게 하는 것은 DDD의 핵심 원칙 중 하나를 엄청나게 위반하는 일입니다.

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

## 보안 및 개인 정보 보호

숫자 ID 열과 달리 UUID는 예측 가능한 패턴을 따르지 않습니다. 이는 공격자가 삽입된 레코드의 순서를 추측하거나 추론하기가 더 어렵도록하여 보안과 개인 정보 보호를 강화할 수 있습니다. 자동 증가 ID는 또한 데이터베이스 테이블에 얼마나 많은 데이터가 있는지 고객에게 노출시킬 수 있으며, 사용 사례에 따라 이 역시 문제가 될 수 있습니다.

# 주요 키로 GUID를 사용하는 것의 단점

유같은 주요 키로 GUID를 사용하는 것에는 몇 가지 큰 단점이 있습니다. 이러한 단점에 대해 설명하기 전에 먼저 이해해야 할 몇 가지 관계형 데이터베이스 개념이 있습니다.

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

## 단편화

데이터베이스의 단편화는 데이터가 디스크나 메모리에 연속적으로 저장되지 않아 비효율적인 액세스 패턴을 유발할 때 발생합니다. 단편화에는 두 가지 유형이 있습니다:

내부 단편화:

데이터베이스 페이지(또는 블록) 내의 저장 공간이 완전히 활용되지 않을 때 발생합니다. 예를 들어, 한 페이지에 100개의 행을 저장할 수 있는 공간이 있지만 실제로는 80개만 저장된 경우, 나머지 공간이 낭비되어 효율성이 떨어집니다.

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

외부 단편화:

데이터의 논리적 순서가 디스크의 물리적 순서와 일치하지 않을 때 발생합니다. 예를 들어, GUID를 사용할 때와 같이 비연속 삽입으로 인해 레코드가 임의의 순서로 저장될 경우, 데이터를 검색하기 위해 데이터베이스가 디스크의 다른 위치를 이동해야 하므로 성능이 느려질 수 있습니다.

## 페이지 분할

페이지 분할은 데이터베이스 페이지(대부분의 시스템에서 일반적으로 8 KB 크기의 고정 크기 저장 블록)가 가득 차고 추가 데이터가 삽입되어야 할 때 발생합니다.

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

- 초기 상태: 100행의 데이터를 담을 수 있는 페이지를 상상해보세요. 현재 페이지가 가득 찬 상태입니다.
- 삽입 트리거: 새로운 행을 삽입해야 할 때, 그리고 페이지가 가득 찼을 때, 데이터베이스 엔진은 이 새로운 행을 수용할 방법을 찾아야 합니다.
- 분할 과정: 데이터베이스 엔진은 가득 찬 페이지를 두 개의 페이지로 분할하여 미래의 삽입을 위한 공간을 확보합니다.
- 영향: 새 페이지를 할당하고, 기존 행을 이동시키고, 새로운 페이지 구조를 반영하기 위해 인덱스를 업데이트하기 위해 I/O가 필요합니다.

## 클러스터형 인덱스

클러스터형 인덱스는 테이블의 데이터의 물리적인 순서를 결정합니다. 따라서 한 테이블당 하나의 클러스터형 인덱스만 있을 수 있습니다. 기본적으로 주 키가 클러스터형 인덱스로 사용됩니다.

단편화와 페이지 분할의 영향으로 인해, 가장 효율적인 클러스터형 인덱스는 예측 가능한 순서를 가진 연속적인 키일 것입니다. 이를 통해 SQL이 효율적으로 테이블 데이터의 끝에 페이지를 채우며, 성능이 향상되고 데이터 및 인덱스를 저장하는 데 필요한 공간이 줄어듭니다.

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

## GUID 주 키의 단점

만약 GUID 주 키가 클러스터드 인덱스인 경우, 데이터는 순서대로 저장되지 않을 것이며 테이블에서 매우 높은 단편화와 페이지 분할이 발생할 것입니다. 데이터 양이 증가함에 따라 성능이 떨어질 수 있습니다.

GUID는 16바이트이며 정수의 4바이트와 비교해 더 큰 용량을 차지합니다. 더 큰 키는 더 많은 저장 공간과 더 오래 걸리는 인덱스 탐색 시간을 의미합니다. 클러스터드 인덱스는 비클러스터드 인덱스에도 저장되어 추가 저장 공간 비용이 두배가 될 수 있습니다.

# 정말 중요한가요?

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

우리 모두는 소프트웨어 아키텍처가 해결하려는 문제에 따른 적절한 균형을 유지해야 한다는 것을 알고 있어요. 여기서는 성능 및 확장성 대 보안 및 앱 기능 사이의 교환 비교를 살펴보고 있습니다. 제 개인적인 견해는 앱이 생성할 수 있는 GUID PK가 숫자 PK가 제공하는 성능 이점보다 훨씬 크다고 생각해요.

현실은 이 결정이 100,000개 이상의 행으로 구성된 데이터에 영향을 미칠 가능성이 거의 없다는 것입니다. 또한 성능 및 확장성이 사용 사례에 얼마나 중요한지에 따라 다릅니다.

다행히도 GUID PK를 사용하는 것이 꼭 필요하다고 결정한다면 성능을 최적화할 수 있는 옵션이 있습니다.

# 벤치마크

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

GUID를 기본 키 및 군집화된 인덱스로 사용하는 예제 테이블을 살펴봅시다. 성능 최적화를 적용하고 벤치마크를 사용하여 비교해보겠습니다.

## 기준 구현

이 예제에서는 varchar(max) 열에 이벤트를 JSON 직렬화한 배열을 저장하는 테이블을 사용합니다. 이는 이벤트 소싱을 사용할 때 SQL에 이벤트 스트림을 저장하는 간단한 사용 사례입니다. 이벤트 소싱을 사용할 때 이벤트 스트림은 항상 ID를 사용하여 쿼리하고 업데이트됩니다.

데이터베이스 스키마 변경을 관리하기 위해 Entity Framework Core 'code-first' 마이그레이션을 사용하는 것을 선호합니다. 위에 표시된 대로 IEntityTypeConfiguration을 사용하여 이를 구현할 수 있습니다. 생성된 기준 테이블 스키마와 일부 이벤트 스트림 데이터가 아래에 표시됩니다.

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

![이미지](/assets/img/2024-08-17-HowWeDoubledthePerformanceofOurAppWhenUsingGUIDUUIDPrimaryKeysinSQL_1.png)

여기에서 볼 수 있듯이 데이터가 삽입된 순서대로 저장되지 않으므로 각 행의 CreatedDate 값이 뒤죽박죽입니다. 이로 인해 상당한 단편화와 페이지 분할이 발생할 수 있습니다.

벤치마크는 관련 테이블에 600만 개의 이벤트 스트림 행을 삽입하고 데이터를 120만 번 업데이트합니다. 벤치마크는 12개 스레드로 병렬로 실행됩니다. 전체 벤치마크 코드는 제 GitHub에서 확인할 수 있습니다.

![이미지](/assets/img/2024-08-17-HowWeDoubledthePerformanceofOurAppWhenUsingGUIDUUIDPrimaryKeysinSQL_2.png)

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

## 최적화 #1: 비 클러스터화된 기본 키

기본 구현 방식은 GUID Id 열을 기본 키 및 클러스터형 인덱스로 사용합니다. 비순차적인 GUID 클러스터형 인덱스가 문제입니다.

대신에 GUID Id를 비 클러스터화된 인덱스로 변경할 수 있습니다. 또한 순차적으로 증가하는 숫자 Identity 열을 사용하여 클러스터형 인덱스에 추가 열을 추가할 수 있습니다. 새 Identity 열은 데이터가 연속적으로 저장되도록 보장합니다. 이렇게 함으로써 데이터가 항상 테이블의 끝에 삽입되어 최소한의 단편화 및 페이지 분할이 발생합니다.

새로운 SequentialId 열을 SequentialEventStream 구현에 추가해야 하지만, 앱에서 다른 용도로 사용할 필요는 없습니다. 우리는 새로운 비 클러스터화된 인덱스를 사용하여 여전히 GUID Id로 조회할 것입니다.

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

이제 우리는 데이터가 삽입된 순서대로 테이블에 저장되어 있다는 것을 알 수 있습니다.

![image](/assets/img/2024-08-17-HowWeDoubledthePerformanceofOurAppWhenUsingGUIDUUIDPrimaryKeysinSQL_3.png)

이벤트 스트림을 순차적으로 저장하는 것은 성능에 미치는 영향이 엄청납니다. 삽입은 벤치마크 전체에서 40% 더 빨랐고 업데이트는 26% 더 빨랐습니다.

![image](/assets/img/2024-08-17-HowWeDoubledthePerformanceofOurAppWhenUsingGUIDUUIDPrimaryKeysinSQL_4.png)

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

## 최적화 #2: 연속 GUID 기본 키

첫 번째 최적화는 엄청난 개선을 가져오지만, 테이블 스키마를 크게 변경해야 한다는 것이 단점입니다. 우리는 연속 GUID를 사용하여 데이터에 대해 연속된 순서를 얻을 수도 있습니다. 새로운 UUID v7 명세는 실제로 이를 달성하기 위해 시간순으로 정렬된 값이 필요합니다. 또한 연속 GUID/UUID를 생성하기 위해 사용할 수 있는 다양한 라이브러리들이 있습니다. 이 예시에서는 Entity Framework Core에 직접 내장된 SequentialGuidValueGenerator를 사용하고 있습니다.

이 구현의 큰 이점 중 하나는 원래 테이블의 스키마를 전혀 변경할 필요가 없다는 것입니다.

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

벤치마크는 분산 시스템을 모방하기 위해 12개 스레드에서 병렬로 실행됩니다. 이로 인해 생성된 GUID Id가 약간 순서에서 벗어날 수 있지만, 모든 삽입은 여전히 테이블의 끝에 있기 때문에 큰 영향을 미치지 않아야 합니다.

이 최적화는 이전 구현보다 심지어 더 빨랐습니다. 이제 원래의 기준선보다 48% 빠른 삽입 및 36% 빠른 업데이트를 달성하고 있습니다. 성능의 추가 증대는 더 이상 추가적인 비클러스터화된 인덱스가 필요하지 않기 때문입니다. 비클러스터화된 인덱스는 데이터를 조회할 때 클러스터화된 인덱스로의 추가 조회가 필요하며, 삽입이 발생할 때 유지되어야 합니다.

<img src="/assets/img/2024-08-17-HowWeDoubledthePerformanceofOurAppWhenUsingGUIDUUIDPrimaryKeysinSQL_6.png" />

## 최적화 #3: 압축

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

최종 최적화는 GUID 기본 키와 관련이 없지만, 이해하는 데 흥미로운 부분입니다. 이벤트 열에 저장된 데이터가 JSON 문자열이기 때문에 상당히 압축될 수 있습니다.

이벤트 데이터는 GZip을 사용하여 압축하고 사용자 지정 Entity Framework ValueConverter를 통해 처리할 수 있습니다.

이제 값은 varchar 열이 아닌 varbinary 열에 저장되어야 합니다.

![이미지](/assets/img/2024-08-17-HowWeDoubledthePerformanceofOurAppWhenUsingGUIDUUIDPrimaryKeysinSQL_7.png)

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

GUID 주 키를 순차적으로 정렬하고 GZip 압축을 사용하면 벤치마크를 통해 Inserts가 51% 더 빠르고 Updates도 48% 더 빠르다고 결과를 얻었어요.

![Check out how to double the performance of your app by using GUID primary keys in SQL](/assets/img/2024-08-17-HowWeDoubledthePerformanceofOurAppWhenUsingGUIDUUIDPrimaryKeysinSQL_8.png)

여기서 논의한 최적화 방법 모두 GUID 주 키 사용 시 앱 성능을 향상시키는 훌륭한 방법입니다. 어떤 옵션을 선택할지는 테이블에 이미 데이터가 있는지와 데이터베이스에 스키마 변경을 도입하기 쉬운지에 따라 다를 겁니다.

데이터가 100,000행 미만인 경우 이러한 최적화 중 어느 것도 성능에 영향을 끼치지 않을 가능성이 높습니다. 그러나 앞으로 데이터가 얼마나 커질지 확신이 없다면 시작부터 순차 GUID를 ID로 사용하는 것에 뭐가 잘못될까요!

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

이 게시물의 모든 벤치마크 코드는 아래의 내 GitHub에서 찾을 수 있습니다.
