---
title: "YAML, YML"
date: "2025-07-08"
tags: ["yaml-configuration", "devops-automation", "infrastructure-as-code"]
summary: "YAML, YML 파일이란 무엇인가? 왜 사용하는가?"
---

# YAML, YML 파일이란 무엇인가? 왜 사용하는가?

1. YML 파일, 또는 YAML 파일(YAML Ain't Markup Language)은 **데이터 직렬화 형식** 중 하나
2. YML은 YAML의 줄임말, 주로 구성 파일(config file)이나 데이터의 저장 및 전달에 사용
3. YAML은 사람이 읽기 쉽고, 다른 프로그래밍 언어와의 호환성도 우수해 널리 사용됨
4. 아래에서는 YML 파일이 무엇인지, 그리고 왜 개발에서 자주 사용되는지에 대해 살펴봄

---

## 📝 YML 파일의 기본 개념

YML 파일은 **구성 파일**이나 **설정 파일**을 저장하는 데 사용  
XML이나 JSON과 같은 다른 파일 형식들과 비교했을 때, YAML은 간결하고 읽기 쉬운 형식으로 데이터를 표현할 수 있어 개발자들 사이에서 인기가 높음

- **YAML:** YAML은 사람이 읽고 쓰기 쉬운 데이터 직렬화 형식으로, 주로 설정 파일이나 데이터를 표현할 때 사용
- **마크업 언어 아님:** YML의 "Ain't Markup Language"라는 이름은 XML과 같은 마크업 언어가 아니며, 단순히 데이터를 표현하는 데 초점을 맞춘다는 의미

YML 파일의 특징은 **들여쓰기**를 통해 데이터의 계층 구조를 표현하며, 이를 통해 복잡한 데이터를 쉽게 관리하고 설정할 수 있음

```yaml
# 간단한 YML 예시
name: "John Doe"
age: 30
address:
  street: "123 Main St"
  city: "Somewhere"
  country: "Countryland"
```

---

## 🔧 YML 파일의 주요 사용 사례

YML 파일은 다양한 분야에서 사용됨  
**CI/CD 파이프라인** 설정, **Docker 설정**, **Kubernetes 설정**, **애플리케이션 설정 파일** 등에서 많이 사용

### 1. CI/CD 파이프라인에서의 사용

YML 파일은 CI/CD 도구에서 설정 파일로 사용되며, 파이프라인의 각 단계를 정의하는 데 매우 유용  
예를 들어, **GitHub Actions**나 **GitLab CI**와 같은 도구는 YML 파일을 사용하여 빌드, 테스트, 배포 작업을 정의

```yaml
# GitHub Actions 예시
name: CI Pipeline

on: [push]

jobs:
  build:
    runs-on: ubuntu-latest
    steps:
      - uses: actions/checkout@v2
      - name: Set up Python
        uses: actions/setup-python@v2
        with:
          python-version: "3.8"
      - name: Install dependencies
        run: |
          pip install -r requirements.txt
      - name: Run tests
        run: |
          python -m unittest discover
```

### 2. Docker와 Kubernetes 설정

Docker와 Kubernetes는 YAML을 사용하여 컨테이너 및 클러스터의 설정을 관리함  
예를 들어, Docker Compose 파일은 여러 컨테이너를 정의하고 연결하는 데 사용되며, Kubernetes의 `deployment.yaml` 파일은 파드(pod) 설정을 정의하는 데 사용

```yaml
# Docker Compose 예시
version: "3"
services:
  web:
    image: nginx
    ports:
      - "80:80"
  db:
    image: mysql
    environment:
      MYSQL_ROOT_PASSWORD: example
```

### 3. 애플리케이션 설정

YML 파일은 애플리케이션의 다양한 설정을 저장하는 데 사용될 수 있음  
예를 들어, **Spring Boot** 애플리케이션에서는 `application.yml` 파일을 사용하여 데이터베이스 연결 정보나 기타 설정을 정의

```yaml
# Spring Boot 애플리케이션 설정 예시
server:
  port: 8080

spring:
  datasource:
    url: jdbc:mysql://localhost:3306/mydb
    username: user
    password: password
```

---

## 🛠️ YML 파일의 장점

YML 파일의 가장 큰 장점은 **간결성**과 **가독성**임  
JSON과 같은 다른 형식에 비해 YML은 들여쓰기를 통해 데이터를 구조적으로 표현할 수 있어 가독성이 뛰어남  
JSON처럼 큰 괄호나 따옴표를 사용할 필요가 없어 코드가 더욱 깔끔해짐

### 1. 간결한 문법

YML은 JSON보다 훨씬 간결하고 직관적인 문법을 사용  
JSON에서의 중괄호와 따옴표를 사용하지 않고도 데이터의 계층 구조를 쉽게 정의할 수 있음

### 2. 가독성 높음

YML 파일은 사람이 읽고 수정하기 쉬운 형태로 구성되어 있어, 설정 파일을 자주 수정하는 개발자들에게 매우 유용함 들여쓰기를 사용하여 계층 구조를 나타내므로, 복잡한 설정도 한 눈에 파악할 수 있음

### 3. 다양한 툴과 호환성

YML은 다양한 툴과 호환됨. Docker, Kubernetes, CI/CD 툴, Ansible, Terraform 등 여러 자동화 및 DevOps 도구에서 YML 파일을 사용하여 환경 설정을 관리

---

## 🏁 결론

1. YML 파일은 개발 및 운영 환경에서 매우 유용한 도구
2. 사람이 읽기 쉬운 형식으로 데이터를 구조적으로 표현할 수 있어 설정 파일을 효율적으로 관리할 수 있음
3. 다양한 DevOps 툴, 애플리케이션 설정, CI/CD 파이프라인에서 사용되는 YML은 현대 소프트웨어 개발에 필수적인 파일 형식
4. YML 파일을 잘 활용하면 자동화 작업을 더 쉽고 빠르게 처리할 수 있음
5. 개발 및 배포 과정에서 발생할 수 있는 오류를 줄이는 데 큰 도움이 됨
