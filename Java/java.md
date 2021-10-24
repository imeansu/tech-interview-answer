## java의 메인 메서드가 왜 public static void 일까?
출처 
[https://madplay.github.io/post/java-main-method-structure](https://madplay.github.io/post/java-main-method-structure)
[https://coco-log.tistory.com/138](https://coco-log.tistory.com/138)

### public

- 접근 지정자 또는 접근 제한자, 제한없이 어디에서나 사용 가능
- JVM이 실행되고 실행된 JVM이 main()을 실행
- 클래스 밖에, 패키지 밖에 존재하는 JVM이 main()에 접근할 수 있어야 하기 때문

### static

- 정적 메서드
- static으로 선언하게 되는 경우 자바가 컴파일 되는 시점에 정의
- static 요소를 static이 아닌 요소에서 호출한느 것은 불가능
- main 메서드처럼 프로그램의 시작점에 되는 요소는 객체를 생성하지 않앙도 작업을 수행해야 하기 때문에 static
- 실행되기 위해서는 메모리에 미리 올라가야 함 → static을 선언하여 메모리 할당을 하지 않아도 사용할 수 있게 만든다

### void

- 멀티 스레드를 염두하고 만들어진 언어이기 때문에
- 자식 스레드를 생성 시키고 실행시키는 데 main이 리턴 코드를 뱉고 프로그램 자체를 종료시켜버리면 안됨..

### main

- main이라는 이름은 JVM이 식별할 수 있는 키워드. 반드시 메인 메서드의 이름은 main

## HashMap, TreeMap, LinkedHashMap 비교
출처
[https://soft.plusblog.co.kr/70](https://soft.plusblog.co.kr/70)

### HashMap

- 내부적으로 Entiry 배열을 만들어  관리
- Key 값으로 넘겨준 객체의 해시 코드를 계산하여 Entry 배열의 접근 인덱스로 사용
- 해시 충돌의 경우에 대비해 equals() 메소드까지 사용해서 Key 값이 정말 같은 경우에만 Value를 리턴
- HashCode를 사용하기 때문에 순서가 뒤섞이게 된다. 다만, 해시 함수를 사용하기 때문에 O(1) 시간복잡도

### TreeMap

- Key-Value 쌍을 내부적으로 RedBlack Tree로 저장하여 관리 → Key 값을 기준으로 정렬된 상태를 유지
- Comparator 인터페이스를 구현하면 사용자가 정렬된 순서를 조정할 수 있다

RedBlack Tree
- balanced binary search tree
- O(logn) 시간 복잡도
- 조건: Root Property(black), External (black), Internal(No Double Red), Depth(모든 리프노드에서 Black Depth는 같다 = 리프노드에서 루트노드까지 가는 경로에서 만나는 븍랙노드의 개수는 같다)
- 삽입되는 노드의 색깔은 무조건 Red
- Double Red 해결 : Restructuring, Recoloring
출처: [https://zeddios.tistory.com/237](https://zeddios.tistory.com/237)

### LinkedHashMap

- 입력된 순서를 기억
- 저장되는 각 항목은 Map.Entry 클래스를 구현한  Node 클래스로 내부에 before, after 멤버를 갖는 연결리스트 구조

## HashMap, HashTable, ConcurrentHashMap 동기화 처리 방식
출처: [https://tomining.tistory.com/169](https://tomining.tistory.com/169)

### HashMap

- Thread-Safe 하지 못함
- Collections.synchronizedMap(hashMap) 등을 이용하여 동기화 처리 후 사용 가능

### HashTable

- 중복을 허용하지 않는다
- 동기화 처리가 되어 있어 Thread-safe 하다
- get(), put() 메서드에 synchronized
- HashMap과 다르게 Key 값으로 null을 허용하지 않는다

### ConcurrentHashMap

- 동일 Entry에 추가 될 경우 해당 Entry에 대해서 synchronized를 적용
- Entiry 배열 아이템 별로 동기화 처리 → MultiThread 환경에서 성능 향상

## [Java] Thread 와 Thread Pool

### Thread 구현

- Runnable 인터페이스 구현
    - 클래스를 인스턴스화 해서 Thread 생성자에 argument로 넘겨줘야 한다
- Thread 클래스 상속
    - 상속 받은 클래스 자체를 스레드로 사용할 수 있다

### Thread 실행

> 스레드의 실행은 run() 호출이 아닌 start() 호출로 해야 한다
> 
- 스레드를 이용한다는 건, JVM이 다수의 콜 스택을 번갈아가며 일처리를 하는 것
- run()은 main()의 콜 스택 하나마 이용 - 그냥 run이라는 메소드를 호출하는 것 뿐이게 되는 것
- start() 메소드를 호출하면, JVM은 알아서 스레드를 위한 클 스택을 새로 만들어주고 context switching을 통해 스레드답게 동작하도록 해준다

### 동기화

- synchronized 활용 : 임계 영역을 설정, 공유 자원 lock
    - wait() 와 notify() 활용
        - wait() : 스레드가 lock을 가지고 있으면, lock 권한을 반납하고 대기하게 만듦
        - notify() : 대기 상태인 스레드에게 다시 lock 권한을 부여하고 수행하게 만듦
- 고유 락 (Intricsic Lock)
    - Intrinsic Lock = monitor lock = monitor : Java의 모든 객체는 lock을 갖고 있음
    - Synchronized 블록은 Intrinsic Lock을 이용해서, 스레디의 접근을 제어함
- 구조적인 락(Structured Lock) vs 명시적인 락(Reentrant Lock)
    - 구조적인 락
        - synchronized 를 이용한 동기화
        - 블록 단위로 락의 획득과 해제
        - 구조적인 락 a,b 가 있을 때, a 획득 → b 획득 → b 해제 → a 해제는 가능하지만 중간에 a해제 불가능
    - 명시적인 락
        - synchronized 와 동일하게 가시성과 상호 배제 기능 제공
        - try 문에서 예외가 발생하면 락이 해제되지 않는 경우가 발생 → catch, finally 블록에서 락을 해제하는 것이 중요
        - 스레드 간의 락을 획득하는 순서 → 공정한 방법과 불공정한 방법 지원
        - 동시성을 보장해야 하는 코드가 블록 형태를 넘어서는 경우 쓰임

### Thread Pool
출처: [https://velog.io/@agugu95/자바와-쓰레드풀-쓰레드의-생성비용](https://velog.io/@agugu95/%EC%9E%90%EB%B0%94%EC%99%80-%EC%93%B0%EB%A0%88%EB%93%9C%ED%92%80-%EC%93%B0%EB%A0%88%EB%93%9C%EC%9D%98-%EC%83%9D%EC%84%B1%EB%B9%84%EC%9A%A9)

- 병렬 작업 처리가 많아지면 스레드 개수 증가 → 스레드 생성 및 스케쥴링을 CPU가 바빠져서 메모리 많이 사용 → 성능 저하
- 갑작스런 병렬 작업 처리가 많아질 때 스레드 풀을 이용
- 스레드를 제한된 개수 만큼 정해 놓고 작업큐(Queue)에 들어오는 작업들을 하나씩 스레드가 맡아서 처리
- 스레드 풀 생성/사용을 위해 Executors 클래스와 ExecutorService 인터페이스를 제공
- Runnable 은 리턴값이 없고 Callable 은 리턴값이 있다
- 단점 : 메모리 낭비, 노는 스레드
- (노는) 스레드 풀 개선, Fork Join Thread Pool
    - 작업을 최대한 균등하게 분배하기 위한 방식
    - 첫 스레드는 작업을 가져와 자신의 로컬 큐에 할당, 분할
    - 두번째 스레드가 가져올 작업이 없다면, 첫 스레드의 큐에 있는 분할된 작업을 훔쳐간다
    - 나머지 스레드도 반복
