## `📌 CPU 스케줄링 알고리즘 (FCFS, SJF, Priority, RR, MLQ, MLFQ)`



📂 **[개발자가 알아야하는 이유]**

- 서버/프로그램 성능 최적화
- OS프로세스 관리 이해

### 1. CPU의 역할

> 중앙 처리 장치(CPU)는 서버의 핵심 계산 장치인 하드웨어 구성 요소
>
- CPU는 동시에 하나의 프로세스만 처리 가능
- 다중 프로그램 실행시, CPU가 빠르게 작업 전환(Context Switching)

### 2. Context Switching

> CPU가 작업을 바꿀 때마다 수행하는 작업 전환과정
>
- 현재 작업 상태 저장(PCB, Process Control Block)
- 이 과정을 통해 CPU는 **마치 동시에 여러 작업을 처리하는 것처럼** 보이게 됨 (시분할)
- **단점** : Overhead(비용) 발생해 효율(성능) 떨어진다.
    - Context Switching 시 해당 CPU는 아무런 일을 하지 못하기 때문
- **`Context Switching`이 발생하는 이유**
    - **멀티태스킹** 환경에서 CPU는 한 번에 하나의 프로세스만 실행 가능
    - 하지만 여러 프로세스를 번갈아 가며 빠르게 실행시켜 사용자에게 “동시 실행”처럼 보이게 함
    - CPU는 주기적으로 또는 이벤트 발생 시 **작업을 전환**해야 함 → 이때 `Context Switching` 발생
- `Context Switching` **수행 과정**
    - 프로그램 실행 -> 프로세스 생성 -> 프로세스 주소 공간에 (코드, 데이터, 스택) 생성 -> 이 프로세스의 메타데이터들이 PCB에 저장

### 3. CPU Scheduling 개념

> 어떤 프로세스에게 CPU를 할당할 것인지 결정하는 것


![img.png](..%2F..%2F..%2F..%2FDocuments%2F%EC%8A%A4%ED%81%AC%EB%A6%B0%EC%83%B7%2F1.png)
- CPU 스케줄링이 작동하는 순간
    - **ready → running**: CPU 스케줄러가 선택
    - **running → ready**: 선점형 스케줄링이 발생 (인터럽트)
    - **waiting → ready**: I/O 끝나고 CPU 할당을 위해 ready 큐로 복귀 → 스케줄러 작동
- CPU 유후 시간, 응답 시간, 반환 시간 최소화를 위해 CPU Scheduling이 필요함

```
[ ready 큐 ]
  ↓  (스케줄러 dispatch 결정)
[ running ]
  ↓ (I/O or event wait → waiting)
  ↓ (interrupt → ready로 복귀)
  ↓ (exit → terminated)

[ waiting ] (I/O 완료) → ready
```

> **CPU → I/O → CPU → I/O**를 반복하면서 실행 (**CPU–I/O Burst Cycle)**
>
![2.png](..%2F..%2F..%2F..%2FDocuments%2F%EC%8A%A4%ED%81%AC%EB%A6%B0%EC%83%B7%2F2.png)
- CPU Brust : CPU가 작업을 처리하는 시간
- I/O Brust : I/O 작업을 기다리는 시간

| **구분** | **CPU Burst** | **I/O Burst** |
| --- | --- | --- |
| 의미 | CPU 가 연산하는 시간 | I/O 장치의 작업 완료를 기다리는 시간 |
| 예시 | 연산 수행, 변수 계산, 루프 | 파일 읽기/쓰기, 네트워크 요청 등 |
| 특징 | CPU 집중적 | I/O 대기 시간 |
| 특징 2 | 프로세스가 CPU 를 점유 | CPU 는 쉬고 I/O 장치가 동작 |

> **Histogram of CPU-burst Times**
>

![3.png](..%2F..%2F..%2F..%2FDocuments%2F%EC%8A%A4%ED%81%AC%EB%A6%B0%EC%83%B7%2F3.png)

### 4. 비선점 스케줄링

> 작업이 스스로 CPU 를 반환할 때까지 계속 사용하는 방식
>
- `I/O or Event Wait`, `Exit`
- FCFS(First Come First Served)
    - 선입선출(FIFO), 단순하고 구현 쉬움
    - 큐에 도착한 순서대로 CPU 할당
    - 실행 시간이 긴 작업이 앞으로 오면 평균 대기 시간이 길어짐

![4.png](..%2F..%2F..%2F..%2FDocuments%2F%EC%8A%A4%ED%81%AC%EB%A6%B0%EC%83%B7%2F4.png)

- SJF(Shortest Job First)
    - 최단 작업 우선 원칙
    - FCFS 보다 평균 대기 시간 감소, 짧은 작업에 유리
    - 실행 시간이 짧은 작업이 늦게 도착할 경우 FCFS와 동일한 convoy 문제 발생

![5.png](..%2F..%2F..%2F..%2FDocuments%2F%EC%8A%A4%ED%81%AC%EB%A6%B0%EC%83%B7%2F5.png)

- HRN(Hightest Response-ratio Next)
    - 우선순위를 계산하여 점유 불평등을 보완하는 방법(SJF의 단점 보완)
        - 대기 시간 오래된 작업을 우선
    - 우선순위 = (대기시간 + 실행시간) / (실행 시간)
    - Starvation 방지

### 5. 선점 스케줄링

> CPU 사용 중에도 Interrupt로 작업을 중단하고 다르 프로세스로 전환하는 방식
>

<aside>
💡

예전 일괄처리 시절에 많은 비선점(non-preemptive) 스케줄러가 개발된다. 이 시스템은 각 작업이 종료될 때까지 계속 실행한다. 이론적으로 모든 현대 스케줄러는 선점형이고, 다른 프로세스를 실행시키기 위하여 필요하면 현재 프로세스의 실행을 중단한다. 이 사실은 스케줄러가 앞에서 배웠던 기법을 채용하고 있다는 것을 의미한다. 구체적으로 스케줄러는 문맥 교환을 수행할 수 있고, 실행 중인 프로세스를 잠시 중단시킬 수 있으며 다른 프로세스를 재개 또는 시작시킬 수 있다.

</aside>

1. STCF( Shortest Time-to-Completion First, =PSJF)
    - 최단 잔여시간 우선
    - 스케줄러에 남아있는 작업과 새로운 작업의 잔여 실행 시간을 계산하여, 가장 적은 잔여 실행 시간을 가진 작업을 스케줄
    - 단점: CPU Burst 예측 필요

![6.png](..%2F..%2F..%2F..%2FDocuments%2F%EC%8A%A4%ED%81%AC%EB%A6%B0%EC%83%B7%2F6.png)

2. Priority Scheduling

- 우선순위가 가장 높은 CPU를 할당하는 스케줄링
- 작은 숫자 우선순위가 높음
- 단점 : starvation(기아 상태), Indefinite blocking(무기한 봉쇄)
- 해결 방법: aging
    - 우선순위가 낮은 프로세스가 오래 기다리면 우선순위를 높여줌
1. Round Robin
    - 작업이 끝날 때까지 기다리지 않음
    - `일정한 시간` (=time slice, =scheduling quantum) 동안 작업을 실행한 후, 실행 큐의 다음 작업으로 전환
        - 일정한 시간은 인터럽트 주기의 배수여야 한다
        - 일정한 시간이 짧을 수록, 응답 시간 기준 RR 성능이 더 좋아짐
        - 단, 너무 짧게 지정할 시 `Context Switching` 비용이 전체 성능에 영향을 미치게됨
    - 시분할 컴퓨터 등장으로 생긴 새로운 평가 기준인 `응답 시간` 이 좋아짐
        - 반환시간은 좋지않음

![7.png](..%2F..%2F..%2F..%2FDocuments%2F%EC%8A%A4%ED%81%AC%EB%A6%B0%EC%83%B7%2F7.png)

2. MLQ(Multi Level Queue, 다단계 큐)
    - 작업 성격 별로 큐 분리 (ex. 시스템 프로세스 / 사용자 프로세스)
    - 큐 간 우선순위 존재
    - 큐 내에서는 FCFS or RR

![8.png](..%2F..%2F..%2F..%2FDocuments%2F%EC%8A%A4%ED%81%AC%EB%A6%B0%EC%83%B7%2F8.png)

1. MLFQ(Multi Level Feedback Queue, 다단계 피드백 큐)
    - 여러 개의 큐로 구성되며, 각각 다른 우선순위가 배정
    - 짧은 작업을 먼저 실행시켜 반환 시간을 최적화하고자 한다
    - 기본 규칙
        - Priority(A) > Priority (B) → A 실행
        - Priority(A) = Priority (B) → A, B RR방식으로 실행
        - 작업이 시스템에 들어가면 최상위 큐에 배치
        - 작업이 지정된 단계에서 CPU를 포기한 횟수와 상관없이 배정 받은 시간을 소진하면, 작업의 우선순위는 감소(= 한단계 아래 큐로 이동)
        - 일정 주기(S)가 지난 후, 시슽엠의 모든 작업을 최상위 큐로 이동
    - 짧게 실행되는 작업에서는 SJF, STCF와 유사한 성능 제공
    - 오래 실행되는 작업에서는 CPU-집중-워크로드에 대해 공정하게 실행하고 조금이라도 실행되게함
    - 이러한 이유로 다양한 OS를 포함한 많은 시스템이 기본 스케줄러로 MLFQ를 사용한다
    - 단점
        - `starvation` (기아 상태)가 발생할 수 있음
            - 시스템에 너무 많은 대화형 작업이 존재하면 그들이 모든 CPU 시간을 소모하게 될 것이고 따라서 긴 실행 시간 작업은 CPU 시간을 할당받지 못함

![9.png](..%2F..%2F..%2F..%2FDocuments%2F%EC%8A%A4%ED%81%AC%EB%A6%B0%EC%83%B7%2F9.png)

| **알고리즘** | **방식** | **특징** | **단점** |
| --- | --- | --- | --- |
| FCFS | 비선점 | 간단, 공정 | 긴 작업 시 Convoy Effect |
| SJF | 비선점 | 평균 대기 시간 최소 | 실행 시간 예측 어려움 |
| HRN | 비선점 | Starvation 방지 | 구현 복잡 |
| STCF | 선점 | 가장 빠른 평균 시간 | CPU Burst 예측 필요 |
| Priority | 선점 | 우선순위 기반 | Starvation, Aging 필요 |
| RR | 선점 | 응답 시간 향상 | Context Switching 비용 증가 |
| MLQ | 다단계 | 큐별 특성 반영 | Starvation |
| MLFQ | 다단계 + 피드백 | 다양성, Starvation 해결 | 복잡성, 기아 가능성 |

💬 **CPU 스케줄링은 언제 작동하나요?**

A. CPU 스케줄링은 ready 큐에 있는 프로세스 중에서 CPU를 할당할 프로세스를 선택할 때 작동합니다. 특히 ready → running, running → ready(선점형), waiting → ready 전환 시 스케줄링이 필요합니다.

💬 **CPU Burst 와 I/O Burst 의 차이는?**

A. CPU Burst 는 CPU 가 작업하는 시간이고, I/O Burst 는 입출력 작업을 기다리는 시간입니다. 프로세스는 이 두 가지를 반복하면서 실행됩니다.

💬 **Starvation이란 무엇이며, 해결 방법은?**

A. STarvation이란 우선순위가 낮은 프로세스가 CPU를 할당 받지 못하는 현상입니다. Priority Scheduling, MLFQ 등에서 발생할 수 있습니다. Aging 기법으로 기다린 시간에 따라 우선순위를 높여 해결할 수 있습니다.

## `📌 인터럽트(Interrupt) 개념 및 종류`

📂 **[개발자가 알아야하는 이유]**

- 컴퓨터 시스템은 항상 예외 상황이 발생하기 때문에, CPU가 신속하게 대응하는 것을 알아야하기 때문
- 디버깅 or 예외 처리 대부분이 Interrupt로 처리되기 때문에 더 빠르게 문제 파악하기 위함
- 시스템 안정성과 성능 최적화의 핵심

### Interrupt 개념

> **CPU가 작업 중일 때 외부나 내부에서 이벤트가 발생하면, CPU가 현재 작업을 일시 중단하고, 시급한 작업을 우선 처리하는 메커니즘**
>

![10.png](..%2F..%2F..%2F..%2FDocuments%2F%EC%8A%A4%ED%81%AC%EB%A6%B0%EC%83%B7%2F10.png)

- 일종의 가로채기(Interruption)
- CPU가 외부/내부 신호에 의해 현재 실행 중인 프로세스의 흐름을 중단하고 Interrupt 서비스 루틴으로 분기
- Interrupt 처리가 끝나면 다시 원래 작업으로 복귀
    - 복귀 주소는 Stack에 저장
- 외부/내부 인터럽트는 CPU의 하드웨어 신호에 의해 발생
- 인터럽트 `벡터 테이블 조회/ 분기 → 처리루틴 수행 → 복귀`

### Interrupt 종류

1. External Interrupt (외부 인터럽트)

> **CPU 외부 하드웨어에서 발생하는 인터럽트로, `비동기적`(예측 불가능)으로 CPU의 주 작업을 중단시킴**
>
- 전원 이상, 기계 착오, 외부 신호, 입출력 장치 요청(I/O Interrupt)

1. Internal Interrupt (내부 인터럽트, =Exception)

> **CPU 내부에서 발생하는 인터럽트, `동기적`**
>
- 잘못된 명령이나 데이터를 사용할 때 발생
- 0으로 나누기, overflow, 잘못된 명령어 실행(Exception)
- `Trap(의도적인 시스템 콜)`  : 의도적으로 커널 기능을 요청하는 소프트웨어 인터럽트(ex. System call)

1. 소프트웨어 인터럽트

> 프로그램 처리 중 명령의 요청에 의해 발생한 것 (SVC 인터럽트)
>

사용자가 프로그램을 실행시킬 때 발생

소프트웨어 이용 중에 다른 프로세스를 실행시키면 시분할 처리를 위해 자원 할당 동작이 수행

| **분류** | **설명** | **예시** |
| --- | --- | --- |
| 외부 인터럽트 | CPU 외부, 비동기 | 전원 이상, 타이머, I/O |
| 내부 인터럽트 (Exception) | CPU 내부, 동기 | 0으로 나누기, overflow |
| 소프트웨어 인터럽트 | 프로그램 명령 | System Call, SVC |

**💡 Trap 과 Internal Interrupt 차이**

> **Trap 은 내부 인터럽트의 한 종류로, 커널 서비스를 요청하거나 디버깅을 위해 의도적으로 발생**
>
- 내부 인터럽트의 범위
    - 의도하지 않은 오류 → 예외 (Exception)
    - 의도적으로 발생하는 인터럽트 → **Trap**

### 인터럽트 번호 (x86 시스템 기준)

| **인터럽트 번호** | **설명** |
| --- | --- |
| 0 | Divide by Zero Exception (0으로 나누기 오류) |
| 1 | Debug Exception |
| 13 | General Protection Fault (일반 보호 오류) |
| 14 | Page Fault (메모리 접근 오류) |
| 32~47 | 하드웨어 인터럽트 (IRQ) |
| 128 | System Call(Linux syscall) |
- 인터럽트 종류를 구분하는 고유 번호
- CPU가 Interrupt 발생 시 Interrupt 백터 테이블 참조
- 인터럽트 번호 → ISR(해당 인터럽트 서비스 루틴) 실행

### 인터럽트 동작 순서

> 발생 → 저장 → 식별 → 처리 → 복구 → 중단된 프로그램 재개
>
1. 인터럽트 발생
2. 현재 작업 상태 저장
- Program Counter(PC), 레지스터, 플래그 저장
1. 인터럽트 번호(Interrupt Vector Number) 조회하여 인터럽트 원인 식별
2. 인터럽트 서비스 루틴(ISR, Interrupt Service Routine) 실행
    - 필요한 서비스 수행
    - ISR 중 더 높은 우선순위 Interrupt 발생 시, 재귀적으로 처리
3. 상태 복구
    - 저장해둔 PC, 레지스터 복원
4. 중단된 프로그램 재개

### 인터럽트 우선순위

> 여러 장치에서 인터럽트가 동시에 발생하거나 인터럽트 서비스 루틴 수행 중 인터럽트가 발생한 경우 우선순위 판별이 필요하다
>
- 일반적으로 Hardware Interrupt > Software Interrupt
- 외부 인터럽트가 내부 인터럽트보다 우선

| **우선순위 (높음 → 낮음)** | **설명** |
| --- | --- |
| 1. 전원 이상 | Power Fail |
| 2. 기계 착오 | Machine Check |
| 3. 외부 신호 | 키보드, I/O 요청 등 |
| 4. 입출력 인터럽트 | 디스크, 네트워크 등 |
| 5. 명령어 오류 | 잘못된 명령어 등 |
| 6. 프로그램 검사 | Divide by zero 등 |
| 7. 소프트웨어 인터럽트 | System Call (SVC) |

### 우선순위 판별 방법

- Polling (소프트웨어 방식)
    - 순차적으로 Interrupt flag 검사
    - 장점 : 회로 간단, 유연
    - 단점: Interrupt가 많을수록 속도 느림
- Vectored Interrupt (하드웨어 방식)
    - Interrupt 요청 장치와 CPU가 직접 연결
    - Daisy Chain, 병렬 방식
        - **Daisy Chain**
            - 인터럽트가 발생하는 모든 장치를 하나의 직렬 회선으로 연결
            - 우선순위가 높은 장치를 상위에 두고 우선순위 차례대로 배치
        - **병렬(Parallel) 우선순위 부여 방식**
            - 인터럽트가 발생하는 모든 장치를 하나의 직렬 회선으로 연결
            - 각 장치별 우선순위를 판별하기 위한 Mask register에 bit를 설정
            - Mask register상 우선순위가 높은 서비스 루틴 수행중 우선순위가 낮은 bit들을 비활성화
            - 반대로 우선순위가 높은 인터럽트는 낮은 인터럽트 수행 중에도 우선 처리
    - 장점: 빠름, 실시간성
    - 단점: 회로 복잡, 유연성 낮음

💬 내부 인터럽트는 Trap이라고 할 수 있나요?

A. Trap 은 내부 인터럽트의 일종으로, 프로그램이 커널 기능을 요청하거나 디버깅을 위해 의도적으로 발생시키는 인터럽트입니다. 내부 인터럽트는 Trap 과 예기치 않은 오류 상황 모두를 포함합니다

<aside>
💡

인터럽트는 CPU 가 외부 또는 내부 이벤트에 빠르게 대응하기 위한 메커니즘으로, 발생 시 현재 상태를 저장하고 인터럽트 서비스 루틴을 수행한 뒤 원래 작업으로 복귀한다.

인터럽트는 외부 인터럽트, 내부 인터럽트(Exception), 소프트웨어 인터럽트로 구분하며, 내부 인터럽트 중 의도적으로 발생하는 Trap 은 커널 기능 요청 등에 사용된다

</aside>

## `📌 시스템 콜(System Call)과 사용자 모드 vs 커널 모드`

📂 **[개발자가 알아야하는 이유]**

- 운영체제 구조 이해 필수(User Mode → Kernel Mode)
- 파일/프로세스/네트워크 등 시스템 자원 관리
- 성능 최적화, 디버깅, 보안 문제 해결

### System Call 이란

> 운영체제(OS)의 서비스를 사용하기 위해 사용자 프로그램이 커널에 요청하는 메커니즘

![11.png](..%2F..%2F..%2F..%2FDocuments%2F%EC%8A%A4%ED%81%AC%EB%A6%B0%EC%83%B7%2F11.png)

- 프로그램은 보안, 안정성 이유로 하드웨어에 직접 접근 불가능
- OS에게 요청하는 방식

### System call 예시

> 파일, 네트워크, 프로세스 작업
>

| **상황** | **설명** |
| --- | --- |
| 파일 열기 | 프로그램 → System Call (open()) → OS 가 파일 찾아줌 |
| 프로세스 실행 | 프로그램 → System Call (fork(), exec()) → OS 가 새 프로그램 실행 |
| 네트워크 통신 | 프로그램 → System Call (socket(), connect()) → OS 가 통신 연결 |

| **예시** | **설명** |
| --- | --- |
| open() | 파일 열기 |
| read() / write() | 파일 또는 장치에서 읽기 / 쓰기 |
| fork() | 새로운 프로세스 생성 |
| exec() | 새로운 프로그램 실행 |
| socket() | 네트워크 통신용 소켓 생성 |
| connect() | 서버와 네트워크 연결 |

### OS에서 System Call이 실행되는 과정

> User Mode → Kernel Mode → User Mode(복귀)
>
1. 사용자가 프로그램 실행 중 OS 기능이 필요할 때 System Call 요청
2. CPU가 `User Mode → Kernel Mode` 전환
3. Kernel이 요청 처리(파일 열기, 데이터 쓰기 등)
4. 처리 결과를 사용자 프로그램에 반환
5. CPU가 다시 `Kernel Mode → User Mode`로 복귀하여 프로그램 실행 재개

### 시스템 콜의 유형

| **범주** | **설명** | **예시** |
| --- | --- | --- |
| 프로세스 제어 | 프로세스 생성/종료/제어 | fork(), exec(), wait() |
| 파일 조작 | 파일 열기, 닫기, 읽기, 쓰기 | open(), read(), write() |
| 장치 조작 | 장치 요청/해제, 읽기/쓰기 | ioctl(), read(), write() |
| 정보 유지보수 | 시간, 날짜 등 시스템 정보 획득 | getpid(), alarm(), time() |
| 통신 | 프로세스 간 통신 | socket(), bind(), connect() |

### 사용자 모드 vs 커널 모드

> 보안과 안정적인 프로그램 실행환경 유지하기 을 위해 구분해야 한다
>

| **구분** | **사용자 모드 (User Mode)** | **커널 모드 (Kernel Mode)** |
| --- | --- | --- |
| 접근 권한 | 제한적 (직접 자원 접근 불가) | 모든 자원 접근 가능 |
| 실행 주체 | 사용자 프로그램 | OS 커널 코드 |
| 목적 | 응용 프로그램 실행 | 하드웨어 제어, 시스템 자원 관리 |
| 전환 | System Call 통해 전환 | 작업 완료 후 복귀 (시스템 콜 리턴) |
- 프로그램이 메모리, CPU, 파일 시스템에 직접 접근한다면,
    - 다른 프로그램이 데이터 침범, 시스템 자원 충돌, 시스템 전체 다운 등 문제 발생할 수 있음
- 이를 방지하기 위해 프로그램은 User Mode에서만 실행
- OS가 필요한 작업을 Kernel Mode에서 실행

💬  System call 이랑 Interrupt차이

A. 둘 다 CPU 모드를 사용자 모드 → 커널 모드로 바꾸지만,

System call은 의도된 요청, Interrupt는 예기치 않은 외부/내부 사건

| **구분** | **시스템 콜 (System Call)** | **인터럽트 (Interrupt)** |
| --- | --- | --- |
| **정의** | 사용자 프로그램이 운영체제(OS)의 서비스를 요청하는 메커니즘 | 하드웨어나 소프트웨어에서 **비동기적**으로 발생하는 CPU 중단 요청 |
| **발생 시점** | 프로그램이 의도적으로 호출할 때 (명령어로 요청) | 예측 불가, 사건 발생 시 즉시 발생 |
| **발생 주체** | 주로 **소프트웨어 (사용자 프로그램)** | 주로 **하드웨어 (입출력 장치)**또는 내부 오류 |
| **목적** | 사용자 프로그램이 커널 기능을 사용하기 위해 (파일 읽기/쓰기, 메모리 할당 등) | 하드웨어나 시스템 이벤트를 CPU에 알려서 빠르게 처리하기 위해 |
| **발생 예시** | 파일 열기, 메모리 할당, 네트워크 통신 | 키보드 입력, 타이머, 전원 이상, 0으로 나누기 |
| **누가 발생?** | 프로그램이 **명령어로 발생** (syscall) | 하드웨어나 CPU 내부 장치가 발생 (타이머, I/O 장치 등) |
| **어떻게 동작?** | 프로그램 → 커널 모드 전환 → OS 서비스 수행 | 인터럽트 핸들러(ISR)가 CPU 제어권 획득 → 작업 수행 |

💬 운영체제에서 Dual Mode에 대해 설명해 주세요

> A. 운영체제는 User Mode와 Kernel Mode 두 가지 모드가 존재한다
>

User Mode는 제한된 권한을 가지며 프로그램 실행 시 사용됩니다

Kernel Mode는 모든 자원에서 접근 가능하며, 시스템 자원 관리와 하드웨어 제어를 담당

참고 자료

- https://pages.cs.wisc.edu/~remzi/OSTEP/Korean/07-cpu-sched.pdf
- https://github.com/Songwonseok/CS-Study/blob/main/OS/Interrupt.md