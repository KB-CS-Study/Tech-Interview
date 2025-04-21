# OSI 7계층 & TCP/IP 모델

## TCP/IP 4계층

장치들이 인터넷 상에서 데이터를 주고받을 때 쓰는 독립적인 프로토콜의 집합

### 애플리케이션 계층

HTTP, SMTP, SSH, FTP가 대표적이며 웹 서비스, 이메일 등 서비스를 실질적으로 사람들에게 제공하는 층

- HTTP (Hypertext Transfer Protocol) : 처음에는 서버와 브라우저간에 데이터를 주고 받기
  위해 설계된 프로토콜이다. 지금은 브라우저 뿐만 아니라 서버와 서버간의 통신할 때도
  많이 이용한다. 헤더를 통한 확장이 쉬우며, 동일한 연결에서 연속적으로 수행되는 두 요청 사이에 연속적인 상태(state)값은 없다.
  ![stateless](https://velog.velcdn.com/images/leesfact/post/dbe7a5d3-a11c-4837-85d9-bdf4ec55b86b/image.png)
- SSH (Secure Shell Protocol) : 보안되지 않은 네트워크에서 네트워크 서비스를 안전하게
  운영하기 위한 암호화 네트워크 프로토콜 이다.
- FTP (File Transfer Protocol) : 노드와 노드간의 파일을 전송하는데 사용되는
  프로토콜이다. 지금은 파일을 암호화해서 전송하는 FTPS 또는 SFTP로 대체되고 있다.
  ![파일질라](https://velog.velcdn.com/images/leesfact/post/9f19a152-aaaf-403d-8464-cc0474f817bd/image.png)
- SMTP (Simple Mail Transfer Protocol) : 인터넷을 통해 메일을 보낼 때 사용되는 프로토콜이다.

### 전송 계층

TCP, UDP가 대표적이며 애플리케이션계층에서 받은 메시지를 기반으로 세그먼트 또는 데이터그램으로
데이터를 쪼개고 데이터가 오류없이 순서대로 전달되도록 도움을 주는 층

- TCP
    - 가상회선 패킷 교환 방식
      ![가상회선 패킷 교환 방식](https://velog.velcdn.com/images/leesfact/post/8adfe070-6636-470a-aa97-c586d9515b26/image.png)
    - 오류검사 메커니즘
        1. 재전송: 시간 초과 기간이 지나면 서버는 전달되지 않은 데이터에 대해 재전송을 시도한다.
        2. 체크섬 : 체크섬을 통해 무결성을 평가한다. 즉, 송신된 데이터의 체크섬과 수신된
           데이터의 체크섬 값을 비교해서 올바르게 왔는지를 확인
    - 헤더 : 20 ~ 60바이트로 가변적


- UDP
    - 데이터그램 패킷 교환 방식
      ![데이터그램 패킷 교환 방식](https://velog.velcdn.com/images/leesfact/post/6e35c055-eed2-4b8c-9c20-3eecb0d39026/image.png)
    - 오류검사는 단순한 체크섬만 지원
    - 헤더 : 8바이트로 고정길이

### 인터넷 계층

IP, ICMP, ARP가 대표적이며 한 노드에서 다른 노드로 전송 계층에서 받은 세그먼트 또는 데이터그램을
패킷화 하여 목적지로 전송하는 역할을 담당

- ICMP (Internet Control Message Protocol) : 노드와 노드 사이에서 통신이 잘되나를
  확인할 때 쓰는 프로토콜이다. 이는 데이터를 교환하는데 사용되지 않는 프로토콜이다. 일반적으로 이 프로토콜은 테스팅에 사용된다. IP와는 달리 TCP 또는 UDP 와 같은 전송
  계층 프로토콜과 연관되지 않고 독립적인 비연결형 프로토콜이다.
  ![ICMP](https://velog.velcdn.com/images/leesfact/post/24a30b9e-0835-4df4-8dae-ac9932bbe02a/image.png)

### 링크 계층

전선, 광섬유, 무선 등으로 데이터가 네트워크를 통해 물리적으로 전송되는 방식을 정의.
데이터링크계층과 물리계층을 합친 계층

<hr />

## 캡슐화와 비캡슐화

### 캡슐화 (encapsulation)

송신자가 수신자에게 데이터를 보낼 때 데이터가 각 계층을 지나며 각 계층의 특징들이 담긴 헤더들이 붙여지는 과정

### 비캡슐화 (decapsulation)

캡슐화의 역과정. 수신자측에서 캡슐화된 데이터를 역순으로 제거하면서 응용계층까지 도달하는 것

<hr />

## PDU (protocol data unit)

각 계층의 데이터 단위

- 애플리케이션 계층 : 메시지
- 전송 계층 : 세그먼트(TCP), 데이터그램(UDP)
- 인터넷 계층 : 패킷
- 링크 계층 : 프레임(데이터 링크 계층), 비트(물리 계층)

<hr />

## CRC와 체크섬 트레일러

데이터의 오류감지를 위한 수학적 함수가 적용된 값들이 있는 필드.
링크의 오류(과도한 트래픽 등) 로 인해 데이터 손상을 감지하는 역할을 한다.
이 과정에서 CRC와 체크섬 두가지의 과정을 기반으로 데이터 전송오류 및 데이터무결성을 방지하게 된다.

- CRC : CRC-1, CRC-16 등의 알고리즘으로 나온 값을 통해 데이터 전송오류감지를 수행한다.
- 체크섬 : MD5, SHA-256 등의 알고리즘으로 나온 값을 통해 데이터 무결성을 방지한다.

<hr />

## OSI 7계층

TCP / IP 4계층은 OSI 7계층 모델로 설명하기도 한다.

TCP/IP 계층과 달리 OSI 계층은 애플리케이션 계층을 세 개로 쪼개고 링크 계층을 데이터
링크 계층, 물리 계층으로 나눠서 표현하는 것이 다르며 인터넷 계층을 네트워크 계층으로
부른다는 점이 다르다.

![계층](https://velog.velcdn.com/images/leesfact/post/c18db20c-576b-49c4-990d-6732f9daed32/image.png)

<hr />

## MTU (Maximum Transmission Unit)

네트워크에 연결된 장치가 받아들일 수 있는 최대 데이터 패킷의 크기.
이크기를 기준으로 데이터는 쪼개져서 패킷화된다.

네트워크 경로 상에 있는 아무 장치나 MTU보다 패킷이 크면 그 패킷은 분할될 수도 있다.

![분할](https://velog.velcdn.com/images/leesfact/post/00789461-7e93-45f5-ab53-7fa8878c50e6/image.png)

Router C의 MTU가 1400바이트이기 때문에 패킷이 분할되는 모습.

### 패킷이 분할되지 않는 경우

패킷을 분할할 수 없어 네트워크 경로 상에 있는 어떠한 라우터나 장치의 MTU를 초과할 때
분할해서 전달하는 것이 아니라 전달을 아예 하지 않을 수도 있다.

- IPv6 : 분할을 허용하지 않는다.
- IPv4 : IPv4 헤더에는 flags라는 필드가 있는데 여기서 bit 이 1이되면 "Don't Fragment" 플래그가
  활성화된다라는 의미다. 이 때 분할은 불가능하다.

## MTU와 MSS

MTU와 MSS의 차이를 보면 다음과 같이 MTU는 IP헤더와 TCP헤더의 크기까지 합치지만
MSS(Maximum Segment Size)는 데이터의 크기(payload의 크기)만을 가리킨다.\

![MSS](https://velog.velcdn.com/images/leesfact/post/98780594-5918-4421-b892-1e7baa4adb2b/image.png)

일반적으로 MTU는 1500바이트이며 MSS는 1460바이트다.
따라서 네트워크를 통해 데이터를 보낼 때 MTU가 1500이라도
데이터는 보통 1460바이트 이하의 크기로 보내야 전달이 된다.

## PMTUD (Path MTU Discovery)

수신자와 송신자의 경로 상에서 장치가 패킷을 누락한 경우 테스트 패킷의 크기를 낮추면서
MTU에 맞게끔 반복해서 보내는 과정
