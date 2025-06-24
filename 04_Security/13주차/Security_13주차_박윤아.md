
## CIA Triad

정보 보안의 핵심 개념인 CIA Triad는 **기밀성**, **무결성**, **가용성**의 약자로 정보 보안에서 필수적으로 고려해야함

이들 요소는 상호 보완적인 관계를 가지며, 하나라도 소홀히 여겨서는 안 됨  
다만, **어느 요소가 더 중요한지에 대해서는 상황에 따라 달라질 수** 있음

> 📌 기밀성이 중요한 조직에서는 기밀성을 최우선으로 고려하고, 그다음으로 무결성, 가용성을 중요시한다.  
> 반면, OT 환경(운영기술 환경)에서는 가용성이 최우선이며, 그다음으로 무결성과 기밀성을 고려하는 순서가 바뀔 수 있다.

[##_Image|kage@7D0hz/btsOOqaw677/AAAAAAAAAAAAAAAAAAAAAPpAkxzRx-ynXfRcrEY4ia4ECw4-0SfSYmkhrRi9RqaD/img.png?credential=yqXZFxpELC7KVnFOS48ylbz2pIh7yKj8&amp;expires=1751295599&amp;allow_ip=&amp;allow_referer=&amp;signature=SOqnMtZm8VX1aq99gdu6F7eep5g%3D|CDM|1.3|{"originWidth":581,"originHeight":429,"style":"alignCenter","width":520,"height":384,"caption":"CIA Triad","filename":"cia.png"}_##]

###   
1) 기밀성 (Confidentiality)

기밀성은 “**_허가받지 않은 자가 정보에 접근할 수 없도록 보호하는 것_**”을 의미

개인의 은행 계좌 정보가 **외부에 노출되지 않도록 보호**해야 하는데, 이를 실현하기 위한 대표적인 방법은 \`**암호화\`**

> 📌 암호화는 데이터를 다른 형태로 변환하여, **허가받지 않은 자가 이해할 수 없게 만드는 기술**

또한, \`**접근 통제**\`도 중요한 기밀성 확보 수단

금고에 중요한 문서를 보관하고, 그 금고에 접근할 수 있는 자격을 제한하는 방식처럼, **정보 시스템에서도 사용자별로 접근 권한을 제한**함으로써 기밀성을 유지할 수 있음

예시: 기업에서는 직급별, 부서별로 권한을 세분화하여 개발자가 회계 데이터를 함부로 열람하지 못하거나, 법무팀이 보관하는 계약서를 타 부서에서 볼 수 없도록 관리!

[##_Image|kage@yMmmo/btsON4MDuYA/AAAAAAAAAAAAAAAAAAAAAN4-cTeDv9VCIib3Ci6oPjH8skUZO4wiJgSyYolP3hM8/img.png?credential=yqXZFxpELC7KVnFOS48ylbz2pIh7yKj8&amp;expires=1751295599&amp;allow_ip=&amp;allow_referer=&amp;signature=O5unDz7Cwj%2BEQn%2FLFOQ6Sg0MGQg%3D|CDM|1.3|{"originWidth":1057,"originHeight":580,"style":"alignCenter","width":762,"height":418,"filename":"기밀성.png"}_##]

### 2) 무결성 (Integrity)

무결성은 “**정보가 허가받지 않은 방식으로 변경되거나 삭제되지 않도록 보호하는 것”**을 의미

은행의 거래 내역이 임의로 조작되지 않도록 보호하는 것이 무결성의 대표적인 사례로,  
무결성을 유지하기 위해서는 \`변경 금지 정책\`이나, 필요한 경우 \`변경 시 승인\`을 받는 절차가 必

또한, 변경 작업이 발생하면 그에 대한 **로그 기록**을 남겨, 문제가  발생했을 때 추적할 수 있어야 함  
이러한 로그는 사후 분석에 중요한 역할을 하며, 문제 해결의 단서를 제공

[##_Image|kage@IkdK5/btsOMQ2D3Xh/AAAAAAAAAAAAAAAAAAAAAB-IqzHvcHl5QWBpVScx84AX9kyfxRL7ZTXkW3TCNX2k/img.png?credential=yqXZFxpELC7KVnFOS48ylbz2pIh7yKj8&amp;expires=1751295599&amp;allow_ip=&amp;allow_referer=&amp;signature=zh1b%2FSX0jRUBYeR7CUFnJuTopk8%3D|CDM|1.3|{"originWidth":590,"originHeight":580,"style":"alignCenter","width":518,"height":509,"filename":"무결성.png"}_##]

### 3) 가용성 (Availability)

가용성은 “_**허가받은 사용자가 적시에 원하는 정보를 접근할 수 있도록 하는 것**_”을 의미  
이는 정보 시스템이 **언제든지 사용 가능하도록 유지**하는 것을 목표로 하며, 백업과 재난 복구 시스템을 통해 실현

데이터 손실에 대비해 정기적으로 데이터를 백업하거나, 2차 데이터센터를 구축해 시스템 장애 시 신속하게 복구할 수 있도록 하며, 정기적인 복구 테스트를 통해 예상치 못한 상황에 대비하는 것이 중요

---

### ❓ 인증과 인가

#### 인증 (Authentication)

사용자의 **신원을 입증하는 과정**을 인증이라고 함

> 예시: 사용자가 사이트에 로그인할 때 누구인지 확인하는 과정

#### 인가 (Authorization)

인가는 **사이트의 특정 부분에 접근할 수 있는지 권한을 확인**하는 작업

> 예시: 관리자는 관리자 페이지에 들어갈 수 있으나, 일반 사용자는 관리자 페이지에 접근 불가

---

## 해시 (Hash)

해시는 **임의 길이의 데이터**를 **고정된 길이의 데이터**(해시 값, 해시 코드, 요약 등)로 변환하는 **단방향 함수**

원본 데이터를 추정할 수 없도록 설계되며, **같은 입력에 대해서는 항상 같은 출력**을 보장

| **용어** | **설명** |
| --- | --- |
| **해시** | 다양한 길이 데이터를 고정 길이 데이터로 매핑한 값 |
| **해싱** | 임의의 데이터를 해시로 바꾸는 일 |
| **해시 함수** | 임의의 데이터를 해시로 바꾸는 함수 |


---

## HTTPS (HyperText Transfer Protocol Secure Socket Layer)

HTTP/2,3은 HTTPS 위에서 동작

HTTPS는 애플리케이션 계층과 전송 계층 사이에 신뢰 계층인 \`SSL/TLS\` 계층을 넣은 신뢰할 수 있는 HTTPS 요청을 말함

HTTPS는 소켓 통신에서 일반 텍스트를 이용하는 대신에, **SSL이나 TLS 프로토콜을 통해** **세션 데이터를 암호화** 함

> ∴ 데이터의 적절한 보호를 보장

HTTPS를 구축하는 방법은 아래와 같이 크게 세 가지로 나뉨

1.  직접 **CA**에서 구매한 인증키를 기반으로 HTTPS 서비스 구축
2.  서버 앞단에 HTTPS를 제공하는 **로드밸런서** 사용
3.  서버 앞단에 HTTPS를 제공하는 **CDN** 사용

---

## SSL/TLS

**SSL** (Secure Soket Layer)은 SSL 1.0 부터 시작하여 SSL 2.0, SSL3.0, TLS 1.0, TLS 1.3까지 버전이 올라감

**TLS(Transport Layer Security Protocol)로 명칭이 변경**되었으나, 통칭하여 SSL/TLS로 부름

-   SSL/TLS는 전송 계층에서 보안을 제공하는 프로토콜
-   제 3자가 메시지를 도청하거나 변조하지 못하도록 함

| **항목** | **SSL** | **TLS** |
| --- | --- | --- |
| **정식 명칭** | Secure Sockets Layer | Transport Layer Security |
| **개발 주체** | Netscape | IETF(인터넷 표준화 기구) |
| **버전** | SSL 2.0, 3.0 (현재 모두 폐기됨) | TLS 1.0, 1.1, 1.2, 1.3 |
| **보안성** | 취약점 존재, 더 이상 사용되지 않음 | 최신 버전은 보안이 강화됨 |
| **호환성** | 과거 시스템과 호환 | SSL과의 하위 호환 제공 (초기 TLS) |
| **사용 여부** | 더 이상 사용하지 않음 | **현재 대부분의 웹에서 사용** 중 |

[##_Image|kage@d7Yp0K/btsNVd5Fhqo/AAAAAAAAAAAAAAAAAAAAAJU3tb04HFGzZnOdCCCiYX4gfVn2g4YG-FdAUHMkxV2H/img.png?credential=yqXZFxpELC7KVnFOS48ylbz2pIh7yKj8&amp;expires=1751295599&amp;allow_ip=&amp;allow_referer=&amp;signature=14bYp8kB9wP1ANgK9ThIbOrvFYc%3D|CDM|1.3|{"originWidth":1569,"originHeight":408,"style":"alignCenter","width":760,"height":198,"filename":"SSL.png"}_##]

SSL/TLS를 통해 공격자가 서버인 척하면서  사용자 정보를 가로채는 네트워크상의 \`**인터셉터\` 방지 가능**

-   SSL/TLS는 **보안 세션을 기반**으로 데이터를 암호화
-   **보안 세션 만들기 👉** _**인증 메커니즘, 키 교환 암호화 알고리즘, 해싱 알고리즘**_ 사용됨

---

### 인증 메커니즘

인증 메커니즘은 \`**CA**\` (Certificate Authorities)에서 발급한 인증서를 기반으로 이루어짐

CA에서 발급한 인증서는 안전한 연결을 시작하는 데 있어 필요한 \`**공개키\`를 클라이언트에 제공**하고,  
사용자가 접속한 **서버가 신뢰할 수 있는 서버**임을 보장

[##_Image|kage@CoZ56/btsOPCJqTvb/AAAAAAAAAAAAAAAAAAAAAPB190PJJcZlU7jAyfurqnDLid8mGgwI9cDmKDNjkOLT/img.png?credential=yqXZFxpELC7KVnFOS48ylbz2pIh7yKj8&amp;expires=1751295599&amp;allow_ip=&amp;allow_referer=&amp;signature=7lq4tLKRw7qMfc%2BYedzy1MtYYBg%3D|CDM|1.3|{"originWidth":864,"originHeight":317,"style":"alignCenter","width":787,"height":289,"caption":"HTTPS 통신 과정","filename":"SSL 인증서.png"}_##]

CA는 아무 기업이나 할 수 있는 것이 아니며, _**신뢰성이 엄격히 공인된 기업들**만 참여_ 가능

> Ex) Comodo, GoDaddy, GlobalSign, 아마존 등

---

### CA 발급 과정

서비스가 CA 인증서를 발급받으려면 \`**사이트 정보**\`와 \`**공개키**\`를 CA에 제출해야 함

이후 CA는 공개키를 해시한 값인 **지문(finger print)**을 사용하는 CA의 비밀키 등을 기반으로 CA 인증서 발급

> **❓ 개인키, 공개키 (쌍을 이루어 존재)  
> \- 개인키 (비밀키)**: 키 발행자만이 갖는 키, 보통 데이터를 복호화 하는데 사용 (암호화도 가능)  
> **\- 공개키**: 누구나 알아도 상관 없는 키, 보통 데이터를 암호화 하는데 사용 (복호화도 가능)  
> \- **대칭키**: 암/복호화에 각기 다른 키를 사용하는 것이 아닌 대칭키라는 하나의 키를 사용

[##_Image|kage@BRWTX/btsOOdRt5vD/AAAAAAAAAAAAAAAAAAAAAA6Q5oMJKWu5sk45PsashyjdZ9jhzf43b0Zl9tJawht8/img.png?credential=yqXZFxpELC7KVnFOS48ylbz2pIh7yKj8&amp;expires=1751295599&amp;allow_ip=&amp;allow_referer=&amp;signature=KR2B6JQ%2FCXHcAxcinzDA9xmT%2BXs%3D|CDM|1.3|{"originWidth":559,"originHeight":822,"style":"alignCenter","caption":"&lt;HTTPS 통신과정 (출처: 미닉스 김인성님 블로그)&gt;","filename":"HTTP 통신 과정.png"}_##]

---

#### 인증서를 사용하는 이유

인증서는 **서버가 진짜 신뢰할 수 있는 주체인지 클라이언트에게 증명**하기 위한 수단임

#### 인증서의 역할

1.  서버의 공개키와 도메인이 진짜인지 검증
2.  공개키가 제 3자 (인증기관, CA)에 의해 발급되었는지 확인
3.  중간자 공격 방지

#### 인증서에 들어있는 것

-   서버의 도메인 정보
-   서버의 공개키
-   인증기관의 전자 서명
-   유효기간 등 메타 정보

브라우저는 내장된 신뢰된 루트 CA 목록을 가지고, 서버 인증서의 서명 검증

> **📌 정리  
> **인증서 없이는 **누가 공개키를 보냈는지 알 수 없어서, 신뢰할 수 없는 암호화**가 됨  
> ∴ SSL/TLS 핸드셰이크 (= HTTPS 핸드셰이크)에서 인증서를 먼저 보내는 것

이렇게, SSL/TLS 인증서를 통해 웹 브라우저가 서버를 신용하게 되었으므로, 그 다음으로 웹 브라우저와 서버는 **데이터를 어떻게 암호화할 것인지에 대해 협상**함.

이 협상에 사용되는 암호화 알고리즘의 집합이 바로 사이퍼 슈트이며, 이 전반적인 과정을 \`SSL/TLS HandShake\`라고 함

---

## SSL/TLS Handshake

**보안이 시작되고 끝나는 동안 유지되는 세션**

SSL/TLS는 핸드셰이크를 통해 보안 세션을 생성, 이를 기반으로 상태 정보 등을 공유

> **📌 세션**   
> 운영체제가 어떠한 사용자로부터 자신의 자산 사용을 허락하는 일정 기간  
> 사용자는 일정 시간 동안 응용 프로그램 & 자원 사용 가능

[##_Image|kage@Lll8o/btsOOIQXMqQ/AAAAAAAAAAAAAAAAAAAAALtUM99CpzhIwvIJUuG9KxrPe4IZpHe1SbTAxycn_or-/img.png?credential=yqXZFxpELC7KVnFOS48ylbz2pIh7yKj8&amp;expires=1751295599&amp;allow_ip=&amp;allow_referer=&amp;signature=eT6tmo%2FTDGd68vfxV7E6g8lvB9w%3D|CDM|1.3|{"originWidth":1259,"originHeight":549,"style":"alignCenter","width":805,"height":351,"caption":"SSL/TLS HandShake 과정 (출처: 부소대장님 tistory)"}_##]

\`TCP Handshake\`: HTTPS가 TCP기반의 프로토콜이므로 암호화 협상 (TLS Handshake)에 앞서 연결을 생성하는 과정

---

### 1) Client Hello

Client가 Server에 연결을 시도하며 전송하는 패킷

전송 내용: 

-   자신이 사용가능한 사이퍼 슈트 목록
-   Session ID
-   SSL Protocol Version
-   Random byte 

#### 사이퍼 슈트 (Cipher Suite) 👇🏻 

더보기

SSL Protocol version, 인증서 검증, 데이터 암호화 프로토콜, Hash 방식 등의 정보를 담고 있는 존재로,  
**선택된 Cipher Suite의 알고리즘에 따라 데이터를 암호화** 함

-   TLS\_AES\_128\_GCM\_SHA256
-   TLS\_AES\_256\_GCM\_SHA384
-   TLS\_ECDHE\_RSA\_WITH\_AES\_128\_GCM\_SHA256
-   TLS\_AES\_128\_CCM\_8\_SHA256

등

_EX) \`TLS\_AES\_128\_GCM\_SHA256\`_

-   TLS = 프로토콜
-   AES\_128\_GCM: AEAD 사이퍼 모드
-   SHA256: 해싱 알고리즘

EX2) \`TLS\_ECDHE\_RSA\_WITH\_AES\_128\_GCM\_SHA256\`

-   ECDHE: 키 교환방식
-   RSA: 인증서 검증

> **❓ AEAD 사이퍼 모드  
> **AEAD(Authenticated Encryption with Associated Data)는 **데이터 암호화 알고리즘**을 의미  
> EX)  AES\_128\_GCM  
> **128비트의 키**를 사용하는 표준 블록 암호화 기술 + **병렬 계산**에 용이한 **알고리즘 GCM**이 결합된 알고리즘

---

### 2-1) Server Hello

Client가 보내온 ClientHello Packet을 받아 사이퍼 슈트 중 하나를 선택한 후, Client에게 알림

또한 자신의 SSL Protocol version 등 도 함께 보냄

---

### 2-2) Certificate

1.  Server가 자신의 SSL/TLS 인증서를 Client에게 전달  
    (인증서 내부에는 **Server가 발행한 공개키**가 들어 있음)
2.  Client는 CA의 개인키로 암호화된 SSL 인증서를 CA의 공개키로 복호화
3.  복호화에 성공하면, 인증서는 진짜임이 증명됨 (실패 시 가짜)

---

#### 2-3) Server Key Exchange / ServerHello Done

Server Key Exchange는 Server의 공개키가 SSL 인증서 내부에 없는 경우, Server가 직접 전달함을 의미

> 인증서에 있는 경우 생략하는 과정

---

### 3-1) Client Key Exchange

Client는 데이터 암호화에 사용할 **대칭키를 생성하여, Server의 공개키로 암호화한 후 Server에 전달**

여기서 전달된 \`대칭키\`가 바로 **SSL/TLS Handshake의 목적**이자, 가장 중요한 수단인 **데이터를 실제로 암호화하는 비밀키**임

이제 Key를 통해 Client와 Server가 교환하고자 하는 데이터를 암호화 함

---

### 3-2) ChangeCipherSpec / Finished

Client, Server 모두가 서로에게 보내는 Packet으로 교환할 정보를 모두 교환한 뒤, **통신할 준비가 다 되었음을 알리는 패킷**

그리고 Finished Packet을 보내어 SSL/TLS Handshake 종료

---

#### **정리**

1.  ClientHello (암호화 알고리즘 나열 & 전달)
2.  ServerHello (암호화 알고리즘 선택)
3.  ServerCertificate (인증서 전달)
4.  Client Key Exchange (데이터 암호화할 대칭키 전달)
5.  Client / ServerHello done (정보 전달 완료)
6.  Finished (SSL/TLS Handshaek 종료)

즉, 클라이언트와 서버가 키를 공유하고, 이를 기반으로 인증, 인증 확인 등의 작업이 발생  
\= **단 한 번의 1-RTT가 생긴 후 데이터를 송수신** 

> 📌 TLS 1.3은 사용자가 이전에 방문한 사이트를 재방문하는 경우, 위 통신을 하지 않아도 됨 **(= 0-RTT)**

---

## 암호화 알고리즘

\`공개키\`는 **보안**을 위해,\` 대칭키\`는 **속도를 위해** 사용됩니다.

HTTPS에서는 처음에 **공개키로 대칭키를 안전하게 주고받고**, 이후는 **대칭키로 빠르게 암호화**합니다.

| **항목** | **공개키 암호화** | **대칭키 암호화** |
| --- | --- | --- |
| **키 사용 방식** | 공개키로 암호화, 개인키로 복호화 | 같은 키로 암호화와 복호화 |
| **속도** | 느림 | 빠름 |
| **사용 시점** | **초기 핸드셰이크에서만 사용** (대칭키 전달용) | 이후 **실제 데이터 전송에 사용** |
| **예시 알고리즘** | RSA | AES |

> **❓ 암호화 알고리즘?, 키 교환 방식?**  
> RSA는 공개키로 암호화 / 복호화를 수행하므로 공개키 암호화 알고리즘임.  
> DH, ECDHE는 공개키를 사용하는 키 교환 방식임 (암호화 xx)

---

### 공개키 암호화: RSA (Rivest–Shamir–Adleman)

#### 공개키 암호화

-   암/복호화에 서로 다른 키 사용
-   송신자는 수신자의 공개키로 암호화, 수신자는 자신의 개인키로 복호화
-   수학적 난제를 기반으로 설계
-   대칭키 암호에 비해 느림
-   키 교환 불필요
-   여러 송신자가 하나의 공개키로 암호화 하므로 사용자가 많아도 키 관리 유리
-   사용자 N명이 각자의 공개키 1개, 개인키 1개를 가지므로 → **키 개수 = \`2N\`**

[##_Image|kage@c0J0bT/btsOOvR1UAN/AAAAAAAAAAAAAAAAAAAAAFfM_ImF9XvktkeXgnYoTP6EVjm_tu5v8UMoIo_2NpP_/img.png?credential=yqXZFxpELC7KVnFOS48ylbz2pIh7yKj8&amp;expires=1751295599&amp;allow_ip=&amp;allow_referer=&amp;signature=opGhYd1kkPrBTpgNtsIBLnIfXgc%3D|CDM|1.3|{"originWidth":1098,"originHeight":613,"style":"alignCenter","width":761,"height":425,"caption":"공개키 암호 방식 (출처: 부소대장님의 Tistory)","filename":"공개키.png"}_##]

---

#### RSA의 핵심 아이디어

> **소인수분해가 매우 어렵다는 수학적 문제**를 기반으로 함

즉,

-   큰 수 n을 두 소수의 곱으로 분해하는 건 매우 어렵고,
-   이 어려움을 이용해서 안전한 암호화 구조를 만듦

#### RSA 암호화/복호화 동작

### ▶️ 암호화:

-   송신자: C = M^e mod n
-   여기서 M은 평문, C는 암호문, e와 n은 수신자의 공개키

### ◀️ 복호화:

-   수신자: M = C^d mod n
-   d와 n은 수신자의 개인키

즉, **누구나 암호화할 수 있지만 복호화는 개인키를 가진 사람만 가능**합니다.

---

#### ✍️ RSA 전자서명

RSA는 **전자서명(디지털 서명)**에도 사용

-   서명자(보내는 사람):
    -   해시(M) → 서명 S = H(M)^d mod n (개인키 사용)
-   검증자(받는 사람):
    -   S^e mod n = H(M)이면 검증 성공 (공개키 사용)

즉, **개인키로 서명 → 공개키로 검증**, 신뢰성을 보장

---

#### 📌 정리

> RSA는 공개키 암호화 알고리즘의 대표격으로,  
> 데이터를 암호화하거나, 전자서명을 생성하거나, 세션 키를 교환하는 데 사용됨  
>   
> TLS 1.3 부터는 속도와 보안성 문제로 ECDHE 등으로 대체되는 중

---

### 대칭키 암호화: AES (Advanced Encryption Standard)

#### 대칭키 암호화

-   암/복호화에 같은 암호키 사용
-   비대칭키(공개키) 암호화에 비해 속도 빠름
-   사전에 송, 수신자 간 키를 교환 (DH, ECDHE, RSA) 해야 함
-   대상이 증가할 수록 많은 키 관리 필요
-   사용자 N명이 서로 안전하게 통신하려면, 각 쌍마다 서로 다른 키가 필요 → **키 개수 = \`N(N-1)/2\`**

[##_Image|kage@nswZC/btsOOIXQp99/AAAAAAAAAAAAAAAAAAAAAMmz-pkn1PaDxIkKgFiHzSVp9Jjqn913cchP03YpIt0E/img.png?credential=yqXZFxpELC7KVnFOS48ylbz2pIh7yKj8&amp;expires=1751295599&amp;allow_ip=&amp;allow_referer=&amp;signature=s3Ghus6eIr3wo%2BKVSPWt8nNax4k%3D|CDM|1.3|{"originWidth":1060,"originHeight":575,"style":"alignCenter","caption":"대칭키 암호 방식 (출처: 부소대장님의 Tistory)","filename":"대칭키.png"}_##]

AES: 가장 **널리 사용**되는 대칭키 블록 암호화 알고리즘

| **항목** | **내용** |
| --- | --- |
| **이름** | **AES, Advanced Encryption Standard** (고급 암호화 표준) |
| **대체한 것** | 과거 **DES**(Data Encryption Standard)를 대체 |
| **암호 방식** | **대칭키 암호화** – 암호화와 복호화에 같은 키 사용 |
| **블록 크기** | **128비트 고정** |
| **키 길이** | **128, 192, 256비트 중 하나 선택** |
| **라운드 수** | 키 길이에 따라 **10, 12, 14 라운드** |
| **사용처** | HTTPS, VPN, 파일 암호화, 하드디스크 암호화 등 |

---

# 🔐 SSL Strip, DNS Spoofing, Man-in-the-Middle 공격 정리

## ✅ 1. Man-in-the-Middle (MitM) 공격

> **중간자 공격**: 공격자가 사용자와 서버 사이의 통신을 가로채 조작하거나 탈취하는 공격

### 📌 개념
- 사용자는 서버와 안전하게 통신 중이라 믿지만, 실제로는 공격자와 통신
- 공격자는 중간에서 데이터를 감청하거나 변조함

### 📌 공격 방식
- 네트워크 스니핑
- ARP 스푸핑
- DNS 스푸핑
- SSL Strip과 연계 가능

### 🛡️ 대응 방안
- HTTPS 사용
- VPN 사용
- 세션 토큰 보호
- 인증서 유효성 확인

---

## ✅ 2. SSL Strip

> **HTTPS → HTTP 다운그레이드**를 유도해 사용자 정보를 평문으로 탈취

### 📌 개념
- 공격자가 HTTPS 요청을 중간에서 가로채 HTTP로 바꿔버림
- 사용자는 평문 HTTP로 민감 정보를 입력하게 되고, 공격자는 이를 수집

### 📌 작동 방식
1. 공격자가 MitM 기법으로 트래픽에 침투
2. 사용자가 `https://example.com` 접속 시, 공격자가 이를 `http://example.com`으로 리디렉션
3. 사용자는 HTTP로 통신하게 되고, 입력한 정보가 평문으로 전송됨
4. 공격자는 이를 탈취하고, 서버에는 HTTPS로 통신해 중계

### 🛡️ 대응 방안
- **HSTS (HTTP Strict Transport Security)** 사용
- HTTPS 리디렉션 강제
- 브라우저 주소창 확인 습관화

---

## ✅ 3. DNS Spoofing (또는 DNS Cache Poisoning)

> **DNS 응답을 위조하거나 조작하여** 사용자를 가짜 사이트로 유도하는 공격

### 📌 개념
- 사용자가 `www.bank.com`에 접속했지만, 실제로는 공격자가 만든 피싱 사이트로 연결됨
- 피해자는 진짜 사이트인 줄 알고 로그인 정보 등 민감 정보를 입력

### 📌 작동 방식
- DNS 응답 패킷을 위조하거나,
- DNS 서버의 캐시를 조작해 잘못된 IP를 반환
- ARP 스푸핑과 함께 사용되기도 함

### 🛡️ 대응 방안
- DNSSEC (DNS 보안 확장) 사용
- 정적 DNS 설정 또는 보안 DNS 서버 사용 (예: 1.1.1.1, 8.8.8.8)
- HTTPS 적용 및 HSTS

---

## 📊 요약 비교

| 공격 기법       | 목적                          | 주요 수단             | 대응 방안                             |
|----------------|-------------------------------|------------------------|----------------------------------------|
| **MitM**        | 통신 중간 탈취 및 조작        | ARP/DNS 스푸핑 등     | HTTPS, VPN, 세션 보호 등               |
| **SSL Strip**   | HTTPS → HTTP 다운그레이드     | MitM 기반              | HSTS, HTTPS 리디렉션 강제              |
| **DNS Spoofing**| 사용자를 가짜 IP로 유도       | DNS 응답 위조          | DNSSEC, 보안 DNS 사용, HTTPS 적용      |
