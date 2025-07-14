---
title: "CMake + Ninja로 STM32 프로젝트 빌드: Make와 Ninja 속도 비교"
date: "2025-07-14"
tags: ["embedded", "build-system", "makefile", "cmake", "ninja", "stm32"]
summary: "CMake 기반 STM32 프로젝트에서 Ninja와 Make 빌드 툴의 속도·사용성 차이를 실제 빌드 로그와 함께 분석"
---

# CMake + Ninja로 STM32 프로젝트 빌드 성능 비교

CMake로 STM32(arm-none-eabi) 프로젝트를 관리할 때  
빌드 툴로 **Make**와 **Ninja** 중 무엇을 쓸지 고민되는 경우가 많음  
두 방식의 실제 속도와 사용성을 실전 로그와 함께 비교

---

## 🛠️ 테스트 환경 및 세팅

- **프로젝트:** STM32F746 + FreeRTOS + C++ (core, drivers, middlewares 포함)
- **Toolchain:** arm-gnu-toolchain-14.3 (Darwin/arm64)
- **CMake:** toolchain 파일 사용
- **Build 시스템:**
  - Unix Makefiles (`cmake .. && cmake --build .`)
  - Ninja (`cmake -G Ninja .. && ninja`)
- **테스트 명령어:**vs
- `rm -rf build mkdir build && cd build cmake -G Ninja .. -DCMAKE_TOOLCHAIN_FILE=../toolchain-arm-none-eabi.cmake ninja`
- `rm -rf build mkdir build && cd build cmake .. -DCMAKE_TOOLCHAIN_FILE=../toolchain-arm-none-eabi.cmake cmake --build .`

---

## 📝 실제 빌드 결과

### 1️⃣ Make (Unix Makefiles) 빌드 로그

```sh
real    0m5.882s
user    0m4.735s
sys     0m0.971s
```

- 상세 빌드 로그(1%씩 증가, 파일별 빌드 내역이 출력)
- 전체 빌드 완료까지 **약 6초(real)**

---

### 2️⃣ Ninja 빌드 로그

```sh
real    0m0.926s
user    0m5.922s
sys     0m1.212s
```

- `[73/74] Linking ...` 등 간결한 로그
- 전체 빌드 완료까지 **1초 미만(real)**

---

## 📊 Make와 Ninja의 속도/사용성 비교

| 구분  | 전체 빌드(real) | user/sys time | 로그 스타일     | 병렬 처리 | 기타 특징                         |
| ----- | --------------- | ------------- | --------------- | --------- | --------------------------------- |
| Make  | 약 6초          | 낮음          | 상세, verbose   | 보통      | 오래된 표준, 범용적               |
| Ninja | 1초 미만(!)     | 더 높음       | 심플, 숫자 위주 | 매우 빠름 | 최신 CMake+CI에서 주류, 매우 빠름 |

---

## ✅ 왜 Ninja가 더 빠를까?

- **병렬 빌드 최적화:**  
  Ninja는 파일 의존성 분석과 스케줄링이 극도로 최적화되어 있음
- **불필요한 파일 검사 최소화:**  
  Make보다 훨씬 빠르게 변경사항 감지
- **실제 작업 처리(실행)는 많지만, 전체 대기(real) 시간은 대폭 단축**

---

## 🛠️ 실전 사용법 요약

### Ninja로 빌드하기

```sh
cmake -G Ninja .. -DCMAKE_TOOLCHAIN_FILE=../toolchain-arm-none-eabi.cmake
ninja
```

- 처음엔 `-G Ninja`로 CMake 빌드 시스템을 생성해야 함
- 이후 빌드는 `ninja` 한 번이면 끝

---

## 🚩 실무 적용 Tip

- **전체 빌드, 부분 빌드 모두 Ninja가 훨씬 빠름**
- 대규모/복잡 프로젝트일수록 체감 차이 커짐
- VSCode, CI 자동화에서도 Ninja 권장
- Ninja는 CMake 3.x 이상에서 기본 지원

---

## 🌟 결론

- **Ninja = 빌드 속도, 효율, 로그 모두 우위**
- Make는 레거시/간단한 환경에서만 선택
- STM32·임베디드·크로스컴파일 등에서도 Ninja 적극 추천
- 실전에서 빌드 시간을 아끼고 싶다면, CMake + Ninja 조합이 정답!
