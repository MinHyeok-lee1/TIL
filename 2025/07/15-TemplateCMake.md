---
title: "CMake 기반 STM32 프로젝트 템플릿화: VSCode + Ninja 올인원 빌드 환경 구성"
date: "2025-07-14"
tags: ["embedded", "cmake", "ninja", "stm32", "vscode", "template"]
summary: "STM32CubeMX 기반 프로젝트를 CMake와 Ninja로 빌드 자동화하고, VSCode에서 쉽게 개발 가능한 템플릿 환경을 구축합니다."
---

# STM32 CMake 프로젝트 템플릿화

현업에서 바로 적용 가능한 **STM32 + CMake + Ninja + VSCode** 기반 템플릿을 소개  
CubeMX로 생성한 코드에 이 템플릿을 입히면, 즉시 빌드/업로드/디버깅 가능한 환경을 만들 수 있음

## 1.디렉토리 구조 (템플릿 예시)

```bash
stm32-cmake-template/
├── CMakeLists.txt
├── toolchain-arm-none-eabi.cmake
├── .vscode/
│   ├── c_cpp_properties.json   # 인텔리센스/컴파일러 경로 자동 완성
│   └── tasks.json              # 빌드&업로드 통합 태스크
├── Core/
│   ├── Inc/
│   └── Src/
├── Drivers/
├── Middlewares/
├── startup_xxx.s
├── STM32F7xxx_FLASH.ld
├── build.sh
├── upload.sh
└── README.md
```

---

## 2.필수 수정/관리 포인트

- **CMakeLists.txt**
  - MCU/칩명(`-DSTM32F767xx` 등)
  - 링커 스크립트(`STM32F767XX_FLASH.ld`)
  - 스타트업 파일(`startup_stm32f767xx.s`)
- **toolchain-arm-none-eabi.cmake**
  - 툴체인(컴파일러) 경로
- **.vscode/c_cpp_properties.json**
  - includePath/컴파일러 경로/define
- **.vscode/tasks.json**
  - 빌드/업로드 명령 및 PATH
- **build.sh, upload.sh**
  - 실제 빌드 및 업로드 커맨드
- **README.md**
  - 사용법/특이사항 등 기록

---

## 3.CMakeLists.txt 템플릿 (예시)

```bash
cmake_minimum_required(VERSION 3.19)
project(my_stm32_project C CXX ASM)

set(TARGET_NAME my_stm32_project)

set(CPU_FLAGS "-mcpu=cortex-m7 -mthumb -mfpu=fpv5-d16 -mfloat-abi=hard")
set(CMAKE_C_FLAGS   "${CPU_FLAGS} -Wall -fdata-sections -ffunction-sections -Og")
set(CMAKE_CXX_FLAGS "${CMAKE_C_FLAGS} -std=c++17 -fno-exceptions -fno-rtti")
set(CMAKE_ASM_FLAGS "${CMAKE_C_FLAGS}")

set(CMAKE_C_FLAGS_DEBUG   "-g -gdwarf-2")
set(CMAKE_CXX_FLAGS_DEBUG "-g -gdwarf-2")

set(LINKER_SCRIPT ${CMAKE_SOURCE_DIR}/STM32F767XX_FLASH.ld)
set(CMAKE_EXE_LINKER_FLAGS
    "${CPU_FLAGS} -T${LINKER_SCRIPT} -Wl,-Map=${TARGET_NAME}.map,--cref -Wl,--gc-sections -specs=nano.specs"
)

include_directories(
  Core/Inc
  Drivers/STM32F7xx_HAL_Driver/Inc
  Drivers/STM32F7xx_HAL_Driver/Inc/Legacy
  Drivers/CMSIS/Device/ST/STM32F7xx/Include
  Drivers/CMSIS/Include
  Middlewares/Third_Party/FreeRTOS/Source/include
  Middlewares/Third_Party/FreeRTOS/Source/CMSIS_RTOS_V2
  Middlewares/Third_Party/FreeRTOS/Source/portable/GCC/ARM_CM7/r0p1
)

add_definitions(
  -DUSE_HAL_DRIVER
  -DSTM32F767xx
)

file(GLOB_RECURSE SOURCES_C    Core/Src/*.c Drivers/**/*.c Middlewares/**/*.c)
file(GLOB_RECURSE SOURCES_CPP  Core/App/**/*.cpp)
file(GLOB_RECURSE SOURCES_ASM  startup_stm32f767xx.s)

add_executable(${TARGET_NAME}.elf ${SOURCES_C} ${SOURCES_CPP} ${SOURCES_ASM})

add_custom_command(
  OUTPUT ${TARGET_NAME}.hex ${TARGET_NAME}.bin
  COMMAND ${CMAKE_OBJCOPY} -O ihex ${TARGET_NAME}.elf ${TARGET_NAME}.hex
  COMMAND ${CMAKE_OBJCOPY} -O binary ${TARGET_NAME}.elf ${TARGET_NAME}.bin
  COMMAND ${CMAKE_SIZE} ${TARGET_NAME}.elf
  DEPENDS ${TARGET_NAME}.elf
  COMMENT "Generating HEX and BIN files"
)

add_custom_target(post_build ALL
  DEPENDS ${TARGET_NAME}.hex ${TARGET_NAME}.bin
)
```

---

## 4.toolchain-arm-none-eabi.cmake 템플릿 (예시)

```bash
set(CMAKE_SYSTEM_NAME Generic)
set(CMAKE_SYSTEM_PROCESSOR arm)

set(TOOLCHAIN_PATH "/Applications/ArmGNUToolchain/arm-gnu-toolchain-14.3.rel1-darwin-arm64-arm-none-eabi/bin")

set(CMAKE_C_COMPILER   ${TOOLCHAIN_PATH}/arm-none-eabi-gcc)
set(CMAKE_CXX_COMPILER ${TOOLCHAIN_PATH}/arm-none-eabi-g++)
set(CMAKE_ASM_COMPILER ${TOOLCHAIN_PATH}/arm-none-eabi-gcc)

set(CMAKE_OBJCOPY      ${TOOLCHAIN_PATH}/arm-none-eabi-objcopy)
set(CMAKE_SIZE         ${TOOLCHAIN_PATH}/arm-none-eabi-size)
set(CMAKE_STRIP        ${TOOLCHAIN_PATH}/arm-none-eabi-strip)

set(CMAKE_FIND_ROOT_PATH_MODE_PROGRAM NEVER)
set(CMAKE_FIND_ROOT_PATH_MODE_LIBRARY ONLY)
set(CMAKE_FIND_ROOT_PATH_MODE_INCLUDE ONLY)
set(CMAKE_FIND_ROOT_PATH_MODE_PACKAGE ONLY)
```

---

## 5.VSCode 설정 예시

`.vscode/c_cpp_properties.json`

```json
{
  "configurations": [
    {
      "name": "STM32",
      "includePath": [
        "${workspaceFolder}/**",
        "/Applications/ArmGNUToolchain/arm-gnu-toolchain-14.3.rel1-darwin-arm64-arm-none-eabi/include",
        "/Applications/ArmGNUToolchain/arm-gnu-toolchain-14.3.rel1-darwin-arm64-arm-none-eabi/arm-none-eabi/include",
        "/Applications/ArmGNUToolchain/arm-gnu-toolchain-14.3.rel1-darwin-arm64-arm-none-eabi/lib/gcc/arm-none-eabi/14.3.1/include"
      ],
      "defines": ["USE_HAL_DRIVER", "STM32F746xx", "USE_CAN_US"],
      "compilerPath": "/Applications/ArmGNUToolchain/arm-gnu-toolchain-14.3.rel1-darwin-arm64-arm-none-eabi/bin/arm-none-eabi-gcc",
      "cStandard": "c11",
      "cppStandard": "c++17",
      "intelliSenseMode": "gcc-x64"
    }
  ],
  "version": 4
}
```

`.vscode/tasks.json`

```json
{
  "version": "2.0.0",
  "tasks": [
    {
      "label": "STM32 Build(Ninja) & Upload",
      "type": "shell",
      "command": "bash",
      "args": ["-c", "./build.sh && ./upload.sh"],
      "group": {
        "kind": "build",
        "isDefault": true
      },
      "presentation": {
        "echo": true,
        "reveal": "always",
        "focus": false,
        "panel": "shared"
      },
      "problemMatcher": [],
      "options": {
        "cwd": "${workspaceFolder}",
        "env": {
          "PATH": "/opt/homebrew/bin:/usr/local/bin:/usr/bin:/bin:/usr/sbin:/sbin"
        }
      }
    }
  ]
}
```

---

## 6.자동화 빌드/업로드 스크립트

**`build.sh`**

```bash
#!/bin/bash
rm -rf build
mkdir build && cd build
cmake -G Ninja .. -DCMAKE_TOOLCHAIN_FILE=../toolchain-arm-none-eabi.cmake
ninja
cd ..
```

**`upload.sh`**

```bash
#!/bin/bash

CUBEPROG_CLI="/Applications/STM32CubeIDE.app/Contents/Eclipse/plugins/com.st.stm32cube.ide.mcu.externaltools.cubeprogrammer.macos64_2.2.100.202412061334/tools/bin/STM32_Programmer_CLI"
BIN_FILE="build/botbox_stm32_v1.0.bin"
ADDRESS=0x08000000

if [ ! -f "$BIN_FILE" ]; then
  echo "❌ $BIN_FILE not found. 먼저 build.sh로 빌드하세요."
  exit 1
fi

echo "🚀 Uploading $BIN_FILE to STM32 via ST-LINK (SWD)..."
"$CUBEPROG_CLI" -c port=SWD -d $BIN_FILE $ADDRESS -rst

if [ $? -eq 0 ]; then
  echo "✅ Upload successful!"
else
  echo "❌ Upload failed!"
fi
```

---

## 7.최종 사용 방법

1. 템플릿 폴더 통째로 복사
2. `CMakeLists.txt`에서 칩/링커스크립트/스타트업 파일명 등 수정
3. `toolchain-arm-none-eabi.cmake`에서 TOOLCHAIN_PATH 수정
4. CubeMX에서 Export한 소스(Core/Drivers/Middlewares) 복사
5. VSCode `.vscode` 설정에서 필요에 맞게 define/include/컴파일러경로만 체크
6. `build.sh` → 빌드, `upload.sh` → 업로드, 또는 VSCode 빌드 태스크(⌘+Shift+B)
7. **README.md**에 프로젝트/보드/특이사항/빌드법 기록하면 팀 공유 완벽

---

## ✅ 현업에서 바로 쓰는 STM32 CMake 템플릿, 완전판

- 환경만 맞추면 칩/보드/링커/툴체인 경로만 바꿔 복붙 재활용
- VSCode 자동완성/빌드/업로드/디버깅까지 올인원 세팅
- 팀/프로젝트/신입/협력사 표준화 모두 적용 가능
