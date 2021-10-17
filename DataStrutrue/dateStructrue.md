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
