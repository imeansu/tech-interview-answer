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
