---
title: "GIMP 활용 가이드 (ROS2 Nav2 시각화)"
date: "2025-08-05"
tags: ["gimp", "ros2", "nav2", "map-editing"]
summary: "GIMP를 활용하여 ROS2 Nav2용 맵(.pgm, .yaml)을 편집 및 최적화하는 방법"
---

# GIMP 활용 가이드 (ROS2 Nav2 시각화)

Ubuntu 24.04.2 환경에서 GIMP를 활용하여 ROS2 Nav2 SLAM 및 Navigation Stack에서 사용하는 맵(.pgm, .yaml)을 시각화·편집하는 방법을 정리

## 📌 개요

- **목적**: SLAM으로 생성된 맵(.pgm)을 편집하여 로봇 주행 경로 최적화 및 장애물/벽 추가
- **대상 환경**:
  - OS: Ubuntu 24.04.2
  - Shell: zsh
  - ROS2: Jazzy
  - Nav2: SLAM Toolbox, AMCL, Behavior Tree 기반
  - Simulator: Gazebo Harmonic
- **편집 도구**: GIMP 2.10.x (무료 오픈소스)

---

## ⚙️ 1. GIMP 설치

```zsh
sudo apt update
sudo apt install gimp -y
```

또는 [공식 사이트](https://www.gimp.org/downloads/)에서 최신 AppImage 다운로드 후 실행

---

## 🗺️ 2. Nav2 맵 파일 구조

ROS2 SLAM 및 Nav2에서 사용되는 맵은 보통 아래와 같은 구성:

```tree
my_map/
 ├─ map.pgm    # 흑백 이미지 (0=장애물, 255=자유공간, 205=미탐색)
 ├─ map.yaml   # 메타데이터 (resolution, origin 등)
```

GIMP로 편집할 때는 **pgm 파일**을 로드하여 작업 후 다시 동일한 형식으로 저장

---

## 🛠️ 3. GIMP 편집 작업

### 3.1 불필요한 영역 잘라내기 (Crop)

- **메뉴**: `Image → Crop to Content`
- Nav2에서 로봇이 불필요하게 탐색하는 외곽 공간 제거

### 3.2 벽(장애물) 추가

- **방법**:

  1. `Brush Tool (B)` 선택
  2. 전경색을 `검정색 (#000000)`으로 설정
  3. 맵 가장자리에 선을 그려서 로봇이 넘어가지 않도록 차단

### 3.3 미탐색 영역(Unknown Space) 채우기

- 색상 코드:

  - 장애물: **#000000** (0)
  - 자유 공간: **#FFFFFF** (255)
  - 미탐색: **#CDCDCD** (205)

- `Bucket Fill (Shift+B)`를 사용하여 필요 영역만 채우기

### 3.4 맵 크기 유지

- `Image → Scale Image` 사용 시 **픽셀 크기 변경 금지**
- Nav2는 픽셀당 resolution(`map.yaml`)에 의존하므로 해상도 변형은 경로 계획 오류를 유발

---

## 💾 4. ROS2 Nav2에서 적용

편집이 끝난 후, `pgm` 파일을 덮어쓴 뒤 `yaml`을 기존과 동일하게 유지하거나 필요 시 아래 항목을 수정

```yaml
image: map.pgm
resolution: 0.05 # 1픽셀당 5cm
origin: [0.0, 0.0, 0.0]
occupied_thresh: 0.65
free_thresh: 0.25
negate: 0
```

Nav2 실행:

```zsh
ros2 launch nav2_bringup navigation_launch.py \
  use_sim_time:=true map:=~/robotware_ws/maps/map.yaml
```

---

## ⌨️ 5. ROS2에서 자주 사용하는 명령어

- SLAM 맵 저장:

```zsh
ros2 run nav2_map_server map_saver_cli -f ~/robotware_ws/maps/map
```

- 편집된 맵 로딩:

```zsh
ros2 launch nav2_bringup navigation_launch.py \
  use_sim_time:=true map:=~/robotware_ws/maps/map.yaml
```

---

## ✅ 팁

- **편집 전 원본 백업 필수**
- **레이어 기능 활용**: 새로운 레이어에 임시 벽을 추가하여 비교 가능
- **가우시안 블러(선택)**: 경계가 너무 날카로우면 `Filters → Blur → Gaussian Blur`로 자연스럽게 처리
- 편집 후 항상 \*\*PGM(Grayscale, Binary)\*\*로 내보내기

---

## 📚 참고

- [GIMP 공식 사이트](https://www.gimp.org/)
- [GIMP Plugins](https://www.gimp.org/downloads/)
