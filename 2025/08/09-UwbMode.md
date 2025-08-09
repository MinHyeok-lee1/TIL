---
title: "UWB 거리 측정 모드 정리 (TWR, TDoA 등)"
date: "2025-08-09"
tags: ["uwb", "ranging", "twr", "tdoa"]
summary: "UWB에서 사용되는 거리 측정 모드(TWR, SS-TWR, DS-TWR, TDoA)의 개념, 동작 원리, 장단점 정리"
---

# 📡 UWB 거리 측정 모드 정리

UWB(Ultra-Wideband) 기반 RTLS(Real-Time Location System)에서는 다양한 거리 측정 방식이 사용  
본 문서에서는 대표적인 측정 모드들의 개념과 특성을 정리

---

## ✅ 전체 모드 요약

| 모드 이름  | 설명                                                  |
| ---------- | ----------------------------------------------------- |
| **SS-TWR** | Single-Sided Two-Way Ranging. 단방향 왕복 측정        |
| **DS-TWR** | Double-Sided Two-Way Ranging. 양방향 왕복 측정        |
| **TDoA**   | Time Difference of Arrival. 도착 시간 차이 기반 측정  |
| **PDoA**   | Phase Difference of Arrival. 위상 차이 기반 각도 측정 |

---

## 🔍 각 모드 설명

### 1. **SS-TWR** (Single-Sided)

- 태그(Tag)가 앵커(Anchor)에 신호를 보내고 응답 시간을 기반으로 거리 계산
- 구조가 단순하고 메시지 수가 적어 빠르지만, **클럭 오차**에 민감

### 2. **DS-TWR** (Double-Sided)

- 태그와 앵커가 서로 두 번씩 신호를 주고받아 양방향 시간 지연을 보정
- **SS-TWR보다 정확도 높음**
- 메시지 교환 횟수 증가 → 약간의 지연

### 3. **TDoA**

- 여러 개의 앵커에서 동일 신호를 수신하고, 수신 시간 차이로 위치 계산
- 태그가 한 번만 송신해도 여러 앵커가 동시에 측정 가능
- **고밀도 네트워크, 다수 태그 추적에 효율적**
- 앵커 간 정밀 동기화 필수

### 4. **PDoA**

- 수신 안테나 배열 간 위상 차이를 측정해 각도(AoA) 계산
- **2D/3D 위치 추정에 활용**
- DW3220 등 일부 하드웨어만 지원

---

## 🧪 모드별 비교

| 항목             | SS-TWR              | DS-TWR           | TDoA        | PDoA      |
| ---------------- | ------------------- | ---------------- | ----------- | --------- |
| 메시지 교환 횟수 | 2회                 | 4회              | 1회         | 1회       |
| 정확도           | 중                  | 높음             | 높음        | 각도 정밀 |
| 속도             | 빠름                | 보통             | 매우 빠름   | 보통      |
| 동기화 필요성    | ❌                  | ❌               | ✅          | ✅ 일부   |
| 활용 예시        | 근거리 단말, 저전력 | 고정밀 거리 측정 | 대규모 RTLS | 방향 추적 |

---

## 🔗 참고

- Decawave DW1000 User Manual, Ch. 4 Ranging
- IEEE 802.15.4a/z Ranging Protocols
- Qorvo UWB SDK 예제: `ss_twr_tag`, `ds_twr_anchor`, `tdoa_anchor`

📌 환경/네트워크 규모에 따라 적절한 모드를 선택해야 하며, TDoA는 대규모, DS-TWR는 고정밀 단말 위치 추적에 적합
