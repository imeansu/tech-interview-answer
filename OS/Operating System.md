## CPU Scheduling

### 스케쥴링

> CPU를 잘 사용하기 위해 프로세스를 잘 배정하기
> 

조건: 오버헤드 ↓ / 사용률 ↑ / 기아 현상 ↓

시스템별 목표

1. Batch System: 가능하면 많은 일을 수행. 시간(time) 보단 처리량(throughout)이 중요
2. Interactive System: 빠른 응답 시간, 적은 대기 시간
3. Real-time System: 기한(deadline) 맞추기

### 스케쥴러

프로세스 스케쥴링을 위한 Queue

- Job Queue: 현재 시스템 내에 있는 모든 프로세스의 집합
- Ready Queue: 현재 메모리 내에 있으면서 CPU를 잡아서 실행되기를 기다리는 프로세스의 집합
- Device Queue: Device I/O 작업을 대기하고 있는 프로세스의 집합

각각의 Queue에 프로세스들을 넣고 빼주는 스케쥴러에도 크게 세 가지 종류가 존재

1. 장기스케쥴러 (Long-term scheduler or job scheduler)
    - 한정된 메모리, 많은 프로세스가 한꺼번에 메모리에 올라올 경우 → 대용량 메모리에 임시로 저장
    - 이 pool에 저장되어 있는 프로세스 중 어떤 프로세스에 메모리를 할당하여 ready queue로 보낼지 결정하는 역할
    - 메모리에 프로그램이 너무 많아도, 너무 적어도 성능은 떨어짐. 참고로 time sharing system에서는 장기 스케쥴러가 없다
    - 프로세스 상태: new → ready(in memory)
2. 단기스케쥴러 (Short-term scheduler or CPU scheduler) 
    - CPU와 메모리 사이의 스케쥴링을 담당
    - Ready Queue에 존재하는 프로세스 중 어떤 프로세스를 running 시킬지 결정 (scheduler dispatch)
    - 프로세스의 상태 : ready -> running -> waiting -> ready
3. 중기스케쥴러 (Medium-term scheduler or Swapper)
    - 여유 공간 마련을 위해 프로세스를 통째로 메모리에서 디스크로 쫓아냄 (swapping)
    - Unix나 Window에서는 장기 스케쥴러가 거의 사용되지 않는다
    - 프로세스 상태 : ready → suspended
- Process state
    - Suspended(stopped): 외부적인 이유로 프로세스의 수행이 정지된 상태로 메모리에서 내려간 상태를 의미한다. 프로세스 전부 디스크로 Swap out 된다
    - Blocked : 다른 I/O  작업을 기다리는 상태이기 대문에 스스로 ready state로 돌아갈 수 있지만, suspended 상태는 외부적인 이유로 suspending 되었기 때문에 스스로 돌아갈 수 없다
- 장기 vs 단기
    - 단기스케쥴러는 상당히 빨라야 하고 호출 빈도수가 많다
    - 새로운 작업이 생성되어 들어오는 것은 분 단위로 장기는 호출 빈도수가 단기에 비해 매우 적다 → 장기는 시간이 꽤 걸리더라도 신중하게 프로세스를 선택
    - 장기가 I/O 프로세스나 CPU 중심 프로세스 중 한쪽으로만 편중해서 받아오면 ready queue, device queue 한쪽에 프로세스가 집중되어 단기의 균형도 붕괴된다

### 선점 / 비선점 스케쥴링

- 선점 (preemptive) : OS가 CPU의 사용권을 선점할 수 있는 경우, 강제 회수하는 경우
- 비선점 (nonpreemptive) : 프로세스 종료 or I/O 등의 이벤트가 있을 때까지 실행 보장 (처리 시간 예측 어려움)

### 프로세스 상태

![Untitled](https://s3-us-west-2.amazonaws.com/secure.notion-static.com/ab549490-5e0a-4805-9be5-4506ef2b25d7/Untitled.png)

- 비선점 스케쥴링: Interrupt, Scheduler Dispatch
- 선점 스케쥴링: I/O or Event Wait
- 프로세스의 상태 전이
    - 승인(Admitted) : 프로세스 생성이 가능하여 승인됨
    - 스케쥴러 디스패치(Scheduler Dispatch) : 준비 상태에 있는 프로세스 중 하나를 선택하여 실행시키는 것
    - 인터럽트(Interrupt) : 예외, 입출력, 이벤트 등이 발생하여 현재 실행 중인 프로세스를 준비 상태로 바꾸고, 해당 작업을 먼저 처리하는 것
    - 입출력 또는 이벤트 대기(I/O or Event wait) : 실행 중인 프로세스가 입출력이나 이벤트를 처리해야 하는 경우, 입출력/이벤트가 모두 끝날 때까지 대기 상태로 만드는 것
    - 입출력 또는 이벤트 완료(I/O or Event Completion) : 입출력/이벤트가 끝난 프로세스를 준비 상태로 전환하여 스케쥴러에 의해 선택될 수 있도록 만드는 것

### CPU 스케쥴링의 종류

- 비선점 스케쥴링
    1. FCFS (First Come First Served)
        - 큐에 도착한 순서대로 CPU 할당
        - 실행 시간이 짧은 게 뒤로 가면 평균 대기 시간이 길어짐
        
        문제점
        
        - convoy effect : 소요시간이 긴 프로세스가 먼저 도달하여 효율서을 낮추는 현상
    2. SJF (Shortest Job First)
        - 수행시간이 가장 짧다고 판단되는 작업을 먼저 수행
        - FCFS 보다 평균 대기 시간 감소, 짧은 작업에 유리
        
        문제점
        
        - starvation
        - 비현실적, 프로세스의 CPU 점유 시간(Burst time)을 알 수 없다
    3. HRN (Highest Response-ratio Next)
        - 우선순위를 계산하여 점유 불평등을 보완한 방법 (SJF의 단점 보완)
        - 우선순위 = (대기시간 + 실행시간) / 실행시간
- 선점 스케쥴링
    1. Priority Scheduling
        - 정적/동적으로 우선순위를 부여하여 우선순위가 높은 순서대로 처리
        - 우선순위가 낮은 프로세스가 무한정 기다리는 Starvation이 생길 수 있음
        
        문제점
        
        - starvation, 무기한 봉쇄(Indefinite blocking)
        - Aging 방법으로 Starvation 문제 해결 가능
    2. SRT(Shortest Remaining Time)
        - 현재 CPU에서 실행 중인 프로세스의 남은 CPU burst 시간보다 더 짧은 CPU burst 를 가진 프로세스가 도착하면 CPU가 선점
    3. Round Robin
        - FCFS에 의해 프로세스들이 보내지면 각 프로세스는 동일한 시간의 Time Quantum 만큼 CPU를 할당 받음 (Time Quantum or Time Slice : 실행의 최소 단위 시간, 일반적으로 10~100msec 사이의 범위)
        - n 개의 프로세스가 ready queue에 있고 할당 시간이 q인 경우 각 프로세스는 q 단위로 CPU 시간의 1/n 을 얻는다. 즉, 어떤 프로세스도 (n-1)q time unit 이상 기다리지 않는다.
        - 할당 시간(Time Quantum)이 크면 FCFS와 같게 되고, 작으면 문맥 교환 (Context Switching)이 잦아져서 오버헤드 증가
    4. Multilevel-Queue (다단계 큐)
        - 작업들을 여러 종류의 그룹으로 나누어 여러 개의 큐를 이용하는 기법
        - 우선순위가 낮은 큐들이 실행 못하는 걸 방지하고자 각 큐 마다 다른 Time Quantum을 설정해주는 방식 사용
        - 우선순위가 높은 큐는 작은 Time Quantum 할당, 낮은 큐는 큰 Time Quantum 할당
        
        ![Untitled](https://s3-us-west-2.amazonaws.com/secure.notion-static.com/abe3b5ea-f09f-4d43-aa6f-51413bc16b75/Untitled.png)
        
    5. Multilevel-Feedback-Queue (다단계 피드백 큐)
        - 다단계 큐에서 자신의 Time Queantum을 다 채운 프로세스는 밑으로 내려가고 자신의 Time Quantum을 다 채우지 못한 프로세스는 원래 큐 그대로
            - Time Quantum을 다 채운 프로세스는 CPU burst 프로세스로 판단하기 때문
        - 짧은 작업에 유리, 입출력 위주(Interrupt가 잦은) 작업에 우선권을 줌
        - 처리 시간이 짧은 프로세스를 먼저 처리하기 때문에 Turnaround 평균 시간을 줄여줌?
        - 대부분의 상용 운영체제는 여러 개의 큐를 사용하고 각 큐마다 다른 스케쥴링 방식을 사용

### CPU 스케쥴링 척도

1. Response Time: 작업이 처음 실행되기까지 걸린 시간
2. Turnaround Time: 실행 시간과 대기 시간을 모두 합한 시간으로 작업이 완료될 때 까지 걸린 시간
