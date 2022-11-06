---
title:  "JAVA Stack, Queue"
categories:
  - dataStructure
---

## Stack
- Class
- LIFO구조 
  - Last In First Out
  - 마지막에 저장된 것을 제일 먼저 꺼내게 된다.
  - 정방향 순차적 추가, 역방향 순차적 삭제
- ArrayList로 구현
  - 장점 ) 간단하고 성능 우수
  - 단점 ) 스택의 크기 고정
- LinkedList로 구현
  - 장점 ) 스택의 크기를 필요에 따라 가변
  - 단점 ) 구현 방법 약간 복잡
- 자료의 출력순서가 입력순서의 역순으로 이루어져야 할 경우 사용
  - ex) 뒤로가기 / 되돌리기
  - ex) 시스템 스택을 이용한 함수 호출
    - 함수 실행 후 자신을 호출한 함수로 되돌아감
    - 시스템 스택 : 함수가 호출될 때마다 활성 레코드가 만들어지며 복귀주소가 저장됨

<br/>
<img src="https://user-images.githubusercontent.com/42172353/184630539-eab351ad-1069-47c1-8065-10f86d96aac0.png" width="70%"/>
<br/>
<br/>
<br/>

```java
  Stack<E> st = new Stack<>();
```
<br/>

| 메서드                      | 설명                                                                       | Exception                     |
|--------------------------|--------------------------------------------------------------------------|-------------------------------|
| boolean empty( )         | Stack이 비어있는지 확인                                                          |                               |
| Object push(Object item) | Stack에 객체 저장                                                             |                               |
| Object peek( )           | Stack의 맨 위에 저장된 객체를 삭제없이 반환                                              | 비어있을 때 EmptyStackException 발생 |
| Object pop( )            | Stack의 맨 위에 저장된 객체를 꺼냄                                                   | 비어있을 때 EmptyStackException 발생 |
| int search(Object o)     | Stack에서 주어진 객체를 찾아서 그 위치를 반환<br/>못찾으면 1 반환<br/>(배열과 달리 위치는 0이 아닌 1부터 시작) |


<br/>
<br/>
<br/>
<br/>
<br/>

---

<br/>

## Queue
- Interface
  - Queue를 직접 구현
  - Queue를 구현한 클래스를 사용 (LinkedList 등)
- FIFO구조 
  - Firtst In First Out
  - 제일 먼저 저장한 것을 제일 먼저 꺼내게 된다. 
  - 정방향 순차적 추가, 정방향 순차적 삭제
  - enqueue : 데이터 삽입
  - dequeue : 데이터 삭제
  - rear : 후단. 삽입 발생하는 곳
  - front : 전단. 삭제 발생하는 곳
- LinkedList로 구현
  - 배열 기반의 컬렉션 클래스 사용 시 데이터를 꺼낼 때마다 빈공간을 채우기 위한 데이터의 복사가 발생하므로 비효율적
- 각기 다른 속도로 실행되는 작업의 처리 기간을 조화시키기 위해서 사용
  - ex) 프린터 - Printer Buffer Queue
  - ex) CPU 스케줄 관리 - Scheduling Queue

<br/>
<img src="https://user-images.githubusercontent.com/42172353/184630547-2b5eddd5-f0eb-4e09-b29e-7331d79afb77.png" width="70%" />
<br/>
<br/>
<br/>

```java
  Queue<E> q = new LinkedList<E>();
```
<br/>

| 메서드                    | 설명                                         | Exception                           |
|------------------------|--------------------------------------------|-------------------------------------|
| boolean add(Object o)  | 지정된 객체를 Queue에 추가<br/>성공 시  true, 실패 시 오류  | 저장공간이 부족하면 IllegalStateException 발생 |
| Object offer(Object o) | Queue에 객체 저장<br/>성공 시  true, 실패 시 false 반환 |                                     |
| Object element( )      | 삭제없이 요소를 반환                                | 비어있을 때 NoSuchElementException 발생    |
| Object peek( )         | 삭제없이 요소를 반환                                |                                     |
| Object remove( )       | Queue에서 객체를 꺼내 반환<br/>비어있으면 null을 반환       | 비어있을 때 NoSuchElementException 발생    |
| Object pool( )         | Queue에서 객체를 꺼내 반환<br/>비어있으면 null을 반환       |

<br/>
<br/>
<br/>

---
<br/>

## 선형 큐 Linear Queue
- 배열의 크기 = 큐의 크기 = 큐에 저장할 수 있는 최대 원소의 개수
- 공백 상태 : front = rear = -1
- 포화 상태 : rear가 배열의 마지막 인덱스를 가리킬 때
- 문제
  1. rear가 배열의 마지막 위치에 있기 때문에 앞에 빈자리가 있는데도 포화 상태로 인식하고 삽입 연산을 수행하지 않음
  2. 큐의 빈자리를 사용하려면 저장되어 있는 원소를 배열의 앞으로 이동 시켜야 함
  3. 이동 작업은 연산이 복잡하여 큐의 효율성을 떨어뜨림

<br/>
<img src="https://user-images.githubusercontent.com/42172353/184626484-ece1babb-3357-4928-934b-72c51113126a.png" width="80%"/>



<br/>
<br/>
<br/>

---
<br/>

## 원형 큐 Circular Queue
- 논리적으로 처음과 끝이 연결되어 있는 형태
- 공백 상태와 포화 상태를 쉽게 구분하기 위해서 자리 하나를 항상 비워둔다
- 공백 상태 : front = rear
- 포화 상태 : rear가 마지막 인덱스인 n-1을 가리키고 그 다음이 0일 때
- rear는 앞으로 이동하면서 원소 삽입
- front는 rear가 이동한 방향으로 따라가면서 원소 삭제

<br/>
<img src="https://user-images.githubusercontent.com/42172353/184626675-b15a0349-5a42-4770-b87f-53e7bb1b5c5e.png" width="80%" />
<img src="https://user-images.githubusercontent.com/42172353/184626845-002105e2-3b02-4c27-9dd6-064abfb6722e.png" width="80%" />

<br/>
<br/>
<br/>

---
<br/>

## 덱 Deque (Double-Ended Queue)
- 양쪽 끝에 추가/삭제 가능
- 스택의 성질과 큐의 설질 모두 가지고 있음

<br/>

|         | Deque        | Queue    | Stack   |
|---------|--------------|----------|---------|
| 마지막에 추가 | offerLast( ) | offer( ) | push( ) |
| 첫번째 반환  | peekFirst( ) | peek( )  |         |
| 마지막 반환  | peekLast( )  |          | peek( ) |
| 첫번째 삭제  | pollFirst( ) | poll( )  |         |
| 마지막 삭제  | pollLast( )  |          | pop( )  |

<br/>
<img src="https://user-images.githubusercontent.com/42172353/184630154-990c77ed-19c9-45cf-b1ed-b69a9cdc7e37.png" width="50%" />
<br/>
<br/>
<img src="https://user-images.githubusercontent.com/42172353/184629876-f6b779fa-6c3f-40cc-a66e-f1621d56629e.png" width="80%"/>

<br/>
<br/>
<br/>

---
<br/>

## 우선순위 큐 PriorityQueue
- 저장한 순서에 관계없이 우선순위가 높은 것부터 꺼낸다
  - 우선순위 : 숫자가 작을수록 높은 우선순위
- null 저장 불가
  - null 저장 시 NullPointerException 발생
- 저장 방법
  - 저장 공간 : 배열
  - 각 요소 저장 형식 : heap
- 대표문제 : 최대 수입 스케쥴

<br/>
<img src="https://user-images.githubusercontent.com/42172353/184631103-fded9f8c-0f8b-4028-9779-9715e2aac369.png" width="70%"/>
<br/>
<br/>
<br/>

```java
Queue<Integer> q = new PriorityQueue<>();
q.offer(new Integer(2)); 
q.offer(4); // 오토박싱
q.offer(5);
q.offer(1);
q.offer(3);

Object obj = null;

// 우선순위 출력 - 12345
// poll() : 없을 시 null 반환
while ((obj = q.poll()) != null) {
  System.out.print(obj);
}
```

[우선순위 큐 활용 문제 - 최대수입스케쥴]()

##### [참고 강의 : 남궁성의 정석코딩](https://www.youtube.com/watch?v=ktvhRSRohR4&list=PLW2UjW795-f6xWA2_MUhEVgPauhGl3xIp&index=123)
##### [참고 서적 : 자바로 배우는 쉬운 자료구조](http://www.kyobobook.co.kr/product/detailViewKor.laf?mallGb=KOR&ejkGb=KOR&barcode=9788998756420)