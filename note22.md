## 큐(queue)
데이터를 저장할 수 있고, 삽입한 순서대로 데이터가 출력되는 자료구조

### 선입선출(FIFO, First-In-First-Out)
    삽입했던 데이터를 출력할 때, 가장 먼저 들어온 데이터가 가장 먼저 나가는 특징
    큐에 새로운 데이터를 삽입할 때는 큐의 뒤(rear)에 추가되고, 데이터를 출력할 때는 큐의 앞(front)에서 삭제됨
### 인큐(Enqueue)와 디큐(Dequeue)
    인큐(Enqueue): 큐에 데이터를 삽입하는 작업
    디큐(Dequeue): 큐에서 데이터를 출력 및 삭제하는 작업

### 큐의 동작 방식
파이프 같은 형태로 주로 표시
빈 큐의 안에 데이터를 인큐하고 디큐하면 먼저 인큐한 데이터 순서대로 출력함

### 자바에서 큐 사용하기
다양한 큐(Queue)의 구현체
종류: 
    ArrayBlockingQueue, ArrayDeque, ConcurrentLinkedDeque, ConcurrentLinkedQueue, DelatQueue,
    LinkedBlockingQueue, LinkedBlockingQueue, LinkedList, LinkedTransferQueue, PriorityBlockingQueue,
    PriorityQueue, SynchronousQueue
실습에 사용할 구현체: LinkedList(연결리스트)
    큐와 리스트, 그리고 데크,(Deque, Double-ended Queue)의 성격을 함께 지닌 자료구조 클래스
    - 데크: 큐의 양쪽으로 인큐 / 디큐가 가능한, 큐를 확장한 자료구조
    인큐를 위해 offer(E e) 함수를, 디큐를 위해 poll() 함수 사용

### 큐 vs 스택

    큐(Queue)
입츌력 순서 : 선입선출(FIFO)
삽입연산의 이름 : enqueue
출력(삭제) 연산의 이름: dequeue
사용 예시:
    대기열 순서
    작업 예약
    너비 우선 탐색(BFS, Breadth-First-Search) 구현
e.g.: 터널, 먼저 진입 한 차가 먼저 나감

    스택(Stack)
입출력 순서: 후입선출(LIFO)
삽입연산의 이름: push
출력(삭제) 연산의 이름:pop
사용 예시:
    웹 브라우저 뒤로가기
    실행 취소(Undo)
    깊이 우선 탐색(DFS, Depth-First Search) 구현
    수식의 괄호와 연산 순서 검사
e.g.: 엘리베이터, 마지막에 탄 사람이 먼저 내림

### 실생활 예시
웹 서버의 트래픽으로 인해 응답 지연이 발생할 때, 사용자의 요청을 큐에 보관하고 순차적으로 처리
약간의 지연을 감수하는 대신에 서버의 응답률이 향상됨
*지연이 너무 길지 않은 이상, 오류를 발생시키는 것보다 약간 느린 것이 나음

IT 개발 현장에서 사용
메시지 브로커
    백엔드의 서비스들 간 데이터를 주고 받는 사이에서 데이터를 보관하는, 큐와 같은 프로그램
    통신 장애 또는 수신 서비스가 다운됐을 때 데이터 유실을 최소화
대표적인 메시지 브로커
    RabitMQ
    Amazon SNS / SQS
    Kafka

### 심화 주제
원형 큐
우선순위 큐
메시지 브로커
큐잉 이론