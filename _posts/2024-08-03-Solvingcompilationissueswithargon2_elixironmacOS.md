---
title: "macOS에서 argon2_elixir 컴파일 문제 해결 방법"
description: ""
coverImage: "/assets/img/2024-08-03-Solvingcompilationissueswithargon2_elixironmacOS_0.png"
date: 2024-08-03 20:16
ogImage: 
  url: /assets/img/2024-08-03-Solvingcompilationissueswithargon2_elixironmacOS_0.png
tag: Tech
originalTitle: "Solving compilation issues with argon2_elixir on macOS"
link: "https://medium.com/@njaustevedomino/solving-compilation-issues-with-argon2-elixir-on-macos-279e9c47c9db"
---


<img src="/assets/img/2024-08-03-Solvingcompilationissueswithargon2_elixironmacOS_0.png" />

엘릭서 개발자로써, 종속성을 부드럽게 컴파일하는 과정을 당연시하는 일이 많습니다. 그러나 문제가 발생하면 빌드 시스템, 컴파일러 플래그 및 플랫폼별 독특한 점으로 인해 끝없는 문제 해결 과정으로 이끌 수 있습니다. 이 글은 macOS에서 argon2_elixir 라이브러리의 지속적인 컴파일 문제를 해결하는 내 여정을 문서화하며, 그 과정에서 얻은 통찰과 배운 교훈을 제시합니다.

## 문제

모든 것은 Phoenix 엘릭서 프로젝트에 사용자 인증을 추가하는 보편적인 작업으로 시작되었습니다. 비밀번호 해싱을 위해 인기 있는 argon2_elixir 라이브러리를 선택했지만, mix deps.get을 실행하고 컴파일을 시도할 때 다음 오류가 발생했습니다:

<div class="content-ad"></div>

```js
➜  project-x git:(add-auth) ✗ mix deps.compile argon2_elixir --force
==> argon2_elixir
mkdir -p /Users/domino/Workspace/project-x/_build/dev/lib/argon2_elixir/priv
clang -g -O3 -pthread -Wall -Wno-format-truncation -I"/Users/domino/.asdf/installs/erlang/27.0/erts-15.0/include" -Iargon2/include -Iargon2/src -Ic_src -shared -fPIC -fvisibility=hidden -Wl,-soname,libargon2.so.0 argon2/src/argon2.c argon2/src/core.c argon2/src/blake2/blake2b.c argon2/src/thread.c argon2/src/encoding.c argon2/src/ref.c c_src/argon2_nif.c -o /Users/domino/Workspace/project-x/_build/dev/lib/argon2_elixir/priv/argon2_nif.so
warning: unknown warning option '-Wno-format-truncation' [-Wunknown-warning-option]
1 warning generated.
ld: unknown option: -soname 
clang: error: linker command failed with exit code 1 (use -v to see invocation)
make: *** [/Users/domino/Workspace/project-x/_build/dev/lib/argon2_elixir/priv/argon2_nif.so] Error 1
could not compile dependency :argon2_elixir, "mix compile" failed. Errors may have been logged above. You can recompile this dependency with "mix deps.compile argon2_elixir --force
could not compile dependency :argon2_elixir, "mix compile" failed. Errors may have been logged above. You can recompile this dependency with "mix deps.compile argon2_elixir --force", update it with "mix deps.update argon2_elixir" or clean it with "mix deps.clean argon2_elixir"
```

이 문제는 macOS Sequoia 15 Beta 운영 체제를 실행하는 M3 기기에서 발생했습니다. 다음 사양을 가지고 있습니다:

```js
➜  project-x git:(add-auth) ✗ sw_vers
ProductName:            macOS
ProductVersion:         15.0
BuildVersion:           24A5298h

➜  project-x git:(add-auth) ✗ uname -a 
Darwin Domino-MacBook 24.0.0 Darwin Kernel Version 24.0.0: Sat Jul 13 00:54:33 PDT 2024; root:xnu-11215.0.165.0.4~50/RELEASE_ARM64_T6030 arm64

➜  project-x git:(add-auth) ✗ gcc --version                        
Apple clang version 15.0.0 (clang-1500.3.9.4)
Target: arm64-apple-darwin24.0.0
Thread model: posix
InstalledDir: /Applications/Xcode.app/Contents/Developer/Toolchains/XcodeDefault.xctoolchain/usr/bin

➜  project-x git:(add-auth) ✗ make --version                        
GNU Make 3.81
Copyright (C) 2006  Free Software Foundation, Inc.
This is free software; see the source for copying conditions.
There is NO warranty; not even for MERCHANTABILITY or FITNESS FOR A
PARTICULAR PURPOSE.
```

이 정보는 문제가 최신 Apple Silicon 하드웨어와 매우 최신 macOS 버전에서 발생하며, 특정 빌드 도구 및 컴파일 플래그와의 호환성에 영향을 줄 수 있다는 점을 강조합니다.


<div class="content-ad"></div>

## 심층 탐구: 에러 이해하기

에러 메시지에서 두 가지 주요 문제가 발견되었습니다:

- 알 수 없는 경고 옵션 -Wno-format-truncation
- 알 수 없는 링커 옵션 -soname

이러한 에러는 macOS 시스템에서 적절하게 사용되지 않은 플랫폼 특정 컴파일레이션 플래그를 가리켰습니다.

<div class="content-ad"></div>

## Cross-Compilation 조사

Makefile에는 교차 컴파일을 확인하는 코드가 포함되어 있어 빌드 프로세스가 복잡해졌습니다:

```js
ifneq ($(CROSSCOMPILE),)
# Cross-compilation settings
else
# Native compilation settings
endif
```

이 확인은 서로 다른 컴파일 시나리오를 처리하기 위해 의도되었으나 macOS의 로컬 개발 환경에서 문제를 일으키고 있었습니다.

<div class="content-ad"></div>

## 문제 해결 과정

## 문제를 일으키는 경고 플래그 제거

먼저 Makefile에서 -Wno-format-truncation 플래그를 제거했습니다. 이는 경고일 뿐 심각하지 않았기 때문입니다.

```js
CFLAGS ?= -g -O3
CFLAGS += -pthread -Wall
# Removed: -Wno-format-truncation
CFLAGS += -I"$(ERTS_INCLUDE_DIR)"
CFLAGS += -I$(SRC_INC) -I$(SRC_DIR) -Ic_src
```

<div class="content-ad"></div>

## 링커 문제 해결

약간의 조사를 통해 -soname 옵션이 Linux에 특화되어 있고 macOS에서는 지원되지 않는다는 결론을 얻었습니다.

먼저, 제 CROSSCOMPILE 플래그를 확인했습니다:

```js
➜  project-x git:(add-auth) ✗ echo $CROSSCOMPILE
true
```

<div class="content-ad"></div>

그리고 그 문제가 그것일지 확인하기 위해 Makefile을 플랫폼별 플래그를 사용하도록 수정했습니다. 이 문제에 대한 3가지 대안을 찾아보았어요:

- CROSSCOMPILE을 사용하지 않도록 설정: /deps/argon2_elixir/Makefile에서 컴파일러를 명시적으로 설정했습니다:

```js
...
unexport CROSSCOMPILE

ifneq ($(CROSSCOMPILE),)
 LIB_CFLAGS := -shared -fPIC -fvisibility=hidden
 SO_LDFLAGS := -Wl,-soname,libargon2.so.0
else
... 
```

2. argon_2_elixir의 컴파일 옵션을 업데이트: /deps/argon2_elixir/mix.exs에서 elixir_make 및 구성 옵션에 대해 더 알아보세요.

<div class="content-ad"></div>

```js
def project do
   [
     # ...other configs...
     # option 1
     make_env: %{"CROSSCOMPILE" => nil},
     # option 2
     make_args: ["CROSSCOMPILE="],
   ]
end
```

3. argon2_elixir를 컴파일할 때에는 이 플래그만 사용하실 수 있습니다: 비어있는 값을 사용하여 값을 재정의하실 수 있습니다:

```shell
CROSSCOMPILE= mix deps.compile argon2_elixir --force
```

# 배운 점


<div class="content-ad"></div>

- 플랫폼별 컴파일: 네이티브 코드를 컴파일하는 라이브러리를 사용할 때는 항상 대상 플랫폼을 고려해야 합니다.
- 빌드 시스템 이해: Makefile과 컴파일 플래그에 대한 기본적인 이해는 이러한 문제의 문제 해결에 중요합니다.
- 개발 대 배포 환경: 로컬 개발을 위한 솔루션은 프로덕션 환경에서 필요한 것과 다를 수 있습니다.
- 크로스 컴파일 복잡성: 크로스 컴파일 설정은 복잡성을 증가시킬 수 있으며 로컬 개발에 항상 필요하지는 않을 수 있습니다.
- 최신 정보 유지: 최신 OS 버전을 사용하는 것은 때로는 구 버전 빌드 도구나 라이브러리와의 호환성 문제로 이어질 수 있습니다.

# 결론

간단한 의존성 설치로 시작된 것이 컴파일 프로세스, 링커 플래그, 그리고 크로스 플랫폼 개발에 대한 심층적인 탐구로 이어졌습니다. 도전적이었지만 이 경험은 엘릭서 프로젝트에서 네이티브 코드 컴파일의 복잡성에 대한 소중한 통찰력을 제공했습니다. 비슷한 문제에 직면한 다른 개발자들을 위해 기억해야 할 사항:

- 플랫폼별 문제에 대한 단서를 위해 에러 메시지를 주의 깊게 읽으세요.
- 의존성 소스 코드와 빌드 스크립트를 자세히 살펴보는 것을 주저하지 마세요.
- 크로스 컴파일과 플랫폼별 플래그의 영향을 고려하세요.
- 로컬 개발 요구와 배포 요구 사이를 구분하세요.
- 프리 릴리스나 매우 최신의 OS 버전을 사용하는 것이 예상치 못한 호환성 문제에 이를 수 있음을 인지하세요.

<div class="content-ad"></div>

이러한 경험을 공유함으로써 우리는 일렀로자 열매 레코시를 개선할 수 있습니다.

# 빠른 패치, 영구적인 해결책이 아닙니다

맥 macOS에서 바로 컴파일 문제를 해결한 이 변경 사항은 단기적인 패치라는 점을 강조해야 합니다. 더 효과적인 해결책은 Makefile을 업데이트하여 서로 다른 플랫폼을 더 우아하게 처리하는 것일 것입니다.

하지만, NIF 컴파일에 대한 전문 지식이 없기 때문에 이러한 빠른 수정은 로컬 개발에 목적을 제공했습니다. 아무튼, 더 많은 개발자들이 최신 하드웨어와 소프트웨어 환경을 채택함에 따라 더 넓은 커뮤니티에 이로운 방법일 것입니다.

<div class="content-ad"></div>

오늘은 여기까지입니다.