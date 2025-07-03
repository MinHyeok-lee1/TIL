---
title: "GitHub Template 정리"
date: 2025-07-03
tags: ["github", "template"]
summary: "GitHub의 다양한 템플릿 기능(issue, PR, workflow 등)에 대한 정리"
---

# GitHub Template

GitHub에서는 프로젝트의 일관성과 협업 효율을 높이기 위해 다양한 템플릿 기능을 제공  
주로 사용되는 템플릿은 다음과 같음

---

## ✅ Issue Template

### 📍 목적

이슈 등록 시 일정한 양식으로 버그 리포트, 기능 요청 등을 받기 위해 사용

### 📁 위치

```bash

.github/ISSUE\_TEMPLATE/

```

### 📄 예시 (`bug_report.yml`)

```yaml
name: 🐛 Bug Report
description: 버그 발생 시 작성해주세요.
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
      description: 문제가 발생한 상황을 구체적으로 적어주세요.
    validations:
      required: true
```

---

## ✅ Pull Request Template

### 📍 목적

PR 작성 시 변경 요약, 관련 이슈, 체크리스트 등을 안내해 리뷰 품질을 높이기 위함

### 📁 위치

```bash
.github/pull_request_template.md
```

### 📄 예시

```markdown
## ✨ 변경 사항

- 어떤 점을 변경했는지 간략히 기술

## 📌 관련 이슈

- Closes #123

## ✅ 체크리스트

- [ ] 테스트를 모두 통과함
- [ ] 문서화가 완료됨
- [ ] 관련 이슈와 연결됨
```

---

## ✅ Workflow Template (Reusable Workflows)

### 📍 목적

여러 리포지토리에서 공통된 GitHub Actions 워크플로우를 재사용하기 위해

### 📁 위치

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

## ✅ 기타 템플릿

| 종류          | 위치                             | 설명                               |
| ------------- | -------------------------------- | ---------------------------------- |
| `FUNDING.yml` | `.github/FUNDING.yml`            | GitHub Sponsors, Patreon 등 연결용 |
| `CODEOWNERS`  | `.github/CODEOWNERS` 또는 `root` | 코드 리뷰 자동 지정자 등록         |
| `SECURITY.md` | `.github/SECURITY.md`            | 보안 이슈 신고 방법 문서           |

---

## 🔚 마무리

GitHub 템플릿은 협업의 질을 높이고, 오픈소스 프로젝트를 체계적으로 운영하는 데 매우 유용한 기능  
특히 팀 작업, PR 리뷰, 외부 기여자가 있는 프로젝트에선 적극적인 활용이 필수
