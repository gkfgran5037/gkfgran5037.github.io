---
title:  "JAVA Collections Framework"
categories:
  - dataStructure
---

## 컬렉션 프레임웍 (Collections Framework)
데이터 그룹을 저장하는 클래스들을 표준화한 설계  
컬렉션(다수의 객체)을 다루기 위한 표준화된 프로그래밍 방식

<br /><br /><br />




---
### 인터페이스

| 인터페이스 | 특징                                                                                 |
|-------|------------------------------------------------------------------------------------|
| List  | 순서가 있는 데이터의 집합. 데이터의 중복을 허용한다.                                                     |
| Set   | 순서를 유지하지 않는 데이터의 집합. 데이터의 중복을 허용하지 않는다.                                            |
| Map   | 키(Key)와 값(Value)의 쌍으로 이루어진 데이터의 집합.<br/>순서는 유지되지 않으며, 키는 중복을 허용하지 않고 값은 중복을 허용한다.  |

<br /><br /><br />




### 메서드

| 메서드                                                              | 설명                      |
|------------------------------------------------------------------|-------------------------|
| boolean add(Object o)<br/>boolean addAll(Collection c)           | 객체 추가                   |
| void clear( )                                                    | 모든 객체 삭제                |
| boolean contains(Object o)<br/>boolean containsAll(Collection c) | 지정한 객체가 포함돼 있는지 확인      |
| boolean equals(Object o)                                         | 동일한 컬렉션인지 확인            |
| int hashcode( )                                                  | hash code 반환            |
| boolean isEmpty( )                                               | 컬렉션이 비어있는지 확인           |
| Iterator iterator( )                                             | Iterator 반환             |
| boolean remove(Object o)<br/>boolean removeAll(Collection c)     | 지정된 객체 삭제               |
| int size( )                                                      | 컬렉션에 저장된 객체의 개수 반환      |
| Object[ ] toArray( )                                             | 저장된 객체를 객체배열로 반환        |
| Object[ ] toArray(Object[ ] a)                                   | 지정된 배열에 컬렉션 객체를 저장해서 반환 |

<br /><br /><br /><br /><br />







---

## List
순서가 있는 데이터의 집합.  
데이터의 중복을 허용한다.

<br />

| 메서드                                                              | 설명                      |
|------------------------------------------------------------------|-------------------------|
| boolean add(Object o)<br/>boolean addAll(Collection c)           | 객체 추가                   |
| Object get(int idex)                                             | 지젇된 위치 객체 반환        |
| int indexOf(Object o)<br/>int lastIndexOf(Object o)              | 지정된 위치 반환            |
| boolean remove(Object o)<br/>boolean removeAll(Collection c)     | 지정된 객체 삭제               |
| Object set(int idex, Object element)                             | 지정된 위치에 객체 저장 (변경)  |
| List subList(int fromIndex, int toIndex)                         | 지정된 범위에 있는 객체 반환    |

<br /><br /><br />




---

## Set
순서를 유지하지 않는 데이터의 집합.  
데이터의 중복을 허용하지 않는다.

<br />

| 메서드                                                              | 설명                      |
|------------------------------------------------------------------|-------------------------|
| boolean addAll(Collection c)                                     | 지정된 Collection 객체들을 추가 (합집합)           |
| boolean containsAll(Collection c)                                | 지정된 Collection 객체들의 포함 여부 확인 (부분집합)  |
| boolean removeAll(Collection c)                                  | 지정된 Collection 객체들을 삭제 (차집합)           |
| boolean reatinAll(Collection c)                                  | 지정된 Collection 객체들만 남기고 삭제 (교집합)     |

<br /><br /><br />




---

## Map
키(Key)와 값(Value)의 쌍으로 이루어진 데이터의 집합.  
순서는 유지되지 않으며, 키는 중복을 허용하지 않고 값은 중복을 허용한다.

<br />

| 메서드                                                              | 설명                      |
|------------------------------------------------------------------|-------------------------|
| boolean containsKey(Object key)                                  | 지정된 key 객체가 있는지 확인     |
| boolean containsValue(Object value)                              | 지정된 value 객체가 있는지 확인     |
| Set entrySet()                                                   | key-value 쌍을 Map.Entry타입의 객체로 저장한 Set으로 반환 |
| Object get(Object key)                                           | key객체에 대응하는 value 반환   |
| Object put(Object key, Object value)                             | key-value 추가             |
| Object remove(Object key)                                        | 지정된 key-value 삭제       |


<br /><br /><br /><br /><br />






---

### Collecions
컬렉션과 관련된 메서드 제공

| 메서드                                    | 설명     |
|----------------------------------------|--------|
| void sort(List<T> list)                | 정렬     |
| T max(Collection<? extends T> coll)    | 최대값 추출 |
| T min(Collection<? extends T> coll)    | 최솟값 추출 |
| void fill(List<? super T> list, T obj) |        |
| Comparator<T> reverseOrder()           |

