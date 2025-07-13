---
title: "임베디드 빌드 시스템"
date: "2025-07-13"
tags: ["embedded", "build-system", "makefile", "cmake", "ninja", "c-cpp"]
summary: "임베디드 빌드 시스템: Makefile, CMake, Ninja의 현업 트렌드"
---

# 임베디드 빌드 시스템 (Makefile, CMake, Ninja)

1. 임베디드 소프트웨어 개발에서는 **Makefile**, **CMake**, **Ninja** 세 가지 빌드 시스템이 가장 널리 쓰임
2. 각 빌드 시스템은 개발 환경, 프로젝트 규모, 협업 방식 등에 따라 선택이 달라지지만, 최근 트렌드의 큰 흐름이 있음
3. 이 문서에서는 임베디드에서의 각 도구의 역할, 장단점, 그리고 현업에서의 주력 사용 양상을 정리함

---

## 📝 Makefile: 임베디드의 오리지널 표준

- **Makefile**은 임베디드, 리눅스, C/C++ 기반 개발에서 **가장 오래된 빌드 자동화 표준**
- GNU Make 등으로 실행
- **간단하고 직접적**

  - 원하는 빌드 과정, 타깃, 의존성을 수동으로 명확하게 정의

- **강점**

  - 소규모, 레거시, 커스터마이즈가 필요한 임베디드 프로젝트에서 활용도가 높음
  - 크로스 컴파일, 작은 MCU, 개발 보드 환경에서 빠르게 적용 가능

- **한계**

  - 프로젝트가 커질수록 관리와 유지보수 난이도 증가
  - 복잡한 라이브러리/모듈 관리, 협업에는 한계

```makefile
# Makefile 예시
main.elf: main.o utils.o
    arm-none-eabi-gcc -o main.elf main.o utils.o

main.o: main.c
    arm-none-eabi-gcc -c main.c

utils.o: utils.c
    arm-none-eabi-gcc -c utils.c

clean:
    rm -f *.o *.elf
```

---

## 🔧 CMake: 현대 임베디드의 새로운 표준

- **CMake**는 점점 더 **임베디드 분야에서도 표준**이 되어가고 있음
- **역할**

  - Makefile, Ninja, Visual Studio 등 다양한 빌드 파일을 자동으로 생성
  - 소스코드의 구조와 빌드 옵션을 플랫폼과 IDE에 상관없이 한 번에 관리

- **장점**

  - 크로스 플랫폼(윈도우/리눅스/맥) 지원
  - 팀/협업, 여러 MCU 지원, 대형/멀티보드 프로젝트에 적합
  - IDE(특히 VSCode, CLion) 연동, CI/CD 자동화에 유리

- **적용 예시**

  - 최신 STM32, ESP32, NXP SDK 등에서 CMake 지원이 빠르게 확산 중

- **한계**

  - 설정이 처음엔 다소 복잡할 수 있음
  - MCU별 특화 빌드 옵션 등은 여전히 Makefile에서만 되는 경우도 있음

```cmake
# CMakeLists.txt 예시
cmake_minimum_required(VERSION 3.13)
project(EmbeddedApp)
add_executable(main main.c utils.c)
```

---

## ⚡ Ninja: CMake의 빠른 빌드 파트너

- **Ninja**는 단독 빌드 시스템이라기보다는, **CMake로부터 빌드 스크립트를 받아 실제 빌드를 빠르게 수행하는 초경량 도구**
- **주요 역할**

  - CMake에서 `-G Ninja` 옵션을 주면, Ninja용 빌드 파일을 생성
  - 대형 프로젝트에서 병렬 빌드가 빠르고 효율적

- **현업에서는**

  - “CMake로 빌드 환경 관리 → Ninja로 빠른 빌드” 조합이 표준화
  - 직접 Ninja 빌드 파일을 쓰기보다는 대부분 CMake와 함께 사용

```sh
mkdir build && cd build
cmake -G Ninja ..
ninja
```

---

## 🏁 임베디드 빌드 시스템 선택 가이드

1. **Makefile**

   - 소규모, 단순, 빠른 빌드가 필요한 임베디드 프로젝트
   - 저사양 MCU, 레거시/자체 개발 빌드 시스템 유지보수

2. **CMake**

   - 협업, 여러 보드/MCU/OS/IDE 환경 지원이 필요한 현대적 프로젝트
   - 오픈소스, 자동화, CI/CD, 대형 프로젝트, 팀 개발에 적합

3. **Ninja**

   - 보통 CMake와 세트로, “빠르고 병렬적인 빌드”가 중요한 환경
   - 직접 설정보다 CMake에서 generator로 사용

---

## ✅ 결론 및 현업 트렌드 요약

- **임베디드**에서의 주력 빌드 시스템은 **Makefile(오래된 표준) → CMake(현대 표준, 점점 증가) → Ninja(빠른 빌드의 조력자)** 순으로 볼 수 있음
- 프로젝트 성격, 팀 규모, 협업 도구, MCU 특성에 따라 적절한 빌드 시스템을 선택하는 것이 중요
- 최신 임베디드 개발/협업 환경에서는 **CMake + Ninja** 조합이 점점 널리 사용되고 있음
