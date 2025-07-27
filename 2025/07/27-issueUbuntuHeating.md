---
title: "Ubuntu 종료 후 발열 문제 해결 기록 (HP OMEN 16)"
date: "2025-07-27"
tags: ["ubuntu", "shutdown", "nvidia", "issue"]
summary: "Ubuntu 24.04에서 전원 종료 후에도 노트북이 계속 뜨거워지는 현상을 해결하기 위한 설정 변경 및 조치 이력 정리"
---

# 🧊 Ubuntu 종료 후 발열 문제 해결기

Ubuntu 24.04에서 `shutdown` 명령을 실행해도 노트북이 완전히 꺼지지 않음  
내부 부품이 계속 작동하여 **발열이 지속되는 문제**가 발생했고 이를 해결하기 위해 아래와 같은 순서로 조치를 진행

---

## ✅ 1. `logind.conf` 설정 수정

`systemd`가 `poweroff` 시 suspend로 빠지지 않도록 설정 변경

```bash
sudo nano /etc/systemd/logind.conf
```

수정 또는 추가:

```ini
HandlePowerKey=poweroff
HandleLidSwitch=poweroff
HandleLidSwitchDocked=poweroff
```

변경 적용:

```bash
sudo systemctl restart systemd-logind
```

---

## ✅ 2. GRUB 커널 파라미터 수정

ACPI/APM 파라미터를 추가하여 시스템이 완전히 꺼지도록 유도

```bash
sudo nano /etc/default/grub
```

아래 항목 수정:

```diff
-GRUB_CMDLINE_LINUX_DEFAULT="quiet splash"
+GRUB_CMDLINE_LINUX_DEFAULT="quiet splash acpi=force apm=power_off"
```

적용:

```bash
sudo update-grub
```

---

## ✅ 3. 종료 경로 로그 확인

정말로 shutdown으로 동작했는지 확인:

```bash
journalctl -b -1 | grep -Ei "poweroff|suspend|shutdown"
```

> 결과: `shutdown` 관련 로그만 출력되어 **Suspend로 빠지지는 않음**

---

## ✅ 4. BIOS 설정 변경 (그래픽 모드)

BIOS → **Graphics Switch Mode**를 변경

- 기존 설정: `Hybrid Graphics`
- 변경 설정: `Integrated Graphics Only`

> 💡 변경 시 “HDMI output will not work” 경고가 출력되나, 외부 모니터 사용 계획이 없으므로 무시함

결과:

- 외장 GPU 전원 차단
- 종료 후 발열 현상이 눈에 띄게 감소

---

## ✅ 5. 기타 점검 사항 (선택적)

- `prime-select intel` 명령으로 NVIDIA GPU 비활성화 가능
- `nvidia-powerd` systemd 서비스 차단

```bash
sudo systemctl mask nvidia-powerd
```

- BIOS에서 Fast Boot, Wake on LAN/USB, ASPM 설정도 확인 필요

---

## 📌 결론

Ubuntu 종료 후에도 노트북에서 발열이 지속되는 문제는 대부분:

- 하이브리드 GPU의 비정상 전력 차단,
- ACPI/APM 설정 누락,
- Display Manager의 suspend 경로 문제

이번 조치를 통해 문제는 거의 해결되었으며, 필요 시 다시 Hybrid 모드로 되돌려 테스트할 수 있음
