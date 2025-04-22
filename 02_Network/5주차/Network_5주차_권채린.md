# OSI 7계층 & TCP/IP 모델

### 📚 목차

- [🧮 OSI 7계층](#-osi-7계층)
  - [1️⃣ 1계층 : 물리 계층 (Physical Layer)](#1️⃣-1계층--물리-계층-physical-layer)
  - [2️⃣ 2계층 : 데이터 링크 계층 (Data Link Layer)](#2️⃣-2계층--데이터-링크-계층-data-link-layer)
  - [3️⃣ 3계층 : 네트워크 계층 (Network Layer)](#3️⃣-3계층--네트워크-계층-network-layer)
  - [4️⃣ 4계층 : 전송 계층 (Transport Layer)](#4️⃣-4계층--전송-계층-transport-layer)
  - [5️⃣ 5계층 : 세션 계층 (Session Layer)](#5️⃣-5계층--세션-계층-session-layer)
  - [6️⃣ 6계층 : 표현 계층 (Presentation Layer)](#6️⃣-6계층--표현-계층-presentation-layer)
  - [7️⃣ 7계층 : 응용 계층 (Application Layer)](#7️⃣-7계층--응용-계층-application-layer)
  - [📝 2,3,4 계층 주요 정리](#-234-계층-주요-정리)
- [📲 TCP/IP 모델](#-tcpip-모델)
  - [🌎 OSI 7계층 vs TCP/IP 4계층](#-osi-7계층-vs-tcpip-4계층)
  - [🌎 웹 브라우저에서 구글을 검색하면 어떤 경로로 서비스되는가?](#-웹-브라우저에서-구글을-검색하면-어떤-경로로-서비스되는가)


<br>

## 🧮 OSI 7계층

네트워크 통신을 7개의 계층으로 나누어 설명한 이론적 모델

![image](https://github.com/user-attachments/assets/da6d74f0-f99e-411b-8f06-07e193803f06)

<br>

### 1️⃣ 1계층 : 물리 계층 (Physical Layer)

- **역할**: 0과 1을 전송하는 물리적 수단 담당, 데이터를 실제 전기 신호나 빛으로 전송
- **데이터 단위**: 비트(Bit)
- **기술/장비**: 케이블, 리피터, 허브, 전압, 핀 배치
- **예시**: RJ-45 케이블, USB, 광섬유

![image](https://github.com/user-attachments/assets/fae44740-0f40-41d6-93f7-0f5eb426c090)

<br>

### 2️⃣ 2계층 : 데이터 링크 계층 (Data Link Layer)

- **역할**: 같은 네트워크 내에서 오류 없이 데이터 전달이 목표
    - 오류 감지, 흐름 제어, MAC 주소 기반 통신
- **데이터 단위**: 프레임(Frame)
- **기술/장비**: 스위치, 브리지
- **프로토콜**: Ethernet, PPP, HDLC

  
<br>

### 3️⃣ 3계층 : 네트워크 계층 (Network Layer)

- **역할**: 다른 네트워크 간 데이터 전송 담당(목적지까지의 경로 설정)
    - 라우팅, IP 주소 기반 논리적 주소 지정
- **데이터 단위**: 패킷(Packet)
- **기술/장비**: 라우터
- **프로토콜**: IP, ICMP, ARP

📌 ICMP**(Internet Control Message Protocol)** : IP 프로토콜의 동작을 보조하는

**제어용 메시지 프로토콜**로, **오류 메시지 전송**, **진단**, **통신 상태 확인** 등에 사용됨.

📌 ARP(Address Resolution Protocol) : **IP 주소를 MAC 주소로 변환**해주는 프로토콜.

<br>

![image](https://github.com/user-attachments/assets/4e8ad66d-ae45-47d2-85e0-7a86721c01bf)


### 4️⃣ 4계층 : 전송 계층 (Transport Layer)

- **역할**: 송수신 간 **신뢰성 있는 데이터 전송**, 오류 복구, 포트 번호로 통신 구분
- **데이터 단위**: 세그먼트(Segment, TCP) / 데이터그램(Datagram, UDP)
- **프로토콜**: TCP(연결형), UDP(비연결형)

<br>

### 5️⃣ 5계층 : 세션 계층 (Session Layer)

- **역할**: 통신 세션 연결, 유지, 종료 관리
- **기능**: 세션 설정, 동기화, 체크포인트 설정
- **예시**: 원격 데스크톱, 스트리밍 서비스 세션 유지 등

<br>

### 6️⃣ 6계층 : 표현 계층 (Presentation Layer)

- **역할**: 데이터의 **형식, 인코딩, 암호화/복호화, 압축/해제**
- **기능**: 서로 다른 시스템 간 데이터 표현 방식 일치
- **예시**: JPEG, MP3, MPEG, TLS, ASCII ↔ EBCDIC

<br>

### 7️⃣ 7계층 : 응용 계층 (Application Layer)

- **역할**: 사용자와 가장 가까운 계층, 애플리케이션이 네트워크에 접근할 수 있도록 제공
- **기능**: 웹브라우저, 이메일, 파일 전송, 메시징 등
- **프로토콜**: HTTP, FTP, SMTP, DNS, Telnet

<br>

### 📝 2,3,4 계층 주요 정리

|  | 2 데이터링크 | 3 네트워크 | 4 전송 |
| --- | --- | --- | --- |
| 데이터 단위 | 프레임 | 패킷 | 세그먼트 |
| 프로토콜 | 이더넷, 맥, ARP | IP, ICMP | TCP, UDP |
| 주요 장비 | 스위치 | 라우터 | 게이트웨이 |
| 주요 기능 | 맥 주소 기반 통신
CRC 오류 감지 | 경로 선택(라우팅) | 종단 간(End to End) 통신
오류 검출 및 복구 |

<br>

## 📲 TCP/IP 모델

### 🌎 OSI 7계층 vs TCP/IP 4계층

- **OSI 7계층** : 네트워크 통신을 이론적으로 표준화한 모델
    - 교육적 모델
- **TCP/IP 4계층** : 실제 인터넷에서 사용되는 통신 프로토콜 구현 모델
    - 실무에서 실제 사용되는 표준

![image](https://github.com/user-attachments/assets/a06a44c9-d097-4082-95dc-b3ed56c0d9a8)

<br>

- 1계층 : 네트워크 액세스 계층
- 2계층 : 인터넷 계층
- 3계층 : 전송 계층
- 4계층 : 응용 계층

<br>

### 🌎 웹 브라우저에서 구글을 검색하면 어떤 경로로 서비스되는가?

**🔷 응용 계층 (Application Layer)**

- 사용자가 **브라우저 주소창에 "구글" 또는 "국민은행" 입력**
- 브라우저는 기본 검색 엔진(예: Google)에 **검색 쿼리 요청** 준비
    - 예: `https://www.google.com/search?q=국민은행`
- DNS 질의: `www.google.com` → IP 주소 요청
    
    → DNS 서버에서 IP 주소 반환
    
- 검색 요청은 **HTTP/HTTPS 프로토콜**을 통해 전송될 준비 완료

📌 DNS : Domain Name System 도메인 이름을 숫자로 된 ip 주소로 변환해주는 인터넷의 전화번호부

<br>

🔷 **전송 계층 (Transport Layer)**

- 브라우저는 구글 서버와 **TCP 3-way 핸드셰이크** 수행
    - SYN → SYN-ACK → ACK
- HTTPS 사용 시 **TLS 핸드셰이크** 병행
    - 서버 인증서 확인, 대칭키 생성
- 이후 **검색 요청 데이터 (HTTP GET 요청)** 전송

<br>

🔷  **인터넷 계층 (Internet Layer)**

- 브라우저는 구글 서버의 IP 주소를 기준으로 **IP 패킷 생성**
- 패킷에는 출발지 IP, 목적지 IP 포함
- IP 패킷은 여러 라우터와 게이트웨이를 거쳐 구글 데이터센터로 전달됨
    
    (클라이언트 → ISP → 백본망 → 구글)

    <br>

🔷 **4. 네트워크 액세스 계층 (Network Access Layer)**

> 실제 물리적 전송 (프레임, 전기신호, MAC 주소 기반)
> 
- IP 패킷이 **이더넷 프레임**으로 감싸짐 (캡슐화)
- 각 구간마다 **MAC 주소를 기준으로 다음 장치로 전송**
- 와이파이, LAN 케이블 등 물리 매체 통해 신호 전송
- 최종적으로 구글 서버에 도달

<br>

✅ 구글 서버 측 

- 구글 서버는 검색어를 분석 → 적절한 검색 결과 생성
- HTML/JSON 형태의 응답 생성
- TCP를 통해 클라이언트로 전송

✅ 브라우저 렌더링

- 응답받은 HTML을 파싱하여 DOM 생성
- 검색 결과를 사용자에게 시각적으로 출력

![image](https://github.com/user-attachments/assets/04304daf-1833-4b67-a4fe-6919b157bae6)
