# 📈 TAI Backend

> Google Trends 데이터 수집 및 AI 분석 백엔드 서버

실시간 Google Trends 데이터를 수집하고, Python LLM 서버와 연동하여 트렌드 키워드를 AI로 분석하는 Spring Boot 기반 백엔드 시스템입니다.

## 🛠 기술 스택

- **Java 21**
- **Spring Boot 3.5.7**
  - Spring Data JPA (데이터 접근)
  - Spring WebFlux (비동기 HTTP 통신)
  - Spring Validation
- **PostgreSQL** (데이터베이스)
- **Rome 1.18.0** (Google Trends RSS 파싱)
- **SpringDoc OpenAPI 2.8.13** (API 문서화)
- **Apache HttpClient 5** (RestClient 기반 HTTP 통신)
- **Lombok** (보일러플레이트 코드 제거)

## ✨ 주요 기능

### 1. Google Trends 실시간 수집
- Google Trends RSS 피드에서 한국 지역 트렌드 데이터 파싱
- 키워드, 순위, 검색량, 카테고리 정보 수집

### 2. 트렌드 조회 API
- 트렌드 목록 조회 (메인 화면용)
- 트렌드 상세 조회 (상세 페이지용)

### 3. Python LLM 서버 연동
- WebClient 기반 비동기 통신
- 트렌드 키워드를 LLM에 전송하여 AI 분석 결과 수신

## 📡 API 명세

### Trend API

#### 트렌드 목록 조회
```http
GET /api/trend
```

**응답 예시:**
```json
[
  {
    "id": 1,
    "region": "KR",
    "rank": 1,
    "keyword": "연말정산",
    "description": "2025년 연말정산 시즌",
    "approx_traffic": "500K+",
    "category": "경제",
    "tags": ["세금", "환급"]
  }
]
```

#### 트렌드 상세 조회
```http
GET /api/trend/{id}
```

**응답 예시:**
```json
{
  "id": 1,
  "region": "KR",
  "rank": 1,
  "keyword": "연말정산",
  "description": "2025년 연말정산 시즌",
  "approx_traffic": "500K+",
  "category": "경제",
  "tags": ["세금", "환급"],
  "content": "AI가 생성한 상세 콘텐츠...",
  "createdAt": "2025-01-15T10:30:00",
  "references": [
    {
      "id": 1,
      "url": "https://example.com/article1",
      "title": "연말정산 가이드"
    }
  ]
}
```

### Test API (개발/테스트용)

#### Python LLM 서버 통신 테스트
```http
POST /api/test/llm
Content-Type: application/json

{
  "keyword": "테스트 키워드"
}
```

#### 헬스 체크
```http
GET /api/test/health
```

### Swagger UI
애플리케이션 실행 후 다음 URL에서 API 문서를 확인할 수 있습니다:
```
http://localhost:8080/swagger-ui/index.html
```

## 📁 프로젝트 구조

```
src/main/java/com/team8/tai_backend/
├── TaiBackendApplication.java       # Spring Boot 메인 클래스
├── _core/
│   └── config/
│       ├── SwaggerConfig.java       # Swagger/OpenAPI 설정
│       └── WebClientConfig.java     # WebClient 설정
├── controller/
│   ├── TrendController.java         # 트렌드 API
│   ├── NewTrendController.java      # 새 트렌드 처리
│   ├── PythonApiController.java     # Python 서버 테스트
│   └── RestClientController.java    # RestClient 테스트
├── service/
│   ├── TrendService.java            # 트렌드 비즈니스 로직
│   ├── NewTrendService.java         # 새 트렌드 서비스
│   ├── GoogleTrendsRssService.java  # Google Trends RSS 파싱
│   ├── WebClientService.java        # Python 서버 통신 (WebFlux)
│   └── RestClientService.java       # Python 서버 통신 (RestClient)
├── dto/
│   ├── request/
│   │   ├── LLMRequest.java          # LLM 요청/응답 DTO
│   │   └── TrendRssRequest.java     # 트렌드 RSS 요청 DTO
│   └── response/
│       ├── TrendResponse.java       # 트렌드 목록 응답
│       ├── TrendDetailResponse.java # 트렌드 상세 응답
│       └── TrendRssResponse.java    # RSS 파싱 응답
├── entity/
│   └── Trend.java                   # 트렌드 엔티티
└── repository/
    └── TrendRepository.java         # Trend JPA Repository
```

## 🚀 설치 및 실행

### 사전 요구사항
- Java 21 이상
- PostgreSQL 데이터베이스
- (선택) [Python LLM 서버](https://github.com/Team8-ToyProject4/tai_python_backend) (AI 분석 기능 사용 시)

### 1. 환경 변수 설정

프로젝트 루트에 `.env` 파일을 생성하고 다음 내용을 입력합니다:

```properties
SPRING_DATASOURCE_URL=jdbc:postgresql://localhost:5432/tai_db
SPRING_DATASOURCE_USERNAME=your_username
SPRING_DATASOURCE_PASSWORD=your_password
```

### 2. 빌드

```bash
./gradlew build
```

### 3. 실행

```bash
./gradlew bootRun
```

또는 빌드된 JAR 파일 실행:

```bash
java -jar build/libs/tai_backend-0.0.1-SNAPSHOT.jar
```

### 4. 애플리케이션 접속

- API 서버: http://localhost:8080
- Swagger UI: http://localhost:8080/swagger-ui/index.html

## 🐳 Docker로 실행

프로젝트는 Docker와 Docker Compose를 지원합니다. 컨테이너 환경에서 PostgreSQL과 Spring Boot 애플리케이션을 쉽게 실행할 수 있습니다.

### 사전 요구사항
- Docker 및 Docker Compose 설치

### Docker Compose로 실행 (권장)

전체 스택(PostgreSQL + Spring Boot)을 한 번에 실행할 수 있습니다.

#### 1. 환경 변수 설정

프로젝트 루트에 `.env` 파일을 생성합니다:

```properties
# PostgreSQL 설정
POSTGRES_DB=tai_db
POSTGRES_USER=postgres
POSTGRES_PASSWORD=your_secure_password

# Spring Boot 데이터소스 설정
SPRING_DATASOURCE_URL=jdbc:postgresql://postgres:5432/tai_db
SPRING_DATASOURCE_USERNAME=postgres
SPRING_DATASOURCE_PASSWORD=your_secure_password
```

#### 2. 컨테이너 실행

```bash
# 전체 서비스 시작 (백그라운드)
docker-compose up -d

# 로그 확인
docker-compose logs -f

# 특정 서비스 로그만 확인
docker-compose logs -f tai-backend
```

#### 3. 애플리케이션 접속

- API 서버: http://localhost:8080
- Swagger UI: http://localhost:8080/swagger-ui/index.html
- PostgreSQL: localhost:5432

#### 4. 중지 및 정리

```bash
# 서비스 중지
docker-compose stop

# 서비스 중지 및 컨테이너 삭제
docker-compose down

# 볼륨까지 모두 삭제
docker-compose down -v
```

### 개별 Docker 명령어로 실행

Docker Compose 대신 개별 명령어로도 실행할 수 있습니다.

#### 1. PostgreSQL 컨테이너 실행

```bash
docker run -d \
  --name postgres-db \
  --network app-network \
  -e POSTGRES_DB=tai_db \
  -e POSTGRES_USER=postgres \
  -e POSTGRES_PASSWORD=password \
  -p 5432:5432 \
  postgres:15-alpine
```

#### 2. Spring Boot 이미지 빌드

```bash
docker build -t tai-backend .
```

#### 3. Spring Boot 컨테이너 실행

```bash
docker run -d \
  --name tai-backend \
  --network app-network \
  -e SPRING_DATASOURCE_URL=jdbc:postgresql://postgres-db:5432/tai_db \
  -e SPRING_DATASOURCE_USERNAME=postgres \
  -e SPRING_DATASOURCE_PASSWORD=password \
  -p 8080:8080 \
  tai-backend
```

### 유용한 Docker 명령어

```bash
# 컨테이너 상태 확인
docker-compose ps
docker ps

# Spring Boot 애플리케이션 로그 실시간 확인
docker logs -f tai-backend

# PostgreSQL 컨테이너 접속
docker exec -it postgres-db psql -U postgres -d tai_db

# 특정 서비스만 재시작
docker-compose restart tai-backend

# 이미지 재빌드 후 실행
docker-compose up -d --build
```

### 네트워크 구성

Docker Compose를 사용하면 `app-network`라는 사용자 정의 네트워크가 생성됩니다:

- **postgres**: PostgreSQL 서비스 (호스트명: `postgres`)
- **tai-backend**: Spring Boot 애플리케이션 (호스트명: `tai-backend`)
- **Python LLM 서버**: [tai_python_backend](https://github.com/Team8-ToyProject4/tai_python_backend)를 같은 네트워크에 추가하거나 외부 서비스로 연동

Spring Boot는 `jdbc:postgresql://postgres:5432/tai_db`로 PostgreSQL에 접근합니다.

> **참고**: Python LLM 서버를 Docker Compose에 추가하려면 `compose.yaml`에 서비스를 정의하세요. 자세한 내용은 [Python 백엔드 리포지토리](https://github.com/Team8-ToyProject4/tai_python_backend)를 참고하세요.

### 문제 해결

#### 포트 충돌
로컬에 이미 PostgreSQL이 실행 중이면 포트 충돌이 발생할 수 있습니다.
`compose.yaml`에서 포트를 변경하세요:

```yaml
ports:
  - "5433:5432"  # 호스트 포트 변경
```

#### 컨테이너 연결 실패
Spring Boot가 PostgreSQL에 연결하지 못하면:

```bash
# PostgreSQL 헬스 체크 확인
docker-compose ps

# PostgreSQL 로그 확인
docker-compose logs postgres

# 네트워크 확인
docker network ls
docker network inspect tai_backend_app-network
```

## 🚀 프로덕션 배포 (CI/CD)

### Self-hosted Runner + Docker

SSH 접속이 불가능한 환경에서 GitHub Actions를 통해 자동 배포합니다.

#### 배포 방식
- **Self-hosted Runner**: 서버가 GitHub에 접속하여 작업을 가져옴 (아웃바운드 통신만 필요)
- **Docker**: 환경 격리 및 빠른 배포/롤백
- **로컬 빌드**: GHCR 없이 서버에서 직접 빌드하여 네트워크 오버헤드 최소화

#### 배포 흐름

```
1. 개발자: git push origin main
   ↓
2. GitHub: 워크플로우 트리거
   ↓
3. Runner (서버): GitHub에서 작업 감지 (polling)
   ↓
4. Runner: Docker 이미지 빌드 (백업 생성)
   ↓
5. Runner: 기존 컨테이너 중지
   ↓
6. Runner: PostgreSQL 시작 및 헬스체크
   ↓
7. Runner: 애플리케이션 컨테이너 시작
   ↓
8. Runner: 실패 시 자동 롤백
   ↓
9. GitHub: 결과 표시 (성공/실패)
```

#### Self-hosted Runner 설치

Self-hosted Runner 설치 및 설정 방법은 [SELFHOSTED_RUNNER_GUIDE.md](SELFHOSTED_RUNNER_GUIDE.md)를 참고하세요.

**주요 내용:**
- GitHub Runner 설치 및 등록
- SystemD 서비스 설정 (`tai-backend.service`)
- 워크플로우 설정 (`.github/workflows/deploy-selfhosted-docker.yml`)
- 문제 해결 및 모니터링

#### SystemD 서비스 (Docker 없이 실행)

Docker 없이 직접 JAR를 실행하려면 `tai-backend.service` 파일을 사용할 수 있습니다:

```bash
# 서비스 파일 복사
sudo cp tai-backend.service /etc/systemd/system/

# 서비스 활성화 및 시작
sudo systemctl daemon-reload
sudo systemctl enable tai-backend
sudo systemctl start tai-backend

# 상태 확인
sudo systemctl status tai-backend
```

## 🗄 데이터베이스 스키마

### Trend 엔티티

| 필드 | 타입 | 설명 |
|------|------|------|
| id | Long | 기본 키 (자동 증가) |
| region | String | 지역 코드 (예: KR) |
| rank | Integer | 트렌드 순위 |
| keyword | String | 트렌드 키워드 |
| description | String | 한줄 설명 |
| ai_description | String | AI가 생성한 요약 |
| category | String | 카테고리 |
| tags | String | 태그 목록 (쉼표 구분) |
| approx_traffic | String | 대략적 검색량 |
| content | String | 상세 콘텐츠 |
| createdAt | LocalDateTime | 생성 시간 |

### Reference 엔티티

| 필드 | 타입 | 설명 |
|------|------|------|
| id | Long | 기본 키 (자동 증가) |
| url | String | 참조 URL |
| title | String | 참조 제목 |
| trend_id | Long | Trend 외래 키 (N:1) |

## 🔗 Python 서버 연동

이 프로젝트는 Python LLM 백엔드 서버와 연동하여 AI 분석 기능을 제공합니다.

**Python 백엔드 리포지토리**: [Team8-ToyProject4/tai_python_backend](https://github.com/Team8-ToyProject4/tai_python_backend)

### WebClient 설정

`WebClientConfig`에서 Python 서버 기본 URL을 설정합니다:

```java
@Bean
public WebClient webClient() {
    return WebClient.builder()
            .baseUrl("http://localhost:5000/api")  // Python 서버 주소
            .build();
}
```

### 통신 흐름

```
Java Backend (Spring Boot)
    ↓ POST /request
    ↓ { "keyword": "트렌드 키워드" }
Python LLM Server
    ↓ AI 분석 처리
    ↓ { "keyword": "...", "summary": "...", "tags": [...] }
Java Backend (Spring Boot)
    ↓ 응답 수신 (비동기)
```

### Python 서버 설치 및 실행

Python LLM 서버를 함께 실행해야 AI 분석 기능을 사용할 수 있습니다.

```bash
# 1. Python 백엔드 클론
git clone https://github.com/Team8-ToyProject4/tai_python_backend.git
cd tai_python_backend

# 2. Python 서버 설치 및 실행 (5000 포트)
# 자세한 내용은 Python 리포지토리의 README 참고
```

### 테스트 방법

```bash
# 1. Java 백엔드 서버 실행 (8080 포트)
./gradlew bootRun

# 2. Python LLM 서버 실행 (5000 포트, 별도 터미널)
cd tai_python_backend
# Python 서버 실행 명령어 (README 참고)

# 3. 테스트 API 호출
curl -X POST http://localhost:8080/api/test/llm \
  -H "Content-Type: application/json" \
  -d '{"keyword": "연말정산"}'
```

### 관련 프로젝트

- **Python LLM 백엔드**: [Team8-ToyProject4/tai_python_backend](https://github.com/Team8-ToyProject4/tai_python_backend)

### 관련 문서

- **Self-hosted Runner 설치 가이드**: [SELFHOSTED_RUNNER_GUIDE.md](SELFHOSTED_RUNNER_GUIDE.md)
- **GitHub Actions 워크플로우**: [.github/workflows/deploy-selfhosted-docker.yml](.github/workflows/deploy-selfhosted-docker.yml)
- **SystemD 서비스 파일**: [tai-backend.service](tai-backend.service)

## 📝 라이선스

