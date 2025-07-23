---
title: "듀얼부팅: Ubuntu 24.04 설치 실패 이슈"
date: "2025-07-23"
tags: ["dualboot", "ubuntu", "windows", "grub", "uefi", "os"]
summary: "24.04 설치가 안 되는 상황에서 22.04로 설치 후 업그레이드까지의 과정을 정리함"
---

# 💻 듀얼부팅 실습기

Ubuntu 24.04 설치가 안 돼서 22.04 설치 후 업그레이드한 경험 정리함  
FreeDOS 노트북 + Windows 11 + Ubuntu 24.04 듀얼부팅 구성 실습

---

## ✅ 설치 환경

- 노트북: HP 15s (FreeDOS 기본 탑재)
- 준비물: USB 2개 (Windows / Ubuntu 설치용), Rufus, 외장 키보드
- 디스크 구조: Windows 150GB + Ubuntu 나머지 공간

---

## 🚫 24.04 설치 실패 이슈

- `ubuntu-24.04-desktop-amd64.iso`로 Rufus 굽고 설치 시도함
- Live 환경은 들어가짐, 설치 아이콘 눌러도 반응 없음
- `ubuntu-desktop-installer` 명령도 먹히지 않음
- 키보드 입력 안 됨 (내장 키보드 비활성, 외장 키보드는 동작)

---

## ✅ 22.04 설치로 우회

- `ubuntu-22.04.4-desktop-amd64.iso` 다시 다운로드
- Rufus로 **ISO 모드** 설정하여 USB 부팅 디스크 다시 생성
- 설치 진입 성공함, 설치 아이콘 클릭 가능
- 수동 파티션 설정 후 설치 완료
- GRUB 정상 동작, Windows와 Ubuntu 선택 가능

---

## ⬆️ Ubuntu 24.04로 업그레이드

설치 후 업데이트 전부 적용:

```bash
sudo apt update && sudo apt upgrade -y
```

LTS 업그레이드 진행:

```bash
sudo do-release-upgrade
```

도중 GRUB 설정창 나옴 → `keep the local version` 선택
**업그레이드 완료 후 내장 키보드 정상 작동됨**

---

## ⚙️ 현재 상태

- Ubuntu 24.04로 업그레이드 완료
- GRUB 부팅 정상 작동
- 내장 키보드 **정상 작동함**
- Windows와 Ubuntu 모두 문제없이 사용 가능

---

## ⚠️ 실전 팁 요약

| 항목           | 내용                                    |
| -------------- | --------------------------------------- |
| Rufus 설정     | GPT / UEFI / ISO 모드                   |
| 설치 순서      | Windows 먼저, Ubuntu 나중               |
| Live 환경 문제 | 24.04에서 설치기 작동 안 될 수 있음     |
| 우회 방법      | 22.04 설치 후 `do-release-upgrade` 이용 |
| 키보드 문제    | 업그레이드 후 내장 키보드 정상 작동     |

---

## 🧾 결론

Ubuntu 24.04 ISO로 설치 실패 시
22.04로 우회 설치 후 업그레이드가 현실적인 대안임
결과적으로 내장 키보드 문제도 해결됨
GRUB 통해 Windows, Ubuntu 듀얼부팅 정상 구성 완료됨
