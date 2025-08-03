---
title: "TurtleBot4 시뮬레이션 LiDAR 값 변경되지 않는 이슈"
date: "2025-08-03"
tags: ["ROS2", "Gazebo", "TurtleBot4", "LiDAR", "issue"]
summary: "TurtleBot4 시뮬레이션 LiDAR 시각화 오류 해결"
---

# TurtleBot4 시뮬레이션 LiDAR 시각화 오류 해결

TurtleBot4 Gazebo 시뮬레이션에서 LiDAR 센서 데이터가 표시되지 않는 문제를 ogre2 렌더링 엔진으로 전환하여 해결한 과정 정리

## 📌 문제 상황

- ROS 2 Jazzy + Gazebo Harmonic 환경에서 TurtleBot4 시뮬레이션 실행 시
  - LiDAR 스캔 데이터가 Gazebo/RViz에서 정상적으로 표시되지 않거나 일부가 누락되는 현상 발생
  - 로그를 찍어보면 /scan 토픽도 정상 발행, 값이 고정되서 계속 들어옴
  - NVIDIA GPU 환경에서 주로 발생하는 그래픽 렌더링 문제로 추정

---

## 🔍 원인

- 기본 렌더링 엔진(`ogre1.x`)이 특정 GPU 드라이버와 충돌을 일으킴
- Gazebo 내 `irobot_create_description` 패키지의 URDF(xacro) 설정에서 렌더링 엔진이 `ogre1` 혹은 `ogre` 로 지정되어 있음

---

## ✅ 해결 방법

### 1️⃣ URDF Xacro 파일 수정

1. 대상 파일 열기

   ```zsh
   sudo nano /opt/ros/jazzy/share/irobot_create_description/urdf/create3.urdf.xacro
   ```

2. `<gazebo>` 태그 내 렌더링 엔진 옵션을 아래와 같이 수정

   ```xml
   <gazebo>
       <plugin name="gazebo_ros_ray_sensor" filename="libgazebo_ros_ray_sensor.so">
           <render_engine>ogre2</render_engine>
       </plugin>
   </gazebo>
   ```

3. 저장 후 시뮬레이션 재실행

   ```zsh
   ros2 launch turtlebot4_gz_bringup turtlebot4_gz.launch.py use_sim_time:=true
   ```

---

## 🎯 결과

- LiDAR 센서 데이터가 정상적으로 Gazebo 및 RViz에 표시됨
- 시뮬레이션 환경에서 라이다 스캔 누락 현상 사라짐

---

## 💡 참고

- 임시 우회 방법으로 아래 옵션을 실행 전에 사용할 수도 있음

  ```zsh
  export LIBGL_ALWAYS_SOFTWARE=true
  ```

  (단, GPU 가속 대신 CPU 렌더링으로 인해 성능 저하 발생 가능)

- 패키지를 최신 상태로 유지하는 것도 권장

  ```zsh
  sudo apt update && sudo apt upgrade
  ```
