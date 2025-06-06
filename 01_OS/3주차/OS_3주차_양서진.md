# `CPU 스케줄링 알고리즘 (FCFS, SJF, Priority, RR, MLQ, MLFQ)`

## Scheduling Algorithm Goals

운영체제에서 **스케줄링 알고리즘**은 CPU를 어떤 프로세스에게 언제 할당할지를 결정하는 규칙

---

## 1. 시스템의 목표는 워크로드에 따라 달라질 수 있음

- 각 시스템은 **서로 다른 환경과 요구**를 가지고 있다.
- 예를 들어:
    - *배치 시스템(batch system)**은 여러 작업을 빠르게 처리하는 게 목표일 수 있고,
    - *인터랙티브 시스템(interactive system)**은 사용자의 요청에 빠르게 반응하는 게 중요할 수 있음.

따라서, **스케줄링 알고리즘의 설계 목표도 상황에 따라 달라진다.**

---

## 2. 보편적으로 중요한 스케줄링 목표 (Commonly)

### No starvation (기아 현상 방지)

- starvation란: 어떤 프로세스가 **계속 CPU를 배정받지 못하고 기다리는 상태**가 되는 것
- 스케줄러는 이러한 일이 발생하지 않도록 **모든 프로세스가 언젠가는 CPU를 받을 수 있도록 해야 한다.**

---

### Fairness (공정성)

- 모든 프로세스가 **CPU를 일정하게, 공정하게** 사용할 수 있어야 해.
- 어떤 프로세스는 계속 CPU를 받는데 어떤 프로세스는 거의 못 받는다면 **공정하지 않음**
- 공정성은 특히 사용자 환경에서 중요해 — 누가 먼저 왔든, 얼마나 오래 기다렸든, 균형 있게 자원을 분배해야 하니까.

---

### Balance (균형)

- 시스템의 모든 부분(CPU, 디스크, 메모리 등)이 **고르게 바쁘게 돌아가야 한다**
- 예를 들어 CPU만 계속 일하고 디스크나 네트워크는 놀고 있으면 자원 활용이 비효율적
- 그래서 스케줄러는 전체 시스템의 리소스를 균형 있게 사용하는 데 도움을 줘야 한다.

---

## 운영체제의 스케줄링 평가 기준(Scheduling Criteria)

### CPU utilization

- CPU 사용량, 스케줄링을 통해 CPU를 얼마나 바쁘게 굴리느냐

높을수록 좋다.

### Throughput

- 단위 시간 동안 완료된 프로세스의 수,

 높을수록 좋다.

### Turnaround time

- 프로세스가 도착해서 종료될 때까지 걸린 전체 시간

포함 요소 → 대기 시간 + 실행 시간 + I/O 시간 등 전체 시간

낮을수록 좋다.

### Waiting time

- Ready Queue에서 기다린 시간
- CPU를 할당받기 전까지 **대기열에서 기다린 시간의 총합**

낮을수록 좋다.

### Response time

- 프로세스가 도착해서, 처음으로 CPU를 받기까지의 시간
- **완료 시간이 아님!**

짧을수록 좋다

## FCFS/FIFO (First-Come, First-Served)

## 개념

> 💡 먼저 도착한 작업부터 먼저 처리하는 방식
> 
- FIFO(First In First Out)라고도 불림
- **현실 세계에서 줄 서는 방식과 유사**
    
    예: 캐셔 앞에 줄 선 손님, 은행 대기 순서 , 키오스크
    
- 스케줄링 방식이 간단하고 직관적

---

## 특징

| 항목 | 설명 |
| --- | --- |
| 처리 순서 | **도착 순서대로 작업 처리** (`scheduled in order that they arrive`) |
| 비선점형 (non-preemptive) | 작업이 시작되면 끝날 때까지 다른 작업은 끼어들 수 없음 |
| 기아 없음 | 도착 순서대로 처리되기 때문에 **starvation(기아 현상)** 없음 |
| 공평함 | 모든 작업이 동일하게 다뤄짐 (`jobs are treated equally`) |

---

## 문제점 : Convoy Effect

- 긴 작업이 먼저 실행되면, 뒤에 짧은 작업들도 오래 기다리는 현상

<img width="441" alt="image1" src="https://github.com/user-attachments/assets/050206d4-22e2-4d72-a7f1-3ca44bc6405f" />


예: 트럭이 앞에서 느리게 가면 뒤 차량들도 전부 느려짐

CPU 스케줄링에서도 **작은 작업들이 큰 작업 때문에 밀려서 기다리는 상황**

### 평균 Turnaround Time 증가

- **짧은 작업이 큰 작업 뒤에 오면 전체 평균 완료 시간(반환 시간)이 커짐**

### I/O 효율이 떨어짐

- 긴 작업이 CPU를 오래 잡고 있어서
    
    **I/O 처리가 필요한 다른 작업들과 CPU/I/O 병행이 잘 안 됨**
    

---

<img width="863" alt="image2" src="https://github.com/user-attachments/assets/a8c5cfb7-eb2f-467e-a5fc-a17f47077c94" />


### Recall

waiting → 와서 계산이 시작될 때 까지의 시간 

turnaround → 줄을 서기 시작한 다음부터 계산 완료되어 나갈때 까지의 시간

---

## Preemptive Scheduling

### Non-preemptive scheduling

> process가 리소스를 다 쓰고 반납 전까지 누구에게도 넘겨주지 않는다.
> 

**The scheduler waits for the running job to voluntarily yield the CPU**

→ **스케줄러는 현재 실행 중인 작업이 자발적으로 CPU를 넘겨줄 때까지 기다린다**

**Jobs should be cooperative**

→ **작업들은 협조적(cooperative)이어야 한다**

- 즉, **자기 차례가 끝났거나 CPU가 필요 없을 때 스스로 양보해야 한다**

### Preemptive scheduling

> Resource를 사용 중간에 급한 프로세스가 가져가서 사용한다.
> 

**The scheduler can interrupt a job and force a context switch**

→ **스케줄러는 작업을 강제로 중단시키고 문맥 교환(context switch)을 수행할 수 있다**

### **If a process is preempted in the middle of updating shared data?**

→ **프로세스가 공유 데이터를 수정하던 도중에 선점되면 어떻게 될까?**

중간에 context switch가 일어나면, **수정이 완전히 끝나지 않은 상태에서 
다른 프로세스가 같은 데이터를 건드릴 수 있음**

→ **race condition (경쟁 상태)** 

→ **동기화가 안 되어 데이터 무결성이 깨질 수 있음**

### **If a process in a system call is preempted?**

→ **프로세스가 시스템 호출 중일 때 선점되면 어떻게 될까?**

이러한 상황들을 방지하기 위해 

- **동기화 도구 사용** (lock, semaphore 등)
- **시스템 콜 중에는 선점 방지** (critical section 보호)
- **커널 수준에서 atomic operation 제공**

후에 언급

---

## SJF (Shortest Job First)

### **Choose the job with the smallest expected CPU burst**

→ **예상되는 CPU 실행 시간이 가장 짧은 작업을 선택한다**

### **Guarantee the minimum average waiting time**

→ **평균 대기 시간을 최소화하는 것을 보장한다**

### **Non-preemptive**

→ **비선점형**이다

- 한 번 실행되기 시작한 작업은 **중간에 끊을 수 없음**
- 예: 짧은 작업이 나중에 도착해도, 현재 실행 중인 작업은 끝까지 감

### **Priority scheduling**

→ **SJF는 일종의 우선순위 스케줄링이다**

<img width="595" alt="image3" src="https://github.com/user-attachments/assets/4835f000-55c7-426c-ae55-73430081b6ed" />


## 그러나 각 프로세스의 처리시간을 알기 힘들다.

- **Non-preemptive 방식이기에 starve의 가능성이 존재한다.**
- future CPU burst의 size를 아는건 불가능하다.

## STCF(Shortest Time-to-Completion First)

- SJF의 Preemptive version
- STCF는 Policy 이다.

**If a new job arrives with the run time less than the remaining time of the current job, preempt it**

→ 현재 실행 중인 작업보다 **짧은 실행 시간의 새 작업이 도착하면**, **현재 작업을 중단(preempt)하고 새 작업을 실행하라**

<img width="749" alt="image4" src="https://github.com/user-attachments/assets/6bf1d43e-3086-484d-be28-4e70c077216a" />


- 프로세스의 입장 타이밍 유의

# Policy vs Mechanism 정리

| 개념 | 설명 | 질문으로 확인 |
| --- | --- | --- |
| **Policy (정책, 전략)** | **“무엇을 할 것인가”에 대한 결정 기준** | 어떤 작업을 먼저 실행할까? 어떤 순서로 처리할까? |
| **Mechanism (기법, 방법)** | **“어떻게 실행할 것인가”에 대한 기술적 방법** | 실제로 그 결정을 어떻게 실행에 옮길까? |

---

## Policy는 **원칙**

> OS가 무엇을 우선으로 처리할지 결정하는 규칙
> 
- "가장 짧은 작업 먼저 실행하자"
- "선착순으로 처리하자"
- "우선순위 높은 프로세스 먼저 처리하자"

→ *어떤 작업을 먼저 고를지* **기준을 세우는 단계**

---

## Mechanism은 **실행 방법**

> 그 정책(Policy)을 실제로 동작하게 만드는 도구와 방법
> 
- context switch (문맥 교환)
- 인터럽트 처리 (timer interrupt)
- ready queue 관리
- 스케줄러 구현 코드

→ *작업을 고른 뒤, 그 작업을 실제로 어떻게 CPU에 태울지*

---

## 운영체제에선 왜 구분이 중요할까?

- 운영체제는 **정책(Policy)**과 **구현(Mechanism)**을 **분리함으로써 유연성**을 얻는다.

예를 들어:

- 정책은 바뀔 수 있어 (예: FCFS → SJF)
- 하지만 메커니즘은 그대로 재사용 가능 (예: context switch, interrupt)

---

## RR Round Robin 스케줄링

순환형(FIFO 기반) 큐에서 돌아가며 작업을 할당하는 방식

실행 큐(run queue)는 **원형 FIFO 큐**처럼 다뤄진다

(먼저 들어온 순서대로, 맨 뒤로 다시 돌아가는 구조)

각 작업은 **타임 슬라이스(time slice)**, 즉 **일정 시간만 CPU를 쓸 수 있는 실행 시간 할당량**을 부여받는다.

### **Usually 10–100ms (will be discussed later)**

→ 일반적으로 **10~100밀리초(ms)** 사이의 타임 슬라이스가 사용된다

각 작업은 **정해진 시간만 실행된 후**, **실행 큐에 있는 다음 작업으로 전환된다**

라운드 로빈은 특히 응답 속도(response time)가 중요한 시스템에서 효과적임

<img width="532" alt="image5" src="https://github.com/user-attachments/assets/bcc72909-048d-4fbb-a389-3dea5ae1bacb" />


---

## Priority Scheduling

각 작업(job)은 **우선순위(priority)**를 가진다.

CPU는 **가장 높은 우선순위**를 가진 작업에게 할당된다.

Can be preemptive or non-preemptive

<img width="485" alt="image6" src="https://github.com/user-attachments/assets/0fd32198-da65-4f82-a4ac-913b32b407fc" />


Burst time → 실제 사용 시간

### Starvation problem

**낮은 우선순위의 프로세스는**, **높은 우선순위 작업이 계속해서 들어오면 절대로 실행되지 못할 수 있다**

- 즉, CPU는 계속 높은 우선순위 작업에게만 할당되므로 **낮은 우선순위 작업은 무한정 대기**하게 됨

### Solution: Aging or priority boosting

### **Increase the priority of the process as it gets aged**

→ **시간이 지나면서 오래 기다린 프로세스의 우선순위를 점점 높여주는 방식**

- 이렇게 하면 결국에는 낮은 우선순위 프로세스도 실행될 수 있게 된다.
    
    → **기아 현상 방지**
    

---

## Multilevel Queue Scheduling (MLQ)

## **There are different types of jobs in the system**

→ 시스템에는 **서로 다른 종류의 작업들**이 존재한다

- 인터랙티브 작업 (사용자 입력 기다림)
- 배치 작업 (백그라운드에서 자동 처리됨)
- 시스템 프로세스 등

ready queue를 **여러 개의 독립된 큐**로 나눈다

예시:

- Foreground queue (전경 큐 → 사용자 중심 작업)
- Background queue (배경 큐 → 백그라운드 작업)

각 작업을 **그 특성에 따라 적절한 큐에 배정**한다

- 인터랙티브 작업 → **Foreground queue**
- 배치(Batch) 작업 → **Background queue**

Each queue has its own scheduling algorithm

즉, **작업의 목적과 성격에 따라 큐를 분류** 

<img width="580" alt="image7" src="https://github.com/user-attachments/assets/1ddfc496-bec7-403a-9b2f-2645fa240f06" />


> 여러 개의 큐가 존재할 때,
> 
> 
> **각 큐 사이에서도 CPU를 어떻게 나눌지 결정하는 스케줄링이 필요하다**
> 

## **Fixed priority scheduling**

→ **고정 우선순위 스케줄링**

### **I.e., serve all from foreground then from background**

→ 예를 들어, **포그라운드 큐의 작업을 모두 처리한 다음**

**백그라운드 큐의 작업을 처리하는 방식**

- 즉, **포그라운드 큐가 우선순위가 더 높고**,
    
    그게 끝나기 전에는 **백그라운드는 실행되지 않음**
    
    **기아(starvation) 현상이 발생할 수 있음**
    
    - **백그라운드 큐는 계속 기다리게 되고**,
        
        포그라운드 작업이 계속 들어오면 **실행 기회가 없음**
        

## **Time slice**

→ **시간 분할 방식 (Time-slice scheduling between queues)**

### **Each queue gets a certain amount of CPU time which it can schedule amongst its processes**

→ 각 큐는 **일정한 비율의 CPU 시간을 배정받고**, **그 시간 안에서 자기 큐의 작업들을 처리**한다

- **공정함**, starvation 방지

---

**작업들의 실행 시간(workload)을 미리 예측하기는 어렵다**

- 현실에서는 어떤 작업이 짧을지, 길지를 **정확히 예측하기 힘들기 때문에**
- 고정된 우선순위만으로는 효율적인 스케줄링이 어려움

→ 이 문제를 해결하기 위해 등장한 것이 **MLFQ**

## MLFQ (Multilevel feedback queue)

- 우선순위 큐를 여러 개 두고, **실행 결과에 따라 프로세스를 이동시키는 방식**

**각 우선순위 수준에 따라 여러 개의 큐를 나눠서 관리**한다

**큐 간에는 우선순위 스케줄링** 을 사용하고, **같은 큐 안에서는 Round Robin 방식으로 실행**

하나의 프로세스는 상황에 따라 **큐 간을 이동할 수 있다**

- 예: 너무 오래 CPU를 점유하면 낮은 큐로 **강등(demote)**
- 오래 기다리면 다시 높은 큐로 **승격(promote)**

## Parameters (MLFQ에서 조정 가능한 요소들)

| 항목 | 설명 |
| --- | --- |
| **Number of queues** | 몇 개의 큐를 둘지 |
| **Scheduling algorithm for each queue** | 각 큐에 어떤 스케줄링을 적용할지 (예: RR, FCFS 등) |
| **When to promote/demote** | **언제** 우선순위를 올릴지/내릴지 결정하는 기준 |
| **Which queue to enter** | 프로세스가 처음 생성될 때 어떤 큐에 들어갈지 결정하는 기준 |

<img width="584" alt="image8" src="https://github.com/user-attachments/assets/aa9aed11-3bf2-4503-9289-1438b5dc99be" />


**3단계의 큐를 사용한 MLFQ 구조**

1. **Q0** – RR (Round Robin), **타임 퀀텀 8ms**
    - → 가장 빠르게 반응해야 하는 작업들이 머무는 곳
2. **Q1** – RR, **타임 퀀텀 16ms**
    - → Q0에서 완료되지 않은 작업이 내려오는 큐
3. **Q2** – **FCFS (First Come First Served)**
    - → 마지막 큐. 오래 걸리는 작업이 도착하는 곳

- 프로세스는 **CPU를 오래 점유할수록** → **우선순위가 낮은 큐로 이동**
- 반대로, **빨리 끝나면 높은 큐에 남아 빠른 응답 가능**
- → 즉, MLFQ는 **실행 시간 예측이 어려운 현실 문제를 해결하는 전략적 구조**!

# ›`인터럽트(Interrupt) 개념 및 종류`

### 인터럽트

→ 프로그램을 실행하는 도중에 예기치 않은 상황이 발생할 경우 현재 실행중인 작업을 중단하고 발생된 상황을 처리한 후 다시 실행중인 작업으로 복귀 

### **Device controller notifies CPU of events by raising an interrupt**

→ 장치 컨트롤러(Device Controller)는 어떤 사건이 발생하면 **인터럽트를 발생시켜 CPU에 알린다**

- 입출력 장치에서 요청이 도착하거나
- 작업을 완료했을 때 등

→ 인터럽트 요청 라인(Interrupt Request Line)에 신호를 보냄

### **CPU detects interrupts and processes accordingly**

→ CPU는 **인터럽트를 감지한 후**, **알맞은 처리 루틴**을 실행함

<img width="425" alt="image9" src="https://github.com/user-attachments/assets/1d763c72-27bc-442f-801a-acf2f9ff6246" />


1. CPU에서 명령어를 순차적으로 실행하는 중

2. 특정 장치에서 예상치 못한 상황 발생

3. 해당 장치에서**인터럽트 요청 라인(Interrupt Request Line)**으로 신호를 보냄

4. CPU가 인터럽트 요청 라인을 통해 인터럽트 감지

5.**인터럽트 백터**를 통해 **인터럽트 서비스 루틴(ISR)**의 주소를 찾음

6. 찾은 인터럽트 서비스 루틴으로 점프

7. 인터럽트 서비스 루틴을 수행 후

**복귀**

## 인터럽트 백터

단순히 마우스 클릭이나 키보드 입력 등등을 생각해봐도 꽤 많이 일어난다고 느낄 수 있습니다.

그렇기 때문에 인터럽트가 발생했을 때 인터럽트 서비스 루틴(ISR)을 빠르게 찾아서 처리해줘야 합니다.

ISR을 빠르게 찾기 위해서 ISR의 시작 주소들이 담긴 배열을 메모리에 만들어 놓습니다.

그리고 인터럽트 요청 라인으로 온 인터럽트 번호를 읽고, 이 번호를 사용해서 배열에 접근해서 ISR의 시작 주소로 점프

## 외부 인터럽트 (하드웨어 인터럽트)

입출력 장치, 타이밍 장치, 전원 등의 외부적인 요인에 의해서 발생하는 인터럽트를 의미합니다.

- 전원 이상 인터럽트: 정전이나 전원이 이상이 있는 경우
- 기계 고장 인터럽트: CPU등의 기능적인 동작 오류가 발생한 경우
- 입출력 인터럽트(I/O Interrupt): 입출력의 종료 등의 이유로 CPU의 수행을 요청하는 경우
- 외부 인터럽트 : 외부장치로부터 인터럽트 요청이 있는 경우(ex - 타이머 인터럽트, 키보드 인터럽트)

앞서 살펴본 예시들과 마찬가지로 하드웨어 장치가 인터럽트 요청 라인에 신호를 보내면서 처리됩니다.

## 내부 인터럽트 (소프트웨어 인터럽트, 트랩)

프로세스(실행중인 명령어) 내부에서 잘못된 명령이나 데이터를 사용할 때 발생하는 인터럽트를 의미합니다.

- 숫자를 0으로 나누는 경우
- 잘못된 메모리에 접근하는 경우
- 메모리가 넘치는 경우
- 시스템 콜을 호출하는 경우

내부 인터럽트의 경우 하드웨어가 아닌 소프트웨어가 인터럽트 요청 라인에 직접 신호를 보냅니다.

즉, 실행 중인 프로그램이 스스로 인터럽트를 발생시킨다고 보면 됩니다.

# `시스템 콜(System Call)과 사용자 모드 vs 커널 모드`

### 시스템 콜

→ 운영체제가 제공하는 **서비스에 접근하기 위한 프로그래밍 인터페이스**

시스템 콜은 보통 **고급 언어(C, C++)를 통해 사용 가능**하다

- 직접 OS 코드를 건드리지 않아도,
- C 언어로 `open()`, `read()` 같은 함수 호출하면
- 내부적으로 **시스템 콜로 연결됨**

대부분의 경우, 프로그램은 **직접 시스템 콜을 호출하지 않고 고수준 API(Application Programming Interface)를 통해 간접적으로 사용**

한다

## **Most common APIs**

→ 대표적인 API들:

### ✅ **POSIX API (유닉스 계열)**

- POSIX = **Portable Operating System Interface**
- UNIX, Linux, macOS 계열 대부분에서 사용
- 표준화된 시스템 콜 인터페이스 제공

### ✅ **Win32 API (윈도우 계열)**

- Windows 운영체제 전용 시스템 API

### ✅ **Java API (JVM 기반)**

- 자바 프로그램은 JVM 위에서 동작
- 직접 시스템 콜을 호출하진 않지만,
    
    **JVM이 내부적으로 OS 시스템 콜을 호출**
    

---

## Multi-mode CPU

CPU는 두가지 모드가 존재한다.

- user mode
- kernel mode

Privileged instruction은 커널 모드에서만 실행이 가능함

- 일반 애플리케이션이 유저모드에서 privileged instruction을 실행하려고 하면 예외가 발생할 수 있다.

이러한 구조 덕분에 **운영체제(OS)는 자신과 시스템의 다른 핵심 구성 요소들을 보호**할 수 있다.

- 즉, 중요한 작업(critical things)과 일반 작업(general things)을 분리해서 보호한다.

**최신 CPU**들은 **2개 이상의 모드(멀티 모드)를 지원**

## 유저모드에서 커널 모드로 전환하기

- **By interrupt**
    - **보통 하드웨어에 의해 발생한다.**
    - 비동기적(Asynchronous)이다. → 언제든지 발생할 수 있다.
    - 인터럽트가 발생하면, **인터럽트 핸들러로 넘어가기 전에 커널 모드로 전환된다.**
- **By system call**
    - 동기적(Synchronous)
    - **보통 소프트웨어에 의해 발생**

✅ CPU는 인터럽트가 발생하면 → **커널 모드로 전환** → **인터럽트 핸들러 실행**

✅ 처리 끝나면 → **다시 User Mode로 복귀**해서 프로그램 계속 실행

<img width="564" alt="image10" src="https://github.com/user-attachments/assets/27967aea-f6d6-477a-8b36-baef75b77649" />


| 모드 | 설명 |
| --- | --- |
| **User Mode** | 일반 애플리케이션이 실행되는 상태. OS의 중요한 자원에 직접 접근 불가 |
| **Kernel Mode** | 운영체제 핵심 기능이 실행되는 상태. 하드웨어 접근, 메모리 제어 가능  |

### `trap → mode bit = 0`

→ CPU가 **트랩(trap)**이라는 특별한 명령을 통해

**모드 비트를 0으로 바꿔 커널 모드로 진입**

- 즉, **User Mode → Kernel Mode**로 전환됨

### `return → mode bit = 1`

→ 작업이 끝나면 CPU는 다시 모드 비트를 1로 바꿔

**User Mode로 복귀**

- 보안(Security): 사용자 코드가 직접 하드웨어 자원에 접근하지 못하게 **제어**
- 안정성(Stability): 시스템 자원을 무분별하게 쓰지 못하도록 **감시**
- 신뢰성(Reliability): 잘못된 사용자 프로그램이 시스템 전체를 망가뜨리는 걸 방지
