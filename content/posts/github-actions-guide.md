---
title: "GitHub Actions 입문 가이드"
date: 2026-06-12
draft: false
tags: ["GitHub Actions", "CI/CD", "DevOps", "자동화"]
categories: ["DevOps"]
summary: "GitHub Actions의 기본 개념과 워크플로우 작성법을 알아봅니다."
---

## GitHub Actions란?

GitHub Actions는 GitHub에서 제공하는 CI/CD 플랫폼입니다. 코드 변경 시 자동으로 빌드, 테스트, 배포를 수행할 수 있습니다.

## 핵심 개념

### 1. Workflow (워크플로우)

`.github/workflows/` 디렉토리에 YAML 파일로 정의합니다.

```yaml
name: CI
on:
  push:
    branches: [main]
jobs:
  build:
    runs-on: ubuntu-latest
    steps:
      - uses: actions/checkout@v4
      - run: echo "Hello, World!"
```

### 2. Event (이벤트)

워크플로우를 트리거하는 조건입니다:

| 이벤트 | 설명 |
|--------|------|
| `push` | 코드 푸시 시 |
| `pull_request` | PR 생성/업데이트 시 |
| `schedule` | 크론 스케줄 |
| `workflow_dispatch` | 수동 실행 |

### 3. Job (작업)

하나의 워크플로우는 여러 Job으로 구성됩니다. 각 Job은 독립적인 가상 머신에서 실행됩니다.

### 4. Step (스텝)

Job 내부의 개별 명령입니다. `uses`로 액션을 사용하거나, `run`으로 쉘 명령을 실행합니다.

## 실전 예제: Node.js 테스트 자동화

```yaml
name: Node.js CI

on:
  push:
    branches: [main]
  pull_request:
    branches: [main]

jobs:
  test:
    runs-on: ubuntu-latest
    strategy:
      matrix:
        node-version: [18.x, 20.x]

    steps:
      - uses: actions/checkout@v4

      - name: Use Node.js ${{ matrix.node-version }}
        uses: actions/setup-node@v4
        with:
          node-version: ${{ matrix.node-version }}
          cache: 'npm'

      - run: npm ci
      - run: npm test
```

## 유용한 팁

1. **캐싱 활용**: 의존성 캐싱으로 빌드 시간을 단축하세요
2. **시크릿 관리**: 민감한 정보는 Repository Secrets에 저장하세요
3. **매트릭스 빌드**: 여러 환경에서 동시 테스트가 가능합니다
4. **재사용 가능한 워크플로우**: 공통 로직을 분리하여 DRY 원칙을 지키세요

## 마무리

GitHub Actions는 설정이 간단하면서도 강력한 자동화 도구입니다. 이 블로그 자체도 GitHub Actions로 자동 빌드/배포되고 있습니다!
