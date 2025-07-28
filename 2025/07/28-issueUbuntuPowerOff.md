---
title: "🖥️ 듀얼부팅(Ubuntu ↔ Windows) 환경에서 종료 방식 정리"
date: "2025-07-28"
tags: ["ubuntu", "shutdown", "nvidia", "issue", "windows"]
summary: "Ubuntu에서 Windows로 자동 부팅하거나 종료하는 다양한 방식 정리. 전원 버튼 클릭 시 동작 방식 구성까지 포함"
---

# 🔌 듀얼부팅 환경에서 종료 방식 처리 정리

Ubuntu 종료 후 발열 문제 해결 기록 (HP OMEN 16)에서 Windows에서 종료 시 정상 종료 후 발열이 생기지 않아 아래와 같이 변경
듀얼부팅(Ubuntu + Windows) 환경에서 Ubuntu에서 Windows로 이동해서 종료하는 방식이 필요할 때, 그에 따른 조치와 스크립트를 정리

---

## ✅ 목표

| 목적                                      | 설명                                               |
| ----------------------------------------- | -------------------------------------------------- |
| ✅ Ubuntu → Windows 자동 부팅             | Ubuntu에서 명령 실행 시 다음 부팅을 Windows로 설정 |
| ✅ GUI 전원 버튼 클릭 시 Windows로 부팅   | Ubuntu의 전원 메뉴를 Windows 부팅으로 연결         |
| ⚠️ Ubuntu에서 Windows로 부팅 후 자동 종료 | 특수 목적(예: 이중 종료)일 경우만 수동 설정        |

---

## 1. Ubuntu에서 Windows로 자동 부팅 명령어

```bash
sudo grub-reboot 'osprober-efi-AD65-DF1D' && sudo reboot
```

- `osprober-efi-AD65-DF1D`는 `/boot/grub/grub.cfg`에서 `menuentry`로 확인 가능
- 위 명령을 실행하면 **다음 재부팅 시 Windows로 자동 부팅됨**

---

## 2. `/usr/local/bin/boot-to-windows` 스크립트로 등록

```bash
sudo nano /usr/local/bin/boot-to-windows
```

```bash
#!/bin/bash
sudo grub-reboot 'osprober-efi-AD65-DF1D' && sudo reboot
```

```bash
sudo chmod +x /usr/local/bin/boot-to-windows
```

> 원하는 경우 `.desktop` 파일을 만들어 GUI에서 바로 실행할 수도 있음

---

## 3. GNOME GUI 전원 버튼을 누르면 Windows로 부팅하게 하기 (선택적)

- `/usr/share/applications/poweroff.desktop` 또는 GNOME 쉘 확장으로 버튼 액션 오버라이드 필요
- **비추천**: 시스템 전반에 영향이 있어 예상치 못한 문제 발생 가능

---

## 4. Ubuntu에서 Windows 부팅 후 자동 종료 처리 (⚠️ 고급/비추천)

### 방법 A. Windows에 종료 예약 등록 (윈도우 측 설정)

```cmd
shutdown /s /t 10
```

- 윈도우 작업 스케줄러 또는 시작 프로그램에 위 명령어 등록
- 단점: 윈도우 진입하자마자 10초 내 종료됨 (실사용 어려움)

---

## 📁 참고 명령어

```bash
# GRUB menuentry 확인
sudo grep -i menuentry /boot/grub/grub.cfg
```

```bash
# GRUB 기본 부팅값 확인
grep GRUB_DEFAULT /etc/default/grub
```

```bash
# 부팅 항목 변경 반영
sudo update-grub
```
