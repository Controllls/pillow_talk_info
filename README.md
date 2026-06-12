# PillowTalk

커플의 친밀감과 소통을 향상시키는 모바일 앱 백엔드 서버입니다.

# ScreenShot
<div>
<img src="https://github.com/Controllls/pillow_talk_info/assets/53941701/42dbfa20-18e0-4ddc-9164-1a61a7b57468" width="200" height="400"/>
<img src="https://github.com/Controllls/pillow_talk_info/assets/53941701/0774d518-6dd3-40d0-8a44-c652016dab4b" width="200" height="400"/>

</div>

## 기술 스택

| 분류 | 기술 |
|------|------|
| Framework | Spring Boot 2.7.7 |
| Language | Java 1.8 |
| Build | Gradle |
| Database | MySQL 8+ (AWS RDS) |
| ORM | Spring Data JPA / Hibernate |
| Security | Spring Security, JWT, OAuth2 |
| Push Notification | Firebase Cloud Messaging (FCM) |
| File Storage | AWS S3 |
| Container | Docker |

## 주요 기능

- **소셜 로그인**: Google, Kakao, Naver, Apple 소셜 로그인 지원
- **커플 매칭**: 고유 커플 코드를 이용한 파트너 연결
- **채팅**: 커플 전용 채팅방
- **질문/답변**: 커플 관계 개선을 위한 질문 시스템
- **챌린지**: 커플 함께 수행하는 챌린지 관리
- **감정 신호**: FCM을 통한 실시간 감정 전송
- **프로필 이미지**: AWS S3 기반 이미지 업로드

## 시작하기

### 사전 요구사항

- JDK 1.8+
- MySQL 8+
- Firebase 프로젝트 및 서비스 계정 키
- AWS S3 버킷 및 IAM 자격증명
- OAuth2 클라이언트 ID/Secret (Google, Kakao, Naver, Apple)

### 환경 설정

`src/main/resources/application.yml`에 아래 항목을 설정합니다.

```yaml
spring:
  datasource:
    url: jdbc:mysql://<DB_HOST>:3306/<DB_NAME>
    username: <DB_USER>
    password: <DB_PASSWORD>

  security:
    oauth2:
      client:
        registration:
          google:
            client-id: <GOOGLE_CLIENT_ID>
            client-secret: <GOOGLE_CLIENT_SECRET>
          kakao:
            client-id: <KAKAO_CLIENT_ID>
          naver:
            client-id: <NAVER_CLIENT_ID>
            client-secret: <NAVER_CLIENT_SECRET>

cloud:
  aws:
    credentials:
      access-key: <AWS_ACCESS_KEY>
      secret-key: <AWS_SECRET_KEY>
    s3:
      bucket: <S3_BUCKET_NAME>
    region:
      static: ap-northeast-2
```

### 빌드 및 실행

```bash
# 빌드 (테스트 제외)
./gradlew build -x test

# 실행
./gradlew bootRun

# 또는 JAR 실행
java -jar build/libs/pillowtalk-0.0.1-SNAPSHOT.jar
```

### Docker

```bash
# 이미지 빌드
docker build -t pillowtalk .

# 컨테이너 실행
docker run -p 8080:8080 pillowtalk
```

기본 포트: **8080**

## 프로젝트 구조

```
src/main/java/com/fgama/pillowtalk/
├── api/          # REST API 컨트롤러
├── auth/         # OAuth2 인증 핸들러
├── components/   # AWS S3 등 유틸리티
├── config/       # Spring 설정
├── domain/       # JPA 엔티티
├── dto/          # DTO 클래스
├── exceptions/   # 커스텀 예외
├── fcm/          # Firebase 푸시 알림
├── filter/       # 요청 필터
├── pageApi/      # 페이지별 API
├── repository/   # JPA 리포지토리
└── service/      # 비즈니스 로직
```

## API 개요

### 인증

| Method | Endpoint | 설명 |
|--------|----------|------|
| POST | `/api/member` | Google 회원가입 |
| POST | `/api/member/kakao` | Kakao 회원가입 |
| POST | `/api/member/naver` | Naver 회원가입 |
| POST | `/api/member/apple` | Apple 회원가입 |
| GET | `/api/v1/auto-login` | 자동 로그인 |
| POST | `/api/v1/renew-access-token` | 액세스 토큰 갱신 |

### 커플

| Method | Endpoint | 설명 |
|--------|----------|------|
| POST | `/api/couple/match` | 커플 매칭 |
| POST | `/api/couple/coupleCode` | 커플 코드 발급 |
| POST | `/api/couple/break` | 커플 해제 |
| GET | `/api/DDay/{accessToken}` | 연애 D-Day 조회 |

### 채팅 / 질문 / 챌린지

| Method | Endpoint | 설명 |
|--------|----------|------|
| POST | `/api/todayQuestion` | 오늘의 질문 조회 |
| POST | `/api/chattingRoom` | 채팅방에 답변 추가 |
| POST | `/api/v1/challenges` | 챌린지 목록 조회 |
| POST | `/api/v1/challenge` | 챌린지 생성 |

### 알림

| Method | Endpoint | 설명 |
|--------|----------|------|
| POST | `/api/fcm/emotion` | 감정 신호 전송 |
| POST | `/api/member/emotion` | 감정 상태 업데이트 |

## 인증 방식

모든 인증이 필요한 API는 요청 헤더에 Bearer 토큰을 포함해야 합니다.

```
Authorization: Bearer <ACCESS_TOKEN>
```
