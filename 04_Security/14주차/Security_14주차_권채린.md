# 웹 보안 + 시스템 및 실무 보안

Created: 2025년 6월 30일 오후 2:59
스터디 날짜: 2025/07/02
Status: In progress
과목: 보안

목차

## 웹 보안

### OWASP Top 10

웹 애플리케이션 보안의 가장 흔하고 치명적인 10가지 취약점을 정리한 목록

- (Open Web Application Security Project)는 웹 보안 향상을 위한 국제 비영리 단체
- 아래 목록은 개발자와 보안 담당자가 알아야 하는 핵심 보안 위협 리스트

| 순번 | 항목 | 설명 |
| --- | --- | --- |
| **A01:2021** | **Broken Access Control (취약한 접근 제어)** | 인증은 되었지만 사용자가 자신이 접근해서는 안 되는 리소스(예: 다른 사람의 계정 정보)에 접근 가능한 상태. |
| **A02:2021** | **Cryptographic Failures (암호화 실패)** | 민감 정보가 암호화되지 않거나, 안전하지 않은 암호 알고리즘을 사용하는 경우. 이전 명칭은 **민감 데이터 노출**. |
| **A03:2021** | **Injection (인젝션)** | SQL, NoSQL, OS 명령어 등 외부 입력이 코드로 실행되는 취약점. 대표적으로 **SQL Injection**이 있음. |
| **A04:2021** | **Insecure Design (보안이 고려되지 않은 설계)** | 구조적으로 보안이 취약한 설계. 예: 인증 기능이 없는 API 설계. |
| **A05:2021** | **Security Misconfiguration (보안 설정 오류)** | 디폴트 설정 그대로 사용하거나, 불필요한 기능이 열려 있는 경우. |
| **A06:2021** | **Vulnerable and Outdated Components (취약하거나 오래된 컴포넌트 사용)** | 오래된 라이브러리, 프레임워크 등을 사용하는 경우. 예: Log4Shell 이슈. |
| **A07:2021** | **Identification and Authentication Failures (인증 관련 취약점)** | 비밀번호 정책 미흡, 계정 잠금 기능 없음, 인증 우회 가능 등. |
| **A08:2021** | **Software and Data Integrity Failures (무결성 위반)** | 코드 또는 데이터가 악의적으로 변조되어도 검증되지 않음. 예: 업데이트 파일 무결성 확인 누락. |
| **A09:2021** | **Security Logging and Monitoring Failures (로깅 및 모니터링 부족)** | 보안 사고 발생 시 탐지, 분석, 대응이 불가능하거나 지연됨. |
| **A10:2021** | **Server-Side Request Forgery (SSRF)** | 서버가 악의적 요청을 외부로 보내도록 속이는 공격. 예: 내부망 접속 유도. |

<br>

### 인풋 검증

== 입력값 검증

- 사용자나 클라이언트로부터 전달되는 모든 입력에 대해 형식, 범위, 허용된 값 등을 검증하는 작업

입력값 검증이 없으면 다음과 같은 공격 발생

- SQL Injection (`' OR 1=1 --`)
- XSS (`<script>alert('xss')</script>`)
- Command Injection (`; rm -rf /`)
- Directory Traversal (`../../etc/passwd`)

<br>

✅ **검증 방식**

| 방식 | 설명 | 예시 |
| --- | --- | --- |
| **화이트리스트 기반** | 허용된 값만 통과시키는 방식 | ID는 숫자만 → `^[0-9]+$` |
| **길이 제한** | 너무 긴 입력 차단 | 사용자 이름 1~20자 제한 |
| **타입 체크** | 숫자, 이메일, 날짜 등 타입 검사 | Java에서 `@Valid`, `@NotNull` 등 사용 |
| **범위 검증** | 값이 특정 범위를 넘지 않도록 | 나이: 0~120 사이만 허용 |
| **정규 표현식** | 포맷이 정확한지 검증 | 이메일 형식, 전화번호 등 |

<br>

### SQL Injection

- 사용자의 입력값이 적절히 필터링되지 않고 SQL 쿼리에 포함되어, 악의적인 SQL이 실행되는 보안 취약점

---

### ✅ 예시

```sql
-- 로그인 로직
SELECT * FROM users WHERE username = '입력값' AND password = '입력값';
```

사용자가 다음과 같이 입력한다면?

- `username`에: `admin' --`
- `password`에: 아무거나

실제로 실행되는 SQL은:

```sql
SELECT * FROM users WHERE username = 'admin' --' AND password = '아무거나';
```

- `-`는 주석이므로 `AND password = ...`는 무시됨 → 인증 우회 가능 😱

---

**SQL 인젝션의 유형**

1. 대역 내 SQL 인젝션: **공격과 결과 수집이 같은 통로(대역)** 를 통해 이루어짐
    1. HTTP 요청과 같은 동일한 매체를 사용하여 공격을 수행하고 결과 수집
    2. 가장 빠르고 공격자 입장에서 편리
    3. 웹서버가 DB 오류나 쿼리 결과를 직접 응답에 보여줄 때 가능
2. 블라인드 SQL인젝션 : 웹 애플리케이션이 DB 에러나 결과 값을 응답에 직접 표시하지 않는 경우 사용되는 기법.
    1. 공격자가 참/거짓 질문을 반복해서 응답의 변화를 관찰하여 정보를 알아냄
3. 대역 외 SQL인젝션 : 응답을 직접 받지 못하고, 다른 채널을 통해 공격 결과를 수집하는 방식
    1. 공격자가 조작한 SQL 쿼리를 통해 서버가 외부의 공격자 서버로 요청을 보내도록 유도

<br>

### XSS(Cross-Site Scripting), CSRF(Cross-Site Request Forgery)

**XSS(Cross-Site Scripting)**

- 공격자가 웹 페이지에 악성 스크립트를 삽입하여, 사용자의 브라우저에서 실행되도록 하는 공격
- 주로 쿠키 탈취, 세션 탈취, 피싱 등이 목적
- 방지 방법 : 입력값 필터링, 출력 시 이스케이프

<br>

**CSRF(Cross-Site Request Forgery)**

- 사용자가 인증된 상태라는 점을 악용하여, 사용자의 의도와는 무관하게 서버에 요청을 보내게 만드는 공격
- 사용자의 세션/쿠키를 이용해 서버에 몰래 요청하는 방식
- 방지 방법 : CSRF 토큰 사용, REFER 검증

<br>

### 보안 헤더

- HTTP 응답 헤더에 설정하여 브라우저의 동작을 제어하고, 보안 관련 공격을 방지하는 기술

✅ **주요 보안 헤더 목록**

| 헤더 | 설명 | 예시 |
| --- | --- | --- |
| **Content-Security-Policy (CSP)** | 허용된 스크립트/리소스 출처만 로딩 | `default-src 'self'; script-src 'self';` |
| **X-Content-Type-Options** | 콘텐츠 타입 추측 방지 | `X-Content-Type-Options: nosniff` |
| **X-Frame-Options** | 클릭재킹 방지 (iframe 내 표시 제한) | `X-Frame-Options: DENY` |
| **Strict-Transport-Security (HSTS)** | HTTPS만 사용하도록 강제 | `Strict-Transport-Security: max-age=31536000; includeSubDomains` |
| **Referrer-Policy** | 리퍼러 정보 제한 | `Referrer-Policy: no-referrer` |
| **Permissions-Policy** *(구. Feature-Policy)* | 카메라, 마이크 등 사용 제한 | `Permissions-Policy: camera=(), microphone=()` |

<br>

### 쿠키 속성 (HttpOnly, Secure, SameSite)

| 속성 | 설명 | 보안 목적 |
| --- | --- | --- |
| **HttpOnly** | JavaScript에서 접근 불가 | **XSS 방어** |
| **Secure** | HTTPS 연결에서만 전송 | **중간자 공격 방지** |
| **SameSite** | 다른 사이트에서의 요청 시 쿠키 전송 제한 | **CSRF 방지** |

<br>

## 시스템 및 실무 보안

<br>

### 권한 상승

- 시스템의 취약점을 악용해 **원래 권한보다 더 높은 권한을 얻는 공격**
    
    (예: 일반 사용자 → 관리자(root) 권한)
    

| 유형 | 설명 |
| --- | --- |
| **수직 권한 상승** | 낮은 권한에서 높은 권한으로 (ex. 일반 사용자 → 관리자) |
| **수평 권한 상승** | 같은 권한이지만 **다른 사용자의 정보나 권한을 침해** (ex. A 사용자가 B의 정보에 접근) |

<br>

### 파일 권한

- 서버나 시스템의 **파일/디렉토리에 대한 읽기, 쓰기, 실행 권한 설정**

![image](https://github.com/user-attachments/assets/d4fcd4a7-62f6-4a08-ac87-0a7b6e78b7a1)


### JWT 보안

- 사용자의 인증 정보를 **토큰**으로 만들어 클라이언트에 저장하고, 이후 요청마다 서버가 이를 **검증하여 인증 처리**하는 방식

✅ **구성**

- Header: 알고리즘 정보
- Payload: 사용자 정보 (ex. userId, role 등)
- Signature: 위조 방지용 서명 (secret key로 HMAC-SHA256 등)

<br>

### 세션 탈취

- **사용자의 세션 식별자(Session ID)를 공격자가 탈취**하여, 피해자의 권한으로 웹 애플리케이션에 접근하는 공격

**세션 탈취 방식**

| 방법 | 설명 |
| --- | --- |
| **XSS (크로스사이트 스크립팅)** | 악성 스크립트로 `document.cookie`를 통해 세션 ID를 탈취 |
| **HTTP 스니핑 (도청)** | HTTPS가 아닌 HTTP 통신 시 중간자가 네트워크 패킷을 가로채서 세션 ID 탈취 |
| **세션 고정(Session Fixation)** | 공격자가 미리 발급받은 세션 ID를 피해자에게 사용하게 만든 후, 같은 세션을 사용하는 방식 |
| **Referrer / URL 노출** | 세션 ID가 URL 파라미터로 전달되면 외부 사이트로 유출 가능 |
| **브루트 포스** | 세션 ID가 너무 단순하거나 예측 가능한 경우, 무작위 대입 시도 |

<br>

### 2FA(2단계 인증, Two-Factor Authentication)

- 요즘 거의 모든 보안 민감 서비스에서 사용하는 인증 강화 방식
- 비밀번호 + **추가 수단(OTP, 문자, 생체인증 등)**

2FA의 핵심 : 인증 요소 3가지 중 2개 이상을 결합

| 인증 요소 종류 | 예시 |
| --- | --- |
| **1. 지식 기반 (Something you know)** | 비밀번호, PIN 번호 |
| **2. 소유 기반 (Something you have)** | OTP 기기, 스마트폰, 보안 토큰 |
| **3. 존재 기반 (Something you are)** | 지문, 얼굴, 홍채 인식 등 생체정보 |
