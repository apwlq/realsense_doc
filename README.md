# 리눅스 배포판

#### 사전 빌드 패키지 사용
**Intel® RealSense™ SDK 2.0**은 Ubuntu 16/18/용 [`dpkg`](https://en.wikipedia.org/wiki/Dpkg) 형식의 Intel X86/AMD64 기반 Debian 배포판용 설치 패키지를 제공합니다. 20/22 [LTS](https://wiki.ubuntu.com/LTS).
Realsense [DKMS](https://en.wikipedia.org/wiki/Dynamic_Kernel_Module_Support) 커널 드라이버 패키지(`librealsense2-dkms`)는 Ubuntu LTS 커널 4.4, 4.8, 4.10, 4.13, 4.15, 4.18*, 5.0*을 지원합니다. 5.3*, 5.4, 5.13 및 5.15. 자세한 내용은 [Ubuntu 커널 출시 일정](https://wiki.ubuntu.com/Kernel/Support)을 참조하세요.

#### 소스 코드에서 구성 및 빌드
가능할 때마다 DKMS 패키지를 사용하는 것이 좋지만 시스템을 수동으로 설치하고 패치해야 하는 특정 경우가 있습니다.
  - LTS가 아닌 Ubuntu 커널 버전에서 SDK 사용: **4.16 **
  - 'librealsense' SDK와 사용자별 패치/모듈 통합.
  - 대체 커널/배포판에 대한 패치를 조정합니다.

단계는 [Linux 수동 설치 가이드](./installation.md)에 설명되어 있습니다.


## 패키지 설치:
- 서버의 공개 키를 등록합니다.
````
sudo mkdir -p /etc/apt/keyrings
컬 -sSf https://librealsense.intel.com/Debian/librealsense.pgp | sudo 티 /etc/apt/keyrings/librealsense.pgp > /dev/null
````

- 적절한 HTTPS 지원이 설치되어 있는지 확인하십시오.
`sudo apt-get 설치 apt-transport-https`

- 리포지토리 목록에 서버를 추가합니다.
````
echo "deb [signed-by=/etc/apt/keyrings/librealsense.pgp] https://librealsense.intel.com/Debian/apt-repo `lsb_release -cs` main" | \
sudo 티 /etc/apt/sources.list.d/librealsense.list
sudo apt-get 업데이트
````

- 라이브러리를 설치합니다(패키지를 업그레이드하는 경우 아래 섹션 참조).
   `sudo apt-get librealsense2-dkms 설치`
   `sudo apt-get 설치 librealsense2-utils`
   위의 두 줄은 librealsense2 udev 규칙을 배포하고 커널 모듈, 런타임 라이브러리, 실행 가능한 데모 및 도구를 빌드 및 활성화합니다.

- 선택적으로 개발자 및 디버그 패키지를 설치합니다.
   `sudo apt-get 설치 librealsense2-dev`
   `sudo apt-get 설치 librealsense2-dbg`
   `dev` 패키지가 설치되면 `g++ -std=c++11 filename.cpp -lrealsense2` 또는 선택한 IDE를 사용하여 **librealsense**로 애플리케이션을 컴파일할 수 있습니다.

Intel RealSense 깊이 카메라를 다시 연결하고 'realsense-viewer'를 실행하여 설치를 확인하세요.

커널이 업데이트되었는지 확인합니다.
`modinfo uvcvideo | grep "version:"`에는 `realsense` 문자열이 포함되어야 합니다.

## 패키지 업그레이드:
다음을 호출하여 로컬 패키지 캐시를 새로 고칩니다.
   `sudo apt-get 업데이트`

다음을 사용하여 `librealsense`를 포함하여 설치된 모든 패키지를 업그레이드하십시오.
   `sudo apt-get 업그레이드`

선택한 패키지를 업그레이드하려면 보다 세부적인 접근 방식만 적용할 수 있습니다.
   `sudo apt-get --only-upgrade install <패키지1 패키지2 ...>`
   예:
   `sudo apt-get --only-upgrade 설치 librealsense2-utils librealsense2-dkms`

## 패키지 제거:
**중요** 데비안 패키지 제거는 설치된 다른 패키지가 이를 직접 참조하지 않는 경우에만 허용됩니다. 예를 들어 `librealsense2-udev-rules`를 제거하려면 `librealsense2`를 먼저 제거해야 합니다.

다음을 사용하여 단일 패키지를 제거합니다.
   `sudo apt-get purge <패키지 이름>`

다음을 사용하여 RealSense™ SDK 관련 패키지를 모두 제거하세요.
   `dpkg -l | grep "realsense" | 컷 -d " " -f 3 | xargs sudo dpkg --purge`

## 패키지 세부 정보:
패키지와 해당 콘텐츠는 다음과 같습니다.

이름 | 내용 | |에 따라 다름
-------- | ------------ | ---------------- |
librealsense2-udev-규칙 | 커널 수준에서 RealSense 장치 권한 구성 | -
librealsense2-dkms | 심도 카메라별 커널 확장용 DKMS 패키지 | librealsense2-udev-규칙
librealsense2 | RealSense™ SDK 런타임(.so) 및 구성 파일 | librealsense2-udev-규칙
librealsense2-utils | RealSense™ SDK의 일부로 사용 가능한 데모 및 도구 | librealsense2
librealsense2-dev | 개발자를 위한 헤더 파일 및 심볼릭 링크 | librealsense2
librealsense2-dbg | 개발자를 위한 디버그 기호 | librealsense2
librealsense2-gl | GLSL 확장 모듈 런타임 및 구성 파일 | librealsense2
librealsense2-gl-dev | GLSL 개발 헤더 파일 및 심볼릭 링크 | librealsense2
librealsense2-gl-dbg | 디버깅 목적에 필요한 GLSL 디버그 기호 | librealsense2

**참고** 패키지에는 바이너리와 구성 파일만 포함됩니다.
소스 코드를 얻으려면 github 저장소를 사용하세요.
