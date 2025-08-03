---
title: "ROS2 개발용 VSCode 확장 프로그램 추천"
date: "2025-08-02"
tags: ["ros2", "vscode", "extension", "jazzy"]
summary: "ROS2 (Jazzy 기준) 개발 환경에서 유용한 Visual Studio Code 확장 프로그램 목록 및 기본 설정 정리"
---

# 🛠️ ROS2 개발용 VSCode 확장 프로그램

ROS2 패키지 개발 시 유용한 VSCode 확장 프로그램을 정리함. (Ubuntu 24.04 + ROS2 Jazzy 기준)

---

## 1️⃣ 필수 확장 프로그램

| 확장명                                          | 설명                                                          | 링크                                                                                       |
| ----------------------------------------------- | ------------------------------------------------------------- | ------------------------------------------------------------------------------------------ |
| **ROS** (ms-iot.vscode-ros)                     | ROS 1/2 패키지 관리, launch 실행, 메시지 타입 지원, TF 시각화 | [🔗 설치](https://marketplace.visualstudio.com/items?itemName=ms-iot.vscode-ros)           |
| **C/C++** (ms-vscode.cpptools)                  | C++ 기반 ROS2 노드 개발 필수, IntelliSense, 디버깅 지원       | [🔗 설치](https://marketplace.visualstudio.com/items?itemName=ms-vscode.cpptools)          |
| **Python** (ms-python.python)                   | Python 기반 ROS2 노드 개발 필수                               | [🔗 설치](https://marketplace.visualstudio.com/items?itemName=ms-python.python)            |
| **colcon Helper** (ms-iot.vscode-colcon-helper) | colcon 빌드 및 워크스페이스 관리 자동화                       | [🔗 설치](https://marketplace.visualstudio.com/items?itemName=ms-iot.vscode-colcon-helper) |
| **ROS Snippets** (ajshort.ros-snippets)         | ROS2 Publisher/Subscriber, Launch 파일 템플릿 제공            | [🔗 설치](https://marketplace.visualstudio.com/items?itemName=ajshort.ros-snippets)        |
| **XML Tools** (dotjoshjohnson.xml)              | `.launch.xml`, `.urdf` 파일 편집 지원                         | [🔗 설치](https://marketplace.visualstudio.com/items?itemName=DotJoshJohnson.xml)          |

---

## 2️⃣ 보조 확장 프로그램

| 확장명                            | 설명                                |
| --------------------------------- | ----------------------------------- |
| **ROS URDF** (smilerobotics.urdf) | URDF 모델 시각화                    |
| **Markdown All in One**           | README 및 문서 작성 편리            |
| **Better Jinja**                  | `.launch.py` 템플릿 문법 하이라이팅 |
| **CMake Tools**                   | CMakeLists.txt 편집 및 빌드 편리    |
| **clangd**                        | 빠른 C++ 코드 분석 및 자동완성      |
| **CodeLLDB**                      | ROS2 C++ 노드 디버깅                |
| **GitLens**                       | Git 이력 추적 및 협업 관리          |

---

## 3️⃣ VSCode 기본 설정 (settings.json)

```jsonc
{
  "ros.distro": "jazzy",
  "ros.rosSetupScript": "/opt/ros/jazzy/setup.bash",
  "editor.formatOnSave": true,
  "python.analysis.extraPaths": ["/opt/ros/jazzy/lib/python3.12/site-packages"]
}
```

> 💡 Python 경로는 시스템 환경에 맞게 수정 필요
