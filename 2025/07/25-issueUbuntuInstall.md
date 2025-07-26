---
title: "Ubuntu 24.04 수동 설치 및 GUI 복구 구축기 (HP OMEN 16, 듀얼부팅)"
date: "2025-07-25"
tags: ["ubuntu", "debootstrap", "lightdm", "issue"]
summary: "정상 설치가 불가능했던 HP OMEN 16 노트북에서 Ubuntu 24.04를 debootstrap으로 수동 설치하고 GUI 및 터치패드까지 복구한 전 과정 기록"
---

# 🛠️ Ubuntu 24.04 수동 설치 구축기

정상 설치가 불가능했던 HP OMEN 16 노트북에서 Ubuntu 24.04를 debootstrap으로 수동 설치하고 GUI 및 터치패드까지 복구한 전 과정 기록

## 🧾 배경

- **노트북 사양**: HP OMEN 16 (2025), Ryzen 9, RTX 5070
- **목표**: Windows 11과 Ubuntu 24.04 듀얼부팅 + ROS 2, Gazebo 개발 환경
- **문제**: 모든 우분투 버전(20/22/24 Desktop/Server)에서 GUI 설치 실패 또는 `cloud-init`, `PPM init failed`, `UnicodeDecodeError`, `Read-only FS` 등 다양한 오류 발생

---

## 🧱 1. 기본 파티션 준비

1. **Windows 11 설치 및 여유 공간 확보 (디스크 관리로 Shrink)**
2. **UEFI 설정**: Secure Boot 해제, Fast Boot 해제, USB 부팅 활성화
3. **Live 환경 진입 후 파티션 확인**

   ```bash
   lsblk -f
   sudo fdisk /dev/nvme0n1  # 파티션 정리
   ```

---

## 🧰 2. debootstrap을 통한 우분투 최소 설치

1. **Ubuntu Live Server 환경 진입 후 설치 도구 준비**

   ```bash
   sudo apt install debootstrap
   sudo debootstrap noble /mnt http://archive.ubuntu.com/ubuntu
   ```

2. **chroot 진입**

   ```bash
   sudo mount --bind /dev /mnt/dev
   sudo mount --bind /proc /mnt/proc
   sudo mount --bind /sys /mnt/sys
   sudo chroot /mnt
   ```

3. **기초 설정**

   ```bash
   echo "ubuntu" > /etc/hostname
   echo "127.0.0.1 localhost" > /etc/hosts
   echo "127.0.1.1 ubuntu" >> /etc/hosts
   ```

4. **네트워크 수동 설정**

   ```bash
   echo "nameserver 8.8.8.8" > /etc/resolv.conf
   apt update
   apt install net-tools network-manager -y
   ```

---

## 🖥️ 3. GUI 환경 설치

1. **Display Manager 설치**

   ```bash
   apt install lightdm -y
   systemctl enable lightdm
   ```

2. **ubuntu-desktop 설치**

   ```bash
   apt install ubuntu-desktop -y
   ```

3. **그래픽 드라이버 설치**

   ```bash
   sudo ubuntu-drivers autoinstall
   ```

---

## 🛠️ 4. 부팅 시 마운트 문제 해결

1. **루트 파티션이 항상 read-write로 마운트되도록 fstab 설정**

   ```bash
   findmnt /
   blkid /dev/nvme0n1pX  # 루트 파티션 UUID 확인
   nano /etc/fstab
   ```

   ```fstab
   UUID=XXXX-XXXX  /  ext4  defaults  0 1
   ```

2. **fstab 적용**

   ```bash
   mount -a
   ```

---

## 🖱️ 5. 터치패드 및 마우스 동작 복구

```bash
apt install xserver-xorg-input-libinput xserver-xorg-input-synaptics -y
```

→ GUI 진입 시 마우스 정상 작동 확인됨

---

## 🧵 6. 마무리 및 팁

- **재부팅 후 GUI가 CLI로 뜰 경우**

  ```bash
  mount -o remount,rw /
  sudo systemctl restart lightdm
  ```

- **필요 패키지 요약**

  ```bash
  apt install ubuntu-desktop lightdm xorg xinput xserver-xorg-input-libinput
  ```

- **부팅 자동화 점검**

  ```bash
  systemctl status lightdm
  ```

---

## ✅ 결과

- ✅ Ubuntu 24.04 GUI 설치 성공
- ✅ 듀얼부팅, 그래픽 드라이버, 마우스 입력, fstab 마운트까지 완전 해결
- ✅ ROS2, Gazebo, SLAM 개발환경 구축 가능한 클린 시스템 확보

---

## 🔚 참고 링크

- [debootstrap 공식 문서](https://wiki.debian.org/Debootstrap)
- [Ubuntu fstab 설정](https://help.ubuntu.com/community/Fstab)
- [Ubuntu minimal GUI 설치](https://wiki.ubuntu.com/Minimal)
