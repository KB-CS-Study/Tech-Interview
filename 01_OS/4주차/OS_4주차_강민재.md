### [4주차] 동기화 문제 (Race Condition, Deadlock) + 세마포어 & 뮤텍스

- 동기화 문제 (Race Condition, Deadlock)
- 세마포어 & 뮤텍스

---

<br>

## Race Condition (경쟁 상태)

공유 자원에 대해 여러 프로세스가 동시에 접근을 시도할 때, 타이밍이나 순서 등이 결과값에 영향을 줄 수 있는 상태를 말한다.

발생하는 경우

- 커널 코드 실행 중 인터럽트 발생할 경우
- 프로세스가 시스템 콜을 하여 커널모드로 진입해 작업을 수행하는 도중 문맥 교환이 발생할 경우
- 멀티 프로세서에서 공유메모리 내의 커널 데이터에 접근할 경우
- 멀티 프로세서에서 공유 메모리 내의 커널 데이터에 접근할 경우

---

<br>

## Deadlock (교착 상태)

둘 이상의 프로세스가 자원을 점유한 채, 서로 다른 프로세스가 점유하고 있는 자원을 기다리면서 무한히 대기 상태에 빠지는 현상.

Deadlock 발생 조건 (4가지 모두 충족할 때 발생)

1. **Mutual Exclusion (상호 배제)**  
   자원은 한 번에 하나의 프로세스만 사용할 수 있어야 한다.

2. **Hold and Wait (점유 대기)**  
   최소한 하나의 자원을 보유한 프로세스가 다른 자원을 기다리며 대기한다.

3. **No Preemption (비선점)**  
   이미 할당된 자원을 강제로 뺏을 수 없다.

4. **Circular Wait (순환 대기)**  
   각 프로세스가 다음 프로세스가 점유하고 있는 자원을 요청하며 원형으로 기다리는 상태.

Deadlock 해결 방법

- 예방 (Prevention): 네 가지 조건 중 하나라도 발생하지 않도록 설계

- 회피 (Avoidance): 자원의 상태를 고려하여 교착 상태가 발생하지 않도록 스케줄링 (ex. Banker’s algorithm)

- 탐지 (Detection): 교착 상태 발생 후 이를 감지하고 복구

- 복구 (Recovery): 프로세스를 강제로 종료하거나 자원을 회수하여 해결.

---

<br>

## Critical Section (임계 영역)

운영체제에서 여러 프로세스가 데이터를 공유하면서 수행될 때, 각 프로세스에서 공유 자원에 접근하는 프로그램 코드 부분을 의미한다.

프로세스 간 공유 자원을 접근하는 데 있어 문제가 발생하지 않도록 공유 자원의 독점을 보장해야 하는 영역이다.

임계 영역 문제를 해결하기 위해 아래의 3가지 조건을 충족해야한다.

- Mutual Exclusion (상호 배제)

  한 프로세스가 자신의 critical section이면 다른 프로세스들은 critical section에 진입할 수 없다.

- Progress (진행)

  아무도 critical section에 있지 않다면, 진입하고자 하는 프로세스를 진입하게 해줘야 한다. 즉 critical section에 아무도 진입하지 못하면 안되며, 다음에 어떤 프로세스가 critical section에 진입해야 하는지는 유한한 시간 내에 결정되어야 한다.

- Bounded Waiting (유한 대기)

  프로세스가 critical section에 진입하기 위해 무한정으로 기다리는 현상(Starvation)이 발생해서는 안된다.

### 생산자-소비자 문제

critical section에 관련된 전통적인 문제로 생산자-소비자 문제가 있다. 생산자-소비자 문제에서 생산자 프로세스와 소비자 프로세는 서로 독립적으로 작업한다.

생산자는 물건을 계속 생산해 버퍼에 넣고, 소비자는 계속해서 버퍼에서 물건을 받아온다.

![producer-consumer problem](https://firebasestorage.googleapis.com/v0/b/portfolio-74c3d.appspot.com/o/CS-study%2F04-1.png?alt=media&token=93384290-666d-46a4-b79e-188de4cb52db)

가운데에는 `buf`라는 원형 큐 자료구조가 존재하고, sum은 큐의 원소 수에 해당한다.

`producer`는 계속해서 값을 넣어주고, `consumer`는 큐의 원소를 빼낸다. producer는 원소르 맨 뒤부터 넣지만, consumer는 맨 앞부터 빼낸다.

겉보기에는 문제 없어보이지만, producer, consumer 둘이 같이 실행되면 문제가 발생한다. 생산자와 소비자가 전역변수 sum에 접근하는 타이밍을 서로 맞추지 않기 때문이다.

즉, sum이 바로 critical section이며 race condition이 발생한다.

---

<br>

## 임계 영역 해결

### **유저모드 동기화 방식**

운영체제나 하드웨어 명령 없이 사용자 수준에서 동기화를 구현하는 방법이다. 대표적인 알고리즘은 다음과 같다

1. 플래그 변수(flag variable) 사용

- 각 프로세스가 진입 여부를 표시하는 Boolean 배열을 사용

- 단점: 상호 배제는 가능하지만 진행(Progress) 조건을 만족하지 못하는 경우가 있음

2. 피터슨 알고리즘 (Peterson’s Algorithm)

- 2개의 프로세스 사이의 상호 배제를 보장하는 대표적인 알고리즘

- 두 가지 공유 변수 사용:

  - flag[i]: 프로세스 i가 임계 영역에 진입하고자 함을 나타냄
  - turn: 누가 우선권을 가지는지를 나타냄

- 장점: Mutual Exclusion, Progress, Bounded Waiting 세 조건 모두 만족

3. Dekker’s Algorithm

- 초기의 소프트웨어 기반 상호 배제 알고리즘

- Peterson보다 복잡하며 덜 효율적이지만, 이론적으로 중요한 의미를 가짐

4. Lamport’s Bakery Algorithm

- 여러 개의 프로세스에 대한 상호 배제를 지원

- 각 프로세스가 “번호표”를 받아 순서대로 critical section에 진입

- 실제 베이커리 대기 방식에서 착안

---

<br>

### **커널모드 동기화 방식**

1. 세마포어 (Semaphore)

   프로세스 간 동기화 및 상호 배제를 위한 정수 변수. 공유 자원에 접근할 수 있는 최대 개수를 제한한다.

   세마포어의 종류

   - Counting Semaphore: 0 이상의 값을 가짐. 자원의 개수를 표현할 때 사용.
   - Binary Semaphore (Mutex와 유사): 값이 0 또는 1만 가짐. 한 번에 하나의 프로세스만 자원에 접근할 수 있음.

   세마포어 연산

   - P(S) 또는 wait(S): 세마포어 값을 1 감소시킴. 값이 음수가 되면 해당 프로세스는 블락됨
   - V(S) 또는 signal(S): 세마포어 값을 1 증가시킴. 블록된 프로세스가 있으면 하나를 깨움

---

<br>

2. 뮤텍스 (Mutex)

   Mutual Exclusion의 줄임말로, 오직 하나의 스레드만 공유 자원에 접근할 수 있도록 하는 상호 배제 도구이다.

   - 일반적으로 lock/unlock 메서드를 사용
   - 한 스레드가 뮤텍스를 lock하면, 다른 스레드는 unlock될 때까지 대기
   - 세마포어보다 단순하고, 1개의 자원에 대한 배타적 접근을 보장할 때 적합

---

<br>
> **Binary Semaphore vs Mutex**
>
> - Binary Semaphore는 리소스의 접근 제어를 위해 사용할 수 있지만, 누구나 signal을 호출할 수 있음
> - Mutex는 lock을 획득한 **소유자만 unlock 가능** → 소유 개념이 존재
