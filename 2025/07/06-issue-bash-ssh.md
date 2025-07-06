---
title: "bash에서 ssh-agent, ssh-add issue (source와 실행의 차이)"
date: 2025-07-06
tags: ["git", "github", "ssh", "bash"]
summary: "bash 환경에서 ssh-agent/ssh-add 스크립트 실행 시 source를 써야 하는 이유와 실전 예시"
---

# bash에서 ssh-agent, ssh-add: 왜 source로 실행해야 할까?

Linux, macOS, WSL, Git Bash 등 **모든 bash 계열 환경**에서  
`ssh-agent`와 `ssh-add`를 쓸 때  
스크립트를 `./파일.sh`로 실행하면 안 되고  
꼭 `source 파일.sh`로 실행해야 제대로 동작함

---

## 🔧 내용

- `ssh-agent`는 SSH 인증 정보를 관리하는 프로세스임
- `ssh-add`로 개인 키를 agent에 올리면  
  같은 터미널 세션에서는 git pull/push 할 때 패스프레이즈를 반복 입력하지 않아도 됨

- 보통 아래처럼 스크립트(`ssh.sh`)에 두 줄을 씀

  ```sh
  eval "$(ssh-agent -s)"
  ssh-add ~/.ssh/id_ed25519
  ```

- **문제:**
  `./ssh.sh`로 실행하면

  - `ssh-agent`가 백그라운드에서 돌긴 하지만
  - \*\*환경변수(SSH_AUTH_SOCK, SSH_AGENT_PID 등)\*\*가
    부모 터미널(현재 셸)에 적용되지 않음
  - 결과적으로 agent 연결이 안 돼서
    `ssh-add -l` 등에서 "Could not open a connection..." 에러 발생

- **해결:**
  반드시

  ```sh
  source ssh.sh
  ```

  또는

  ```sh
  . ssh.sh
  ```

  처럼 실행해야 함
  → 이렇게 해야 `ssh-agent`의 환경변수가 현재 터미널에 적용되어
  ssh-add, git pull/push 모두 정상 동작

---

## 📝 실전 예시

```sh
$ ./ssh.sh
Agent pid 719
Enter passphrase for /c/Users/User/.ssh/id_ed25519:
Identity added: /c/Users/User/.ssh/id_ed25519 (minhyeok.lee1@gmail.com)

$ ssh-add -l
Could not open a connection to your authentication agent.
```

→ **agent가 뜨긴 했지만 환경변수 적용 안 됨**

```sh
$ source ssh.sh
Agent pid 776
Enter passphrase for /c/Users/User/.ssh/id_ed25519:
Identity added: /c/Users/User/.ssh/id_ed25519 (minhyeok.lee1@gmail.com)

$ ssh-add -l
256 SHA256:1CFnBcpDCsb+A0ffmquB17ArEeWbD5WsD7mIUFy2hvg minhyeok.lee1@gmail.com (ED25519)
```

→ **정상적으로 ssh-agent와 연결, 키 관리 가능!**

---

## 📌 참고

- bash/zsh 계열 환경에서 환경변수 세팅/유지 필요할 때는
  **무조건 `source`로 실행**해야 함
- Windows cmd, PowerShell 등은 동작 방식 다름
- ssh-agent와 관련된 환경변수(SSH_AUTH_SOCK, SSH_AGENT_PID)가
  현재 세션에 적용되어야 git 명령, ssh-add 등 제대로 동작

---

> **정리:**
>
> bash, zsh, WSL, macOS, Linux 등 bash 계열 터미널에서는
> ssh-agent/ssh-add 세팅 스크립트는 반드시 `source`로 실행
> (`./파일.sh`는 환경변수 적용이 안 되니 주의!)
