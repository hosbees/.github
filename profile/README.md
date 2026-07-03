<img width="192" height="192" alt="apple-icon-precomposed" src="https://github.com/user-attachments/assets/f35364d7-1308-492c-8b62-7a287ae75144" />

# Hosbee - 프리랜서 개발자를 위한프로젝트 수주 플랫폼

---

## 프로젝트 구조

```
hosbee/
├── hosbee-common/       # 공통 유틸리티 및 공유 컴포넌트
├── hosbee-admin-api/    # 관리자 API 서버
├── hosbee-admin-ui/     # 관리자 웹 UI
├── hosbee-user-api/     # 사용자 API 서버
└── hosbee-web-ui/       # 사용자 웹 UI
```

---

## 기술 스택

| 항목 | 내용 |
|------|------|
| Language | Java 17 |
| Framework | Spring Boot 3.4.3 |
| Build | Gradle 8.5 |
| Database | MySQL |
| Container | Docker / Docker Compose |
| CI/CD | Jenkins |

---

## 포트 정보

| 모듈 | 개발(dev) | 운영(prod) |
|------|-----------|------------|
| hosbee-admin-api | 9130 | 9133 |
| hosbee-admin-ui | 9030 | 9033 |
| hosbee-user-api | 9092 | 9192 |
| hosbee-web-ui | 8081 | 8001 |

---

## URL 정보

| 모듈 | 개발(dev)                | 운영(prod)               |
|------|------------------------|------------------------|
| hosbee-admin-api | https://dev-admin-api.hosbee.com | https://admin-api.hosbee.com |
| hosbee-admin-ui | https://dev-admin.hosbee.com | https://admin.hosbee.com |
| hosbee-user-api | https://dev-api.hosbee.com | https://api.hosbee.com |
| hosbee-web-ui | https://dev.hosbee.com | https://www.hosbee.com |

---

## 실행 방법

### 사전 요구사항

- Java 17 이상
- Docker / Docker Compose
- Gradle 8.5

### 빌드

```bash
./gradlew clean build -x test
```

### 테스트

```bash
./gradlew test
```

### 개발 환경 실행

```bash
export DB_USER=<DB_USER>
export DB_PASSWORD=<DB_PASSWORD>
export BUILD_VERSION=latest
export DOCKER_REGISTRY=localhost:5000

docker compose -f docker-compose.dev.yml up -d
```

### 운영 환경 실행

```bash
export DB_USER=<DB_USER>
export DB_PASSWORD=<DB_PASSWORD>
export BUILD_VERSION=<버전>
export DOCKER_REGISTRY=<레지스트리 주소>

docker compose -f docker-compose.prod.yml up -d
```

---

## CI/CD

Jenkins 파이프라인을 통해 자동 빌드 및 배포가 수행됩니다.

| 브랜치       | 배포 환경 | 특이사항                             |
|-----------|-----------|----------------------------------|
| `main`    | 운영(prod) | 수동 승인 후 배포 <br/> (매일 16시 관리자 승인) |
| `develop` | 개발(dev) | 자동 배포                            |

### Jenkins Credentials 등록 필요 항목

| ID | 설명 |
|----|------|
| `docker-registry-credentials` | Docker Registry 인증 정보 |
| `db-dev-credentials` | 개발 DB 계정 정보 |
| `db-prod-credentials` | 운영 DB 계정 정보 |

---

## Health Check

각 서비스는 `/actuator/health` 엔드포인트로 상태 확인이 가능합니다.

```bash
# 예시
curl http://localhost:9130/actuator/health
```

### 컨테이너 로그 확인

```bash
# 전체 서비스
docker compose -f docker-compose.prod.yml logs -f

# 특정 서비스
docker compose -f docker-compose.prod.yml logs -f hosbee-user-api
```

