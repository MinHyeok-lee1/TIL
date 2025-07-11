---
title: "CMake 정리"
date: "2025-07-11"
tags: ["cmake", "build-system", "cross-platform", "c-cpp"]
summary: "CMake란 무엇이며, 왜 사용하는가?"
---

# CMake란?

1. **CMake**는 다양한 플랫폼에서 사용할 수 있는 **빌드 자동화 도구 생성기(Build System Generator)**
2. 직접 소스를 빌드하지 않고, 각 환경에 맞는 빌드 스크립트(Makefile, Visual Studio Project 등)를 자동으로 만들어줌
3. C/C++ 프로젝트를 비롯한 여러 언어에서 널리 쓰이며, 크로스 플랫폼(Windows, Linux, macOS) 개발 환경에 매우 유리
4. 아래에서는 CMake가 무엇인지, 그리고 왜 개발에서 자주 사용되는지 살펴봄

---

## 📝 CMake의 기본 개념

CMake는 **소스코드 빌드를 자동화**하기 위한 도구
프로그래머가 직접 Makefile이나 프로젝트 파일을 일일이 작성하는 대신,
**CMake 설정 파일(CMakeLists.txt)**만 만들면 다양한 환경에 맞는 빌드 파일을 자동으로 생성해줌

- **CMake:** "Cross-platform Make"라는 이름에서 유래, 크로스 플랫폼 지원에 중점
- **빌드 스크립트 생성:** CMake는 직접 빌드하지 않고, 각 환경에 맞는 빌드 스크립트(예: Unix에서는 Makefile, Windows에서는 Visual Studio 프로젝트 파일 등)를 만듦

CMake의 특징은 **설정 파일(CMakeLists.txt)** 한 번만 작성해두면
운영체제나 개발 툴에 따라 손쉽게 프로젝트를 빌드할 수 있다는 것

```cmake
# 간단한 CMakeLists.txt 예시
cmake_minimum_required(VERSION 3.10)
project(HelloCMake)
add_executable(hello main.cpp)
```

---

## 🔧 CMake의 주요 사용 사례

CMake는 다양한 환경과 프로젝트에서 사용됨
**오픈소스 라이브러리 빌드**, **임베디드 시스템 개발**, **대형 C/C++ 프로젝트**, **크로스 플랫폼 개발** 등에서 특히 많이 활용

### 1. 크로스 플랫폼 빌드 자동화

CMake를 사용하면 한 번의 설정으로 Windows, macOS, Linux에서 동일한 코드를 빌드할 수 있음
개발팀 내에서 각기 다른 운영체제/툴체인을 사용하는 경우에도 CMake 설정만 공유하면 됨

```sh
# 사용 예시 (리눅스, 맥)
mkdir build
cd build
cmake ..
make

# 사용 예시 (윈도우)
cmake .. -G "Visual Studio 17 2022"
```

### 2. 대형 프로젝트와 의존성 관리

여러 개의 서브 프로젝트, 외부 라이브러리 등을 포함한 대형 프로젝트에서
CMake는 디렉토리 구조와 의존성 관리를 쉽게 할 수 있음

```cmake
# 여러 라이브러리/서브디렉토리 추가 예시
add_subdirectory(src)
add_subdirectory(lib/somelib)
```

### 3. 다양한 개발 도구/IDE와 연동

CMake는 Visual Studio, CLion, VSCode, Xcode 등 다양한 IDE에서 지원
CMake 기반 프로젝트는 IDE에서 인텔리센스, 디버깅, 빌드 설정 등이 매우 수월

---

## 🛠️ CMake의 장점

CMake의 가장 큰 장점은 **유연성**, **이식성**, **자동화**임
직접 Makefile이나 IDE별 프로젝트 파일을 관리하지 않아도,
CMake 설정만으로 여러 환경에서 동일하게 빌드 가능

### 1. 크로스 플랫폼/툴체인 지원

한 번의 설정으로 여러 OS와 빌드 시스템 지원
Windows, Linux, macOS, 다양한 임베디드 환경 등 모두 대응

### 2. 대형 프로젝트 관리 용이

서브 프로젝트, 외부 라이브러리, 조건별 빌드 옵션 등 복잡한 프로젝트도 효과적으로 관리

### 3. 다양한 툴 및 자동화 시스템과 호환

Jenkins, GitHub Actions, CI/CD 파이프라인, Conan 등 여러 도구와 연동하여 자동화된 빌드 및 테스트 환경 구축 가능

---

## 🏁 결론

1. **CMake**는 크로스 플랫폼, 크로스 툴체인 환경에서 매우 유용한 빌드 자동화 도구 생성기
2. 한 번의 설정(CMakeLists.txt)만으로 다양한 개발 환경에서 빌드를 손쉽게 수행할 수 있음
3. 대형 프로젝트, 팀 협업, 오픈소스 개발, 자동화 빌드 환경 구축 등 현대 소프트웨어 개발에서 필수적인 역할을 담당
4. CMake를 잘 활용하면 빌드 시스템 유지보수와 협업 효율성이 크게 높아짐
5. 개발 및 배포 과정에서 발생할 수 있는 다양한 빌드 이슈를 사전에 예방할 수 있음
