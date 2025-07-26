---
title: "Ubuntu 24.04.2 + Zsh + ROS 2 Jazzy 생산성 향상 스택 가이드"
date: "2025-07-26"
tags: ["ubuntu", "zsh", "ros2", "jazzy", "gazebo", "devtools"]
summary: "Ubuntu 24.04.2 Noble 기반 개발환경에서 zsh, AI CLI 자동완성, ROS2, Gazebo 등으로 생산성을 향상시키는 스택 및 설정 가이드"
---

# 🛠️ Ubuntu 24.04.2 기반 ROS 2 + Zsh 생산성 향상 개발환경 구성 가이드

---

## 1. 기본 환경

| 구성 요소 | 버전 / 패키지명                                     |
| --------- | --------------------------------------------------- |
| OS        | Ubuntu 24.04.2 (Noble Numbat)                       |
| Shell     | `zsh` (Oh My Zsh + Powerlevel10k + autosuggestions) |
| ROS       | ROS 2 Jazzy Jalisco                                 |
| Simulator | Gazebo Harmonic                                     |
| 터미널    | Terminator 또는 WezTerm                             |
| Python    | Python 3.12 (기본 내장)                             |
| 에디터    | Visual Studio Code (w/ ROS Extension Pack)          |

---

## 2. Zsh + Powerlevel10k + AI CLI 자동완성 설정

### 🔧 설치 및 설정

```bash
sudo apt update && sudo apt install -y zsh git curl

# Zsh 기본 쉘 설정
chsh -s $(which zsh)

# Oh My Zsh 설치
sh -c "$(curl -fsSL https://raw.githubusercontent.com/ohmyzsh/ohmyzsh/master/tools/install.sh)"

# Powerlevel10k 설치
git clone --depth=1 https://github.com/romkatv/powerlevel10k.git $ZSH_CUSTOM/themes/powerlevel10k

# .zshrc 수정
sed -i 's/ZSH_THEME=.*/ZSH_THEME="powerlevel10k\/powerlevel10k"/' ~/.zshrc

# 플러그인 추가
git clone https://github.com/zsh-users/zsh-autosuggestions $ZSH_CUSTOM/plugins/zsh-autosuggestions
git clone https://github.com/zsh-users/zsh-syntax-highlighting $ZSH_CUSTOM/plugins/zsh-syntax-highlighting

# .zshrc에 plugins 설정
plugins=(git zsh-autosuggestions zsh-syntax-highlighting)

source ~/.zshrc
```

---

## 3. ROS 2 Jazzy 설치 및 설정

```bash
sudo apt install software-properties-common
sudo add-apt-repository universe
sudo apt update && sudo apt upgrade -y

# ROS 2 키 및 소스 추가
sudo apt install curl gnupg lsb-release
sudo curl -sSL https://raw.githubusercontent.com/ros/rosdistro/master/ros.key -o /usr/share/keyrings/ros-archive-keyring.gpg
echo "deb [signed-by=/usr/share/keyrings/ros-archive-keyring.gpg] http://packages.ros.org/ros2/ubuntu $(lsb_release -cs) main" | sudo tee /etc/apt/sources.list.d/ros2.list > /dev/null

sudo apt update
sudo apt install -y ros-jazzy-desktop

echo "source /opt/ros/jazzy/setup.zsh" >> ~/.zshrc
```

---

## 4. Gazebo Harmonic 설치

```bash
sudo apt install gazebo-harmonic
```

또는 전체 개발환경 세트:

```bash
sudo apt install ros-jazzy-desktop ros-jazzy-gazebo-ros-pkgs ros-jazzy-navigation2 ros-jazzy-slam-toolbox
```

---

## 5. 생산성 향상 도구 스택

| 도구             | 설명                                                            |
| ---------------- | --------------------------------------------------------------- |
| `terminator`     | 다중 터미널 탭, 단축키로 작업 분할                              |
| `tmux`           | 터미널 세션 멀티플렉싱                                          |
| `ripgrep` (`rg`) | 빠른 텍스트 검색                                                |
| `fd`             | `find` 대체 고성능 명령어                                       |
| `htop`, `btop`   | 리소스 모니터링 도구                                            |
| `zoxide`         | `cd`의 AI 보완판 (`z <dir>`)                                    |
| `lazygit`        | CLI에서 Git UI 제공                                             |
| `bat`            | `cat` 대체로 코드 하이라이팅 지원                               |
| `exa`            | `ls` 대체로 컬러 및 Tree 보기 지원                              |
| `vscode`         | ROS 2 Extension Pack, Remote SSH, clangd, Python, C++ 설정 추천 |

설치 예

```bash
sudo apt install exa bat ripgrep fd-find lazygit zoxide htop tmux
```

---

## 6. .zshrc 추천 alias 및 설정

```zsh
alias ll='exa -al --color=always --group-directories-first'
alias gs='git status'
alias gb='git branch'
alias gco='git checkout'
alias rbuild='cd ~/ros2_ws && colcon build --symlink-install && source install/setup.zsh'
alias rsrc='source ~/ros2_ws/install/setup.zsh'
alias rclean='cd ~/ros2_ws && rm -rf build install log'
alias rosdep-init='sudo rosdep init && rosdep update'
```

---

## 7. 추천 개발 워크플로우

1. `terminator` 혹은 `tmux` 로 작업 공간 구분
2. zsh + autosuggestions로 반복 명령 자동 완성
3. `colcon build` 시 별도 로그 창에서 확인
4. `vscode`로 ROS 2 패키지 디버깅 및 `launch.json` 설정
5. 필요 시 `rviz2`, `gazebo`를 별도 화면으로 실행
