---
title: "ROS2"
date: "2025-07-17"
tags: ["ros2", "robotics", "middleware", "ubuntu"]
summary: "ROS2 이론 정리: 구조와 핵심 개념"
---

# ROS2 이론 정리: 구조와 핵심 개념

ROS2는 분산형 로봇 소프트웨어 시스템의 표준으로, DDS 기반 통신 구조와 모듈화된 구성 요소를 통해 확장성과 실시간성을 제공

---

## ❓ ROS란?

**ROS(Robot Operating System)**는  
로봇 애플리케이션을 쉽게 개발할 수 있도록 지원하는 **오픈소스 미들웨어 프레임워크**

- '운영체제(OS)'라는 이름이 붙어 있지만 실제로는 **분산형 메시지 기반 미들웨어**
- 센서, 제어기, 알고리즘 등을 모듈화하고 **Node 간 통신을 자동화**하는 데 초점

---

## 🌱 ROS1 vs ROS2

| 항목        | ROS1                          | ROS2                        |
| ----------- | ----------------------------- | --------------------------- |
| 통신 방식   | TCP/UDP 기반 (ROSMaster 필요) | DDS 기반 (분산 시스템)      |
| 실시간성    | 낮음                          | RTOS/실시간 대응            |
| 보안/확장성 | 한계 있음                     | QoS, 보안, 멀티플랫폼 강화  |
| 플랫폼      | 리눅스 중심                   | 리눅스, 윈도우, macOS, RTOS |
| 지원 종료   | 2025년 ROS1 Noetic EOL 예정   | 장기 유지 중 (LTS 중심)     |

---

## 🧩 ROS2의 핵심 구성 요소

### 1. Node

- 로직 단위 실행 유닛 (ex. 센서 노드, 제어 노드)
- 하나의 실행 파일 내에 여러 Node 포함 가능

### 2. Topic

- **Publisher** → 데이터를 발행
- **Subscriber** → 데이터를 수신
- 비동기, 단방향 메시지 통신

### 3. Service

- 요청(Request)과 응답(Response) 구조
- 동기 통신 방식

### 4. Action

- 장기 작업 처리용 인터페이스
- 진행률(feedback), 결과(result), 취소(cancel) 지원

### 5. Parameter

- 런타임 중 노드 동작 설정값을 변경 가능
- ros2 param 명령으로 조회/변경 가능

---

## 🔧 개발 도구/환경

| 도구            | 설명                                               |
| --------------- | -------------------------------------------------- |
| **colcon**      | 빌드 및 패키지 관리 도구 (`colcon build`)          |
| **ros2 CLI**    | `ros2 run`, `ros2 topic`, `ros2 service` 등 명령어 |
| **launch 파일** | 여러 노드 실행을 한 번에 설정                      |
| **rviz2**       | 시각화 도구                                        |
| **rqt**         | GUI 기반 디버깅 도구                               |
| **tf2**         | 로봇 프레임(좌표계) 전파 관리                      |

---

## 📦 패키지 구조 예시

```bash
my_robot_package/
├── package.xml       # 패키지 메타 정보
├── CMakeLists.txt    # 빌드 설정
├── src/              # 노드 코드 (C++/Python)
├── launch/           # 실행 설정 파일
└── msg/ / srv/       # 사용자 정의 메시지/서비스
```

---

## 🔄 ROS2 메시지 구조 예시

### Python 예제 (Publisher)

```python
import rclpy
from std_msgs.msg import String

def main():
    rclpy.init()
    node = rclpy.create_node("talker")
    pub = node.create_publisher(String, "chatter", 10)
    msg = String()
    msg.data = "Hello ROS2!"
    pub.publish(msg)
    rclpy.shutdown()
```

---

## 💡 ROS2의 장점

- **분산 시스템**: 중앙 노드 없이 동작 (ROSMaster 없음)
- **QoS 지원**: 통신 신뢰성 조절 가능
- **실시간성 대응**: RTOS, embedded 환경 대응
- **보안 (SROS2)**: DDS 기반 암호화 및 인증 지원
- **멀티 언어 지원**: C++, Python, Rust 등

---

## ⚠️ 학습/실전 시 유의점

- `colcon`과 `ament_cmake` 빌드 시스템에 익숙해져야 함
- 노드, 토픽, 메시지 간 이름 충돌을 방지하기 위해 **네임스페이스** 관리 필요
- TF2, QoS, Lifecycle 같은 고급 개념은 별도 학습 필요

---

## 📝 결론

- ROS2는 **현대적인 로봇 소프트웨어 개발을 위한 표준 플랫폼**
- **Node-Topic-Service-Action-Launch** 구조를 이해하면 대부분의 프로젝트에 적용 가능
- ROS1보다 강력하지만, 설정과 구조가 조금 더 엄격해 학습 곡선은 다소 있음
