---
title: "GitHub Actions 정리"
date: "2025-07-04"
tags: ["github", "CI/CD"]
summary: "GitHub Actions의 개념과 왜 사용하는 지에 대한 정리"
---

# GitHub Actions

**GitHub Actions**는 GitHub에서 제공하는 강력한 CI/CD 도구  
이를 통해 개발자가 **빌드, 테스트, 배포 및 자동화 작업을 쉽게 수행**할 수 있도록 지원

이 글에선 GitHub Actions의 기본 개념과 사용 예시, 활용 방법까지 깔끔하게 정리

---

## 📌 GitHub Actions 개념

GitHub Actions는 아래 3가지 개념을 중심으로 동작

- **Workflow** (`.yml` 파일): 어떤 작업을 할지 정의한 스크립트
- **Job**: 하나의 Workflow가 수행하는 개별 작업
- **Step**: Job을 구성하는 실제 작업 단위 (명령어, Action 등)

예를 들면

```yaml
# .github/workflows/example.yml
name: Example Workflow

on: [push]

jobs:
  build:
    runs-on: ubuntu-latest

    steps:
      - uses: actions/checkout@v4

      - name: 프로젝트 빌드
        run: |
          npm install
          npm run build
```

---

## 📦 왜 GitHub Actions를 쓰는가?

GitHub Actions를 사용하는 주요 목적은 다음과 같음

- 코드의 빌드 및 테스트를 자동화하여 개발 생산성 향상
- 코드 변경 시 바로 배포하여 배포 속도 증가
- 코드 리뷰, 코드 품질 체크 등 반복적 작업 자동화

개발자의 귀중한 시간을 아껴주는 강력한 도구임

---

## 🛠️ GitHub Actions 사용법 (기초 예제)

### 1️⃣ Workflow 파일 생성하기

리포지토리 내 `.github/workflows/` 폴더에 `.yml` 파일 생성

```bash
.github/workflows/
└── deploy.yml
```

### 2️⃣ Workflow 정의 (간단한 예제)

```yaml
name: 🚀 자동 빌드 및 배포

on:
  push:
    branches: [main] # main 브랜치에 push되면 실행됨

jobs:
  deploy:
    runs-on: ubuntu-latest
    steps:
      - name: 코드 가져오기
        uses: actions/checkout@v4

      - name: Node.js 설정
        uses: actions/setup-node@v4
        with:
          node-version: "20"

      - name: 종속성 설치
        run: npm install

      - name: 프로젝트 빌드
        run: npm run build

      - name: 정적 페이지 배포
        uses: peaceiris/actions-gh-pages@v3
        with:
          github_token: ${{ secrets.GITHUB_TOKEN }}
          publish_dir: ./build
```

이렇게 하면 코드가 푸시될 때마다 자동으로 빌드 후 GitHub Pages로 배포

---

## 🌟 GitHub Actions 실제 활용 사례

GitHub Actions로 실제로 할 수 있는 일은 매우 다양함

- **정적 웹사이트 배포 자동화** (React, Nextra, Vue 등)
- **Markdown 문서 → 블로그 포스팅 자동화**
- **자동 테스트 (Jest, pytest 등)**
- **코드 품질 검사 (ESLint, Prettier 등)**
- **도커 컨테이너 자동 빌드 및 배포**

---

## 🔖 TIL에서 GitHub Actions 활용 사례 소개 (Markdown 기반)

> GitHub Actions로 TIL을 손쉽게 Tistory나 블로그 플랫폼에 발행하는 사례를 소개

**목표**:
`TIL/*.md` → 블로그 게시용 Markdown으로 자동 변환

**Workflow 예시** (`til-to-blog.yml`):

```yaml
name: 📚 TIL → Blog 자동 변환

on:
  push:
    paths:
      - "TIL/**/*.md"

jobs:
  export-md:
    runs-on: ubuntu-latest
    steps:
      - uses: actions/checkout@v4

      - name: Python 환경 구성
        uses: actions/setup-python@v5
        with:
          python-version: "3.12"

      - name: 필요한 라이브러리 설치
        run: pip install pyyaml

      - name: 블로그용 Markdown 생성
        run: python scripts/export_to_blog.py

      - name: 변환된 Markdown 커밋 & 푸시
        uses: stefanzweifel/git-auto-commit-action@v5
        with:
          commit_message: "📝 블로그용 Markdown 자동 변환"
```

이렇게 하면 TIL이 올라갈 때마다 블로그용 글을 자동으로 준비할 수 있음

---

## 🎯 GitHub Actions를 활용할 때 꿀팁

1. **Secrets 활용**
   API 키나 배포 정보는 GitHub Secrets로 안전하게 관리

2. **Marketplace 적극 활용**
   [GitHub Marketplace](https://github.com/marketplace?type=actions)에 미리 준비된 수많은 Actions가 존재 (빌드, 배포, 알림 등)

3. **Workflow 시각화 활용**
   Workflow 결과를 GitHub UI에서 바로 시각화하여 작업 흐름을 쉽게 확인할 수 있음

---

## 🚩 결론

1. GitHub Actions는 자동화를 통해 개발 생산성을 크게 높일 수 있는 강력한 도구
2. 작은 반복 작업부터 복잡한 배포 시스템까지, GitHub Actions를 적극적으로 활용 가능함
