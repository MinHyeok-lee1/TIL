---
title: "ROS 2 내비게이션(Nav2) 시스템 개요 및 구성 요소 정리"
date: "2025-07-21"
tags: ["ros2", "nav2", "gazebo", "slam"]
summary: "ROS 2 내비게이션(Nav2) 시스템 개요 및 구성 요소 정리"
---

# ROS 2 기반 내비게이션 시스템 개요

ROS 2 기반 내비게이션 스택인 Nav2의 구성과 동작 원리를 정리  
시뮬레이션부터 SLAM, 맵핑, 경로 계획, Behavior Tree에 이르기까지 핵심 위주로 정리

---

## 1. 시뮬레이션의 필요성

- 실제 하드웨어 없이 알고리즘을 **안전하게 검증**
- 다양한 센서, 맵, 환경 조건을 반복 재현 가능
- 실험 도중 로봇 손상 없이 위험 상황 테스트
- 주요 툴: **Gazebo (Ignition)**, **RViz**, **Gazebo Harmonic (ROS2 지원 강화)**

---

## 2. Nav2 개요

- ROS 2용 **표준 내비게이션 스택**
- 목표 위치까지 로봇을 자율 이동시키는 **계획-추종-회복 구조**
- 구성 요소
  - SLAM / Localization / Mapping
  - Path Planning (Global, Local)
  - Controller / Recovery
  - Behavior Tree 기반 제어
  - Costmap (Global, Local)

---

## 3. 시뮬레이션 환경 구축

| 구성 요소  | 내용                      |
| ---------- | ------------------------- |
| OS         | Ubuntu 22.04 이상         |
| ROS 2 버전 | **Jazzy (추천)** / Humble |
| 시뮬레이터 | Gazebo Harmonic           |

---

| 기본 구성 순서 | 내용                                   |
| -------------- | -------------------------------------- |
| ①              | ROS 2 + Gazebo 설치                    |
| ②              | TurtleBot3 or custom robot 패키지 설치 |
| ③              | URDF → Gazebo 로봇 스폰                |
| ④              | sensor plugin + controller 설정        |
| ⑤              | nav2_bringup + RViz 실행               |

---

## 4. SLAM 개요

- SLAM (Simultaneous Localization and Mapping)
- 로봇이 미지의 환경에서 **자기 위치 + 맵 동시 생성**
- 종류
  - 2D SLAM: LiDAR 기반 (`slam_toolbox`, `cartographer`)
  - 3D SLAM: RGB-D 기반 (`RTAB-Map`, `LIO-SAM` 등)

---

## 5. Mapping

- SLAM 중 **지도 생성 기능만 분리 수행**
- Localization이 정지된 상태에서 정확한 맵 생성
- 사용 예: `slam_toolbox`, `gmapping` (구버전)

---

## 6. Localization

- 고정된 맵 기반 로봇 위치 추정
- 패키지: `amcl` (Adaptive Monte Carlo Localization)
- 필수 입력: 레이저 스캔, 맵, 오도메트리
- 맵 매칭 기반으로 정확도 확보

---

## 7. SLAM에서 Ground Truth

- 실세계는 정확한 좌표(정답) 파악 어려움
- 시뮬레이터에서는 `/gazebo/model_states` 사용 가능
- 평가 방법:

  - ATE (Absolute Trajectory Error)
  - RPE (Relative Pose Error)
  - 툴: `evo`, `slam_eval`, `rpg_trajectory_evaluation`

---

## 8. SLAM 파라미터 (slam_toolbox 기준)

| 파라미터                 | 설명                      |
| ------------------------ | ------------------------- |
| `resolution`             | 맵 해상도 (m/pixel)       |
| `use_sim_time`           | 시뮬레이터 시간 사용 여부 |
| `max_laser_range`        | 라이다 최대 거리          |
| `scan_topic`             | 라이다 입력 토픽명        |
| `optimization_frequency` | 그래프 최적화 주기        |

→ 성능, 속도, 리소스 모두에 영향 → 실환경에 맞게 튜닝 필수

---

## 9. Path Planning 개요

- 로봇이 장애물을 피해 **목표까지 이동하는 경로** 계산
- 구성
  - **Global Planner**: 전체 경로
  - **Local Planner**: 실시간 장애물 회피 및 추종

---

## 10. Path Planning 구성 요소 (Nav2 기준)

| 구성 요소           | 역할                                        |
| ------------------- | ------------------------------------------- |
| `Planner Server`    | Global Path 생성                            |
| `Controller Server` | Local Path 추종                             |
| `Smoother Server`   | 경로 곡선화 및 속도 조절                    |
| `Behavior Server`   | 실패 대응 (회전, 후진 등)                   |
| `BT Navigator`      | 전체 내비게이션 실행 흐름                   |
| `Costmap`           | 장애물/장애물 회피용 맵 (Global/Local 분리) |

---

## 11. Behavior Tree 개요

- 로봇 행동을 **트리 기반으로 정의**
- 구성 노드
  - `Sequence`, `Selector`, `Fallback`, `Retry`, `Timer` 등
- 장점
  - 모듈화 / 디버깅 용이 / 플로우 직관적
- 사용 도구: `BehaviorTree.CPP` + XML 정의

---

## 12. Nav2와 Behavior Tree 연동

- `bt_navigator` 노드가 Behavior Tree 기반으로 동작
- 기본 트리: `NavigateToPose.xml`
- 사용자 정의 트리 구성 및 액션 등록 가능

예시 XML:

```xml
<BehaviorTree>
  <Sequence>
    <ComputePathToPose />
    <FollowPath />
  </Sequence>
</BehaviorTree>
```

---

## 13. 실무 적용을 위한 Nav2 심화

- Custom 로봇 URDF 정교화 + 센서 인터페이스 구성
- Costmap Layer 파라미터 튜닝
  - `inflation_layer`, `obstacle_layer`, `voxel_layer` 등
- Controller/Planner 교체
  - DWB → TEB / RPP / MPC 등
- Behavior Tree 확장
  - 다중 목표 / 순차 행동 / 미션 분기 처리
- 실외 환경 고려
  - GPS + IMU → NavSat → odom fusion
  - 지형 인식 및 필드맵핑 대응
- 테스트 자동화
  - Launch 파일 구성, Lifecycle Node 제어 흐름 구성

---

## ✅ 결론

- ROS 2의 Nav2 스택은 자율주행 로봇의 내비게이션에 특화
- 시뮬레이션 → SLAM → 맵핑 → 플래닝 → 행동 제어까지 전체 흐름 통합
- Behavior Tree 기반으로 로직 확장성/유지보수성 우수
- 실제 프로젝트에서는 파라미터 튜닝, 센서 보정, 알고리즘 대체 등의 커스터마이징 역량이 핵심
