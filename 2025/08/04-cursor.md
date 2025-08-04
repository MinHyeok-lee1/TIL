---
title: "Cursor AI Editor 설치"
date: "2025-08-04"
tags: ["cursor", "ros2", "ubuntu", "vscode"]
summary: "Cursor AI Editor 설치 및 활용 가이드 (Ubuntu 24.04.2)"
---

# Cursor AI Editor 설치 및 활용 가이드

Ubuntu 24.04.2에서 Cursor AI Editor를 설치하고 ROS2 Jazzy 개발 환경에 연동하는 방법 정리

## 📌 개요

- **목적**: AI 기반 코드 편집기(Copilot, Claude 통합) 사용
- **대상 환경**:
  - OS: Ubuntu 24.04.2
  - Shell: zsh
  - ROS2: Jazzy
  - Simulator: Gazebo Harmonic
- **설치 방식**: AppImage (Cursor 1.3.9 기준)

---

## 1️⃣ 설치 과정

### 1.1 AppImage 다운로드 및 권한 부여

```zsh
cd ~/Downloads
wget https://downloader.cursor.sh/linux/appimage/x86_64
mv x86_64 Cursor-1.3.9-x86_64.AppImage
chmod +x Cursor-1.3.9-x86_64.AppImage
```

---

### 1.2 설치 디렉토리 이동

```zsh
sudo mkdir -p /opt/cursor
sudo mv Cursor-1.3.9-x86_64.AppImage /opt/cursor
```

---

### 1.3 Desktop Entry 생성

```zsh
sudo nano /usr/share/applications/cursor.desktop
```

내용:

```ini
[Desktop Entry]
Name=Cursor
Exec=/opt/cursor/Cursor-1.3.9-x86_64.AppImage
Icon=/usr/share/pixmaps/cursor.png
Type=Application
Categories=Development;IDE;
Terminal=false
```

---

### 1.4 아이콘 등록

```zsh
sudo mv ~/Downloads/cursor.png /usr/share/pixmaps/
sudo update-desktop-database
sudo gtk-update-icon-cache /usr/share/icons/hicolor
```

---

### 1.5 zsh alias 추가

```zsh
echo "alias cursor='setsid /opt/cursor/Cursor-1.3.9-x86_64.AppImage \"\$@\" >/dev/null 2>&1 &'" >> ~/.zshrc
source ~/.zshrc
```

---

## 2️⃣ 실행 및 문제 해결

- 정상 실행:

```zsh
cursor
```

- Sandbox 오류 발생 시:

```zsh
/opt/cursor/Cursor-1.3.9-x86_64.AppImage --no-sandbox
```

---

## 3️⃣ 기본 사용법

### 3.1 워크스페이스 열기

```zsh
cd ~/robotware_ws
cursor .
```

### 3.2 AI 프롬프트 예시

```text
[에러 로그 분석]
다음 ROS2 launch 에러를 분석하고 해결 방법을 제시해줘
<여기에 에러 로그 붙여넣기>
```

```text
ROS2 Jazzy 환경에서 slam_toolbox와 teleop_twist_keyboard를 활용해
간단한 SLAM 네비게이션 패키지를 만들어줘
```

---

## 4️⃣ 개발 팁

- ROS2 환경에서 AI 모델에게 요청할 때:

  - `Shell=zsh`, `OS=Ubuntu 24.04`, `ROS2=Jazzy`, `Gazebo=Harmonic` 정보를 프롬프트에 명시
  - `source install/setup.zsh` 명령어를 기준으로 안내 요청

- AI를 통해:

  - 코드 리뷰
  - launch 파일 작성
  - 에러 로그 자동 분석
  - 최적화 파라미터 추천
  - README.md 자동 생성

---

## ✅ 설치 확인

- 메뉴 검색 → `Cursor` 실행
- 터미널 실행:

```zsh
cursor
```

- 정상 실행되면 ROS2 워크스페이스(`~/robotware_ws`)와 연동하여 사용 가능

---

## 🔗 참고

- window 설치 등 다른 설치방법을 하고자 하면 아래 링크 참고
  - [Cursor 공식 사이트](https://cursor.sh/)
