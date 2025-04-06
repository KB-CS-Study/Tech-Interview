# 3️⃣ CPU 스케줄링 + 인터럽트&시스템 콜


**목차**
#### 1. CPU 스케줄링 알고리즘
- [💔 선점형 스케줄링(Preemptive scheduling) vs 비선점형 스케줄링(Non-Preemptive scheduling)](#-선점형-스케줄링preemptive-scheduling-vs-비선점형-스케줄링non-preemptive-scheduling)
- [🍀 FCFS(First Come, First Served)](#-fcfsfirst-come-first-served)
- [🍀 SJF(Shortest Job First)](#-sjfshortest-job-first)
- [🍀 STCF(Shortest Time-to-Completion First)](#-stcfshortest-time-to-completion-first)
- [🍀 Priority Scheduling](#-priority-scheduling)
- [🍀 RR(Round Robin)](#-rrround-robin)
- [🍀 MLQ(Multi-Level Queue)](#-mlqmulti-level-queue)
- [🍀 정리](#-정리)

#### 2. 인터럽트 개념 및 종류
- [🧵 인터럽트 개념](#-인터럽트-개념)
- [💾 하드웨어 인터럽트](#-하드웨어-인터럽트)
- [🧠 소프트웨어 인터럽트](#-소프트웨어-인터럽트)
- [😡 인터럽트 vs 폴링](#-인터럽트-vs-폴링)

#### 3. 시스템 콜과 유저 모드 vs 커널 모드
- [👤 유저 모드(User Mode) vs 커널 모드(Kernel Mode)](#-유저-모드user-mode-vs-커널-모드kernel-mode)
- [📞 시스템 콜 (System Call)](#-시스템-콜-system-call)

<br>

## 1. CPU 스케줄링 알고리즘

Scheduling은 CPU 가상화의 핵심

![image](https://github.com/user-attachments/assets/739821ff-e284-4220-b5f5-8acd4a65960b)


**용어**

- **Workload** : CPU에서 실행되는 컴퓨팅 작업들
- **Scheduler** : 다음에 어떤 프로세스를 골라서 실행할 지 정의된 로직
- **Metric** : 성능 지표
    - Turnaround time(반환 시간)
        ![image](https://github.com/user-attachments/assets/f63a0655-360c-4b28-a1af-d2076e445315)
    - Fairness(공정성)

<br>

### 💔 선점형 스케줄링(Preemptive scheduling) vs 비선점형 스케줄링(Non-Preemptive scheduling

![image](https://github.com/user-attachments/assets/fec93215-5013-44a7-b31a-7bab9cebb895)

<br>

**선점형 스케줄링(Preemptive scheduling)** 

***⇒ “운영체제”가 CPU 사용 권한을 선점***

- 프로세스의 실행과 정지를 운영체제가 개입해서 관리
- 적합한 경우 : 현대의 거의 모든 OS

**비선점형 스케줄링(Non-Preemptive scheduling**

- 실행 중인 프로세스가 끝날 때까지 개입하지 못함
- 적합한 경우 : 임베디드 시스템

<br>

### 🧩 FCFS(First Come, First Served)

**Ready Queue에 도착한 순**서대로 CPU를 할당하는 방식

**특징**

- **비선점형 스케줄링**

**장점**

- 구현이 쉬움
- 공정성(먼저 온 순으로 실행)

**단점**

- Convoy Effect(호위 효과) : 긴 작업이 실행 중이면, 짧은 작업이 줄줄이 대기
- 평균 대기 시간 증가
  ![image](https://github.com/user-attachments/assets/a67fe24a-ee8a-4c77-9005-6f7f85d9c2de)


![짧은 작업인 B와 C가 A 작업이 끝날 때까지 계속 기다려야 함]

짧은 작업인 B와 C가 A 작업이 끝날 때까지 계속 기다려야 함

<br>

### 🧩 SJF(Shortest Job First)

**실행 시간이 가장 짧은 프로세스**에게 먼저 CPU를 할당하는 방식

**특징**

- **비선점형 스케줄링** 또는 선점형(SRTF)로 구현 가능

**장점**

- 이론적으로 평균 대기시간 최소화
- 짧은 작업 빠르게 실행 가능

**단점**

- 각 프로세스의 실행 시간 미리 파악 필요
- 모든 작업이 동시에 도착해야 최적의 알고리즘
    
    ![image](https://github.com/user-attachments/assets/d191befb-a4ef-42ae-b6db-ddda5b90d645)

    
- 긴 작업은 계속 순서가 밀림 → **기아(Starvation) 발생 가능**

<br>

### 🧩 STCF(Shortest Time-to-Completion First)

SRTF(Shortest Remaining Time First) == STCF(Shortest Time-to-Completion First)

최단 잔여시간 프로세스 우선 실행

![image](https://github.com/user-attachments/assets/ce8b7da7-8362-4b98-99d8-5f948d093a53)


**특징**

- **선점형 스케줄링**

**장점**

- 짧은 작업을 빠르게 처리
- 평균 대기시간 최소화

**단점**

- 남은 시간 예측이 어려움
- 긴 작업은 계속 순서가 밀림 → **기아(Starvation) 발생 가능**

<br>

### 🧩 Priority Scheduling

각 프로세스에 우선순위를 주고, 우선순위가 높은 프로세스부터 실행

**특징**

- **선점형, 비선점형 스케줄링 모두 가능**
- 우선순위 지정 방식 : 운영체제나 사용자가**정적으로 지정** or **프로세스의 특성과 상태에 따라 동적으로 조정**
- Ready Queue를 우선순위 순으로 정렬

**장점**

- 유연한 자원 관리 가능

**단점**

- **우선순위 낮은 프로세스가 계속 밀림** → **기아 발생**
- 해결책: **Aging** (대기 시간이 길면 우선순위 상승)

<br>

### 🧩 RR(Round Robin)

모든 프로세스에 동일한 시간을 할당 → 돌아가면서 실행

![image](https://github.com/user-attachments/assets/3f61bd4c-ae28-41c9-a71b-1936439da8e7)


**특징**

- **선점형 스케줄링**
- 일정 시간 단위로 실행, 시간이 초과되면 다음 프로세스로 넘어감

**장점**

- 응답 속도 빠름( 인터렉티브 환경에 유리)
- 공정한 CPU 배분

**단점**

- **시간 단위 설정 중요**
    - **너무 짧음** → 컨텍스트 스위칭 증가로 오버헤드 ⬆️
    - **너무 긺** → FCFS와 다를 바 없음

<br>

### 🧩 MLQ(Multi-Level Queue)

프로세스를 우선순위/성격 별로 여러 큐로 나눔, 각 큐에 다른 스케줄링 적용

![image](https://github.com/user-attachments/assets/2ad3defc-330d-46d6-a4cf-5fb683684b02)


**특징**

- 큐 간 분류 고정, 이동 불가
- EX)  큐1: 시스템 프로세스 (RR)
    
            큐2: 사용자 백그라운드 작업 (FCFS)
    

**장점**

- 다양한 작업 특성에 맞춘 스케줄링 가능
- 실시간 시스템 등에서 유용

**단점**

- **큐 간 우선순위 고정** → 한 큐에 너무 많은 프로세스가 몰리면 다른 큐는 오래 기다려야 함
- 해결책: **Aging** (대기 시간이 길면 우선순위 상승)

<br>

### 🧩 정리

![image](https://github.com/user-attachments/assets/365efe9a-4e5a-4fc2-ae09-965865886565)
![image](https://github.com/user-attachments/assets/1bf59544-738b-42e0-b312-1474f778a965)


<br>

## 2. 인터럽트 개념 및 종류

### 🪢 인터럽트 개념

CPU가 실행 중인 작업을 잠시 멈추고, 긴급한 사건을 먼저 처리하도록 해주는 메커니즘

![image](https://github.com/user-attachments/assets/3a7d8d89-a8e9-4b37-8867-6753dfb338d5)

<br>

**왜 필요한가?**

<aside>
📝

CPU는 일반적으로 **순차적인 명령어만 실행**

하지만 갑자기 **I/O 완료**, **사용자 입력**, **에러 발생** 같은 이벤트 발생 가능

👉 CPU는 현재 작업을 저장

👉 **인터럽트 서비스 루틴(ISR)**을 호출해 해당 사건을 처리한 후

👉 다시 원래 작업으로 복귀함

<br>

### 💻 하드웨어 인터럽트

**CPU 외부 장치**가 CPU에게 “지금 처리해줘!”라고 알리는 인터럽트

**예시**

- **I/O 완료** (디스크에서 데이터 읽기 끝남)
- **키보드 입력 발생**
- **타이머 인터럽트** (시간 도달: RR 스케줄링 등에서 활용)
- **전원 이상 감지**

**비동기적으로 발생**

<br>

### 🧠 소프트웨어 인터럽트

프로그램 코드 내에서 **의도적으로** 발생시키는 인터럽트

(= 명령어 기반으로 발생하는 인터럽트)

**예시**

- **시스템 콜**: 유저 프로그램이 운영체제 기능을 요청할 때
    - 예: `read()`, `write()`, `fork()` → 커널 모드 진입 시 발생
- **예외(Exception)**: 프로그램 실행 중 오류 발생 시
    - 예: 0으로 나누기, 페이지 폴트, 접근 권한 위반

**동기적으로 발생**

<br>

### 😡 인터럽트 vs 폴링

폴링 : CPU가 장치의 상태를 **주기적으로 직접 확인하면서 이벤트 발생 여부를 감지하는 방식**

![image](https://github.com/user-attachments/assets/281b3019-f052-49c3-a8e0-b493058882a9)

<br>

📦 **택배 기다릴 때 비유:**

- **폴링**: 내가 계속 문을 열고 **“왔나?” “아직도 안 왔나?”**
    
    → 반복해서 확인하는 것
    
- **인터럽트**: 택배가 오면 **직접 문을 두드려서 알려주는 것**

<br>

## 3. 시스템 콜과 유저 모드 vs 커널 모드

### 🌓 유저 모드(User Mode) vs 커널 모드(Kernel Mode)

![image](https://github.com/user-attachments/assets/31a67d5b-3c01-4ed1-ac8c-45168e6f4c4e)


**💨 유저 모드**

- 일반 사용자 프로그램을 실행
- 하드웨어에 직접 접근 불가능
- 시스템 콜을 통해 커널 모드로 진입

**💨 커널 모드**

- 모든 CPU 명령 실행 가능
- 모든 시스템 메모리 등 하드웨어에 직접 접근
- 작업 완료 후 다시 User Mode로 전환

![image](https://github.com/user-attachments/assets/982b6d20-5288-4e28-a374-e17cc3c2935c)

<br>

### 🌓 시스템 콜 (System Call)

유저 프로그램이 운영체제의 기능을 요청할 수 있게 해주는 인터페이스

![image](https://github.com/user-attachments/assets/5d760185-971c-4947-86bf-0e3acde31122)


❓ **왜 필요할까** ❓

- 사용자 프로그램이 **직접 커널 메모리나 디바이스 접근**하면 시스템이 위험
- 그래서 **운영체제에게 우회 요청**하는 방식 필요 → 이게 시스템 콜!

**시스템 콜 종류**
![image](https://github.com/user-attachments/assets/ad9546a3-e898-4ed9-9505-85249340e350)




