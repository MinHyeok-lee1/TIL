---
title: "Ubuntu에서 한글 입력이 안 될 때 ibus 설정으로 해결하기"
date: "2025-07-24"
tags: ["ubuntu", "hangul", "ibus", "fcitx"]
summary: "Ubuntu에서 한글 입력이 되지 않을 때 ibus 입력기를 기준으로 문제를 진단하고 설정을 통해 해결하는 전체 과정을 정리"
---

# Ubuntu에서 한글 입력이 안 될 때 ibus 설정으로 해결하기

Ubuntu 24.04.2 등의 환경에서 한글 입력이 안 되는 경우, 키보드 언어 추가만으로는 해결되지 않으며 입력기 설정이 제대로 되어야 함  
아래는 `ibus`를 기준으로 한글 입력 문제를 해결하는 절차

---

## ✅ 현재 입력기 확인 방법

```bash
echo $XMODIFIERS
echo $GTK_IM_MODULE
echo $QT_IM_MODULE
ps aux | grep -E 'ibus|fcitx'
```

출력 예시:

- `@im=ibus`, `GTK_IM_MODULE=ibus` 등 → ibus 사용 중
- `@im=fcitx` → fcitx 사용 중
- 아무 값도 없거나 비어 있음 → 환경변수 미설정 가능성 있음

---

## ✅ ibus 입력기 재설정 절차

### 1️⃣ ibus가 실행 중인지 확인

```bash
ps aux | grep ibus
```

`ibus-daemon`이 보이지 않으면 수동 실행

```bash
ibus-daemon -drx
```

---

### 2️⃣ 한글 입력기 추가

1. **설정 > Region & Language > Input Sources** 진입
2. `+` 버튼 클릭 → `Korean (Hangul)` 선택
3. 영어 키보드와 함께 `Korean - Hangul`이 있는지 확인

---

### 3️⃣ 한/영 전환 단축키 설정

```bash
ibus-setup
```

- `Input Method` 탭에서 `Korean - Hangul`이 등록되어 있는지 확인
- `General` 탭 → 전환 단축키 확인 (기본값: `Ctrl + Space` 또는 `Shift + Space`)

---

### 4️⃣ 환경변수 설정 (.xprofile에 등록)

```bash
echo 'export GTK_IM_MODULE=ibus' >> ~/.xprofile
echo 'export QT_IM_MODULE=ibus' >> ~/.xprofile
echo 'export XMODIFIERS=@im=ibus' >> ~/.xprofile
source ~/.xprofile
```

> 환경변수는 재로그인 또는 재부팅 후 적용됨

---

### 5️⃣ 필요한 패키지 재설치 (선택)

```bash
sudo apt install --reinstall ibus ibus-hangul
```

> `mailcap` 관련 경고는 무시해도 무방

---

## ✅ 테스트

1. 시스템 재부팅 또는 로그아웃 후 로그인
2. 텍스트 에디터(예: `gedit`) 또는 터미널에서 한글 입력 시도
3. `Ctrl + Space` 또는 `Shift + Space`로 입력 전환되는지 확인

---

## ✅ 한글 입력 확인 예시

```bash
한글 입력 테스트 완료! 가나다라마바사
```

---

## 💡 참고

| 항목               | 내용                           |
| ------------------ | ------------------------------ | ---------- |
| 입력기 프레임워크  | ibus                           |
| 입력기 상태 확인   | `echo $XMODIFIERS`, `ps aux    | grep ibus` |
| 실행 명령어        | `ibus-daemon -drx`             |
| 한글 입력기 추가   | `Settings > Region & Language` |
| 단축키 설정        | `ibus-setup`                   |
| 환경변수 설정 파일 | `~/.xprofile`                  |
