# 웹 보안 & 시스템 보안 정리

## 1. OWASP Top 10 (2023 기준)

OWASP(Open Web Application Security Project)에서 선정한 주요 웹 보안 위협

| 순위 | 항목                                       | 설명                                          |
| ---- | ------------------------------------------ | --------------------------------------------- |
| A01  | Broken Access Control                      | 인증되지 않은 접근/허가되지 않은 기능 노출    |
| A02  | Cryptographic Failures                     | 암호화 미흡 또는 민감 정보 노출               |
| A03  | Injection                                  | SQL, NoSQL, OS 명령어 삽입 공격               |
| A04  | Insecure Design                            | 보안 고려가 부족한 시스템 설계                |
| A05  | Security Misconfiguration                  | 기본 설정, 잘못된 권한, 불필요한 포트 등      |
| A06  | Vulnerable and Outdated Components         | 사용 중인 컴포넌트의 취약점                   |
| A07  | Identification and Authentication Failures | 인증 로직 실패, 세션 관리 취약점 등           |
| A08  | Software and Data Integrity Failures       | 무결성 검증 없는 소프트웨어 배포              |
| A09  | Security Logging and Monitoring Failures   | 로그 미비, 이상 징후 탐지 실패                |
| A10  | Server-Side Request Forgery (SSRF)         | 서버가 공격자의 요청을 내부로 프록시하는 구조 |

## Injection (SQL Injection, Command Injection 등)

### SQL Injection

- 입력값에 쿼리 구문을 삽입하여 DB를 조작하는 공격
- 예시:
  ```sql
  SELECT * FROM users WHERE id = '1' OR '1'='1';
  ```

### 방어 방법

- Prepared Statement / ORM 사용
- 입력값 이스케이프 처리

## 3. XSS (Cross-Site Scripting)

### 정의

- 공격자가 악성 스크립트를 삽입하여 클라이언트에서 실행

### 유형

- Stored XSS (DB에 저장)
- Reflected XSS (URL 등 통해 즉시 반영)
- DOM-based XSS (JS 렌더링 중 발생)

### 방어 방법

- 출력 시 HTML 이스케이프
- Content-Security-Policy 헤더 설정
- React/Vue는 기본적으로 auto-escape 처리됨

## 4. CSRF (Cross-Site Request Forgery)

### 정의

- 사용자의 인증 정보를 도용하여 의도치 않은 요청을 서버에 전달

### 방어 방법

- SameSite=Strict / Lax 쿠키
- CSRF Token 사용
- Referer 검증

## 5. 입력값 검증 (Input Validation)

### 원칙

- 신뢰하지 않는 입력은 절대 사용하지 말 것

### 적용 전략

- 서버 측 유효성 검사 필수
- 화이트리스트 기반 필터링
- 문자열 길이 제한, 정규표현식 사용

## 6. 보안 관련 HTTP 헤더

| 헤더                      | 설명                              |
| ------------------------- | --------------------------------- |
| Content-Security-Policy   | 스크립트/리소스 로딩 제어         |
| X-Content-Type-Options    | 콘텐츠 타입 스니핑 방지 (nosniff) |
| X-Frame-Options           | 클릭재킹 방지 (DENY, SAMEORIGIN)  |
| Strict-Transport-Security | HTTPS 강제 (HSTS)                 |
| Referrer-Policy           | Referer 제한 설정                 |

## 7. 쿠키 속성

| 속성     | 설명                                                   |
| -------- | ------------------------------------------------------ |
| HttpOnly | JS 접근 차단 (XSS 방지)                                |
| Secure   | HTTPS에서만 전송                                       |
| SameSite | 외부 사이트 요청 시 쿠키 전송 제한 (Lax, Strict, None) |

## 8. 시스템/실무 보안

### 권한 상승 (Privilege Escalation)

- 일반 유저가 root 권한으로 상승하는 취약점
- setuid 프로그램, 파일 권한 설정 실수 등

### 파일 권한

- 불필요한 쓰기 권한 제거
- 민감 정보는 chmod 600 등 최소 권한 설정

## 9. JWT 보안

### 주의할 점

- 민감 정보는 payload에 넣지 않기
- 토큰 탈취 시 바로 무력화 어려움 → 짧은 유효기간 + Refresh Token 사용
- 비밀키 노출 주의

## 10. 세션 탈취 (Session Hijacking)

### 방식

- XSS를 통한 쿠키 탈취
- 세션 ID 예측
- 네트워크 스니핑 (HTTPS 미사용)

### 방지

- HTTPS 사용
- HttpOnly 쿠키
- 세션 재발급
- 로그인/로그아웃 시 세션 무효화

## 11. 2FA (Two-Factor Authentication)

- 비밀번호 + OTP / 기기 인증 등의 조합
- Google Authenticator, SMS, WebAuthn 등

## 12. 보안 설계 시 고려사항 정리

- 최소 권한 원칙 (Least Privilege)
- 보안 모듈화 (보안 로직을 서비스 로직과 분리)
- 모든 입력에 대한 서버 유효성 검사
- HTTPS 적용 (전 구간 암호화)
- 오류 메시지에 민감 정보 노출 금지
- 의심스러운 행위 로깅 및 모니터링
- 인증/인가 분리 설계
- 주기적인 패치 & 의존성 업데이트
