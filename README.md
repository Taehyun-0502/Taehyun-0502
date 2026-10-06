# 송태현 | Backend Developer

Java·Spring을 중심으로 **인증, 데이터 정합성, 실시간 통신, 배포**를 구현해 온 백엔드 개발자입니다.

기능 구현에서 멈추지 않고 다음을 고민하며 개발합니다.

- 실패했을 때 데이터가 어떤 상태로 남는가
- 인증과 권한은 어느 계층에서 검증해야 하는가
- 트랜잭션 경계를 어디에 두고, 데이터는 언제 확정된 것으로 볼 것인가

📧 uiop7000@naver.com

---

## Tech Stack

| 분야 | 기술 |
|---|---|
| Backend | Java 21, Spring Boot 3.5, Spring Security, Spring Data JPA, MyBatis |
| Database | PostgreSQL (Supabase) |
| Realtime | WebSocket / STOMP, SSE |
| Infra | AWS EC2, nginx, GitHub Actions, systemd |
| Frontend | React, Vite, JSP / JSTL |

---

## Projects

### 🐶 댕댕댕 — 반려동물 통합 케어 플랫폼

`2026.08 ~ 2026.09` · 4인 팀  
**담당: 회원·인증, 반려동물, 오픈채팅, 공통 백엔드 규약, 배포**

[Repository](https://github.com/Taehyun-0502/pet_project)

- **Refresh Token 재사용 감지**
  - 재사용 감지 후 예외가 발생하면서 토큰 폐기까지 함께 Rollback되는 문제
  - 폐기 로직을 별도 Bean의 `REQUIRES_NEW` 트랜잭션으로 분리해 해결
- **채팅 메시지 정합성**
  - DB Commit 이전에 메시지가 방송될 수 있는 문제
  - `AFTER_COMMIT` 이벤트를 사용해 저장이 확정된 메시지만 STOMP로 전달
- REST 송신 + STOMP 수신 기반 실시간 오픈채팅
- JPA `@Version` 기반 낙관적 잠금과 사용자별 반려동물 데이터 격리
- AWS EC2 · nginx · GitHub Actions 기반 배포 및 **배포 실패 시 이전 버전 자동 복구**

---

### 🏋️ Haru Health — 헬스장 운영 관리 서비스

`2026.06 ~ 2026.07` · 4인 팀  
**담당: 결제·매출, 정산·물품, 출석·PT, SSE 알림**

[Repository](https://github.com/Taehyun-0502/health_Project)

- **결제 정합성**
  - 현장 결제(외부 PG 미연동) 확정 과정에서 결제·매출 저장과 계약 상태 변경 중 일부만 성공할 수 있는 문제
  - 결제 흐름과 계약 서비스 호출을 하나의 트랜잭션 경계로 구성해 부분 성공 방지
- **정산 동시성**
  - 동일 지점·월의 정산이 동시에 생성될 수 있는 문제
  - PostgreSQL `pg_advisory_xact_lock`으로 지점별 정산 생성 직렬화
- PT 사용량을 계약 원본이 아닌 별도 원장에서 관리하고 조건부 `UPDATE`로 초과 차감 방지
- SSE 기반 실시간 알림과 알림 이력 관리

---

### 🍳 Cooking Star — 레시피·요리 커뮤니티

`2026.05.08 ~ 2026.05.26` · 2인 팀  
**담당: 회원·인증, 댓글·좋아요·북마크·팔로우, Kakao/Naver 연동, 관리자·방문 집계**

[Repository](https://github.com/Taehyun-0502/cooking_star)

- Spring Security Form Login 기반 세션 인증·인가
- `Principal`과 SQL 소유자 조건을 이용한 사용자 데이터 접근 제어
- 댓글 · 좋아요 · 북마크 · 팔로우
- PostgreSQL `ON CONFLICT`를 활용한 방문 기록 중복 방지
- Kakao Local · Naver Search API 연동
