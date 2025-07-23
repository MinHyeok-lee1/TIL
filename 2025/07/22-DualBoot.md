---
title: "듀얼부팅 정리(Windows + Ubuntu)"
date: "2025-07-22"
tags: ["dualboot", "ubuntu", "windows", "grub", "uefi", "os"]
summary: "듀얼부팅 개념과 실습 가이드: Windows + Ubuntu"
---

# 듀얼부팅 개념과 실습 가이드: Windows + Ubuntu

하나의 PC에 Windows와 Ubuntu를 함께 설치해 사용하는 듀얼부팅의 개념과 설치 순서, GRUB 구성, UEFI 설정 등 실무적인 팁을 정리

---

## ❓ 듀얼부팅이란?

듀얼부팅(Dual Boot)은 **한 대의 컴퓨터에 2개 이상의 운영체제를 설치**하고, 부팅 시 선택하여 실행할 수 있도록 구성한 환경

> 예: 하나의 노트북에 Windows 11과 Ubuntu 24.04를 모두 설치하고, 시작 시 어떤 OS로 부팅할지 선택

---

## 💡 왜 듀얼부팅을 사용하는가?

- Windows 기반 업무와 Linux 기반 개발을 동시에 활용
- 리눅스 전용 소프트웨어 또는 로봇/임베디드 개발환경 구축
- 클린한 테스트/실험 환경 유지
- 가상머신보다 **하드웨어 직접 접근**이 필요한 작업에서 유리 (ex. USB, GPU, 시리얼포트)

---

## ⚙️ 기본 구성 원리

- 하나의 디스크에 여러 개의 파티션을 만들어 OS를 각각 설치
- **부트로더(GRUB)**가 설치되어, 부팅 시 어떤 OS를 실행할지 선택 메뉴 제공
- 보통 **Windows → Ubuntu 순서로 설치**하면 자동으로 GRUB 설정됨

---

## 🧭 설치 순서 가이드

### ✅ 1단계: BIOS/UEFI 설정

- UEFI 모드 유지 권장 (Windows, Ubuntu 둘 다 UEFI 설치)
- **Secure Boot 비활성화**
- 부팅 순서: USB 우선

### ✅ 2단계: Windows 설치

- 디스크 공간을 적절히 분할하여 Ubuntu용 빈 파티션 확보
- ex: 500GB SSD → Windows 350GB + Ubuntu 150GB

### ✅ 3단계: Ubuntu 설치

- USB 부팅 → Ubuntu 설치 실행
- 설치 중 `기존 Windows 유지 + Ubuntu 병행 설치` 옵션 선택
- Ubuntu 설치 시 GRUB 부트로더 자동 설정

---

## 🔄 부팅 구조 (GRUB)

설치 후 재부팅하면 **GRUB 메뉴**가 표시됨

```text
GNU GRUB
--------------
* Ubuntu
  Windows Boot Manager
```

- 기본 OS 설정, 부팅 순서, 시간 설정 가능 (`/etc/default/grub` 수정)
- Windows가 GRUB을 덮어쓰는 경우 → Ubuntu Live USB로 복구 가능

---

## ⚠️ 실습 시 주의사항

| 항목                 | 설명                                                                  |
| -------------------- | --------------------------------------------------------------------- |
| 설치 순서            | 반드시 **Windows → Ubuntu** 순으로 설치                               |
| Secure Boot          | Ubuntu 설치 위해 **비활성화 필요**                                    |
| GPT/MBR 파티션       | 둘 다 GPT(UEFI 기반) 권장                                             |
| 디스크 백업          | 파티션 실수 방지 위해 필수                                            |
| 키보드/트랙패드 문제 | Ubuntu 설치 후 입력 불가 → 외장 키보드로 대응 가능                    |
| FreeDOS 노트북       | Windows 미설치 기기 → Ubuntu, Windows 모두 수동 설치 가능 (문제 없음) |

---

## 🛠️ 듀얼부팅 환경 커스터마이징

- GRUB 테마/순서 수정: `grub-customizer` 툴 활용
- 기본 부팅 OS 설정: `/etc/default/grub` → `GRUB_DEFAULT=0 or Windows`
- 부팅 시간 단축: `GRUB_TIMEOUT=3` 등 설정 가능

---

## 💬 FreeDOS 노트북도 가능한가?

> ✅ 예. FreeDOS 노트북은 **운영체제가 설치되지 않은 상태**
> Windows, Ubuntu를 자유롭게 설치 가능하며, 듀얼부팅 구성에 전혀 제약이 없음

---

## 📝 결론

- 듀얼부팅은 **하나의 물리적 PC로 두 환경을 오가는 유연한 구성**
- 설치 순서와 GRUB 설정만 주의하면 누구나 가능
- 리눅스 개발 환경, ROS/STM32, 머신러닝 등 다양한 실습에 유리
