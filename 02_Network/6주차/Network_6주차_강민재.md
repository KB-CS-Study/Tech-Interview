### [6주차] TCP, UDP, 흐름 제어 / 혼잡 제어

- Transport Layer
- TCP, UDP 특징
- TCP의 신뢰성 보장 메커니즘
- TCP의 흐름 제어 (Flow Control)
- TCP의 혼잡 제어 (Congestion Control)
- TCP 연결 과정: 3-Way Handshake와 관련 이슈
- 흐름 제어와 혼잡 제어의 차이
- UDP가 흐름/혼잡 제어를 하지 않는 이유
- DHCP
- 멀티플렉싱과 디멀티플렉싱(포트 번호와 TCP/UDP 연결)

---

<br>

## 🔵 Transport Layer

다른 호스트에서 실행되는 application process 사이에 논리적인 통신을 제공한다.

Application 관점에서는 서로 다른 호스트의 프로세스들이 **직접 연결된 것처럼** 동작한다.

### 데이터 흐름

sender (송신 측): 어플리케이션 메세지를 세그먼트 단위로 쪼개어 Network Layer로 전달한다.

receiver (수신 측): 수신한 세그먼트들을 조립하여 Application Layer로 전송한다.

### network vs transport

Network Layer: host 사이의 논리적 통신

Transport Layer: process 사이의 논리적 통신

### 주요 Transport Layer 프로토콜

TCP (Transmission Control Protocol)

UDP (User Datagram Protocol)

---

<br>

## 🔵 TCP, UDP 특징

| 항목        | TCP                   | UDP                 |
| ----------- | --------------------- | ------------------- |
| 신뢰성      | O (재전송, 확인 응답) | X (손실되어도 무시) |
| 순서 보장   | O                     | X                   |
| 연결 설정   | O (3-way handshake)   | X (연결 없이 전송)  |
| 혼잡 제어   | O                     | X                   |
| 지연 보장   | X                     | X                   |
| 대역폭 보장 | X                     | X                   |

### 항목 별 용어 설명

#### 신뢰성 (Reliability)

데이터가 전송 중 손실되거나 손상되더라도, 송신자가 재전송하거나 수신자가 확인 응답(ACK) 을 보내어 정확히 데이터를 수신하는 것을 보장하는 것

- TCP는 신뢰성을 제공한다. (손실되면 재전송)
- UDP는 신뢰성을 제공하지 않는다. (손실돼도 무시)

#### 순서 보장 (In-order Delivery)

여러 개의 데이터 조각이 전송될 때, 수신자가 원래 송신자가 보낸 순서대로 정확하게 데이터를 재조립하여 전달받는 것

- TCP는 순서를 보장한다. (순서 엇갈려도 정렬)
- UDP는 순서를 보장하지 않는다. (순서 무시)

#### 연결 설정 (Connection Setup)

데이터를 보내기 전에 송신자와 수신자가 서로 연결을 확립하는 과정

- TCP는 3-way handshake 과정을 통해 연결을 설정한다.
- UDP는 연결 없이 바로 데이터 전송을 시작한다.

#### 혼잡 제어 (Congestion Control)

네트워크가 혼잡할 때, 송신자가 전송 속도를 조절하여 네트워크 부하를 줄이는 메커니즘

- TCP는 혼잡을 감지하고 전송 속도를 줄인다.
- UDP는 혼잡 상황을 고려하지 않고 계속 전송한다.

#### 지연 보장 (Delay Guarantee)

데이터가 얼마나 빨리 도착하는지를 보장해주는 기능

- TCP와 UDP 모두 지연 시간을 보장하지 않는다.
- 네트워크 상황에 따라 도착 시간이 변동될 수 있다.

#### 대역폭 보장 (Bandwidth Guarantee)

일정한 전송 속도를 지속적으로 확보해주는 기능

- TCP와 UDP 모두 특정 대역폭을 보장하지 않는다.
- 트래픽 상황에 따라 대역폭이 달라질 수 있다.

---

<br>

## 🔵 TCP의 신뢰성 보장 메커니즘

TCP는 데이터 전송 중 발생할 수 있는 오류, 손실, 중복, 순서 어긋남 문제를 해결하기 위해 다양한 신뢰성 보장 매커니즘을 사용한다. 이 매커니즘들은 rdt (reliable data transfer) 프로토콜 발전 과정을 통해 이해할 수 있다.

TCP가 적용한 신뢰성 매커니즘은 다음과 같은 흐름으로 발전해 왔다.

- **[rdt 1.0] 완벽한 채널 가정**

  처음에는 채널이 완벽하다고 가정하여, 데이터가 손상되거나 손실되지 않는다고 생각했다. 이 경우 송신자는 데이터를 전송하기만 하면 되었고, 별다른 추가 제어가 필요 없었다.

- **[rdt 2.0] 비트 오류 복구 (ACK/NAK)**

  현실에서는 채널에서 비트 오류가 발생할 수 있으므로, 수신자는 데이터에 오류가 있는 경우 **NAK**를 보내어 송신자가 재전송하도록 하고, 데이터가 정상적이면 **ACK**를 보내 확인하는 메커니즘이 도입되었다.

- **[rdt 2.1] ACK/NAK 깨짐 대비 (Sequence Number 추가)**

  ACK/NAK 신호 자체가 깨질 수 있는 문제를 대비해, **시퀀스 번호(Sequence Number)** 를 도입했다. 이를 통해 송신자와 수신자는 어떤 데이터가 정상적으로 전송/수신되었는지를 번호를 통해 명확히 확인할 수 있게 되었다.

- **[rdt 2.2] NAK 없이 ACK만 사용 (TCP 스타일)**

  추가적인 최적화를 위해, **NAK 없이 ACK만 사용**하는 방식이 제안되었다. 수신자가 정상적으로 수신한 마지막 데이터에 대해 ACK를 보내고, 송신자는 중복된 ACK을 통해 문제를 감지하고 필요 시 재전송한다. **TCP는 이 방식(rdt 2.2) 을 기본으로 사용**한다.

- **[rdt 3.0] 패킷/ACK 유실까지 대비 (Timeout 추가)**

  이후, 네트워크에서는 **데이터 패킷이나 ACK 자체가 유실(loss) 되는 상황**도 고려해야 했다. 이를 해결하기 위해 송신자는 **타이머(timeout)** 를 설정하여 일정 시간 동안 ACK이 도착하지 않으면 해당 패킷을 재전송하는 방식으로 대응한다. 이로써 유실된 데이터도 복구할 수 있다.

- **[GBN/SR] Stop-and-Wait 한계 극복 (Window 기반)**

  Stop-and-Wait 방식은 한 번에 하나씩만 전송하고 기다리는 방식이기 때문에 전송 효율이 매우 낮았다. 이를 해결하기 위해 **Go-Back-N(GBN)** 과 **Selective Repeat(SR)** 같은 **윈도우 기반 프로토콜**이 도입되었다. TCP도 이와 유사하게 여러 개의 패킷을 동시에 보내고, ACK을 받아가며 윈도우를 조정하는 방식을 사용한다.

### 항목 별 용어 설명

#### Sequence Number (시퀀스 넘버)

패킷의 고유 번호로, 데이터의 순서 보장과 중복 감지를 위해 사용

- 송신자가 전송하는 각 패킷에 고유 번호를 붙인다.
- 수신자는 시퀀스 번호를 기준으로 정확한 순서로 재조립하거나 중복 패킷을 구별할 수 있다.
- 번호를 통해 이건 이미 받은 데이터인지 확인할 수 있다.

#### ACK (Acknowledgement, 확인 응답)

데이터가 정상적으로 도착했음을 알리는 신호

- 수신자가 데이터(패킷)를 정상적으로 받았을 때 송신자에게 보낸다
- 송신자는 ACK를 받으면 다음 데이터를 전송한다.
- TCP에서는 일반적으로 “어떤 시퀀스 번호까지 받았는지“를 함께 보낸다 (누적 ACK)

#### NAK (Negative Acknowledgement, 부정 응답)

데이터에 오류가 발생했음을 알리는 신호

- 수신자가 패킷을 제대로 받지 못했거나 오류가 있으면 송신자에게 보낸다
- 송신자는 NAK를 받으면 해당 데이터를 즉시 재전송
- TCP는 NAK를 명시적으로 사용하지 않고, 중복 ACK로 오류를 감지하고 처리한다

#### Retransmission (재전송)

데이터가 손실되었거나 손상되었을 때, 송신자가 같은 데이터를 다시 보내는 것

- NAK를 받거나
- 일정 시간 안에 ACK를 받지 못하거나 (timeout)
- 중복된 ACK를 여러 번 받으면
- 송신자는 해당 패킷을 다시 전송한다

#### Timeout (타이머)

송신자가 ACK를 기다리는 최대 시간

- 데이터 전송 후, 타이머를 시작한다
- 타이머가 만료되었는데 ACK가 도착하지 않으면
- 해당 데이터를 재전송한다
- 타이머의 시간 길이(Timeout Interval)는 네트워크 상황에 따라 동적으로 조정될 수 있다 (ex: RTT 기반)

> TCP는 패킷마다 번호(Sequence Number)를 붙이고, 도착 확인(ACK)을 받고, 오류나 손실이 발생하면 재전송(Retransmission)하며, 일정 시간(Timeout) 내에 확인이 안 되면 다시 보내는 구조

---

<br>

## 🔵 TCP의 흐름 제어 (Flow Control)

TCP 흐름 제어는 송신자가 수신자의 처리 능력을 초과하지 않도록
데이터 전송 속도를 조절하는 메커니즘이다.

### Sliding Window란?

- 송신자가 수신자로부터 받은 수신 윈도우 크기(rwnd)만큼
  데이터를 한 번에 전송할 수 있다.
- 수신자는 매번 ACK를 보낼 때 자신이 추가로 받을 수 있는 버퍼 크기를 알려준다.
- 송신자는 이 rwnd를 보고 자신의 송신 윈도우 크기를 조절한다.

### 주요 특징

- 수신자의 버퍼 오버플로우 방지
- 연속적인 데이터 전송 가능 (ACK 기다리지 않고 윈도우 내에서 계속 전송)
- ACK을 받으면 윈도우가 슬라이딩(slide) 되어 다음 전송 준비

### 흐름

1. 수신자는 현재 가능한 버퍼 크기(rwnd)를 송신자에게 통보
2. 송신자는 rwnd를 초과하지 않도록 데이터 전송
3. 수신자가 버퍼를 비우면 ACK + 새로운 rwnd를 송신자에게 전달
4. 송신자는 윈도우를 슬라이딩하여 전송 지속

---

<br>

## 🔵 TCP의 혼잡 제어 (Congestion Control)

TCP 혼잡 제어는
네트워크 자체가 과부하되지 않도록
송신자가 전송 속도를 동적으로 조절하는 메커니즘이다.

### Slow Start (느린 시작)

![slow start](https://firebasestorage.googleapis.com/v0/b/portfolio-74c3d.appspot.com/o/CS-study%2F06-2.png?alt=media&token=13a3704b-bdac-452a-b18a-ce97263af9c8)

- 처음 연결이 열리면, TCP는 작은 전송 속도(cwnd = 1 MSS)부터 시작한다.
- 매 ACK마다 전송 윈도우(cwnd)를 2배씩 지수적으로 증가시킨다.
- 네트워크 상태를 빠르게 파악하면서 적응해간다.

```
cwnd = 1 → 2 → 4 → 8 → 16 ... (ACK마다 두 배 증가)
```

### AIMD (Additive Increase Multiplicative Decrease)

![AIMD](https://firebasestorage.googleapis.com/v0/b/portfolio-74c3d.appspot.com/o/CS-study%2F06-1.png?alt=media&token=1184dfbe-d42e-4516-817d-6ec04ab136fc)

- 혼잡이 감지되기 전까지는 1 RTT마다 cwnd를 선형(+)으로 증가시킨다 (Additive Increase)
- 혼잡(패킷 손실)이 발생하면 cwnd를 절반(×0.5)로 감소시킨다 (Multiplicative Decrease)

```
- 정상: cwnd += 1 MSS (선형 증가)
- 손실 발생: cwnd = cwnd / 2 (감소)
```

---

<br>

## 🔵 TCP 연결 과정: 3-Way Handshake와 관련 이슈

### 3-Way Handshake란?

TCP는 연결형 프로토콜로, 통신을 시작하기 전에 반드시 연결 설정 과정을 거친다. 이 연결 설정은 3-Way Handshake라는 절차를 통해 이루어진다.

3-Way Handshake는 TCP 통신을 시작할 때 송신자와 수신자가 양방향 연결을 확립하는 과정이다.

![3-Way handshake](https://firebasestorage.googleapis.com/v0/b/portfolio-74c3d.appspot.com/o/CS-study%2F06-3.png?alt=media&token=ea468fd0-c434-4627-8834-b630e09547b2)

**[step 1]**

- 클라이언트가 서버에 접속을 요청하는 SYN 패킷을 보낸다.
- 클라이언트는 SYN을 보내고 SYN/ACK 응답을 기다리는 SYNSENT 상태, 서버는 LISTEN 상태다.

**[step 2]**

- 서버는 SYN 요청을 받고 클라이언트에게 요청을 수락한다는 ACK와 SYN 패킷을 보낸다.
- 서버는 ACK을 기다리고 SYN RCVD 상태가 된다.

**[step 3]**

- 클라이언트는 ACK과 SYN 패킷을 받고 연결이 ESTAB 된다.
- 클라이언트는 SYN/ACK을 잘 받았으므로 ACK을 보낸다.
- 서버는 ACK을 받고 연결을 ESTAB 한다.

---

### ACK, SYN 정보 전달 방식

![TCP segment structure](https://firebasestorage.googleapis.com/v0/b/portfolio-74c3d.appspot.com/o/CS-study%2F06-4.png?alt=media&token=9b3d52aa-9fc2-4715-b9bb-69621fdfb942)

TCP 패킷의 헤더에 SYN, ACK, FIN 등의 플래그(flag) 비트가 존재한다.

이 플래그 비트를 설정하여 SYN 요청, ACK 응답, 연결 종료(FIN) 같은 상태 정보를 전달한다.

> 즉, SYN/ACK은 TCP 헤더 플래그를 통해 표시된다.

**3-Way Handshake 시**

1. 첫 번째 패킷: SYN 플래그만 켜짐 (S 비트 = 1)

2. 두 번째 패킷: SYN + ACK 플래그 켜짐 (S 비트, A 비트 = 1)

3. 세 번째 패킷: ACK 플래그만 켜짐 (A 비트 = 1)

| 비트    | 의미                      | 역할                             |
| ------- | ------------------------- | -------------------------------- |
| C (CWR) | Congestion Window Reduced | 혼잡 윈도우 감소 통지            |
| E (ECE) | ECN-Echo                  | 혼잡 경험 알림                   |
| U (URG) | Urgent                    | 긴급 데이터 표시                 |
| A (ACK) | Acknowledgment            | 확인 응답 존재 여부 표시         |
| P (PSH) | Push                      | 수신자가 즉시 데이터 처리 요청   |
| R (RST) | Reset                     | 연결 강제 종료 요청              |
| S (SYN) | Synchronize               | 연결 설정 요청 (초기 SYN 보내기) |
| F (FIN) | Finish                    | 연결 종료 요청                   |

---

### 2-Way Handshaking을 쓰지 않는 이유

만약 2번만 통신한다면,
서버는 클라이언트의 응답 없이도 연결이 성립됐다고 착각할 수 있다.

- 중간에 패킷이 유실될 경우
- 또는 클라이언트가 응답을 못하는 경우

서버는 **유령 연결(half-open connection)** 을 유지하게 된다.

3-Way Handshake는 송신자와 수신자 모두가 연결 의사를 명확히 확인하게 하여,
연결이 진짜 준비 완료됐는지 검증하는 역할을 한다.

---

### 두 호스트가 동시에 연결 시 연결 방법

**동시 오픈(Simultaneous Open)**

클라이언트와 서버가 동시에 SYN 을 보내고, 서로의 SYN에 대해 ACK를 보내면서 연결이 성립된다.

> 즉, 양방향으로 SYN, ACK가 교환되면서 정상적으로 3-Way Handshake가 완료된다.

---

### SYN Flooding 공격

SYN Flooding은 TCP 3-Way Handshake 과정을 악용한 DoS(Denial of Service) 공격이다.

- 공격자가 수많은 SYN 패킷을 보내고
- 서버는 SYN-ACK을 응답했지만
- 공격자는 최종 ACK를 보내지 않는다

이로 인해 서버는 대량의 미완료 연결(Half-open Connection) 을 계속 유지하게 되고,
결국 서버 자원이 고갈되어 정상 사용자 요청을 처리할 수 없게 된다.

---

### 0-RTT 기법

0-RTT(Zero Round Trip Time) 기법은 3-Way Handshake 없이 빠르게 데이터를 전송하려는 최적화 기술이다.

- 이전에 통신한 적이 있는 서버라면,
- 과거 세션 정보를 저장해두고,
- SYN 보내자마자 바로 데이터도 함께 전송한다.

즉, 연결이 완전히 성립되기 전에 미리 데이터를 보내서 시간을 절약하는 방식이다.

### 4-Way Handshake

---

<br>

## 🔵 흐름 제어와 혼잡 제어의 차이

| 항목 | 흐름 제어 (Flow Control) | 혼잡 제어 (Congestion Control) |
| ---- | ------------------------ | ------------------------------ |
| 대상 | 송신자 -> 수신자 간      | 송신자 -> 네트워크 간          |
| 목적 | 수신자 과부하 방지       | 네트워크 혼잡 방지             |
| 방법 | 수신자가 rwnd로 통제     | 송신자가 cwnd로 조절           |

- **TCP 흐름 제어**: 수신자의 버퍼 상황에 맞춰 보내는 양 조절 (Sliding Window, rwnd)
- **TCP 혼잡 제어**: 네트워크 상태에 따라 보내는 속도 조절 (Slow Start, AIMD, cwnd)

---

<br>

## 🔵 UDP가 흐름/혼잡 제어를 하지 않는 이유

UDP(User Datagram Protocol)는 **빠른 전송** 을 목적으로 설계된 프로토콜이다.
따라서 흐름 제어(Flow Control) 와 혼잡 제어(Congestion Control) 를 일부러 적용하지 않는다.

- UDP는 비연결형 프로토콜이다. (연결 설정 없이 바로 데이터 전송)
- 빠른 전송이 목표이기 때문에, 수신자 상태나 네트워크 상황을 고려하지 않는다.
- 흐름 제어, 혼잡 제어를 하게 되면 오버헤드가 생겨 전송 속도가 느려질 수 있다.
- 예를 들어, DNS, VoIP(인터넷 전화), 스트리밍 같은 서비스는 약간의 데이터 손실보다 속도가 훨씬 더 중요하다.

> 즉, UDP는 빠른 전송을 위해 수신자 버퍼 초과, 네트워크 혼잡 여부를 신경 쓰지 않는다.
> 데이터가 손실되면 상위 Application Layer가 별도로 복구할 수 있도록 한다.

---

<br>

## 🔵 DHCP

DHCP는 **IP 주소를 자동으로 할당해주는 프로토콜**이다.

클라이언트가 네트워크에 접속할 때, 수동 설정 없이도 자동으로 IP 주소, 서브넷 마스크, 게이트웨이, DNS 등을 할당받을 수 있게 해준다.

### DHCP 작동 방식: DORA 과정

1. **Discover**: 클라이언트가 네트워크에 접속하면, 브로드캐스트로 DHCP 서버를 찾는다.

2. **Offer**: DHCP 서버가 IP 주소 제안과 함께 설정 정보를 보낸다.

3. **Request**: 클라이언트가 하나의 Offer를 선택하고 요청을 보낸다.

4. **Acknowledge**: 서버가 해당 요청을 승인하고, 설정 정보를 최종 전송한다.

### DHCP의 장점

- 수동 설정 없이 자동 IP 할당 가능
- IP 주소 관리 효율화 (중복 방지, 갱신 가능)
- 모바일/유동 IP 환경에 적합

---

<br>

## 🔵 멀티플렉싱과 디멀티플렉싱(포트 번호와 TCP/UDP 연결)

멀티플렉싱과 디멀티플렉싱은 Transport Layer의 핵심 역할 중 하나로,
하나의 호스트에서 여러 애플리케이션 간의 데이터를 구분하고 전달하는 기능을 한다.

### 멀티플렉싱 (Multiplexing)

- 송신 측에서 수행
- 여러 애플리케이션의 데이터(예: HTTP, FTP 등)를 **하나의 전송 통로(IP 주소)** 로 묶어서 전송
- Transport Layer는 각각의 애플리케이션 데이터를 포트 번호로 구분하여 전송

### 디멀티플렉싱 (Demultiplexing)

- 수신 측에서 수행
- 도착한 세그먼트를 포트 번호를 기반으로 해당 애플리케이션에 정확히 전달

### 포트 번호의 역할

- IP 주소는 호스트를 식별
- 포트 번호는 호스트 내 애플리케이션 프로세스를 식별
- TCP, UDP 모두 포트 번호를 통해 각 데이터를 구분
