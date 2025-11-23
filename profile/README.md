# 🧩 MoA – 소셜 커뮤니티 플랫폼

실시간으로 소통할 수 있는 소셜 커뮤니티 플랫폼입니다.  
회원가입/로그인, 소셜 로그인, 이메일 인증, 실시간 알림 및 채팅 등  
실서비스 수준의 기능들을 구현한 프로젝트입니다.

## 🚀 주요 기능

- **로그인/회원가입 (JWT 기반)**
- **OAuth2 소셜 로그인 (Google·Kakao)**
- **JavaMailSender 이메일 인증**
- **실시간 알림 (댓글/좋아요)**
- **실시간 채팅 (WebSocket/STOMP)**
- **AccessToken 자동 재발급**
- **회원 정보 관리 / 비밀번호 변경**

## 🛠 기술 스택

### Backend
- Java 21  
- Spring Boot 3.5.x  
- Spring Security · JWT · OAuth2  
- JavaMailSender  
- WebSocket & STOMP  
- MySQL 8.0  
- Docker  
- Gradle (Kotlin DSL)

### Frontend
- React 18  
- TypeScript  
- Vite  
- Zustand  
- Axios  
- SockJS + Stomp.js  
- React Router

## 🏗 아키텍처

### 📌 Backend 구조

```
src/main/java/com/moa
 ┣ global/            # Security, CORS, WebSocket, 예외처리
 ┣ auth/              # JWT, OAuth2
 ┣ user/              # 회원가입/로그인/이메일 인증
 ┣ post/              # 게시글
 ┣ comment/           # 댓글
 ┣ like/              # 좋아요
 ┣ chat/              # WebSocket 기반 채팅
 ┗ notification/      # 실시간 알림
```

### 📌 Frontend 구조

```
src
 ┣ pages/
 ┣ components/
 ┣ services/          # Axios API
 ┣ store/             # Zustand
 ┣ websocket/         # SockJS/STOMP client
 ┗ utils/
```

## 🔐 인증 구조

- **AccessToken + RefreshToken** 이중 구조  
- RefreshToken은 HttpOnly Cookie로 저장  
- AccessToken 만료 5분 전 자동 갱신  
- OAuth2 로그인 → JWT로 통합  

### 이메일 인증

- 인증코드 6자리 생성  
- JavaMailSender(SMTP)로 이메일 발송  
- DB에 인증코드 + 만료시간 저장(expireAt)  
- 만료 시간 검증 후 최종 회원가입 완료  

## 🔔 실시간 알림 & 채팅

### WebSocket/STOMP 구조

```
/ws                      # WebSocket endpoint
/topic/notification/{id} # 실시간 알림
/topic/chat/{roomId}     # 실시간 채팅
```

- Handshake 단계에서 JWT 인증  
- 댓글/좋아요/채팅 메시지 푸시  
- unreadCount, isRead 상태 관리  

## 🧪 Trouble Shooting

| 문제 | 해결 |
|------|------|
| OAuth 성공 후 SecurityContext 비어있음 | OAuthSuccessHandler에서 JWT 발급 시점 분리 |
| WebSocket 연결 시 인증 실패 | HandshakeInterceptor에서 JWT 직접 검증 |
| 알림 broadcast 문제 | userId 기반 topic 분리 (`/topic/notification/{id}`) |
| 이메일 인증 지연 | SMTP 보안 포트/TLS 설정 수정 |
| AccessToken 재발급 오류 | RefreshToken 검증 로직 분리 및 개선 |

## ▶️ 실행 방법

### Backend

```bash
docker-compose up -d
./gradlew bootRun
```

### Frontend

```bash
npm install
npm run dev
```
