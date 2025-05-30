# 메모리 관리 (페이징 & 세그멘테이션) + 가상 메모리

## 가상메모리 (virtual memory)

컴퓨터가 실제로 이용가능한 메모리 자원을 추상화하여 이를 사용하는 사용자에게 
매우 큰 메모리로 보이게 만드는 메모리 관리기법이다.

이는 그냥 커보이게만 하는게 아니라 그 안에 들어가있는 
페이지 테이블, MMU, TLB, 페이지 폴트 등이 어우러진 시스템을 통틀어서 말한다.

<br />

[참고]

가상 주소는 MMU와 페이지테이블(page table)에 의해 실제 주소로 변환된다.

MMU는 CPU와 메모리 사이에 위치한 하드웨어 컴포넌트로, 가상 주소와 물리 주소 사이의 변환(주소 변환)을 담당한다. 
프로그램이 가상 주소로 접근하면, MMU는 이를 페이지 테이블에 기반해 실제 물리 주소로 변환하여 
올바른 메모리 위치에 접근할 수 있도록 돕는다.

페이지 : 가상 메모리를 사용하는 최소 크기 단위

프레임 : 실제 메모리를 사용하는 최소 크기 단위

<br />

### 페이지테이블

각 프로세스는 자신만의 가상 주소 공간을 가지고 있다.
가상 주소는 프로세스가 메모리에 접근할 때 사용하는 주소이다.

물리 메모리는 실제로 존재하는 RAM의 주소 공간이며 
가상 주소는 실제로 물리 메모리의 주소와 직접적인 관계가 없다.

이를 매핑해주는 페이지 테이블이 있고 
이걸 기반으로 CPU의 메모리 관리 유닛(MMU, Memory Management Unit)이 가상주소를 물리주소로 변환한다.
 
이 때 속도 향상을 위해 캐싱계층인 TLB를 쓴다.

가상주소에서 바로 페이지테이블을 가는게 아니라 TLB에 있는지를 확인하고 
만약 없다면 페이지테이블로 가서 실제주소를 가져온다.

프로세스가 종료되면, 가상 주소 공간과 페이지 테이블은 해제된다.

다시 실행되면, 운영 체제는 새로운 가상 주소 공간을 할당한다.

하지만 이 가상 주소 공간이 이전과 동일하지 않을 수 있다.

또한 중요한 점은 프로세스A와 프로세스B의 가상주소가 동일하더라도, 실제 물리 메모리 주소는 다를 수 있다.

각 프로세스의 별도의 페이지 테이블에 의해 서로 다른 물리 메모리 영역에 매핑되며 
가상 주소는 해당 프로세스 내에서만 유효한 값이기 때문에 충돌이 발생하지 않는다.


<br />

### 페이지폴트

가상메모리는 작은 메모리를 큰 메모리로 보이게끔 하는 것이기 때문에 
참조하려는 메모리 영역이 상대적으로 작은 실제 메모리에 없을 수도 있다.

이 때 페이지폴트가 일어난다.

페이지 폴트는 프로세스가 참조하려는 주소가 가리키는 페이지가 현재 물리 메모리(RAM)에 없을 때 발생하는 현상이다.

디스크는 프로세스에 필요한 데이터 등을 모두 가지고 있는데,
운영체제는 전체 프로세스의 메모리 내용을 디스크로부터 한 번에 RAM에 올리지 않는다.
처음에 필요한 부분들만 효율적으로 올린다.

그렇기 때문에 이런 현상이 일어난다.

<br />

#### 페이지 폴트 이후의 과정

1. 페이지폴트 이후 OS에게 트랩을 전송한다. OS는 먼저 페이지 폴트가 유효한 첩근인지 점검한다. 만약 비정상적인 접근이라면 프로세스를 강제 종료할 수 있다.
2. OS는 물리 메모리에서 빈 프레임이 있는지 확인하고 다음의 과정이 일어난다.
   1. 빈 프레임이 있을 경우 : 디스크에서 필요한 페이지를 바로 메모리로 가져와 적재한다(Swap-in). 이때 기존 페이지를 내보내는 작업(Swap-out)은 없다.
   2. 빈 프레임이 없을 경우 : 페이지 교체 알고리즘(Page Replacement Algorithm)을 수행하여 기존의 한 페이지를 선택한다. 선택된 페이지를 디스크로 내보낸다(Swap-out). 빈 프레임을 확보한 뒤 필요한 페이지를 디스크에서 메모리로 가져온다(Swap-in).

<br />

[참고]
- 이 때 디스크 I/O 작업은 CPU 처리 속도에 비해 매우 느리므로, 페이지 폴트가 자주 발생할 수록 CPU는 작업을 하지 못하고 대기 시간이 증가하여 CPU 이용률이 저하된다.
- 운영체제에서 스와핑은 일반적으로 디스크와 메모리 간에 데이터를 교환하는 전체 과정을 의미하며 아래 두 과정을 합쳐 스와핑으로 부르거나 각각 스와핑으로 부르기도 한다.
  - Swap-in : 디스크 -> 메모리
  - Swap-out : 메모리 -> 디스크
- 디스크 스왑 영역 : 물리 메모리가 부족할 때, 사용하지 않는 메모리 페이지를 하드 디스크의 특정 영역으로 옮겨 저장하는 공간

<br />

### 가상 메모리의 장점

#### 1. 메모리 효율적 사용

가상 메모리는 프로그램 실행 시 모든 데이터를 한꺼번에 메모리에 적재하지 않고, 
실제로 필요한 시점에 해당 메모리 페이지만 동적으로 불어온다.
이 덕분에 제한된 RAM 내에서도 대용량 프로그램을 효율적으로 실행할 수 있으며, 
이러한 방식을 Demand Paging(요구 페이징)이라고 한다.

#### 2. 메모리 과잉 할당

운영체제는 각 프로세스에 가상 주소 공간을 넉넉하게 할당하여 실제 RAM보다 훨씬 큰 주소 공간을 
사용할 수 있어서 메모리 자원을 최대한 효율적으로 배분할 수 있다.

#### 3. 프로세스 간 메모리 보호

각 프로세스는 독자적인 가상 주소 공간을 갖고, 동일한 가상 주소라도 서로 다른 물리 메모리 영역에 매핑된다.
이를 통해 오류나 데이터 충돌이 한 프로세스에 국한되어 다른 프로세스에 영향을 주지 않고 
특정 프로세스의 악의적 접근을 방지할 수 있다.

#### 4. 메모리 접근 제어

가상 메모리는 메모리를 일정한 크기의 **페이지**로 나누어 관리하는 데 이때 읽기, 쓰기, 실행 권한을 설정할 수 있다.

-> 권한 없는 접근 시도가 즉시 감지되고 차단되어, 버퍼 오버플로우나 코드 인젝션 같은 보안 위협을 예방한다.

---

## 캐시

데이터를 미리 복사해 놓는 임시 저장소이자 빠른 장치와 느린 장치에서 
속도 차이에 따른 병목 현상을 줄이기 위한 메모리를 말한다.
이를 통해 데이터접근시간의 단축, 데이터를 다시 계산하는 등의 시간을 절약할 수 있다.

캐시의 예는 CPU 레지스터가 대표적인데 CPU가 메모리로부터 데이터를 가져올 때의 시간이 너무 크기 때문에 
그 중간에 레지스터 계층을 둬서 속도차이를 둬서  해결한다.

![img](https://velog.velcdn.com/images/leesfact/post/b9a25582-d0f7-49a9-b695-78f774ea3e49/image.png)

- 캐시히트 : 캐시에서 원하는 데이터를 찾은 것
- 캐시미스 : 캐시에서 원하는 데이터를 찾지 못한 것

캐시는 우리가 사용하는 서비스 내부에서도 많이 찾아볼 수 있다.

ex) 데이터베이스에서의 redis데이터베이스를 캐시계층으로 둔 사례

![img](https://velog.velcdn.com/images/leesfact/post/ba83fff5-99a7-4fb9-be3a-147ec636c2f2/image.png)

ex) 웹 서버 앞단에 nginx 서버를 캐시계층으로 둔 사례

![img](https://velog.velcdn.com/images/leesfact/post/bf7d186c-1c7c-4eaf-821a-e92b709bbc1f/image.png)

---

## 메모리 할당

### 연속할당

연속 할당(contiguous memory allocation)은 메모리에 '연속적으로' 공간을 할당하는 것을 말한다.
사용 가능한 모든 메모리 공간이 같은 위치에 함께 있다. 
즉, 메모리 파티션이 전체 메모리 공간에서 여기저기서 분산되어있지 않다.
이는 고정분할방식과 가변분할방식이 있다.

#### 고정분할방식

고정분할방식(fixed partition allocation)은 메모리를 미리 같은 크기로 분할해서 할당하는 방식이다.
이는 내부단편화(internal fragmentation)가 발생한다.

![img](https://velog.velcdn.com/images/leesfact/post/2e0e6452-89de-40ea-b821-189fd3fb63f7/image.png)

내부단편화 : 프로그램이 필요한 공간보다 더 많은 메모리가 할당되어 내부적으로 조각이 많이 생기는 것을 의미한다.
이를 통해 추후에 프로그램에 필요한 메모리를 할당하지 못하는 현상이 일어나게 된다.

<br />

#### 가변분할방식

가변분할방식(variable partition allocation)은 프로그램에 필요한만큼 동적으로 할당하는 방법이다. 
이를 통해 내부단편화를 줄일 수 있지만 외부단편화가 발생할 수 있다.

![img](https://velog.velcdn.com/images/leesfact/post/ea672244-6716-445d-92c6-83355a687866/image.png)

외부단편화 : 동적으로 할당한 외부에 작은 조각들이 생기게 되는 현상을 말한다.

가변분할방식은 최초적합, 최적적합, 최악적합이 있다.

- 최초적합(first fit) : 위쪽이나 아래쪽부터 시작해 홀을 찾으면 바로 할당한다.
- 최적적합(best fit) : 필요한 메모리 크기 이상인 공간 중 가장 작은 홀부터 할당한다.
- 최악적합(worst fit) : 프로세스의 크기와 가장 많이 차이가 나는 홀에 할당한다.

[참고] 홀(hole) : 할당할 수 있는 비어 있는 메모리 공간

<br />

### 불연속할당

불연속할당(non-contiguous memory allocation)이란 메모리를 연속적으로 할당하지 않는 방법으로 
현대 운영체제가 쓰는 방법이다.
프로그램에 필요한 메모리를 쪼개어 서로 다른 위치에 있는 메모리 공간에 할당한다.

이는 대표적으로 페이징, 세그멘테이션, 페이지드세그멘테이션기법이 있다.

#### 페이징

![img](https://velog.velcdn.com/images/leesfact/post/26886391-24c6-4302-9c3d-0fe73adf0e4f/image.png)

페이징(memory paging)은 동일한 크기(보통 4kb)의 페이지 단위로 나누어 
메모리의 서로 다른 위치에 프로세스를 할당한다.
홀의 크기가 균일하지 않는 문제가 없어지지만 
주소 변환을 페이지별로 해야 하기 때문에 주소변환이 복잡해지는 단점이 있다.

- 내부단편화 : 마지막 페이지가 채워지지 않을 때 발생할 수 있다.
- 외부단편화 : 모든 페이지와 프레임이 동일한 크기이므로 외부 단편화가 생기지 않는다.

<br />

#### 세그멘테이션

세그멘테이션(memory segmentation)은 페이지 단위가 아닌 의미 단위인 세그먼트(segment)로 나누는 방식이다.
프로세스는 코드, 데이터, 스택, 힙으로 나누어져서 메모리가 할당되는데 
코드와 데이터 또는 코드와 스택 등으로 나눌 수도 있으며 함수 단위로 나눌 수도 있음을 의미한다.
공유와 보안 측면에서 좋지만 홀 크기가 균일하지 않게 된다. 

- 내부단편화 : 필요한 크기만큼 할당되므로 내부 단편화가 발생하지 않는다.
- 외부단편화 : 세그먼트의 크기가 다르기 때문에 외부단편화가 일어날 수 있다.

<br />

#### 페이지드 세그멘테이션

페이지드 세그멘테이션(paged segmentation or segmentation with paging)은 
세그멘테이션으로 나누되 해당 세그멘테이션을 동일한 크기의 페이지로 다시 나누는 방법을 말한다.
