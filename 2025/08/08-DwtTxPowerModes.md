---
title: "UWB 전송 전력 모드 정리 (DWT_TXRF_NAL, PA 등)"
date: "2025-08-08"
tags: ["uwb", "dwt", "dw3000"]
summary: "DWT_NAL, DWT_PA를 포함한 Decawave DW1000/3000 시리즈의 UWB 전송 전력 모드 종류와 차이점을 정리한 문서"
---

# UWB 전송 전력 모드(DWTTXRF) 정리

Decawave(Qorvo)의 UWB 모듈(DW1000, DW3000 시리즈 등)에서는 다양한 송신 전력 모드가 있으며, 보드 회로 구성 및 용도에 따라 설정  
본 문서에서는 주요 TX 모드의 차이와 사용 조건을 정리

---

## ✅ 전체 모드 요약

| 모드 이름            | 설명                                           |
| -------------------- | ---------------------------------------------- |
| `DWT_TXRF_NOMINAL`   | 기본 전송 전력 설정 (대부분의 모듈에서 기본값) |
| `DWT_TXRF_LOW_POWER` | 저전력 송신. 배터리 절약을 위한 설정           |
| `DWT_TXRF_PA`        | 외부 Power Amplifier(PA) 회로용. 고출력 송신   |
| `DWT_TXRF_NAL`       | No Antenna Linearization. 보정 없는 기본 송신  |
| `DWT_TXRF_CUSTOM`    | 사용자 커스텀 설정 (레지스터 직접 설정)        |
| `DWT_TXRF_AUTO`      | DW3000 전용. 자동 감지 기반 모드 선택          |

---

## 🔍 각 모드 설명

### 1. `DWT_TXRF_NOMINAL`

- 일반적인 송신 품질과 소비 전력의 균형
- 특별한 설정이 없으면 기본적으로 이 모드 사용됨

### 2. `DWT_TXRF_LOW_POWER`

- 배터리 기반 시스템에서 유용
- 송신 전력 ↓ → 통신 거리 ↓

### 3. `DWT_TXRF_PA` (또는 `DWT_PA`)

- 외부 전력 증폭기(PA)를 사용하는 보드에 적합
- 장거리 통신, 고출력 응용에서 사용
- 외부 회로(Skyworks, Qorvo 등) 없으면 설정하지 말 것

### 4. `DWT_TXRF_NAL`

- 내장 안테나 보정 기능 없이 송신
- 저가형/기본형 모듈에서 가장 널리 사용됨

### 5. `DWT_TXRF_CUSTOM`

- 아래처럼 직접 레지스터 설정 필요

  ```c
  dwt_write32bitreg(TX_POWER_ID, 0x0E082848);
  dwt_write8bitoffsetreg(TX_CAL_ID, PG_DELAY_OFFSET, 0xC0);
  ```

- RF 인증 초과 가능성 있음 → 주의 필요

### 6. `DWT_TXRF_AUTO` (DW3000 전용)

- 외부 PA 유무를 자동으로 감지하여 적절한 설정 적용
- 고급 보드나 자동화된 환경에 적합

---

## 🧪 보드별 권장 설정

| 보드/모듈 종류            | 권장 TX 모드                  |
| ------------------------- | ----------------------------- |
| DWM3000, DW1000 기본 모듈 | `DWT_TXRF_NAL` 또는 `NOMINAL` |
| 외부 PA 회로 포함 보드    | `DWT_TXRF_PA`                 |
| 배터리 기반 저전력 시스템 | `DWT_TXRF_LOW_POWER`          |
| RF 튜닝 실험용            | `DWT_TXRF_CUSTOM`             |

---

## 🔗 참고

- DW3000 API: `dwt_configuretxrf(mode)`
- 관련 레지스터: `TX_POWER`, `PG_DELAY`, `TX_CAL`
- 외부 PA 예시: Skyworks SE2431L, Qorvo QPF0xxx 시리즈

---

📌 보드에 따라 설정이 다르므로, 반드시 회로 구성 확인 후 올바른 모드를 선택할 것
