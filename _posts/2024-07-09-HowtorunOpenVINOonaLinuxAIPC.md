---
title: "Linux AI PC에서 OpenVINO 실행하는 방법"
description: ""
coverImage: "/assets/img/2024-07-09-HowtorunOpenVINOonaLinuxAIPC_0.png"
date: 2024-07-09 10:00
ogImage: 
  url: /assets/img/2024-07-09-HowtorunOpenVINOonaLinuxAIPC_0.png
tag: Tech
originalTitle: "How to run OpenVINO™ on a Linux AI PC"
link: "https://medium.com/openvino-toolkit/how-to-run-openvino-on-a-linux-ai-pc-52083ce14a98"
---


## CPU, GPU 및 NPU의 혜택

![이미지](/assets/img/2024-07-09-HowtorunOpenVINOonaLinuxAIPC_0.png)

저자: 에드리안 보구셰브스키와 주오 우 - 인텔 AI 소프트웨어 열사

![이미지](/assets/img/2024-07-09-HowtorunOpenVINOonaLinuxAIPC_1.png)

<div class="content-ad"></div>

# AI PC이 무엇이며, 왜 특별한 이름을 가지고 있나요?

AI PC는 현재 핫한 주제입니다. 일반 PC와 달리 AI PC는 강력한 GPU와 같은 고급 하드웨어를 탑재하여 AI 모델 인퍼런싱을 매우 빠르게 진행할 수 있습니다. 특히 Gen AI 인퍼런싱에 대한 고성능 계산이 필요한 경우, 그리고 더 중요하게는 Neural Processing Unit(NPU)라 불리는 특수한 AI 칩을 갖추고 있습니다. NPU는 저전력 소비를 최적화하면서 높은 계산 성능을 유지하여 배터리 전력을 빨리 소모하지 않고 배경 흐리게 만들기, 노이즈 제거 등의 작업을 지속적으로 수행하는 AI 워크로드를 효과적으로 처리합니다. 따라서 고성능 AI 작업이나 복잡하고 다중 스레드 AI 응용 프로그램을 쉽게 처리하고 싶다면, Intel® Core™ Ultra 프로세서가 장착된 AI PC가 준비되어 있습니다. AI PC에 대한 자세한 내용은 이 AI PC 블로그를 참조해 주세요.

# OpenVINO™에서 장치 가용성 확인하기

OpenVINO™은 다양한 하드웨어 플랫폼에 대한 AI 모델 최적화, 추론 가속화 및 간편한 배포를 위한 오픈 소스 AI 툴킷입니다. OpenVINO을 사용하면 AI 모델 인퍼런싱을 AI PC의 CPU, GPU 및 NPU에서 쉽게 실행할 수 있습니다. 그 전에 우분투 24.04를 사용하겠습니다. 이는 최신 우분투 LTS 버전입니다.

<div class="content-ad"></div>

신선한 우분투 설치에서 시작해보세요. 그런 다음 다음 명령어를 사용하여 가상 환경을 만들고 OpenVINO을 설치해보세요:

```js
python3 -m venv venv
source venv/bin/activate
pip install openvino
```

파이썬 인터프리터에서 이 코드를 실행하여 사용 가능한 장치를 확인해보세요.

```js
import openvino as ov
core = ov.Core()
print(core.available_devices)
```

<div class="content-ad"></div>

이 시점에서 CPU에만 액세스 권한이 있어야 합니다. 그건 좋지 않아요! 결국, 노트북에는 GPU와 NPU도 있어요! 그러니 그 문제를 해결해 봐요.

# 리눅스에서 모든 장치를 활성화하는 방법

우리는 GPU부터 시작할 거에요. 첫 번째 단계는 모든 드라이버와 필요한 패키지를 설치하는 것입니다. 그러려면 GPG 키로 서명된 인텔 패키지 리포지토리를 추가해야 해요. 다른 방법들도 있지만(여기에서 확인할 수 있어요), 이 방법이 설치와 미래 업데이트에 가장 간단해요.

```js
sudo apt update
sudo apt install -y gpg-agent wget

wget -qO - https://repositories.intel.com/gpu/intel-graphics.key | sudo gpg - yes - dearmor - output /usr/share/keyrings/intel-graphics.gpg
echo "deb [arch=amd64,i386 signed-by=/usr/share/keyrings/intel-graphics.gpg] https://repositories.intel.com/gpu/ubuntu jammy client" | sudo tee /etc/apt/sources.list.d/intel-gpu-jammy.list

sudo apt update
```

<div class="content-ad"></div>

알겠어요. GPU 추론을 위해 필요한 모든 것을 설치할 시간이에요.

```js
sudo apt install -y \
 intel-opencl-icd intel-level-zero-gpu level-zero \
 intel-media-va-driver-non-free libmfx1 libmfxgen1 libvpl2 \
 libegl-mesa0 libegl1-mesa libegl1-mesa-dev libgbm1 libgl1-mesa-dev libgl1-mesa-dri \
 libglapi-mesa libgles2-mesa-dev libglx-mesa0 libigdgmm12 libxatracker2 mesa-va-drivers \
 mesa-vdpau-drivers mesa-vulkan-drivers va-driver-all vainfo hwinfo clinfo
```

이 작업 후 재부팅해주세요.

```js
sudo reboot
```

<div class="content-ad"></div>

아래의 파이썬 코드를 다시 실행해 보겠어요.

```js
import openvino as ov
core = ov.Core()
print(core.available_devices)
```

이제 CPU와 GPU가 보여야 할 거예요. 이렇게 되면 NPU만 남게 됩니다. intel-driver-compiler-npu의 의존성인 oneTBB를 먼저 설치해야 합니다.

```js
sudo apt install libtbb12
```

<div class="content-ad"></div>

그 후, NPU 드라이버와 관련 패키지를 다운로드하고 설치해야 합니다.

```js
wget https://github.com/intel/linux-npu-driver/releases/download/v1.5.0/intel-driver-compiler-npu_1.5.0.20240619-9582784383_ubuntu22.04_amd64.deb
wget https://github.com/intel/linux-npu-driver/releases/download/v1.5.0/intel-fw-npu_1.5.0.20240619-9582784383_ubuntu22.04_amd64.deb
wget https://github.com/intel/linux-npu-driver/releases/download/v1.5.0/intel-level-zero-npu_1.5.0.20240619-9582784383_ubuntu22.04_amd64.deb
wget https://github.com/oneapi-src/level-zero/releases/download/v1.17.2/level-zero_1.17.2+u22.04_amd64.deb

sudo dpkg -i *.deb
```

또한, 가속기(NPU)에 적절한 그룹과 권한을 할당하고 사용자를 render 그룹에 추가해야 합니다 (사용자 이름 대신에 `your-user-name`을 교체해주세요).

```js
sudo bash -c "echo 'SUBSYSTEM==\"accel\", KERNEL==\"accel*\", GROUP=\"render\", MODE=\"0660\"' > /etc/udev/rules.d/10-intel-vpu.rules"
sudo usermod -a -G render <your-user-name>
```

<div class="content-ad"></div>

마지막으로 변경 사항을 적용하기 위한 재부팅이 필요합니다. 

```js
sudo reboot
```

한 번 더 사용 가능한 장치를 확인해보겠습니다.

```js
import openvino as ov
core = ov.Core()
print(core.available_devices)
```

<div class="content-ad"></div>

그리고 CPU, GPU 및 NPU가 이제 보입니다! 즐기세요!

NPU 표시에 문제가 있을 경우 이 페이지를 방문해주세요.

# 주의 사항 및 면책 사항

성능은 사용, 구성 및 기타 요인에 따라 다를 수 있습니다. 성능 지수 사이트에서 자세히 알아보세요.

<div class="content-ad"></div>

성능 결과는 구성에 표시된 날짜의 테스트를 기반으로 하며 모든 공개 업데이트를 반영하지 않을 수 있습니다. 구성 세부 정보는 백업을 참조해 주세요. 어떤 제품이나 구성 요소도 완벽하게 안전할 수 없습니다. 비용과 결과는 다를 수 있습니다. 인텔 기술은 활성화된 하드웨어, 소프트웨어 또는 서비스 활성화가 필요할 수 있습니다.

© Intel Corporation. Intel, 인텔 로고 및 기타 Intel 상표는 Intel Corporation 또는 해당 계열사의 상표입니다.