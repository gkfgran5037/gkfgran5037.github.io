---
title:  "JPA 02. 영속성"
categories:
  - jpa
--- 

## 목차

- [목차](#목차)
- [EntityManagerFactory](#entitymanagerfactory)
- [EntityManager](#entitymanager)
- [영속성 컨텍스트](#영속성-컨텍스트)
- [엔티티의 생명주기](#엔티티의-생명주기)
- [영속성 컨텍스트의 특징](#영속성-컨텍스트의-특징)
- [영속성 컨텍스트가 엔티티 관리 시 장점](#영속성-컨텍스트가-엔티티-관리-시-장점)
  - [1. 1차 캐시](#1-1차-캐시)
  - [2. 엔티티의 동일성 보장](#2-엔티티의-동일성-보장)
  - [3. 트랜잭션을 지원하는 쓰기 지연 (transactional write-behind)](#3-트랜잭션을-지원하는-쓰기-지연-transactional-write-behind)
  - [4. 변경 감지 (dirty checking)](#4-변경-감지-dirty-checking)


---
<br/>

## EntityManagerFactory
- EntityManager 인스턴스를 제공
- 서로 다른 DB에 접근할 경우, 복수 개의 EntityManagerFactory 생성 
- 생성 오버헤드가 있으므로 싱글톤 형태로 관리
  - 애플리케이션 전체에서 딱 한 번만 생성하고 공유해서 사용해야 함
  - 여러 스레드가 동시에 접근해도 안전하므로 서로 다른 스레드 간에 공유해도 됨

<br/>
<img width="80%" src="https://user-images.githubusercontent.com/42172353/197914684-6b115024-eb16-4564-9679-3a3a5954b2ba.png"/>
<h5>[출처] 김영한 - 자바 ORM 표준 JPA 프로그래밍 </h5>
<br/><br/><br/>




## EntityManager
- DB 연결을 유지
- JPA의 대부분의 기능(DB와 연동되는 부분)을 제공
- 내부 데이터베이스(데이터베이스 커넥션)를 유지하면서 데이터베이스와 통신
- 데이터베이스 커넥션과 밀접한 관계가 있으므로 스레드간에 공유하거나 재사용하면 안 됨
  - 여러 스레드가 동시에 접근하면 동시성 문제가 발생하므로 스레드 간에 절대 공유하면 안 됨

<br/>
<img width="80%" src="https://user-images.githubusercontent.com/42172353/197370885-f73778c4-9682-44d7-b31d-fd485ad27fed.png">
<h5>[출처] https://letslearnjava.quora.com/Entity-Manager-in-JPA</h5>
<br/>
<img width="80%" src="https://user-images.githubusercontent.com/42172353/197920238-06507727-7c3e-4616-8c07-a45b730561bd.png">
<h5>[출처] 김영한 - 자바 ORM 표준 JPA 프로그래밍 </h5>
<br/><br/><br/><br/><br/>







---
<br/>

## 영속성 컨텍스트
- 엔티티를 영구 저장하는 환경
- 엔티티 매니저로 엔티티를 저장하거나 조회하면 엔티티 매니저는 영속성 컨텍스트에 엔티티를 보관하고 관리함

<br/><br/><br/>




## 엔티티의 생명주기
1. 비영속(new/transient)
   - 영속성 컨텍스트와 전혀 관계가 없는 상태
   - ex) 엔티티 객체 생성
2. 영속(managed)
   - 영속성 컨텍스트에 저장된 상태
   - 영속성 컨텍스트가 관리하는 엔티티 = 영속 상태
   - ex) em.persist(), em.find()
3. 준영속(detached)
   - 영속성 컨텍스트에 저장되었다가 분리된 상태
   - 영속성 컨텍스트가 관리하던 영속 상태의 엔티티를 영속성 컨텍스트가 관리하지 않으면 중영속 상태가 됨
   - ex) em.detach(), em.close(), em.clear()
4. 삭제(removed)
   - 삭제된 상태
   - 엔티티를 영속성 컨텍스트와 데이터베이스에서 삭제

<br/>
<img width="80%" src="https://user-images.githubusercontent.com/42172353/197922134-dcaadc8a-8b8f-4ed3-a092-2b268bd55627.png" />
<h5>[출처] 김영한 - 자바 ORM 표준 JPA 프로그래밍 </h5>
<br/><br/><br/><br/><br/>






---
<br/>

## 영속성 컨텍스트의 특징
1. 영속성 컨텍스트는 엔티티를 식별자 값(@Id로 테이블의 PK와 매핑한 값)으로 구분
   - 영속 상태는 식별자 값이 반드시 있어야 함
2. flush
   - 보통 트랜잭션을 커밋하는 순간 영속성 컨텍스트에 새로 저장된 엔티티를 데이터베이스에 반영
   - 영속성 컨텍스트의 변경 내용을 데이터베이스에 동기화
   - 동작
     1. 변경 감지 - 스냅샷 비교 후 수정된 엔티티 찾음
     2. 수정 쿼리를 쓰기 지연 SQL에 저장
     3. 쓰기 지연 SQL 저장소의 쿼리를 데이터베이스에 전송
<br/><br/><br/>




## 영속성 컨텍스트가 엔티티 관리 시 장점
<br/>

### 1. 1차 캐시
영속성 컨텍스트 내부 캐시   
영속 상태의 엔티티는 모두 이곳에 저장됨   
find(Entity.class, id)
  1. 1차 캐시에서 엔티티 조회
  2. 캐시에 없을 시  데이터베이스에서 조회

<br/>

```java
Member member = new Member();
memeber.setId("member1");
memeber.setUsername("회원1");

// 1차 캐시에 저장
em.persist(member);

// 1차 캐시에서 조회
Member findMember = em.find(Memeber.class, "member1");
```
<br/>

<img width="80%" src="https://user-images.githubusercontent.com/42172353/197925173-8765babd-3d1f-415a-b6b3-e766438daea3.png" />
<h5>[출처] 김영한 - 자바 ORM 표준 JPA 프로그래밍 </h5>
<br/><br/><br/>




### 2. 엔티티의 동일성 보장
em.find()를 반복해서 호출해도 영속성 컨텍스트는 1차 캐시에 있는 같은 엔티티 인스턴스를 반환함
  - 동일성 : 실제 인스턴스가 같음. ==
  - 동등성 : 실제 인스턴스는 다를 수 있지만 인스턴스가 가지고 있는 값이 같음. equals()

<br/>

```java
Member a = em.find(Member.class, "memeber1");
Member b = em.find(Member.class, "memeber1");

// 동일성 비교
System.out.println(a == b); // true
```
<br/><br/><br/>


### 3. 트랜잭션을 지원하는 쓰기 지연 (transactional write-behind)
트랜잭션을 커밋하기 직전까지 데이터베이스에 엔티티를 저장하지 않고 내부 쿼리 저장소에 SQL을 모아둠

<br/>

<img width="80%" src="https://user-images.githubusercontent.com/42172353/197927379-5c878b06-672c-43be-a9c9-fe5610b9af0a.png" />
<h5>[출처] 김영한 - 자바 ORM 표준 JPA 프로그래밍 </h5>
<br/>
<img width="80%" src="https://user-images.githubusercontent.com/42172353/197927132-2af5f512-087c-44f3-983f-1c8d84f1de97.png" />
<h5>[출처] 김영한 - 자바 ORM 표준 JPA 프로그래밍 </h5>
<br/><br/><br/>



### 4. 변경 감지 (dirty checking)
엔티티의 변경사항을 데이터베이스에 자동으로 반영하는 기능   
영속성 컨텍스트가 관리하는 영속 상태의 엔티티에만 적용됨

<br/>
<img width="80%" src="https://user-images.githubusercontent.com/42172353/197928825-0cc3c6f7-daea-436f-a661-136906e8f583.png" />
<h5>[출처] 김영한 - 자바 ORM 표준 JPA 프로그래밍 </h5>






<br/><br/><br/><br/><br/>
<h5>

[참고 강의 : 스프링과 JPA를 이용한 웹개발](http://www.kocw.net/home/cview.do?cid=5e6aec4a9ae2dd45)   
[참고 서적 : 김영한 - 자바 ORM 표준 JPA 프로그래밍](https://product.kyobobook.co.kr/detail/S000000935744)

</h5>