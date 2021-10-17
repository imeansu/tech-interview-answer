## 복잡도 (Complexity)
출처: [https://velog.io/@leobit/복잡도Complexity](https://velog.io/@leobit/%EB%B3%B5%EC%9E%A1%EB%8F%84Complexity)

### 시간복잡도

- 문제를 해결하는데 걸리는 시간과 입력한 함수 관계로, "연산의 횟수"(시행 횟수)를 센도

### Big-O Notation (빅O 표기법)

- 계수와 낮은 차수의 항을 제외시키는 방법

### 공간복잡도

- 프로그램 실행 후, 완료하는데 필요로 하는 자원 공간의 양
- 시간과 공간은 반비례적 경향

## B-tree vs RedBlack tree
출처
[https://inuplace.tistory.com/376](https://inuplace.tistory.com/376)
[https://stackoverflow.com/questions/647537/b-tree-faster-than-avl-or-redblack-tree](https://stackoverflow.com/questions/647537/b-tree-faster-than-avl-or-redblack-tree)

### B Tree

- 데이터베이스, 파일 시스템에서 널리 사용되는 트리 자료구조의 일종
- 이진 트리를 확장해서, 더 많은 수의 자식을 가질 수 있게 일반화
- key 값을 담을 수 있는 block이 커질수록 하나의 노드에 많은 정보를 넣을 수 있다
- 대량의 데이터를 처리해야 할 때, 검색 구조의 경우 하나의 노드에 많은 데이터를 가질 수 있다는 점은 상당히 큰 장점
    - 대량의 데이터는 메모리보다 블럭 단위로 입출력하는 하는 HDD 나 SSD에 저장하기 때문
    - HDD나 SSD에서는 정보량에 따른 속도 차이가 거의 없음

### B+tree

- index 부분과 leaf 노드로 구성된 순차 데이터 부분으로 이루어 진다
- 장점
    - 블럭 사이즈를 더 많이 이용할 수 있음 (key 값에 대한 하드디스크 엑세스 주소가 없기 때문)
    - leaf 노드 끼리 연결 리스트로 연결되어 있어서 범위 탐색에 매우 유리
- 단점
    - B-tree의 경우 최상 케이스에서는 루트에서 끝날 수 있지만, B+tree는 무조건 leaf 노드까지 내려가봐야 함

### B-tree vs RedBlack tree

1. B-tree는 디스크 기반 자료구조 vs RB tree는 인메모리 자료구조
    
    > RB trees are typically in-memory structures used to provide fast access (ideally O(logN)) to data. [...]
    > 
    
    > B-trees are typically disk-based structures, and so are inherently slower than in-memory data.
    > 
2. B-tree는 디스크 엑세스를 최소화
    
    > B-tree try to minimize the number of disk accesses so that data retrieval is reasonably deterministic.
    > 
3. binary가 정보 탐색에 더 쉽다, B-tree는 여러개의 자식 노드를 가지므로 - RB이 더 빠름
    
    > Because the lookup is binary it's very easy to find something. B-tree can have multiple children per node, so on each node you have to scan the node to look for the appropriate child. This is an O(N) operation.
    >

## 해시 (Hash)

- 해시함수: 데이터의 효율적 관리를 목적으로 임의의 길이의 데이터를 고저오딘 길이의 데이터로 매핑하는 함수
- 해시값의 개수보다 더 많은 키값을 해쉬값으로 변환하기 때문에 서로 다른 두 키에 대해 동일한 해시값을 내는 해시충돌을 발생시킴
- 색인(index)에 해시값을 사용함으로써 검색과 삽입, 삭제를 빠르게 할 수 있다 / 계산복잡성O(1) / 내부적으로 배열을 사용하여 데이터를 저장하기 때문
- 모든 해시함수는 충돌을 일으킴, 해시충돌이 해시값 전체에 걸쳐 균등하게 발생하게끔 하는 것이 중요
- 1:1 대응이 되도록 만드는 해시함수는 array와 다를바 없고 메모리를 너무 차지, Collision이 많아질수록 시간복잡도는 O(n)에 가까워짐
- 해시충돌 문제 해결 아이디어
    1. chaining
        - 한 버킷당 들어갈 수 있는 엔트리의 수에 제한을 두지 않음(연결리스트로 저장), 유연하다는 장점이 있지만 메모리 문제를 야기할 수 있음
        - 계산복잡성: n(키)/m(해시테이블 크기) (균등하다면)
        - separate chaining 방식
            - 자바 8부터 Separate Chaining에서 데이터 개수가 많아지면 LinkedList 대신 Tree를 사용해 성능 향상
            - 6개 LinkedList, 8개 Tree - 2차이의 Threshold를 두면 차이가 1일 때의 삽입/삭제에 따른 빈번한 자료구조 변경을 막을 수 있음
    2. open addressing
        - 해시 충돌이 발생하면, 다른 해시 버킷에 해당 자료를 삽입하는 방식
        - 그 칸에 값을 넣었다가 삭제하면 다음에 해시 충돌이 발생한적 없다고 생각하고 탐색을 끝낼 수 있음 따라서 DEL 등으로 표시해줘야 함
        - 특정 해시값에 키가 몰리게 되면 효율성이 매우 떨어지게 됨
            - 선형 탐사(Linear Probing?): 고정폭으로 옮겨 엑세스
            - 제곱 탐사: 폭이 제곱수로 늘어남: 1**2, 2**2, 3**2
        - 이중해싱(Double hashing probing?): 탐사할 해시값의 규칙성을 없애버려서 clustering을 방지
            - 2개의 해시함수로 하나는 최소, 하나는 충돌시 탐사 이동폭을 얻기 위해

            → primary secondary clustering 모두 완화

        - 알파(테이블이 차있는 비율)에 크게 영향을 받음
- Resizing
    - 저장 공간이 일정 수준 채워지면 Separating Chaining의 경우 성능 향상을 위해, Open addressing의 경우 배열 크기 확장을 위해 Resizing
    - 보통 두배로 확장
    - 확장 임계점은 현재 데이터 개수가 Hash Bucket 개수의 75%가 될 때

## Tree

- 비선형 자료구조
- 계층적 관계를 표현하는 자료구조. 표현에 집중
- 용어
    - 포화 이진 트리: 모든 레벨이 꽉 찬 이진 트리
    - 완전 이진 트리: 위에서 아래, 왼쪽에서 오른쪽으로 순서대로 차곡차곡 채워진 이진 트리
    - 정 이진 트리: 모든 노드가 0개 혹은 2개의 자식 노드만을 갖는 이진 트리
- BST(Binary Search Tree)
    - 규칙
        - 규칙 1: 노드에 저장된 키는 유일하다
        - 규칙 2: 부모의 키가 왼쪽 자식 노드의 키보다 크다
        - 규칙 3: 부모의 키가 오른쪽 자식 노드의 키보다 작다
        - 규칙 4: 왼쪽과 오른쪽 서브트리도 이진 탐색 트리이다
    - 탐색 연산은 O(log n), 편향 트리의 경우 O(n)
- Binary Heap
    - 완전 이진 트리 중 하나
    - 보통 트리는 linkedList로 구현하는 것이 효율적이나 heap은 배열로 많이 구현
    - 삽입: 맨 마지막 노드로 삽입 후 부모 노드와 비교하며 스왑
    - 삭제: 루트 노드를 삭제하고 맨 마지막 노드를 루트노드로 올린다. 양쪽 노드가 모두 작으면 stop,  둘 중 더 큰 노드와 스왑
- Red Black Tree
    - BST를 기반으로 하는 트리 형식의 자료구조
    - Search, Insert, Delete 시간 복잡도: O(log n)
    - 동일한 노드의 개수일 때, depth를 최소화하여 시간 복잡도를 줄이는 것이 핵심 아이디어 (완전 이진 트리의 경우 처럼)
    - Java Collection의 ArrayList도 내부적으로 RBT, HashMap에서의 Separate Chaing에서도 사용
    - 구체적이 내용은 보강 필요!
