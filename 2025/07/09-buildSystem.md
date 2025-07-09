---
title: "Build System 정리"
date: "2025-07-09"
tags: ["build-system", "cmake", "makefile", "ninja"]
summary: "언어별 빌드 시스템, 요즘 주류 트렌드"
---

# 빌드 시스템

1. 빌드 시스템은 소프트웨어 개발에서 **코드 컴파일·테스트·배포 등의 작업을 자동화**하는 핵심 도구
2. Makefile, CMake 외에도 언어와 환경에 따라 다양한 빌드 도구가 존재
3. 최근 현업에서는 협업·대형 프로젝트·크로스플랫폼 지원 등으로 인해 새로운 도구들이 많이 사용됨
4. 아래에서는 대표적인 빌드 시스템의 종류와 최근 현업 트렌드를 정리함

---

## 📝 대표적인 빌드 시스템 종류

### C/C++ 계열

- **CMake**
  크로스플랫폼 빌드 자동화의 표준.
  다양한 OS/IDE 지원, 대형 오픈소스 및 기업 프로젝트에서 많이 사용
- **Makefile(GNU Make)**
  전통적 빌드 자동화, 리눅스/임베디드/소규모 프로젝트에서 여전히 널리 사용
- **Ninja**
  빌드 속도가 매우 빠름.
  보통 CMake의 백엔드 빌드 도구로 많이 쓰임(`cmake -G Ninja`)
- **Meson**
  Python 기반, 설정이 간단하고 빠른 빌드 속도가 강점
  GNOME/Wayland 등에서 채택
- **Bazel**
  Google에서 개발, 대형 멀티언어/멀티플랫폼 프로젝트, hermetic 빌드에 강함
- **SCons**
  Python 스크립트 기반으로 유연한 빌드 설정 지원

### 기타 언어별 주요 빌드 도구

- **Java**: Maven, Gradle (현업 표준)
- **JavaScript/TypeScript**: npm, yarn, pnpm, webpack, vite (프론트엔드 빌드)
- **Python**: setup.py, Poetry, pipenv (패키징/의존성/빌드)
- **Rust**: Cargo (Rust 공식 빌드 도구)
- **Go**: go build (Go 공식 빌드 도구)

---

## 🔧 최근 현업(2020\~2025)에서 많이 쓰는 빌드 시스템 트렌드

### C/C++ 개발 현업 트렌드

- **CMake + Ninja**

  - 대형 프로젝트 및 오픈소스에서 사실상 표준
  - CMake로 빌드 파일 생성 → Ninja로 빠른 빌드
  - VSCode, CLion 등 최신 IDE에서도 강력 지원

- **Meson**

  - 일부 신규 오픈소스, 빌드 속도·설정 간결성으로 채택 증가

- **Bazel**

  - Google, 대기업, 멀티언어 대규모 프로젝트에서 사용

- **Makefile**

  - 소규모/임베디드/레거시 환경에서 여전히 활발하게 사용

### 자동화와 CI/CD 환경

- **Jenkins, GitHub Actions, GitLab CI 등과 연계해 빌드 자동화**가 일반적
- 다양한 플랫폼, 다양한 언어에 맞는 빌드 시스템과 자동화 도구를 조합해 사용

---

## 🛠️ 분야별 빌드 도구 요약표

| 분야       | 전통/기본   | 최근 대세/신규                 |
| ---------- | ----------- | ------------------------------ |
| C/C++ 빌드 | Makefile    | CMake + Ninja, Meson, Bazel    |
| Java       | Ant         | Maven, Gradle                  |
| JS/TS      | Gulp, Grunt | npm, yarn, pnpm, webpack, vite |
| Python     | setup.py    | Poetry, pipenv                 |
| Rust       | -           | Cargo                          |
| Go         | -           | go build                       |

---

## 🏁 결론 및 선택 기준

1. **신규/대형 C/C++ 프로젝트**
   → **CMake + Ninja** 조합이 현업 표준
2. **소규모, 임베디드, 레거시**
   → **Makefile**
3. **각 언어별 주류 도구**(Java: Gradle/Maven, JS: npm/webpack, Python: Poetry, Rust: Cargo 등)
4. **현대 소프트웨어 개발**에서는 CI/CD(자동화)와 연동해 빌드 시스템을 운영하는 것이 보편적
5. **프로젝트 특성(규모, 언어, 협업 환경, 자동화 필요성 등)에 맞는 빌드 시스템 선택이 중요**
