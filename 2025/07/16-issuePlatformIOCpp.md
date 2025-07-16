---
title: "PlatformIO에서 STM32 + C++17 빌드 오류: GCC 버전 문제 정리"
date: "2025-07-15"
tags: ["embedded", "stm32", "platformio", "cpp17", "gcc", "toolchain"]
summary: "PlatformIO에서 발생하는 C++17 문법 관련 빌드 오류와 그 원인, 해결 방법을 STM32 프로젝트 사례로 정리"
---

# PlatformIO에서 STM32 + C++17 코드 빌드시 발생하는 GCC 이슈 정리

---

## ❓ 문제 상황

STM32CubeIDE로는 문제 없이 빌드되는 프로젝트를 PlatformIO에서 빌드할 경우, 다음과 같은 오류가 반복 발생

```text
sorry, unimplemented: non-trivial designated initializers not supported
```

또는

```text
no match for 'operator=' (operand types are 'SomeStruct' and '<brace-enclosed initializer list>')
```

---

## 🧩 원인 분석

### 1. PlatformIO 기본 툴체인(GCC) 버전 문제

- PlatformIO의 STM32 toolchain(`toolchain-gccarmnoneeabi`)은 **기본적으로 GCC 7.x**를 사용 (2017년 출시)
- 하지만 C++17의 구조체 지정자 초기화 등은 **GCC 8.x 이상부터 정식 지원**
- 반면 STM32CubeIDE는 **GCC 10.x\~12.x**를 사용하므로 문제 없음

### 2. C++17 문법 사용

예시 코드:

```cpp
const GpioControl RC_FWD = {.type = GpioType::DIGITAL, .gpio = {PORT, PIN}};
```

- 위 문법은 C++17에서 허용되지만, GCC 7.x에서는 **non-trivial designated initializer**로 분류되어 미지원
- 비슷한 사례: `operator=`에 중괄호 초기값 넘기기 등

---

## 🚩 증상 로그 예시

```text
src/App/Inc/pin_config.hpp:11:98: sorry, unimplemented: non-trivial designated initializers not supported
const GpioControl RC_FWD = {.type = GpioType::DIGITAL, .gpio = {PORT, PIN}};
```

---

## 💡 해결/우회 방안

### 1. 코드 수정 (구버전 호환 스타일로)

C++17 문법 대신 생성자 방식으로 변경

```cpp
const GpioControl RC_FWD = GpioControl(GpioType::DIGITAL, GPIO_Type(PORT, PIN));
```

또는 중첩 구조체 멤버에 수동 대입

---

### 2. PlatformIO의 GCC Toolchain 수동 업그레이드

- 최신 `arm-none-eabi-gcc`(예: 10.x, 12.x)를 수동 설치
- `platformio.ini` 혹은 툴체인 경로를 직접 수정해 사용 (공식 지원 아님)
- 참고: [PlatformIO 커뮤니티 팁](https://community.platformio.org/)

---

### 3. CubeIDE / Makefile 기반 빌드 유지

- 최신 C++ 문법 + STM32 HAL을 쓸 경우, PlatformIO 대신 **CubeIDE**, **CMake**, **Makefile** 환경 유지 권장
- PlatformIO는 **cross-platform 자동화가 꼭 필요할 때만** 고려

---

## 📝 결론

- PlatformIO는 편리하지만 **기본 GCC 버전이 낮아 최신 C++ 문법과 충돌**이 발생할 수 있음
- 특히 **STM32 + C++17** 조합에서는 빌드 오류가 잦음
- 해결하려면 다음 중 하나를 선택
  - 코드를 구버전 문법으로 변환
  - GCC toolchain을 수동 업그레이드
  - CubeIDE 등 공식/현대적인 빌드 시스템 유지

---

## 🤦 실전 경험

> PlatformIO가 편해서 썼는데, C++17같은 최신 문법 조금만 쓰면 빌드가 안 됨
> 결국 CubeIDE로 돌아왔고, PlatformIO는 데모/자동화용으로만 사용 중
> "Cross-platform + Modern C++" 조합은 아직 상용화 안됨
