# 웹 보안 정리

## 웹 보안 기초

### OWASP Top 10 (2021 기준)
1. **Broken Access Control** - 권한 우회가 가능한 설계나 구현
2. **Cryptographic Failures** - 암호화 미흡으로 인한 민감 정보 노출
3. **Injection** - SQL, OS 명령 등 사용자 입력으로 인한 주입
4. **Insecure Design** - 보안을 고려하지 않은 시스템 설계
5. **Security Misconfiguration** - 잘못된 서버/프레임워크 설정
6. **Vulnerable and Outdated Components** - 보안 패치되지 않은 컴포넌트 사용
7. **Identification and Authentication Failures** - 인증 절차의 허점
8. **Software and Data Integrity Failures** - 무결성 검증 미흡
9. **Security Logging and Monitoring Failures** - 이상 탐지를 위한 로깅 부재
10. **Server-Side Request Forgery (SSRF)** - 서버 내부 요청 위조

---

### SQL Injection
- **정의**: 사용자 입력을 통해 SQL 쿼리를 조작하는 공격  
- **대응**: `PreparedStatement`, ORM 사용, 입력값 검증

---

### XSS (Cross-Site Scripting)
- **정의**: 악성 스크립트를 사용자 브라우저에 삽입하는 공격  
- **유형**: 저장형(Stored), 반사형(Reflected), DOM 기반(DOM-based)  
- **대응**: HTML 이스케이프, CSP(Content Security Policy)

---

### CSRF (Cross-Site Request Forgery)
- **정의**: 인증된 사용자의 권한으로 악의적 요청을 보내는 공격  
- **대응**: CSRF Token 적용, `SameSite` 쿠키 설정, Referer 검증

---

### 입력 검증 (Input Validation)
- **목적**: 악의적인 입력을 통한 공격 방지  
- **방법**: 화이트리스트 기반 검증, 클라이언트/서버 모두에서 처리, 길이/형식 제한

---

### 보안 헤더 (Security Headers)
- `Content-Security-Policy`: XSS 방지, 리소스 로딩 제어
- `X-Content-Type-Options`: MIME 타입 스니핑 방지
- `X-Frame-Options`: 클릭재킹 방지
- `Strict-Transport-Security`: HTTPS 강제
- `Referrer-Policy`: Referer 헤더 제어

---

### 쿠키 속성 (Cookie Attributes)
- `HttpOnly`: JS 접근 불가, XSS 방어
- `Secure`: HTTPS에서만 전송
- `SameSite`: 교차 사이트 요청 제한 (`Strict`, `Lax`, `None`)

---

## 시스템 및 실무 보안

### 권한 상승 (Privilege Escalation)
- **설명**: 일반 사용자가 루트/관리자 권한을 얻는 현상  
- **대응**: 최소 권한 원칙, SUID 점검, 불필요한 권한 제거

---

### 파일 권한
- `chmod`, `chown` 등으로 권한 관리  
- 업로드 파일의 실행 제한, 웹 루트 내 실행 방지

---

### JWT 보안
- **주의점**:
  - `alg: none` 취약점
  - 토큰 탈취 시 무효화 어려움  
- **대응**:
  - 서명 알고리즘 검증
  - 짧은 만료 시간
  - 리프레시 토큰 분리

---

### 세션 탈취 (Session Hijacking)
- **기법**: 세션 ID 가로채기 (XSS, 스니핑 등)  
- **대응**: `HttpOnly` 설정, 세션 고정 방지, IP/User-Agent 바인딩

---

### 2FA (Two-Factor Authentication)
- **설명**: 두 가지 인증 요소 사용 (예: 비밀번호 + OTP)  
- **방식**: 문자, 이메일, OTP 앱, 하드웨어 키 등

---

### 보안 면접 질문 예시
- **Q: SQL Injection 방어법?**  
  A: PreparedStatement, 입력 검증 사용

- **Q: XSS 방어법?**  
  A: HTML 이스케이프, CSP, 사용자 입력 필터링

- **Q: CSRF 방어법?**  
  A: CSRF 토큰, SameSite 쿠키

- **Q: JWT와 세션의 차이?**  
  A: JWT는 무상태 방식, 세션은 서버 상태 유지 방식 → 각 방식의 장단점 설명

---

### 보안 설계 시 고려사항
- 인증과 인가 분리
- 저장/전송 시 민감 정보 암호화
- 에러 메시지에 시스템 정보 노출 금지
- 모든 외부 입력 검증
- 보안 로그 및 이상 징후 모니터링
- 운영환경 배포 시 디버그 정보 제거
