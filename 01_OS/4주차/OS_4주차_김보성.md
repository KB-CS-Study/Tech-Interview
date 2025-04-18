# **✅ Race Condition**

## 개념

- 둘 이상의 입력 또는 조작의 타이밍이나 순서 등이 결과값에 영향을 줄 수 있는 상태 (경쟁 상태)
- Race condition은 공유(공통) 자원을 둘 이상의 스레드 혹은 프로세스가 읽거나 쓰면서 결과값이 의도와 달라질 수 있는 문제!

![1](https://github.com/user-attachments/assets/52d18e5c-1024-4c7c-81a3-04cea96f1032)


스레드 여러 개가 전역 변수 counter를 ++ 연산하는 상황을 시뮬레이션

```jsx
#include <stdio.h>
#include <stdlib.h>
#include <pthread.h>

#define NUM_THREADS 5
#define NUM_INCREMENTS 1000000

int counter = 0; // 공유 자원 (전역 변수)

void* thread_func(void* arg) {
    for (int i = 0; i < NUM_INCREMENTS; i++) {
        counter++; // 임계구역 (무방비 상태)
    }
    return NULL;
}

int main(void) {
    pthread_t threads[NUM_THREADS];

    // 스레드 생성
    for (int i = 0; i < NUM_THREADS; i++) {
        if (pthread_create(&threads[i], NULL, thread_func, NULL) != 0) {
            perror("pthread_create");
            exit(1);
        }
    }

    // 스레드 종료 대기
    for (int i = 0; i < NUM_THREADS; i++) {
        pthread_join(threads[i], NULL);
    }
    printf("Final Counter: %d\n", counter);

    return 0;
}
```

실제 CPU 레벨에서 메모리에서 값 읽기 → 1 증가 → 메모리에 쓰기 의 여러 단계 연산으로 분해

→ 그러니까 그냥 단순한 counter ++ 단일 연산이 아닌거임!

```jsx
load counter -> register                  1. 메모리에서 counter 값을 읽는다
add 1                                     2. 읽은 값에 1을 더한다
store register -> counter                 3. 결과를 다시 counter 변수에 쓴다
```

하는 과정이 thread_func인데 이 함수를 실행 시킬 스레드 5개가 동시에 진행하기에 counter라는 전역변수를 불러오고 값을 더하고 다시 쓰는 과정에서 각 단계가 겹쳐 레이스 컨디션이 발생하고 값이 5,000,000이 아닌 다른 값이 나올 수 있음!

## 문제점

![2](https://github.com/user-attachments/assets/db0565c6-f55f-451a-a789-4357d4d0d07e)


이렇게 지금 멀티 스레드는 힙, 공유데이터, 코드를 공유함!

그래서 여러 일들을 병렬로 처리할 수 있는 장점도 있지만 공유 영역을 쓰는 만큼 문제가 발생할 위험이 있음

1. 예측 불가능한 결과
   - 여러 스레드나 프로세스마다 실행 속도가 달라서 잘못된 값을 읽거나 수정할 수 있음
   - A 스레드가 수정 한 결과를 B가 수정하면서 A가 수정한 결과는 없어지게 됨
2. 일관성 손실
   - 여러 스레드나 프로세스가 데이터를 수정 시 예기치 않은 상태로 데이터가 변경될 수 있음
3. 디버깅의 어려움
   - 여러 스레드나 프로세스의 각각 다른 실행 속도로 인해 실행 흐름을 읽기 힘들어 디버깅이 힘듬
4. 잠금 대기 시간
   - Race condition을 방지하기 위해 무분별한 락(Lock)을 사용할 시, 대기 시간으로 인해 성능 저하가 발생할 수 있음

## 임계 구역(Critical Section)

멀티 스레드 환경에서 공유 데이터를 읽거나 쓰는 코드 영역

임계 구역에서 발생하는 오류(값 꼬임, 일관성 손실 등)를 막기 위해서는 해당 구간에 동시에 접근할 수 없도록(상호 배제) 설계해야 함!

→ 아까처럼 counter 했는데 여러개의 스레드가 못 들어오게 하는 느낌!

- 임계 구역 지정
  - 예) 전역 변수 수정, 은행 계좌 잔액 갱신, 재고 처리 로직 등
  - 이 부분은 절대 동시에 실행되면 안 된다고 개발자가 명시하는 구간
- 임계 구역 보호
  - 상호 배제(Mutual Exclusion)를 통해, 한 번에 하나의 스레드만 임계 구역에 들어갈 수 있도록 제어해야 함!
  - 이를 위해 보편적으로 사용되는 도구가 뮤텍스(Mutex), 세마포어(Semaphore)
- 주의할 점
  - 데드락(Deadlock) 위험: 여러 개의 락을 잘못 사용하면 스레드들이 서로를 기다리는 교착 상태에 빠질 수 있음
  - 성능 저하: 임계 구역이 과도하게 크거나 너무 자주 락을 사용하면, 동시에 실행할 수 있는 부분이 줄어들어 병렬성이 떨어짐

## 예방 방법!

위에서 말한 임계 구역을 안전하게 보호하기 위해, 아래와 같은 기법을 적용

- 상호 배제(Mutual Exclusion)
  - 공유 데이터에 접근하는 부분을 임계 영역(Critical Section)으로 지정하고, 한 번에 하나의 실행 흐름만 해당 영역에 들어가도록 하기!
- 공유 데이터를 최소화한 병렬 처리 설계
  - 공유해야 할 데이터는 철저히 관리하고, 나머지는 최대한 각자 스레드 내부에서만 처리하도록 설계
- 스레드 안전성 보장(Thread-safe) 구현
  - 공유 데이터를 수정하는 함수나 메서드를 스레드 안전하도록 구현
  - 스레드 동기화 기법을 사용하거나 불변 객체(Immutable Object)패턴 적용
    - 불변 객체 : 생성 시점 이후로 내부 상태 변경이 불가능하도록 설계한 방법
- 테스트와 검증
  - 다양한 스레드 개수, 타이밍, 입력 조건으로 스트레스 테스트를 하여 레이스 컨디션 여부를 점검
  - 테스트 코드를 작성해서 원하는 결과가 나오는지 확인!

## 해결방안! - 동기화

여러 실행 흐름(스레드)의 순차적 실행을 보장해주는 것!

→ 임계 구역에 한 번에 하나의 스레드만 들어가도록 해서 경쟁상태를 예방!

### 그래서 동기화의 주요 목표는?

- 상호 배제
  - 임계 구역에는 동시에 둘 이상 들어갈 수 없도록 한다
  - A 스레드가 데이터를 갱신 중이면, B 스레드는 그 작업이 끝날 때까지 대기
- 공정성
  - 특정 스레드가 너무 오랫동안 진입하지 못하고 대기하지 않게 한다
  - 모든 스레드가 적절히 자기 차례를 얻을 수 있도록 해야 함
- 데드락 방지
  - 서로 다른 락을 잘못된 순서로 획득해 교착상태에 빠지지 않도록 주의
  - 락 순서를 통일하거나, 여러 동기화 기법을 적절히 설계해야 함

### 대표 기법

1. 뮤텍스
2. 세마포어
3. 모니터

# **✅** Deadlock(교착 상태)

• 경쟁상태(raceCondition)에서 서로 lock을 가졌을 때 생길 수 있는 상태

- 락을 가졌다 = 스레드가 임계구역 또는 공유 자원을 독점 사용하는 권한(열쇠)을 획득했다는 의미

![3](https://github.com/user-attachments/assets/5afa905e-b9c8-457c-b797-dca4a6543209)


자동차(프로세스)들이 현재 위치한 길(자원)을 점유함과 동시에 다른 차가 사용하는 길(자원)을 사용하려고 대기하고 있지만, 다른 길(자원)을 사용할 수 없으며 현재 길(자원)에서도 벗어나지 못하는 원형 대기상태

추가 예시 ) 스레드 A는 `lockA`를 쥐고 `lockB`를 요청, 스레드 B는 `lockB`를 쥐고 `lockA`를 요청할 때 발생할 수 있음

## 발생 조건

- **상호 배제(Mutual Exclusion)**
  - 자원(락)은 한 번에 한 스레드만 사용 가능
- **점유 및 대기(Hold and Wait)**
  - 최소한 하나의 자원을 점유하고 있으면서 다른 프로세스에 할당되어 사용되고 있는 자원을 추가로 점유하기 위해 대기하는 프로세스가 있어야 함
- **비선점(Non-Preemptive)**
  - 자원을 강제로 뺏을 수 없음
- **환형 대기(Circular Wait)**
  - 자원 할당 관계가 원형 구조로 서로를 기다림

## 해결 기법

### 1. 예방

교착상태가 발생하지 않도록 사전에 시스템을 제어하는 방법

앞서 살펴본 교착상태 발생의 네 가지 조건 중 어느 하나를 제거함으로써 수행됨

단, 자원의 낭비가 심한 기법

### 2. 회피

교착상태가 발생할 가능성을 배제하지 않고 교착상태가 발생하면 적절히 피해가는 방법

- Banker's Algorithm이 사용됨
  > 자원 요청이 들어올 때, 미래의 데드락 가능성을 미리 검사하고,
  > 안전하면 자원을 할당하는 방식!
  💡 마치 은행원이 고객에게 대출을 줄 때, 다른 고객들 대출 가능성까지 고려하고
  → "안전한 상태"일 때만 대출해주는 것과 비슷함!
  ### 간단 용어
  Max : 각 프로세스가 최대로 필요한 자원의 양
  Allocation : 현재 프로세스가 이미 가지고 있는 자원의 양
  Need : 남은 필요량 = Max - Allocation
  Request : 현재 프로세스가 추가로 요청한 자원의 양
  Available : 시스템 전체가 현재 줄 수 있는 자원 수
  → ex) Total = [10, 5, 7]
  | 프로세스 | Allocation |
  | -------- | ---------- |
  | P0       | [0, 1, 0]  |
  | P1       | [2, 0, 0]  |
  | P2       | [3, 0, 2]  |
  | P3       | [2, 1, 1]  |
  | P4       | [0, 0, 2]  |
  = [10, 5, 7] - [7, 2, 5]
  = [3, 3, 2]
  ### ✅ 작동 순서
  1. 프로세스가 자원을 요청함
  2. 요청량 ≤ Need & 요청량 ≤ Available 확인

     → 아니면 오류

     available = [3, 3, 2]

     | 프로세스 | Max       | Allocation | Need (Max - Allocation) |
     | -------- | --------- | ---------- | ----------------------- |
     | P1       | [3, 2, 2] | [1, 0, 1]  | [2, 2, 1]               |

     그래서 여기서 P1이 [1, 1, 0]을 요청하면 지금 할 당하고 있는 것과 더해서

     [1, 0, 1] + [1, 1, 0] → [2, 1, 1]

     Need ( [2, 2, 1] )보다 값이 작아서 성공!

     전체 availalbe( [3, 3, 2] \*\*\*\*) 보다 작아서 성공!

  3. 임시로 자원 할당 (Allocation & Available 수정)
  4. 안전성 검사 (Safety Algorithm) 수행

     → 모든 프로세스가 차례로 끝낼 수 있는 순서가 있으면 OK

  5. 안전하다면 → 진짜로 자원 할당 → 진짜 할당되면 요청한 만큼 더해 줘서 Allocation = [2, 1, 1]

     아니면 → 요청 거절하고 원상 복구
  | 장점                                     | 단점                                                     |
  | ---------------------------------------- | -------------------------------------------------------- |
  | 데드락을 미리 회피할 수 있음             | 자원 사용에 **제약이 많음** (최대 자원량 사전 제출 필요) |
  | **모든 프로세스가 안전하게 실행됨 보장** | 계산 복잡도 증가 → **실시간 시스템엔 부적합**            |

### 3. 검출

현재 운영체제는 이 검출방식으로 사용하고 있음!

시스템에 교착상태가 발생했는지 점검하여 교착상태에 있는 프로세스와 자원을 발견하는 것

그 중에는 **타임아웃**, 자원 할당 그래프 등을 사용할 수 있음

- 흔히 볼 수 있는 타임아웃
  ![4](https://github.com/user-attachments/assets/5f6a51c2-62ab-469f-862c-1c7985428784)


### 4. 회복

교착상태를 일으킨 프로세스를 종료시키거나, 교착상태의 프로세스에 할당된 자원을 선점(preempt)하여 프로세스나 자원을 회복하는 것

### 4가지 예방 방법 요약!

| 전략 | 설명                     | 장점                | 단점                               |
| ---- | ------------------------ | ------------------- | ---------------------------------- |
| 예방 | 발생 조건 중 하나 제거   | 확실히 막을 수 있음 | 자원 낭비, 과한 제약               |
| 회피 | 발생 가능성 평가 후 회피 | 성능 효율적         | 복잡한 예측 필요                   |
| 검출 | 발생 후 탐지             | 자원 효율적 사용    | 교착상태는 발생함                  |
| 회복 | 교착 상태 해소           | 시스템 유지 가능    | 처리 과정 복잡 or 데이터 손실 가능 |

# ✅ 기아 상태 (Starvation)

## 개념

- 기아 상태는 Deadlock과 유사한 진행 불능 상태
- 자원의 분배 우선순위에 의해 한 프로세스가 자원을 무한정 기다리는 상태
- 보통 우선순위 기반 스케줄링, 또는 자원을 계속 빼앗기는 구조에서 발생

### 예시

- 높은 우선순위의 프로세스들이 계속 CPU를 점유
  → 낮은 우선순위의 프로세스가 영원히 실행되지 못하는 경우

### 예방 기법 - Aging (에이징 기법)

- 기아 상태를 해결하기 위해 도입된 기법
- 대기 시간이 길어질수록 우선순위를 점차 높여주는 방식
- 오래 기다린 프로세스가 결국 CPU나 자원을 할당받을 수 있도록 유도함

### ✅ Deadlock과 Starvation의 차이

| 항목 | Deadlock                               | Starvation                      |
| ---- | -------------------------------------- | ------------------------------- |
| 원인 | 프로세스들이 서로 자원을 점유하고 대기 | 자원의 우선순위 할당 불균형     |
| 상태 | 여러 프로세스가 모두 멈춤              | 일부 프로세스만 영원히 대기     |
| 해결 | 교착 상태 검출/회복                    | **우선순위 조정** or Aging 기법 |

# **✅** 세마포어

## 개념

- 공유 자원의 접근을 제한하는 카운터 기반 동기화 도구
- 여러 스레드나 프로세스가 자원에 접근할 수 있는 최대 개수를 제어
- 값을 증가하거나 감소시키는 연산(P, V)을 통해 동기화 제어하는 도구

![5](https://github.com/user-attachments/assets/d9f618ee-07db-4f70-ae37-31067052b6ab)


wait → 들어갈 수 있으면 자원 점유

못 들어가면 queue에서 대기

release → 다음 대기 중인 스레드 입장

이렇게 공유된 자원에 여러 프로세스와 스레드가 들어가서 사용 할 수 있도록 해줌!

대신 세마포어 크기를 넘긴 상태에서 다음 스레드가 오면 이전에 들어간 스레드가 나올 때까지 기다려야함!

## 구성 요소

| 요소                | 설명                       |
| ------------------- | -------------------------- |
| **정수값 (count)**  | 사용 가능한 자원의 개수    |
| **대기 큐 (queue)** | 자원을 기다리는 프로세스들 |
| **P 연산 (wait)**   | 자원을 요청 (count 감소)   |
| **V 연산 (signal)** | 자원 반납 (count 증가)     |

## P, V 연산 설명

```
P(S):   // wait
  while (S <= 0) wait;
  S--;

V(S):   // signal
  S++;
```

- **P 연산**: 세마포어 값이 1 이상이면 1 감소하고 자원 사용
  - 0 이하면 대기 상태
- **V 연산**: 자원 사용 완료 후 세마포어 값 1 증가
  - 대기 중인 프로세스가 있으면 깨움

## 작동 예시

### 화장실 줄서기

- 화장실 정원: 3명 → `Semaphore = 3`
- 5명이 줄을 섬

1. A → 입장 (S = 2) P 연산
2. B → 입장 (S = 1) ‘’
3. C → 입장 (S = 0) ‘’
4. D, E는 대기 상태로 큐에 들어감 (→ Blocked 상태)
5. A → 나감 (S = 1) → D 입장 V연산

이런식으로 세마포어에 들어갈 수 있는지 없는지를 확인 한 후 사용할 수 있게 끔!

## 종류

| 종류                   | 설명                                   | 예시                  |
| ---------------------- | -------------------------------------- | --------------------- |
| **Binary Semaphore**   | 값이 0 또는 1만 가짐 (뮤텍스처럼 동작) | 임계 구역 보호        |
| **Counting Semaphore** | 자원의 개수만큼 정수로 관리            | 프린터 3대, 화장실 등 |

## 주의점

- 세마포어 남용 시 데드락 위험 있음
  → 자원을 P하고 V하지 않으면 계속 점유 상태 유지됨
- 순서 꼬이면 문제 발생
  → 항상 정확히 짝맞춰야 함 (P → V)

# **✅** 뮤텍스

## 개념

- 하나의 스레드만 공유 자원에 접근할 수 있도록 보장하는 동기화 도구
- Mutual Exclusion(상호 배제)의 줄임말
- 자원에 잠금(Lock)을 걸어서 하나만 들어오게 하고 끝나면 해제(Unlock) 하는 방식

![6](https://github.com/user-attachments/assets/6960f8fc-2932-46fb-a1df-5a5416b77d93)


## 작동 원리

1. 어떤 스레드가 자원을 쓰기 위해 **mutex.lock()** 요청
2. **다른 스레드는 대기** 상태로 전환 (Block)
3. 해당 스레드가 사용을 마치면 **mutex.unlock()**
4. 대기 중인 다음 스레드가 들어감

## 예시

- 화장실은 1칸 → 열쇠는 1개 → mutex = 1
- A가 열쇠를 들고 들어감 → 다른 사람은 대기
- A가 나와서 열쇠 놓음 → B가 들어감

## 세마포어 vs 뮤텍스

| **항목**                                   | **세마포어 (Semaphore)**                      | **뮤텍스 (Mutex)**                    |
| ------------------------------------------ | --------------------------------------------- | ------------------------------------- |
| 접근 허용 수                               | 여러 개의 스레드 동시 허용                    | 1개의 스레드만 접근 가능              |
| 제어 대상                                  | 자원 개수 (공유 자원)                         |
| - 임계구역 보호 가능 (BinarySemaphore일때) | 임계 구역 (Critical Section)                  |
| 값                                         | 정수형 (0 이상)                               | 0 또는 1 (잠금/해제)                  |
| 소유권                                     | ❌ 없음 (누구나 `wait` / `signal` 가능)       | ✅ 있음 (잠근 스레드만 `unlock` 가능) |
| 용도                                       | 자원 사용 개수 조절 (ex. DB 커넥션)           | 스레드 간 충돌 방지 (ex. 계좌 처리)   |
| 종류                                       | Counting, Binary (0 또는 1로 제한한 세마포어) | 단일 객체                             |
