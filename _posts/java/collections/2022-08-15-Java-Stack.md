---
title:  "JAVA Stack"
categories:
  - dataStructure
---

## Stack
LIFO구조 
  - Last In First Out
  - 마지막에 저장된 것을 제일 먼저 꺼내게 된다.
배열 기반의 컬렉션 클래스로 구현
  - 정방향 순차적 추가
  - 역방향 순차적 삭제
  - ex) ArrayList

| 메서드                      | 설명                                                                       | Exception                     |
|--------------------------|--------------------------------------------------------------------------|-------------------------------|
| boolean empty( )         | Stack이 비어있는지 확인                                                          |                               |
| Object push(Object item) | Stack에 객체 저장                                                             |                               |
| Object peek( )           | Stack의 맨 위에 저장된 객체를 삭제없이 반환                                              | 비어있을 때 EmptyStackException 발생 |
| Object pop( )            | Stack의 맨 위에 저장된 객체를 꺼냄                                                   | 비어있을 때 EmptyStackException 발생 |
| int search(Object o)     | Stack에서 주어진 객체를 찾아서 그 위치를 반환<br/>못찾으면 1 반환<br/>(배열과 달리 위치는 0이 아닌 1부터 시작) |