---
title: "ROS2 Jazzy, Gazebo 설치"
date: "2025-08-01"
tags: ["ros2", "jazzy", "gazebo", "cli"]
summary: ROS2 Jazzy + Gazebo 설치 및 실행 확인 명령어 정리"
---

# 🛠️ ROS2 Jazzy + Gazebo 명령어 정리

ROS2 Jazzy와 Gazebo 패키지 설치 후 환경변수 및 시뮬레이터 실행 확인을 위한 기본 명령어 모음

---

## 1️⃣ OS 정보 확인

```bash
lsb_release -a
```

- 현재 Ubuntu 배포판 및 버전 확인용
- ROS2 Jazzy는 Ubuntu 24.04(Noble) 지원함

---

## 2️⃣ ROS 환경 로드

```bash
source /opt/ros/jazzy/setup.zsh
```

- ROS2 설치 경로 로드
- `.zshrc`에 추가하면 매번 실행 안 해도 됨

---

## 3️⃣ ROS 배포판 확인

```bash
echo ${ROS_DISTRO}
printenv ROS_DISTRO
```

- 로드된 ROS 배포판 확인
- 정상 출력값: `jazzy`

---

## 4️⃣ 패키지 업데이트

```bash
sudo apt-get update
sudo apt-get upgrade
```

- 시스템 및 ROS 패키지 최신 상태로 업데이트

---

## 5️⃣ Gazebo 브릿지 설치

```bash
sudo apt-get install ros-${ROS_DISTRO}-ros-gz
```

- ROS ↔ Gazebo 연동 패키지 설치
- `${ROS_DISTRO}`는 자동으로 `jazzy` 치환됨

---

## 6️⃣ 환경 재로드

```bash
source /opt/ros/jazzy/setup.zsh
```

- 새 패키지 설치 후 환경변수 다시 로드 필요

---

## 7️⃣ Gazebo 실행 확인

```bash
gz sim
```

- Gazebo Harmonic 실행
- GUI 정상 표시되면 설치 완료

---

✅ **팁**

```bash
echo "source /opt/ros/jazzy/setup.zsh" >> ~/.zshrc
```

- 위 명령어 추가하면 새 터미널 실행 시 자동 적용됨
