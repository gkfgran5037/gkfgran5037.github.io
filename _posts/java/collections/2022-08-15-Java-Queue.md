---
title:  "JAVA Queue"
categories:
  - dataStructure
---

## Queue
FIFO구조 
  - Firtst In First Out
  - 제일 먼저 저장한 것을 제일 먼저 꺼내게 된다.
 기반의 컬렉션 클래스로 구현
  - 정방향 순차적 추가
  - 정방향 순차적 삭제
  - 배열기반의 컬렉션 클래스 사용 시 데이터를 꺼낼 때마다 빈공간을 채우기 위한 데이터의 복사가 발생하므로 비효율적
  - 데이터의 추가 삭제가 쉬운 컬렉션 클래스 사용
  - ex) LinkedList

| 메서드                    | 설명                                         | Exception                           |
|------------------------|--------------------------------------------|-------------------------------------|
| boolean add(Object o)  | 지정된 객체를 Queue에 추가<br/>성공 시  true, 실패 시 오류  | 저장공간이 부족하면 IllegalStateException 발생 |
| Object offer(Object o) | Queue에 객체 저장<br/>성공 시  true, 실패 시 false 반환 |                                     |
| Object element( )      | 삭제없이 요소를 반환                                | 비어있을 때 NoSuchElementException 발생    |
| Object peek( )         | 삭제없이 요소를 반환                                |                                     |
| Object remove( )       | Queue에서 객체를 꺼내 반환<br/>비어있으면 null을 반환       | 비어있을 때 NoSuchElementException 발생    |
| Object pool( )         | Queue에서 객체를 꺼내 반환<br/>비어있으면 null을 반환       |