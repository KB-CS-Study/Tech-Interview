# 보안의 기본 개념

## 기밀성·무결성·가용성(CIA)

### 1. **기밀성 (Confidentiality)**

정보의 소유자가 원하는 대로 정보의 비밀이 유지되어야 한다는 원칙

- 기밀성을 위협하는 공격 : sniffing, snooping, 도청 등
- 기밀성을 보존하기 위한 활동 : 식별, 인증, 권한 부여, 암호화, 모니터링 등

### 2. **무결성 (Integrity)**

비인가된 자에 의한 정보의 변경, 삭제, 생성 등으로부터 보호하여 정보의 정확성이 보장되어야 한다는 원칙

- 무결성을 위협하는 공격 : 변경, 가장, 재전송, 부인 등
- 무결성을 보존하기 위한 활동 : 접근제어, 메시지인증 등

### 3. **가용성 (Availability)**

정상적인 사용자에게 정보 서비스를 거부해서는 안된다는 원칙

- 가용성을 위협하는 공격 : Dos, DDos 등
- 가용성을 보존하기 위한 활동 : 백업, 침입 탐지 시스템 운용 등

✨ 최근의 보안 추세는 기밀성 < 무결성 < 가용성 순으로 관심을 받고 있음

## 인증(Authentication)과 인가(Authorization)

|  | 설명 | 예시 |
| --- | --- | --- |
| 인증 | 누구인지 확인하는 것 | 로그인 (ID/PW, OTP, 생체인증 등) |
| 인가 | 무엇을 할 수 있는지 확인하는 것 | 관리자만 접근 가능한 기능 제한 등 |

## 해시(Hash)

임의의 데이터를 길이가 정해진 값으로 변환 혹은 압축한 것

- 특징:
    - 입력이 같으면 출력도 같다
    - 일방향성: 출력값(해시값)만 보고는 원본 복구 불가능
- 사용처:
    - 정보의 무결성을 확인하기 위한 목적으로 사용
    ![Image](https://github.com/user-attachments/assets/0e6cbcd2-fedd-4b09-9f0e-1b91287c81f5)
    - 비밀번호 저장 (비교 시 원본 대신 해시 사용)
- 종류 : MD5, SHA-1 등
- 해시 함수의 보안 요구사항
    - 역상 저항성 (Pre-image Resistance) : 해시값 `H(x)`가 주어졌을 때, 원래 입력값 `x`를 찾기 어려워야 함
    - 제2 역상 저항성 (Second Pre-image Resistance) : 어떤 입력 `x`가 주어졌을 때, 같은 해시값을 가지는 다른 입력 `y`를 찾기 어려워야 함.
    - 충돌 저항성 (Collision Resistance) : `H(x) == H(y)`를 만족하는 (x, y) 쌍을 찾는 것이 불가능해야 함

## 대칭/비대칭 키 암호화

| 구분 | 대칭키 | 비대칭키  |
| --- | --- | --- |
| 키 종류 | 암호화/복호화에 **같은 키 사용** | 암호화는 **공개키**, 복호화는 **개인키** 사용 |
| 암•복호화 속도 | 빠름 | 느림 |
| 사용 예 | 대용량 데이터(파일, 데이터베이스) 암호화 | 키 분배, 전자서명 등 |
| 알고리즘 예시 | AES, DES, 3DES 등 | 디피-헬만 키 교환, RSA, ECC 등 |

## 전자서명

전자문서에 대해 **서명자의 신원**과 **문서의 무결성**을 보장하기 위해 사용하는 **디지털 기술**

- 핵심 역할
    
    
    | 기능 | 설명 |
    | --- | --- |
    | 인증(Authentication) | 누가 서명했는지 확인 (신원 증명) |
    | 무결성(Integrity) | 문서가 중간에 변경되지 않았는지 확인 |
    | 부인방지(Non-repudiation) | 서명자가 나중에 “나 아님”이라고 주장하지 못함 |
- 작동 원리
![Image](https://github.com/user-attachments/assets/0262c5ae-b272-4498-844d-594022db1647)
    - **해시 함수 + 비대칭키 암호화** 조합으로 작동
    - 서명 과정 (서명자 쪽)
        1. 문서의 해시값 계산:
            
            `H(document) → 해시값`
            
        2. 이 해시값을 개인키(Private Key)로 암호화:
            
            `Sign = Encrypt(H(document), PrivateKey)`
            
        3. 문서 + 전자서명을 전송
    - 검증 과정 (수신자 쪽)
        1. 받은 문서에서 다시 해시값 계산:
            
            `H(document)`
            
        2. 받은 서명을 공개키(Public Key)로 복호화해서 해시 추출
            
            `Decrypted = Decrypt(Sign, PublicKey)`
            
        3. 둘이 같으면 유효한 서명

# 네트워크 보안

## HTTPS 동작 원리

HTTPS = HTTP + TLS

→ 평문인 HTTP 통신을 TLS(전 SSL)로 감싸서 암호화하는 방식

1. 클라이언트가 HTTPS 요청 → `https://...`
2. 서버가 **디지털 인증서(공개키 포함)** 전송
3. 클라이언트가 인증서 검증 (신뢰된 CA 발급 여부 등)
4. TLS Handshake 진행 (공개키로 세션키 교환)
5. 세션키로 데이터 **대칭키 암호화**하여 HTTP 통신 시작

## TLS 핸드셰이크

클라이언트와 서버가 안전한 통신을 시작하기 전 ****서로의 신원을 확인하고 암호화된 연결을 설정하기 위해 수행하는 절차

**→ 서로를 인증**하고 암호화 방식(암호 스위트)을 합의하고 **세션 키를 교환**하여 안전한 통신 채널을 구축한다.

- TLS 핸드셰이크 흐름 (TLS 1.2 기준)
    
    ```
    Client                            Server
      | -------- ClientHello --------> |
      |                                |
      | <-------- ServerHello -------- |
      | <-------- Certificate -------- |
      | <---- ServerKeyExchange ------ |
      | <------- ServerHelloDone ----- |
      |                                |
      | --- ClientKeyExchange -------> |
      | --- ChangeCipherSpec -------> |
      | --- Finished ---------------> |
      |                                |
      | <--- ChangeCipherSpec -------- |
      | <--- Finished ---------------- |
    ```
    

## 인증서 구조

- 디지털 인증서 : 서버 또는 사용자의 **공개키와 신원을** 제3자인 **인증기관(CA)**이 **전자서명**한 문서
- TLS/HTTPS에서 사용되는 인증서는 대부분 X.509 v3 포맷을 따름
    
    
    | 필드명 | 설명 |
    | --- | --- |
    | **Issuer** | "이 인증서는 어느 CA가 발급했는가?" (ex. Let's Encrypt, DigiCert 등) |
    | **Subject** | "이 인증서는 누구를 위한 것인가?" (ex. `CN=example.com`) |
    | **Public Key Info** | 인증 대상이 사용하는 공개키 |
    | **Signature** | **CA가 서명한 전자서명** (검증용) |
    | **Extensions** | SAN(DNS 이름), OCSP, 인증서 사용 범위 등 |
- 인증서 검증 과정
    1. 클라이언트는 서버에서 인증서를 받음
    2. 서명값(Signature)을 공개키로 복호화
    3. 복호화한 해시와 실제 인증서 데이터의 해시값이 같으면 위조되지 않음
    4. Issuer의 공개키(→ 상위 CA의 인증서)를 통해 검증 반복
        
        → 루트 인증서까지 이어지면 신뢰성 확인 완료
        

## SSL Strip

사용자가 `http://`로 접속했을 때, `HTTPS`로 넘어가는 리디렉션(보안 연결 업그레이드)을 중간에서 가로채고 제거해서 평문 HTTP로 고정시키는 공격

→ 사용자가 **HTTPS로 접속했다고 착각**하게 만들고 실제로는 HTTP로 통신하게 해서 암호화되지 않은 데이터(로그인, 비밀번호 등)를 탈취

- 대응 방법
    - HSTS (HTTP Strict Transport Security) 사용 → 브라우저가 HTTP 요청 자체를 차단
    - VPN 사용

## DNS Spoofing

- DNS(Domain Name Service) : 접속하려는 URL 주소 이름으로 IP 주소를 구하는 서비스
- DNS Spoofing : 공격 대상에게 전달되는 IP 주소를 변조하거나, DNS server에 변조된 IP 주소가 저장되게 해서 공격 대상이 의도하지 않은 주소로 접속하게 하는 공격 방법
    - 올바른 URL을 입력해도 이상한 사이트로 유도됨 → 사용자는 자신이 공격을 당했는지 판단하기 쉽지 않음
    - 예시 : 가짜 은행 사이트
- 대응 방법
    - DNS server의 소프트웨어를 최신 버전으로 업데이트
    - DNSSEC 사용 → DNS 응답에 전자서명을 포함시켜, 사용자가 받은 IP 주소가 진짜 도메인 소유자가 제공한 것인지 검증할 수 있도록 해줌

## Man-in-the-Middle 공격

공격자가 **사용자와 서버 사이의 통신 경로에 몰래 끼어들어** 데이터(로그인 정보, 결제 정보 등)를 **가로채거나 변조**하는 공격 방식

- 동작 과정 예시
    1. 사용자가 **공용 Wi-Fi**에 연결 (예: 카페)
    2. 공격자는 **해당 네트워크에서 패킷을 스니핑**
    3. 사용자가 웹사이트에 로그인
    4. 로그인 요청이 공격자에게 먼저 도착
    5. 공격자는 로그인 정보를 **가로채거나 조작**
- 대응 방법
    - TLS 강제 적용 (HSTS)
    - 인증서 핀닝
    - 보안 쿠키 설정