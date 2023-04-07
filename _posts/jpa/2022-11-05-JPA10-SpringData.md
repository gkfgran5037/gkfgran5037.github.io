---
title: "JPA10. Spring Data JPA"
categories:
  - jpa
---

## 목차

- [목차](#목차)
- [스프링 데이터 JPA](#스프링-데이터-jpa)





<br/><br/><br/><br/><br/>



---
<br/>

## 스프링 데이터 JPA
- 스프링 프레임워크에서 JPA를 편리하게 사용할 수 있도록 지원하는 프로젝트
- 데이터 접근 계층을 개발할 떄 구현 클래스 없이 인터페이스만 작성해도 개발을 완료할 수 있음
- 구조
  - Repository : 최상위 인터페이스
  - CrudRepositry : CRUD 관련 기능 명세
  - PagingAndSortingRepository : 페이징 및 sorting 관련
  - JpaRepositry : JPA 특화 기능
  
<br/>
<img width="80%" src="https://user-images.githubusercontent.com/42172353/200109767-4ccb9db7-1256-43ae-bb81-b99d763850f4.png" />
<img width="80%" src="https://user-images.githubusercontent.com/42172353/200109824-1f72abca-73be-4553-9e7c-23a838b0c284.png" />
<h5>[출처 - spring.io] https://spring.io/projects/spring-data </h5>
<br/><br/>















<br/><br/><br/><br/><br/>
<h5>

[참고 강의 : 스프링과 JPA를 이용한 웹개발](http://www.kocw.net/home/cview.do?cid=5e6aec4a9ae2dd45)   
[참고 서적 : 김영한 - 자바 ORM 표준 JPA 프로그래밍](https://product.kyobobook.co.kr/detail/S000000935744)

</h5>