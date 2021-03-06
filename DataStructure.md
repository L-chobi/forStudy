## 자료구조
- 데이터를 원하는 규칙 또는 목적에 맞게 저장하기 위한 구조를 의미.

### ArrayList
- 원하는 데이터에 무작위로 접근 가능. 시간복잡도 O(1).
- 리스트의 크기가 제한되어 있으며, 크기 재조정에는 많은 연산이 필요.
- 데이터의 추가/삭제를 위해서는 임시 배열을 생성하여 복제하고 있어 시간이 오래 걸림.
- 요소들은 인접한 메모리 위치에 연이어 저장됨. 다음 데이터가 바로 옆집인 느낌. 
- 끝에서의 삽입 삭제는 O(1)이나, 중간일 경우 삽입, 삭제에 O(N)이 소요됨. 
- 메모리가 선언 시 컴파일 타임에 할당됨(정적 메모리 할당)

### LinkedList
- 리스트의 크기에 영향 없이 데이터를 추가 가능.
- 데이터를 추가하기 위해 새로운 노드를 생성하여 연결하므로 추가/삭제 연산이 빠름.
- 무작위 접근이 불가능하고 순차 접근만 가능(조회시 O(N)).
- 새로운 요소에 할당된 메모리 위치 주소가 linkedList의 이전 요소에 저장됨. 다음 데이터의 주소를 이전 데이터가 가지고 있는 느낌.
- 마지막 요소를 알고 있다면 삽입, 삭제에 O(1)이 소요됨(다음 요소의 주소를 이전 요소에 가지고 있게 하면 됨).
- 마지막 요소를 모른다면 조회가 필요하므로 O(N)까지 증가.
- 리스트 중간에 요소를 추가한다면 추가할 위치의 노드를 찾는 시간 + O(1)
- 동적 메모리 할당방식. 새로운 요소가 추가될 때 런타임에 메모리를 할당.

### LinkedList와 ArrayList 차이
- arrayList의 경우 데이터들이 순서대로 늘어선 배열의 형식이지만 linkedList의 경우 자료의 주소값으로 서로 연결된 형식을 가지고 있음.
- arrayList는 결국 배열이므로 길이가 고정됨. 그러나 linkedList는 노드만 추가해주면 되기에 공간적 제약이 없음.

### Array와 ArrayList의 차이
#### 길이의 변경 가능성. 
- (JAVA 기준) array는 고정된 길이를 가짐. 새로운 데이터를 추가하고 싶다면 새로운 배열을 만들어야함.
- arrayList의 경우 내부적으론 배열로 구성되어있으나 가변적임. 새로운 데이터를 추가할 경우 알아서 길이를 늘림.

### 셋(Set)
- 순서를 보장하지 않으며 중복을 허용하지 않음.
- 해시셋: 해시맵에서 key 값이 없는 자료형.
### 맵(Map)
- 해시맵: 가장 일반적으로 사용되는 맵. 해시 테이블을 사용하며, key 값에 해시함수를 적용하여 나온 값을 index에 value를 저장.

### 스택(Stack)
- 세로로 된 바구니와 같은 구조로, 먼저 넣게 되는 자료가 마지막으로 나오게 되는 FILO(First-In Last-Out)구조.

### 큐(Queue)
- 가로로 된 통과 같은 구조로, 먼저 넣게 되는 자료가 가장 먼저 나오는 FIFO(First-In First-Out)구조.

#### 스택과 큐의 차이
- 스택은 FILO, 큐는 FIFO 방식.
- 스택은 인덱스만 조절하고 초기화하면 되므로 array로 구현하는 것이 좋으며, top을 통해서 push, pop을 하며 삽입, 삭제가 일어남. DFS나 재귀에 사용됨.
- 큐는 list로 구현하는 것이 좋음. 주로 데이터가 입력된 시간 순서대로 처리되어야 하는 경우 사용. BFS나 캐시를 구현할 때 사용 

### 트리(Tree)
- 정점과 간선을 이용해 사이클을 이루지 않도록 구성한 그래프의 특수한 형태로, 계층이 있는 데이터를 표현하기에 적합.

#### BST(이진탐색 트리)
- 이진 탐색과 연결 리스트를 결합한 자료구조. 이진 탐색의 탐색 능력을 유지하면서 빈번한 자료 입력과 삭제가 가능.
- 왼쪽 트리의 모든 값이 부모 노드보다 작아야하고, 오른쪽 트리의 모든 값은 부모 노드보다 커야함.
- 탐색, 삽입, 삭제의 시간복잡도는 O(h).

### 힙(Heap)
- 최댓값 또는 최솟값을 찾아내는 연산을 쉽게 하기 위해 고안된 구조로, 각 노드의 키값이 자식의 키값보다 작지 않거나(최대힙) 그 자식의 키값보다 크지 않은(최소힙) 완전이진트리.

### 우선순위 큐(Priority Queue)
- 가장 우선순위가 높은 데이터를 먼저 꺼내기 위해 고안된 자료구조. 일반적으로 힙을 사용하는데, 그렇기에 시간복잡도는 O(logn).

### 해시 테이블(Hash Table)
- (key, value) 로 데이터를 저장하는 자료구조. 빠른 데이터 검색이 필요할 때 유용.
- key 값에 해시함수를 적용해 고유한 index를 생성하여 그 index에 저장된 값을 꺼내오는 구조.
- 평균적으로 O(1)의 시간복잡도를 가지지만 index 값 충돌이 발생할 경우 그에 대한 값들을 조회하기 때문에 O(N)까지 증가할 수 있음.

#### 해시맵과 해시 테이블의 차이
- 동기화 지원 여부의 차이. 
- 병렬 처리를 하면서 자원의 동기화를 고려해야 하는 상황이라면 해시테이블을 사용해야 하며, 병렬 처리를 하지 않거나 자원의 동기화를 고려하지 않는 상황이라면 해시맵을 사용.
