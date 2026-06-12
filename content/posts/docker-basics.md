---
title: "Docker 기초: 컨테이너의 세계로"
date: 2026-06-11
draft: false
tags: ["Docker", "컨테이너", "DevOps", "인프라"]
categories: ["DevOps"]
summary: "Docker의 기본 개념부터 실전 활용까지 알아봅니다."
---

## Docker란?

Docker는 애플리케이션을 컨테이너로 패키징하여 어디서든 동일하게 실행할 수 있게 해주는 플랫폼입니다.

## 왜 Docker를 써야 할까?

- **환경 일관성**: "내 컴퓨터에서는 되는데..." 문제 해결
- **빠른 배포**: 이미지 기반으로 즉시 실행
- **격리**: 애플리케이션 간 충돌 방지
- **확장성**: 컨테이너 오케스트레이션과 연계

## 기본 명령어

```bash
# 이미지 빌드
docker build -t myapp .

# 컨테이너 실행
docker run -d -p 8080:80 myapp

# 실행 중인 컨테이너 확인
docker ps

# 로그 확인
docker logs <container-id>

# 컨테이너 중지
docker stop <container-id>
```

## Dockerfile 예제

```dockerfile
# Node.js 애플리케이션
FROM node:20-alpine

WORKDIR /app

COPY package*.json ./
RUN npm ci --only=production

COPY . .

EXPOSE 3000
CMD ["node", "server.js"]
```

## Docker Compose

여러 컨테이너를 한 번에 관리할 때 사용합니다:

```yaml
version: '3.8'
services:
  web:
    build: .
    ports:
      - "3000:3000"
    depends_on:
      - db

  db:
    image: postgres:15
    environment:
      POSTGRES_DB: myapp
      POSTGRES_PASSWORD: secret
    volumes:
      - db-data:/var/lib/postgresql/data

volumes:
  db-data:
```

## 베스트 프랙티스

1. **멀티 스테이지 빌드**로 이미지 크기 최소화
2. **.dockerignore** 파일로 불필요한 파일 제외
3. **비루트 사용자**로 보안 강화
4. **레이어 캐싱**을 고려한 Dockerfile 작성

## 다음 단계

Docker를 익혔다면 다음을 학습하세요:
- Kubernetes (컨테이너 오케스트레이션)
- Docker Swarm
- CI/CD 파이프라인 통합
