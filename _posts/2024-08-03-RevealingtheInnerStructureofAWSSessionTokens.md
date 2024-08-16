---
title: "AWS 세션 토큰의 내부 구조를 밝히는 방법"
description: ""
coverImage: "/assets/img/2024-08-03-RevealingtheInnerStructureofAWSSessionTokens_0.png"
date: 2024-08-03 20:07
ogImage: 
  url: /assets/img/2024-08-03-RevealingtheInnerStructureofAWSSessionTokens_0.png
tag: Tech
originalTitle: "Revealing the Inner Structure of AWS Session Tokens"
link: "https://medium.com/@TalBeerySec/revealing-the-inner-structure-of-aws-session-tokens-a6c76469cba7"
---


요약: AWS 세션 토큰에 대한 세계 최초의 역공학 분석입니다. 우리의 연구 이전에는 이러한 토큰이 완전한 블랙박스였습니다. 오늘, 우리는 코드와 도구를 공유하여 AWS 세션 토큰을 프로그래밍적으로 분석하고 수정할 수 있도록 만들어 더 투명한 상자로 만들고 있습니다. 이 코드를 사용하여 AWS 세션 토큰의 내용을 깊이 들여다보고 AWS 암호화 및 인증 프로토콜에 대한 알려지지 않은 사실을 드러내며 마지막으로 위조 공격에 대한 그들의 저항성을 시험했습니다 (스포일러 경고: 의외로 좋은 결과!)

아마존 AWS는 세계에서 가장 인기 있는 클라우드 컴퓨팅 서비스입니다. 그러나 내부 인증 (AuthN) 및 권한 부여 (AuthZ) 프로토콜에 대해 공개적으로 알려진 것은 매우 적습니다.

공격자의 주요 작업법 (MO)는 초기 감염된 기기에서 목표로 이동하는 것인데, 이를 위해 환경의 인증 및 권한 부여 프로토콜을 유효한 자격 증명을 통해 악용합니다. 이러한 자격 증명은 공격자가 추측, 도난 또는 위조를 통해 획득합니다. 기술적 세부 사항은 온프레미스 환경 또는 클라우드에 따라 다르지만, 공격자의 목적과 목표는 동일하게 유지됩니다.

그러나 온프레미스 환경에서는 관련 인증 및 권한 부여 프로토콜이 공개 표준입니다 (예: Kerberos 프로토콜). 반면 AWS 환경에서는 이러한 세부 사항이 대부분 공개되지 않고 벤더에 의해 알려지지 않은 상태로 남아 있습니다.

<div class="content-ad"></div>

우리는 수비수와 건설자가 그들의 환경의 인증 및 권한 부여 프로토콜과 그로 인해 발생하는 자격 증명 구조를 이해하는 것이 극히 중요하다고 믿기 때문에 AWS 공식 문서에서 누락된 부분들을 역공학 해야 합니다.

이전에 우리는 AWS KEY IDs 자격 증명의 내부 구조를 탐구했습니다.

오늘은 훨씬 다양하고 풍부한 세션 토큰에 대해 다루려고 합니다!

# AWS 세션 토큰 소개

<div class="content-ad"></div>

'원래 사용자들은 AWS 서비스에 대한 각각의 요청에 사인하기 위해 지정된 장기 자격 증명(ID 및 Secret으로 구성)을 사용했습니다.

![Image](/assets/img/2024-08-03-RevealingtheInnerStructureofAWSSessionTokens_0.png)

요청을 유효하게 하기 위해 AWS는 사용자의 Secret을 DB에서 검색( IAM / ARS 서비스를 통해), 해당 Secret으로 요청에 사인을 하고 이와 일치하는지 확인했습니다.

그러나 이 방법은 모든 종류의 고급 시나리오를 지원하는 데 충분하지 않았습니다.'

<div class="content-ad"></div>

- 고급 인증 방식: Okta와 같은 연합 로그인 및 Yubikey와 같은 다중 인증
- 역할 가정: 때로는 사용자가 특정 작업을 수행하기 위해 다른 권한 집합으로 연결하길 원할 수도 있습니다. 예: 관리자로서 로그인하려고 하는 경우

이러한 문제를 해결하기 위해 AWS는 Security Token Service (STS)를 소개했습니다. 사용자가 STS 서비스에 인증하면, 장기 자격 증명과 마찬가지로 단기 ID와 비밀번호를 받게 되지만 추가로 세션 토큰도 받습니다.

이전과 같이 사용자는 AWS 서비스로의 각 요청에 할당된 ID와 비밀번호를 사용해야 하지만 이제 세션 토큰도 포함해야 합니다.

세션 토큰에는 암호화된 사용자 ID와 비밀번호가 포함되어 있으므로 AWS가 세션 토큰을 해독하고 추출할 수 있습니다. 비밀번호가 추출되면 AWS 서비스는 요청을 검증하는 데 사용할 수 있습니다.

<div class="content-ad"></div>

![AWS Session Tokens](/assets/img/2024-08-03-RevealingtheInnerStructureofAWSSessionTokens_1.png)

또한, 세션 토큰에는 유출된 자격 증명의 노출을 제한하기 위해 15분부터 36시간 사이의 유효 기간을 가질 수 있습니다.

결과적으로 AWS STS 세션 토큰은 일시적이고 제한된 권한을 갖는 자격 증명을 통해 AWS 자원에 액세스할 수 있도록 하여 AWS의 전체적인 보안과 액세스 제어를 강화하는 데 중요한 역할을 합니다.

# 연구 동기

<div class="content-ad"></div>

내부 구조에 대해 왜 신경 써야 하는지:

-개인 정보 보호: 세션 토큰에는 흥미로운 및 민감한 데이터가 포함되어 있습니다. 만료된 토큰을 안전하게 공유할 수 있는지, 아니면 반드시 먼저 삭제해야 하는지 알아야 합니다. 그렇다면, 어떻게 삭제해야 하며 민감한 데이터가 더 이상 포함되지 않도록 보장할까요? 로그에 토큰을 저장해도 안전할까요?
- 무결성: 위에서 설명한대로, 공격자의 목표는 공격자가 선택한 역할(예: 관리자)을 포함한 유효한 토큰을 위조하거나 만료 시간을 편집하여 영구 토큰을 생성하는 골든 크레덴셜 공격입니다. 공격자가 골든 토큰을 만드는 데 어떤 것을 방해할까요?

![이미지](/assets/img/2024-08-03-RevealingtheInnerStructureofAWSSessionTokens_2.png)

- 철학: 세션 토큰 구조에 대한 깊은 이해는 아마도 전체 AWS 인증 시스템을 이해하는 데 도움이 될 것입니다.
- 호기심: 그저 존재하기 때문에요!

<div class="content-ad"></div>

# 연구

가독성을 위해, 연구과정을 여러 단계의 선형 프로세스로 설명했습니다. 그러나 실제로는 새로운 통찰력과 가설이 수집될 때마다 도구 및 샘플 수집에 변화를 초래해 주기적인 성격을 띄고 있습니다.

## 단계 1: 원시 토큰부터 파싱된 필드까지

충분히 메타적인 이야기는 여기까지!

<div class="content-ad"></div>

일반적인 세션 토큰이 보통 이렇게 생겼어요: 인쇄 가능한 문자로 이루어진 긴 문자열이에요.

```js
"IQoJb3JpZ2luX2VjEDoaCXVzLWVhc3QtMiJIMEYCIQDQh4gelDqno96q39RwiPT5x7K7SyVOSmeDpUMd9SthWAIhAP5tT81Cb+Rb2zN85delmYB4KECmW1uL7Tr36C/M2GaJKr0DCKP//////////wEQARoMNjY2MzU5NzY0NTI4Igyu9F2yAqZN3dG0q9YqkQMVrg/4mCJjDxg0QmplU581Z2P8LGhGfr9vgei6SaONhhfks5Kt9Ikbh61G9UiQ3SXgPLbHjOfTUueaIIcBz1Y3LcW+WajtfsGfB8CqT76lkJLtkvl+1KjSCVn6k+/K/iWgr3Zc1Ej+qT2djTH4x1OWFNS6i6iCtlUy/Z6i3P2fziHGsEmafkH3ict+07dFb3DA2aRnUhnaCHfQDNd/5ub70oILwB4UgtgGNkbM9SE/NxKgPZY9qIktYifqcgfDyYMYHlvY9XEc0UT2jfaQKDYVgMCdsdsW5mkoBYzLRisQhKxjfwaBpkRtdW8dEHFAG04eV4JSAbOSat3bgUwahATGizOdsMz/qhnS9qzShQGgSR6OU6pDDUtuHCGh0sgwrjsZ+bGDfzkw5Sy3JhjQpozfinCsAmDZ1t3nX6llw9OR9B2mdDHCeccsWGwjIvmprs21FtgjDuKGzaAET6HgQAR+pkFUgxBWVmZArtck1ziG21FEN8pFR75rOgxSkQ3yEZeDZkIIZ/aJnABGvbC3Fbq9ATD6ycuKBjqlAaGPeFKzdCR1dBh4sHQVHejXNegWWZV72n4MLyZx2FE9wLUfPGXXW+pYZg4SySvN0Z4OnGoYdlO/pjKvdRa507mSD8N8EhkwgpJMatFobJb0hsz7GY5flutVSkDfBDYkU91vpl7YCJ5rlvuR0I6iWe+K7smYj5hzm16YokWsRQ4EeWHo0peEJuqTZrZt/U4gHVsFpG44V8Yb6iRdZL78E+5xcgjeFw=="
```

당연히, 이것은 base64로 인코딩되어 있으며, 디코딩하면 다음과 같은 바이너리 버퍼(16진수로 인코딩)가 생성됩니다.

```js
21 0a 09 6f 72 69 67 69 6e 5f 65 63 10 3a 1a 09 75 73 2d 65 61 73 74 2d 32 22 48 30 46 02 21 00 d0 87 88 1e 94 3a a7 a3 de aa df d4 70 88 f4 f9 c7 b2 bb 4b 25 4e 4a 67 83 a5 43 1d f5 2b 61 58 02 21 00 fe 6d 4f cd 42 6f e4 5b db 33 7c e5 d7 a5 99 80 78 28 40 a6 5b 5b 8b ed 3a f7 e8 2f cc d8 66 89 2a bd 03 08 a3 ff ff ff ff ff ff ff ff 01 10 01 1a 0c 36 36 36 33 35 39 37 36 34 35 32 38 22 0c ae f4 5d b2 02 a6 4d dd d1 b4 ab d6 2a 91 03 15 ae 0f f8 98 22 63 0f 18 34 42 6a 65 53 9f 35 67 63 fc 2c 68 46 7e bf 6f 81 e8 ba 49 a3 8d 86 17 e4 b3 92 ad f4 89 1b 87 ad 46 f5 48 90 dd 25 e0 3c b6 c7 8c e7 d3 52 e7 9a 20 87 01 cf 56 37 2d c5 be 59 a8 ed 7e c1 9f 07 c0 aa 4f be a5 90 92 ed 92 f9 7e d4 a8 d2 09 59 fa 93 ef ca fe 25 a0 af 76 5c d4 48 fe a9 3d 9d 8d 31 f8 c7 53 96 14 d4 ba 8b a8 82 b6 55 32 fd 9e a2 dc fd 9f ce 21 c6 b0 49 9a 7e 41 f7 89 cb 7e d3 b7 45 6f 70 c0 d9 a4 67 52 19 da 08 77 d0 0c d7 7f e6 e6 fb d2 82 0b c0 1e 14 82 d8 06 36 46 cc f5 21 3f 37 12 a0 3d 96 3d a8 89 2d 62 27 ea 72 07 c3 c9 83 18 1e 5b d8 f5 71 1c d1 44 f6 8d f6 90 28 36 15 80 c0 9d b1 db 16 e6 69 28 05 8c cb 46 2b 10 84 ac 63 7f 06 81 a6 44 6d 75 6f 1d 10 71 40 1b 4e 1e 57 82 52 01 b3 92 6a dd db 81 4c 1a 84 04 c6 8b 33 9d b0 cc ff aa 19 d2 f6 ac d2 85 01 a0 49 1e 8e 53 aa 43 0d 4b 6e 1c 21 a1 d2 c8 30 ae 3b 19 f9 b1 83 7f 39 30 e5 2c b7 26 18 d0 a6 8c df 8a 70 ac 02 60 d9 d6 dd e7 5f a9 65 c3 d3 91 f4 1d a6 74 31 c2 79 c7 2c 58 6c 23 22 f9 a9 ae cd b5 16 d8 23 0e e2 86 cd a0 04 4f a1 e0 40 04 7e a6 41 54 83 10 56 56 66 40 ae d7 24 d7 38 86 db 51 44 37 ca 45 47 be 6b 3a 0c 52 91 0d f2 11 97 83 66 42 08 67 f6 89 9c 00 46 bd b0 b7 15 ba bd 01 30 fa c9 cb 8a 06 3a a5 01 a1 8f 78 52 b3 74 24 75 74 18 78 b0 74 15 1d e8 d7 35 e8 16 59 95 7b da 7e 0c 2f 26 71 d8 51 3d c0 b5 1f 3c 

<div class="content-ad"></div>

바이너리를 살펴보면 일부 복잡하지만 Type Length Value (TLV) 튜플과 비슷한 구조가 나타납니다.

많은 시도 끝에 깨달음이 찾아왔어요: 혹시 첫 번째 바이트가 메시지의 유형을 표시하고 나머지 메시지와는 다른 방식으로 인코딩되어 있는 것은 아닐까요?

첫 번째 바이트(메시지 버전을 나타냄, 아래 참조)를 제거하자 버퍼가 프로토버프 구조로 잘 디코딩되었어요!

프로토버프 디코더를 적용하면 AWS 세션 토큰의 필드로 구분된 구조적인 모습이 처음으로 나타납니다.

<div class="content-ad"></div>

```
![image](/assets/img/2024-08-03-RevealingtheInnerStructureofAWSSessionTokens_3.png)

이 단계의 결과를 요약하면:

- 메시지의 첫 번째 바이트는 버전을 나타냅니다.
- 메시지의 나머지 부분은 protobuf로 인코딩되어 있습니다.

이제 Token이 필드로 구문 분석되었으므로 “분할하고 정복할 수 있습니다”: 전체 버퍼를 검사하는 대신 각 필드로 따로 나누어 해당 부분을 개별적으로 분석할 수 있습니다.


<div class="content-ad"></div>

## 단계 2: 연구 도구 구축

각 토큰 필드를 효율적으로 식별, 분석 및 조작하기 위해 두 가지 새로운 오픈 소스 도구를 만들었습니다:

- AWS 토큰 디코더 웹 앱: protobuf-decoder를 기반으로 AWS 토큰 전용으로 특화된 새로운 도구를 만들었습니다. 이 도구는 첫 번째 바이트를 제거한 후 버퍼를 protobuf로 구문 분석을 시도합니다.

- STS 토큰 디코더: 위에서 언급한 AWS 토큰 디코더의 느슨한 protobuf 파서 출력을 사용하여 토큰 필드의 유형 (예: 문자열, 숫자, 구조체)을 분석하고 엄격한 protobuf 토큰 스키마(.proto 파일)를 생성할 수 있습니다.

<div class="content-ad"></div>

이 .proto 파일 schemes를 사용하여 클래스로 컴파일할 수 있으며 (우리 경우 파이썬), 이러한 구문 분석된 버퍼의 필드 내용에 프로그래밍적으로 액세스할 수 있습니다. 이를 수행하고 이러한 클래스를 공유 STS 토큰 디코더를 오픈 소스로 공유하여 기능을 추가했습니다.

이를 통해 다음과 같은 기능을 얻을 수 있습니다:

- 프로그래밍 방식으로 토큰 분석: 많은 토큰을 신속하게 분석할 수 있습니다.
- 프로그래밍 방식으로 토큰 합성: 기존 토큰을 편집할 수 있으며 protobuf가 생성한 클래스가 버퍼를 올바르게 인코딩하고 조정하도록 처리할 것이며(크기 업데이트 포함)

새롭게 개발된 도구뿐만 아니라 이전에 존재하던 awscurl을 활용하여 임의의 토큰을 사용하여 AWS 서명된 요청을 보낼 수 있습니다. 이는 STS 토큰 디코더로 생성된 합성 및 수정된 토큰을 포함하여 귀하의 임의의 토큰을 사용하여 AWS에 서명된 요청을 보낼 수 있도록합니다.

<div class="content-ad"></div>

## 단계 3: 연구 말뭉치 구축

이 필드들의 역할과 의미를 정확히 이해하기 위해, 우리는 여러 계정과 환경에서 샘플을 많이 포함한 다양한 연구 말뭉치를 만들어야 했습니다.

우리는 이러한 데이터를 얻는 두 가지 주요 방법이 있었습니다.

- 제1자 생성 토큰: 우리가 제어하는 AWS 계정을 활용하여 AWS CLI get-session-token을 사용해 토큰을 생성할 수 있습니다.

<div class="content-ad"></div>


```js
aws sts get-session-token
```

우리가 계정을 제어할 수 있다는 사실은 환경 매개변수를 변경하고 해당 토큰에 미치는 영향을 관찰할 수 있게 됩니다. 이러한 매개변수에는 AWS 지역, 사용자 이름, 사용자 권한, 요청된 토큰 기간 등이 포함될 수 있습니다.

- 인터넷에서 공개적으로 공개된 토큰 수집: 이러한 토큰을 인터넷에서 검색하여 수집하면, 주로 지원 사례의 일환으로 게시된 것으로 얻어진 몇 가지 샘플이 있습니다.

이러한 토큰에 대해서는 생성 매개변수를 알거나 제어할 수 없지만, 이들은 토큰 랜드스케이프의 보다 다양한 그림을 제시해 줍니다. 그것은 공간 (다른 계정의 토큰) 뿐만 아니라 시간 (오래된 토큰)에 따라서도 새로운 면을 공개함으로써 토큰 포맷의 일부 오래된 버전을 밝혀냅니다.


<div class="content-ad"></div>

## 세션 토큰 역사에 대한 간략한 메모

세션 토큰의 주요 두 가지 변형을 확인했습니다.

- 버전 1 ("Global") 토큰: 2015년까지 STS 서비스는 전역 서비스로만 제공되었습니다. 여전히 STS 서비스와 상호 작용할 때 기본 설정입니다.
- 버전 2 ("Regional") 토큰: AWS는 지역 STS 서비스에서 얻은 이러한 토큰을 사용하는 것을 권장하며, 버전 1 토큰보다 여러 가지 이점을 제공합니다. 지역성이 토큰의 유효성에 부정적인 영향을 미치지 않지만, 오히려:

그러나 이 토큰은 기본 설정이 아니므로 아마도 버전 1 토큰의 크기를 기반으로 최대 토큰 크기가 더 짧다고 가정하는 시스템을 망가뜨릴 수 있기 때문입니다.

<div class="content-ad"></div>


![2024-08-03-RevealingtheInnerStructureofAWSSessionTokens_4](/assets/img/2024-08-03-RevealingtheInnerStructureofAWSSessionTokens_4.png)

이러한 버전들 속에서 우리는 다른, 더 낮은 유형 값으로 표시된 예전 변형을 식별했습니다. 총 2011년부터 오늘까지 이어지는 5가지 다른 STS 세션 토큰 변형을 발견했습니다.

버전은 첫 번째 Base64 디코딩된 바이트로 쉽게 식별할 수 있거나, 심지어 Base64로 인코딩된 토큰의 접두사로도 식별할 수 있습니다. 왜냐하면 첫 번째 필드들이 버전 당 고정되어 있기 때문입니다 (자세한 내용은 아래 참조).

![2024-08-03-RevealingtheInnerStructureofAWSSessionTokens_5](/assets/img/2024-08-03-RevealingtheInnerStructureofAWSSessionTokens_5.png)


<div class="content-ad"></div>

이전 버전 샘플은 주로 더 간단한 버전을 대표하므로 핵심 기능을 다루며 그것에 집중하는 데 도움이 됩니다.

## 단계 4: 연구 코퍼스에 도구 적용하기

이제 새로운 엄격한 프로토콜 버퍼 토큰 파서와 수집한 통찰력(아래 설명됨)을 위로한 원시 토큰에 적용해보겠습니다.

```js
 % python3 STS-session.py "IQoJb3JpZ2luX2VjEDoaCXVzLWVhc3QtMiJIMEYCIQDQh4gelDqno96q39RwiPT5x7K7SyVOSmeDpUMd9SthWAIhAP5tT81Cb+Rb2zN85delmYB4KECmW1uL7Tr36C/M2GaJKr0DCKP//////////wEQARoMNjY2MzU5NzY0NTI4Igyu9F2yAqZN3dG0q9YqkQMVrg/4mCJjDxg0QmplU581Z2P8LGhGfr9vgei6SaONhhfks5Kt9Ikbh61G9UiQ3SXgPLbHjOfTUueaIIcBz1Y3LcW+WajtfsGfB8CqT76lkJLtkvl+1KjSCVn6k+/K/iWgr3Zc1Ej+qT2djTH4x1OWFNS6i6iCtlUy/Z6i3P2fziHGsEmafkH3ict+07dFb3DA2aRnUhnaCHfQDNd/5ub70oILwB4UgtgGNkbM9SE/NxKgPZY9qIktYifqcgfDyYMYHlvY9XEc0UT2jfaQKDYVgMCdsdsW5mkoBYzLRisQhKxjfwaBpkRtdW8dEHFAG04eV4JSAbOSat3bgUwahATGizOdsMz/qhnS9qzShQGgSR6OU6pDDUtuHCGh0sgwrjsZ+bGDfzkw5Sy3JhjQpozfinCsAmDZ1t3nX6llw9OR9B2mdDHCeccsWGwjIvmprs21FtgjDuKGzaAET6HgQAR+pkFUgxBWVmZArtck1ziG21FEN8pFR75rOgxSkQ3yEZeDZkIIZ/aJnABGvbC3Fbq9ATD6ycuKBjqlAaGPeFKzdCR1dBh4sHQVHejXNeg...
```


<div class="content-ad"></div>

필드 유형은 렉스 프로토 버퍼 파서의 결과에서 얻었지만, 필드 이름과 의미는 이후의 연구를 통해 우리가 할당한 것입니다. (잘못된 경우가 있을 수 있습니다!)

각 필드 설명

- 유형: 33. 버전 2 토큰, 위 참조.
- 이름: 인쇄 가능한 문자열. 이 토큰 버전을 고유하게 식별하는 긴 접두사 base64를 생성하는 데 도움이 됩니다. (위 참조). “ec”는 타원 곡선 (아래의 DERSig 참조)를 나타낼 수 있습니다.
- signKeyId: DERSig의 서명 키 ID (아래 참조). 이 ID는 매 시간 증가합니다.
- 지역: 이 버전 2 토큰을 생성하는 STS 서비스의 AWS 지역.
- DERSig: DER 인코딩된 ECDSA 서명. 그 r, s 매개변수도 출력됩니다. 이것은 NIST256p 곡선과 sha256 해시를 사용하여 아래의 사용자 부분을 서명합니다. 자세한 내용은 이후의 “Token signature in depth” 섹션을 참조하세요.
- 사용자: 이 필드에는 다음 하위 필드가 포함됩니다

  - encryptKeyId: userEncryptedData의 암호화 키 ID. 이 ID는 매 시간 증가합니다.

<div class="content-ad"></div>

6.2. someId: 해당 의미를 찾지 못했습니다. 샘플에서 상대적으로 낮은 값을 가집니다 (0-5).

6.3. accountId: 토큰 발급자의 AWS 계정 ID입니다. 이 경우, 666359764528입니다.

6.4. IV: 모든 버전의 샘플에서 이 필드는 길이가 12바이트이며 무작위로 보입니다.

AWS 문서에 따르면

<div class="content-ad"></div>

그래서, AES-GCM로 암호화된 userEncryptedData의 초기화 벡터(IV)로 생각됩니다.

6.5. userEncryptedData: 이는 세션 토큰의 핵심입니다.

- 이 필드는 무작위로 보이지만 동일한 사용자의 연이은 요청에 대해서도 같은 길이를 유지하며, 아마도 AES-GCM으로 암호화되었을 가능성이 높습니다. (위 참조)
- 그 크기는 다른 사용자들 사이에서 변경될 수 있어서 사용자를 고려한 것으로 보입니다.
- 최소 세션 기간(900초)과 최대 기간(129600초)을 요청할 때, 이 필드는 한 바이트 증가합니다(변곡점은 16384), 이를 통해 만료 기간이 인코딩되어 있을 가능성이 큽니다.

<div class="content-ad"></div>

그래서 암호화된 사용자 ID 및 비밀값이 그곳에 암호화되어 있다고 가정합니다.

![이미지](/assets/img/2024-08-03-RevealingtheInnerStructureofAWSSessionTokens_6.png)

7. creationUnixtime: 이 경우에는 1632822522로 표시된 값으로, 2021년 09월 28일 12시 48분 42초로 매치되는 epoch 시간으로 표현된 생성일입니다. 이 필드는 로깅 목적으로만 포함되었을 가능성이 높습니다. 시간을 변경해도 토큰이 무효화되지 않으며 만료된 토큰을 유효하게 만들지도 않습니다. 앞서 말한대로, 토큰의 유효성과 만료에 대한 실제 중요 데이터는 userEncryptedData 필드에 인코딩되어 있는 것으로 가정합니다.

8. auxData: creationUnixtime과 마찬가지로 이 필드는 서명으로 보호되지 않으며 편집이 가능합니다. 따라서, 미션 중요 데이터가 포함되어 있지 않을 것으로 가정합니다. 사용자와 관련된 일부 암호화된 데이터일 것으로 가정하며, 동일한 사용자의 경우 길이는 동일하지만 다른 사용자 사이에서는 다를 수 있습니다.

<div class="content-ad"></div>

요약해 드리자면, 이 문서는 최신 버전의 버전 2 토큰에 대한 분석을 제공하고 있습니다. 이전 버전은 이 필드들의 부분 집합을 포함하고 있기 때문에 생략했습니다.

하지만 버전 1의 마지막 필드 (유형 23)에는 예외가 하나 있습니다.

```js
% python3 STS-session.py "FwoGZXIvYXdzEBAaDLHxhjed4A6ABQplMyKBAd0Jzohb7hRtcvWvjWSNw5bVcn5al0jGu9Cl7W2ijDztOnmLZICjbsFBYgO7mt2J1AM9CO0nrL9qBatm9+ytKde5MXuKyzMGY6J8YDLoXU625FQKpnGXelSQxA1mYI/VOjaSa2MP4gPZsgOBjyOuiRxUKmkgYglbzl8sGYco9KWSNyjK5/aKBjIoKnYXwjdTkOt7/Bw6HMETrjPUPyHStdSfCjt4IwGvu2ox5Xo8VHAp5g=="

유형:  23
{
  "name": "er/aws",
  "encryptKeyId": "16",
  "IV": "sfGGN53gDoAFCmUz",
  "userEncryptedData": "3QnOiFvuFG1y9a+NZI3DltVyflqXSMa70KXtbaKMPO06eYtkgKNuwUFiA7ua3YnUAz0I7Sesv2oFq2b37K0p17kxe4rLMwZjonxgMuhdTrbkVAqmcZd6VJDEDWZgj9U6NpJrYw/iA9myA4GPI66JHFQqaSBiCVvOXywZhyj0pZI3",
  "creationUnixtime": 1633530826,
  "unknown3": "KnYXwjdTkOt7/Bw6HMETrjPUPyHStdSfCjt4IwGvu2ox5Xo8VHAp5g=="
}
생성 시간: 2021년 10월 6일 17시 33분 46초
```

unknown3: 우리의 모든 샘플에서 이 필드는 40바이트이며 무작위로 보입니다. 따라서 이것은 특정 사용자와 무관한 암호화 관련 매개 변수라고 가정합니다. 이 필드가 버전 1 토큰의 이전 버전 (즉, 유형 21)에는 존재하지 않았기 때문에, 이것은 아마도 이 프로토콜의 핵심이 아님을 의미할 것입니다. 우리의 최상의 추측은 AWS가 2.0.x 버전에서 선택 사항이었던 AES-GCM 키 커밋먼트와 관련이 있다는 것입니다.

<div class="content-ad"></div>

## 토큰 서명 자세히 살펴보기

DERSig 필드는 처음에 우리의 느슨한 구문 분석기에 의해 잘못 분류되어 protobuf 구조체로 간주되었지만 실제로는 ASN.1 DER 인코딩 된 서명입니다.

![이미지](/assets/img/2024-08-03-RevealingtheInnerStructureofAWSSessionTokens_7.png)

우리는 이 버퍼 값이 Pieter Wuille에 의해 설명 된 해당 서명의 특성을 따르기 때문에 이 교육적 추측을 했습니다:

<div class="content-ad"></div>

ECDSA 서명의 좋은 기능 중 하나는 올바른 매개변수가 제공되면 다음을 복구할 수 있습니다:

- 서명할 텍스트
- 해싱 함수
- 타원 곡선

서명자의 공개 키를 복구할 수 있습니다. (실제로 두 개의 가능한 공개 키)

따라서 동일한 키로 서명된 두 개의 토큰을 얻을 수 있다면, 매개변수 추측의 정확성을 확인할 수 있습니다.

<div class="content-ad"></div>

당연히 다양한 가능성의 숫자가 매우 많기 때문에 다시 교육된 추측이 필요합니다.

우리는 타원 곡선이 NIST P-256이고 해싱 함수가 SHA-256임을 추측했는데, 이러한 매개변수는 AWS 인증에서 다른 곳에서 사용됩니다:

그런 다음 "user" 부분이 원시 프로토버프 인코딩으로 시그니처에 의해 보호된 부분인 것을 추측했습니다.

```js
vks = ecdsa.VerifyingKey.from_public_key_recovery(self.session_pb.DER_Sig, self.session_pb.user.SerializeToString(), curve, sha256, sigdecode_der )
```

<div class="content-ad"></div>

운이 좋게도 (실제로 많은 시행착오 끝에) 우리는 옳았습니다!

특정 시간에 특정 지역의 서명 키 공개 키를 식별하고 다른 사용자들 간에 동일한 임을 확인할 수 있었습니다.

```js
-----BEGIN PUBLIC KEY-----
MFkwEwYHKoZIzj0CAQYIKoZIzj0DAQcDQgAEjzuplh/vDM621Y4qNPmaVUM8TfyMstLGlu/9wT3M\nizt8SCDxslIHbNYu36khLM7mxqocy7jU3tJfNZKg+X2p3g==
-----END PUBLIC KEY-----
```

이 깨달음을 바탕으로, 이러한 키가 매 시간 변화하고 그 때마다 signKeyId가 증가함을 관찰할 수 있었습니다. 게다가, 이 통찰력을 사용하여 signKeyId가 실제로 이 키를 나타낸다는 것을 확인할 수 있었으며, 두 토큰 모두 동일한 지역에서 동일해야만 공개 키 추출이 성공할 수 있음을 알 수 있었습니다.

<div class="content-ad"></div>

# 보안 측면 고려 사항

## 위조 공격:

저희 도구를 사용하여 토큰을 편집하고 값 변경을 할 수 있었습니다. 그러나 이러한 수정된 토큰을 가지고 서명된 요청은 아래와 같았습니다:

- 영향을 줄 수 있는 필드를 변경할 때 AWS 서비스에 의해 거부됨 (서명이 무효화되기 때문)
- AWS 서비스에 의해 허용되었지만 시각적인 효과는 없었습니다 (예: 위에서 변경된 creationUnixtime), 아마도 AWS가 처음부터 서명이 없이 봐줄만한 이유입니다.

<div class="content-ad"></div>

## 골드 자격 증명 공격:

AWS 인증 시스템은 이러한 공격에 비교적 강건한 것으로 보입니다. AWS 암호화 및 서명 키는 빠르게(1시간) 업데이트되는 것으로 보입니다. 따라서, 공격자가 이러한 키를 얻더라도 무제한 액세스 시간이 제공되지 않고, 이 키가 사용 불가능해질 때까지 제한된 시간 동안만 액세스할 수 있을 것으로 보입니다.

## 좀비 토큰:

버전 2(권역 토큰)에서는 AWS가 설명한 대로 "자가 포함(self-contained)"되어 있습니다.

<div class="content-ad"></div>

따라서 SSO 제공업체에서 제거된 사용자의 경우 세션 토큰이 만료일까지 유효한 상태로 남아 있을 수도 있습니다.

## 개인정보 및 비공개정보:

일부 토큰에는 최소한 계정 ID와 생성 시간이 포함되어 있습니다. 이 정보를 비밀로 유지해야 한다고 생각한다면, 토큰을 공유하기 전에 필히 비공개 처리해야 합니다.

# 기여 및 핵심 내용

<div class="content-ad"></div>

인증 시스템과 프로토콜은 우리 정보 보안의 기초이며 AWS 세션 토큰은 AWS 인증 시스템의 중요한 부분을 차지합니다.

우리의 연구 이전에는 AWS 세션 토큰의 내부 작업이 완전한 블랙 박스였으며, 이 연구를 통해 그들을 좀 더 투명한 상자로 만들었습니다.

구체적으로 우리의 주요 기여는 다음과 같습니다:

- 이러한 토큰의 내부 구조를 발견하고 필드로 분해하는 것.
- 거의 모든 이러한 필드의 의미를 설명하는 것.
- AWS가 이러한 토큰에 사용하는 암호학적 기본 요소(예: 곡선, 해시 함수, 알고리즘)를 식별하는 것.
- 약 5가지의 이러한 세션 토큰 변형을 발견하는 것.
- 사용자 및 연구원이 이러한 토큰의 콘텐츠를 보고 조작할 수 있도록 두 개의 오픈 소스 도구를 만들고 공유하는 것.
- AWS 내부 암호 키 관리의 시간당 업데이트 및 관련 공개 키를 결정하는 방법을 제공하는 것.
- 이러한 토큰이 위조 공격에 대항하는 강도를 테스트하는 것.

<div class="content-ad"></div>

이 글의 다양한 잠재 독자를 위해 다음 요점을 강조하고자 합니다:

- 빌더들: 새로운 것을 배우고, 토큰을 공개적으로 공유할 때 적절하게 수정할 수 있게 되었으면 좋겠습니다.
- Amazon AWS: AWS 인증 프로토콜은 매우 안전한 것으로 보이며, 다른 프로토콜 (예: Kerberos, SAML)의 남용에서 배운 교훈을 적극 활용합니다. AWS가 이러한 프로토콜을 개방 표준으로 만들어주기를 희망하며, 이를 통해 보안 연구자들이 직접 보안을 평가할 수 있도록 하고, 프로토콜의 역분석에 시간을 투자하지 않아도 되기를 희망합니다.
- 동료 연구자들: 저희는 아직 이해하지 못한 부분이 남아있음을 알 수 있습니다. 다른 사람들이 이 연구와 도구를 추가 연구의 발판으로 활용하기를 희망합니다. 만약 이 글에서 새로운 발견이나 오류를 발견하신다면 알려주시기 바랍니다!

리뷰와 기타 지원에 감사드립니다. 