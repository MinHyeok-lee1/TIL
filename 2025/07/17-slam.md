---
title: "SLAM"
date: "2025-07-17"
tags:
  ["slam", "robotics", "localization", "mapping", "sensor-fusion", "navigation"]
summary: "SLAM(Simultaneous Localization and Mapping) 이론 정리"
---

# SLAM(Simultaneous Localization and Mapping) 이론 정리

---

## ❓ SLAM이란?

SLAM은 **Simultaneous Localization and Mapping**의 약자로,  
로봇이 **알 수 없는 환경에서 스스로 지도를 생성(Map)** 하면서  
**자신의 위치(Localization)를 동시에 추정**하는 기술

> **한마디로:**  
> 지도가 없는데, 내가 어디 있는지도 모르겠지만,  
> 동시에 지도도 만들고 위치도 파악해야 하는 문제

---

## 🧩 SLAM의 핵심 문제

SLAM은 두 가지 문제를 동시에 해결해야 함

1. **Localization**: 내가 지금 어디에 있는가? (자기 위치 추정)
2. **Mapping**: 주변 환경은 어떤 구조인가? (지도 작성)

이 둘은 서로 영향을 주기 때문에, **위치를 알아야 지도를 만들 수 있고**,  
**지도를 알아야 위치를 더 정확히 추정**할 수 있음

---

## 🔧 SLAM 구성 요소

### 1. 센서 입력 (Observation)

- **IMU**: 관성 센서 (가속도/자이로)
- **LiDAR**: 거리 측정 기반 포인트 클라우드
- **카메라**: Visual SLAM에 사용 (RGB, RGB-D)
- **초음파/UWB/GPS**: 보조 센서로 활용

### 2. 위치 추정 (Motion Model)

- 바퀴 속도 / IMU 등으로 추정한 이동 거리 및 방향
- 오도메트리 또는 Visual Odometry

### 3. 센서 모델 (Observation Model)

- 특정 위치에서의 센서 관측값 예측
- 관측값과 실제 센서값의 차이로 위치 보정

### 4. 지도 표현 (Map)

- **Grid Map** (Occupancy Grid)
- **Feature-based Map**
- **3D Point Cloud / Mesh**

---

## 🔁 SLAM 동작 루프 (이론적 흐름)

1. 초기 위치 및 맵 설정 (보통 0,0에서 시작)
2. 센서 데이터를 기반으로 **이동 추정**
3. 관측 결과를 기반으로 **지도 업데이트**
4. 관측값과 예측값 비교 → **위치 보정**
5. 위 과정을 반복하여 위치/지도 정교화

---

## 🔍 SLAM의 분류

### ✅ 센서 기준

| 분류            | 설명                             |
| --------------- | -------------------------------- |
| **LiDAR SLAM**  | 2D/3D 라이다 기반 정밀 지도 작성 |
| **Visual SLAM** | 카메라 기반, 구조 복원           |
| **RGB-D SLAM**  | 깊이 정보를 포함한 카메라 사용   |

### ✅ 알고리즘 기준

| 분류             | 설명                                      |
| ---------------- | ----------------------------------------- |
| **EKF-SLAM**     | 확장 칼만필터 기반                        |
| **Graph-SLAM**   | 그래프 최적화 기반                        |
| **FastSLAM**     | 파티클 필터 + landmark 기반               |
| **ORB-SLAM**     | Visual SLAM의 대표 알고리즘 (특징점 기반) |
| **Cartographer** | Google의 2D/3D LiDAR SLAM                 |

---

## 💡 SLAM의 난이도 요소

- 센서 노이즈 → 오차 누적
- 데이터 어소시에이션 → 관측값이 어떤 지점인지 구별 어려움
- 계산량 → 실시간 처리 필요
- 루프 클로징 → 같은 장소에 다시 왔음을 인식하고 오차 보정

---

## 📦 SLAM 주요 오픈소스

| 이름             | 특징                               |
| ---------------- | ---------------------------------- |
| **GMapping**     | 2D 라이다 기반, ROS1에서 널리 사용 |
| **Cartographer** | Google이 만든 2D/3D SLAM, ROS 지원 |
| **ORB-SLAM3**    | Monocular, Stereo, RGB-D 모두 지원 |
| **LIO-SAM**      | LiDAR + IMU 융합 기반 고정밀 SLAM  |

---

## 📝 결론

- SLAM은 자율주행, 모바일 로봇, AR 등에서 핵심 기술
- 이론적으로는 **Bayesian Filter** 구조를 기반으로 하며,
- 실제로는 **센서 퓨전 + 최적화 + 매핑 기술의 조합**

---

## 🤖 실전 활용 팁

> UWB, IMU, LiDAR가 섞인 SLAM 프로젝트를 할 경우,  
> 센서 타이밍 동기화와 프레임 변환(TF) 체계 설계가 핵심  
> 실시간 성능 확보를 위해 **ROS2 + C++ + RTOS** 환경이 유리
