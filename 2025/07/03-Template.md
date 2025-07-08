---
title: "GitHub Template 정리"
date: "2025-07-03"
tags: ["github", "template"]
summary: "GitHub의 다양한 템플릿 기능(issue, PR, workflow 등)에 대한 정리"
---

# GitHub Template

GitHub를 사용하며 프로젝트를 진행하다 보면, 이슈와 PR이 무작정 쌓이면서 관리가 어려워지는 경험을 누구나 한 번쯤은 겪었을 것  
프로젝트를 체계적으로 관리하고 협업의 효율을 높일 수 있는 **GitHub 템플릿**에 대해 정리

---

## ✅ Issue Template

### 📍 사용 이유

Issue 템플릿을 사용하면, 버그 리포트나 새로운 기능 요청 등을 일정한 양식으로 받을 수 있어 효율적인 관리와 신속한 대응이 가능

### 📁 파일 위치

```bash
.github/ISSUE_TEMPLATE/
```

### 📄 예시 (`bug_report.yml`)

```yaml
name: 🐛 Bug Report
description: 발견한 버그에 대해 알려주세요.
title: "[BUG] "
labels: ["bug"]
body:
  - type: input
    id: environment
    attributes:
      label: 환경
      description: OS, 브라우저, Node.js 버전 등
      placeholder: 예) macOS 14, Chrome 115, Node 18.x
    validations:
      required: true
  - type: textarea
    id: description
    attributes:
      label: 문제 설명
      description: 발생한 문제를 최대한 구체적으로 적어주세요.
    validations:
      required: true
```

---

## ✅ Pull Request Template

### 📍 리뷰의 질을 높이는 방법

PR 템플릿을 통해 코드 변경의 의도를 명확히 전달하고, 관련된 이슈와 체크리스트를 함께 작성하면 코드 리뷰 품질이 향상

### 📁 파일 위치

```bash
.github/pull_request_template.md
```

### 📄 예시

```markdown
## ✨ 변경 사항

- 변경된 사항을 간략히 설명해주세요.

## 📌 관련 이슈

- Closes #123

## ✅ 체크리스트

- [ ] 모든 테스트가 통과했나요?
- [ ] 문서화가 완료되었나요?
- [ ] 관련된 이슈가 연결되었나요?
```

---

## ✅ Workflow Template (Reusable Workflows)

### 📍 워크플로우 재사용의 강력한 이점

Reusable Workflows를 사용하면 여러 리포지토리에서 공통된 GitHub Actions 워크플로우를 쉽게 공유하고 재사용할 수 있음

### 📁 파일 위치

```bash
.github/workflows/template.yml
```

### 📄 예시

```yaml
name: Reusable Build

on:
  workflow_call:

jobs:
  build:
    runs-on: ubuntu-latest
    steps:
      - uses: actions/checkout@v3
      - run: echo "🏗️ Building..."
```

---

## ✅ 알아두면 유용한 기타 템플릿

GitHub에서는 이 외에도 프로젝트 운영에 도움을 주는 다양한 템플릿을 제공

| 종류          | 위치                                    | 설명                               |
| ------------- | --------------------------------------- | ---------------------------------- |
| `FUNDING.yml` | `.github/FUNDING.yml`                   | GitHub Sponsors, Patreon 등 연결용 |
| `CODEOWNERS`  | `.github/CODEOWNERS` 또는 루트 디렉터리 | 코드 리뷰 시 자동 리뷰어 지정      |
| `SECURITY.md` | `.github/SECURITY.md`                   | 보안 이슈 신고 방법 안내           |

---

## 🔚 GitHub 템플릿, 협업을 더 쉽고 명확하게 작성

GitHub에서 제공하는 다양한 템플릿 기능을 잘 활용하면 프로젝트 관리가 쉬워지고, 팀원 및 외부 기여자와의 협업 효율도 극대화 됨
