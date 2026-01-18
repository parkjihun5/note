## 집합(Set)
값이 다른 원소를 순서 없이 저장하는 자료구조
특징
    같은 값의 원소는 중복되지 않음
    *같은 값을 여러 번 입력해도 하나만 유지됨
    색인되지 않으며, 순서가 없고 정렬되지 않음
    *배열 또는 리스트와 다르게 인덱스로 접근할 수 없음
    *다른 자료구조와 조합하여 순서나 색인, 또는 정렬이 있는 집합을 만들 수도 있음
    두 개의 집합의 연산이 가능
    *합집합(Union), 교집합(Intersection), 차집합(Difference), 대칭 차집합(Symmetric) 등

### 자바에서 집합 사용하기
실습에 사용할 구현체: HashSet
    집합의 구현을 위해 해시 맵(Hash map)을 사용
        *해시 맵(Hash map)
        해시 함수(Hash function)는 임의의 길이를 가진 데이터를 입력받아 고정된 길이의 값, 즉 해시 값(Hash value)을 출력하는 함수
        해시 테이블(Hash table)은 해시 값을 배열의 인덱스처럼 활용해 데이터를 저장하는 자료구조
        해시 맵(Hash map)은 키-값 쌍을 함께 저장할 수 있는 자료구조로, 키에 해시 함수를 적용한 해시 값을 인덱스로 사용
        자바에서 해시 맵(Hash map) 구현체를 제공함
    기본적인 삽입, 삭제 및 원소 포함 검사의 성능이 좋음
기본 기능
    삽입은 add(E e) 메소드 사용
    삭제는 remove(Object o) 메소드 사용
    원소 포함 검사는 contains(Object o) 메소드 사용
반복문(Iteration)을 통해 순차적으로 각 원소에 접근할 수 있음
집합연산
    합집합(Union)은 addAll(Collection<? extends E> C) 메소드 사용
    교집합(Intersection)은 retainAll(Collection<?> c) 메소드 사용
    차집합(Difference)는 removeAll(Collection <?> c) 메소드 사용

### 다양한 집합 구현체
자바 Set 인터페이스의 구현체들
    ConcurrentSkipListSet, CopyOnWriteArraySet, EnumSet, HashSet, JobStateReasons,
    LinkedHashSet, TreeSet
LinkedHashSet
    HashSet 구현체에 연결 리스트(Linked list)를 접목한 구현체
    원소를 입력한 순서를 보존
TreeSet
    Set과 SortedSet 인터페이스를 구현하는 구현체
    입력 순서와 상관없이 입력된 원소를 정렬하여 보관

### 집합은 어디에 사용?
중복제거
    데이터에서 중복을 제거하고 유일한 값들만을 보유하는데 유용함
멤버십 확인
    특정요소가 집합에 속해 있는지 여부를 효과적으로 확인할 수 있음
    데이터 구조나 알고리즘에서 특정 요소의 존재 여부를 판별할 때 유용함
집합연산
    합집합, 교집합, 차집합, 여집합 등
    데이터를 조작하거나 분석하는데 활용
데이터 필터링
    특정 조건을 충족하는 요소들만을 집합으로 추출하여 처리할 수 있음
수학적 모델링
    수학적인 개념을 모델링하여 수학적 문제를 해결할 수 있음

### IT 개발 현장에서의 쓰임새
캐싱
    이미 계산된 데이터를 임시로 저장하고, 나중에 동일한 요청이 있을 때 저장된 결과를 사용해서 성능을 향상시키는 기술
    순차적으로 집합을 사용해 LRU(Least Recently Used) 캐시를 구현할 수 있음
순차적 집합(Oedered set)
    집합과 큐(Queue)를 조합하여 구현
    집합을 이용해 캐시 검사(Cache hit)
    데이터가 사용되지 않을수록 큐의 뒤로 밀려나다, 큐에서 삭제될 때 집합에서도 함께 삭제
    자바의 경우, LinkedHashSet으로 구현에 필요한 기능을 제공

### 심화 주제
해시테이블
다익스트라 최단거리(Dijkstra'sshortest path)
LRU / LFU 캐시