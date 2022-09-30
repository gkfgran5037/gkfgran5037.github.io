---
title:  "JAVA 기초"
categories:
  - dataStructure
---

## 변수 (variable)
- 하나의 값을 저장하기 위한 메모리 공간
- 값 변경 가능
- 하나의 값만 저장할 수 있으므로, 새로운 값을 저장하면 기존의 값은 사라짐
- 변수명 생성 규칙
  - 영문자, 숫자, 언더스코어(_), 달러($)로만 구성 가능
  - 숫자로 시작할 수 없음
  - 공백 포함할 수 없음
  - 자바의 예약어를 사용할 수 없음
- 초기화
  - 초기화 : 변수를 사용하기 전에 처음으로 값을 저장하는 것
  - 지역변수 : 사용되기 전에 초기화를 반드시 해야 함
  - 클래스변수, 인스턴스변수 : 초기화 생략 가능

<br/>

```java
int score;   // 변수 초기화 방법 1 ) 변수의 선언만 하는 방법
score = 100;

// 변수타입 변수명 = 값;
int age = 2; // 변수 초기화 방법 2 ) 변수 선언과 동시에 초기화
```
  
<br/><br/>



### 기본형 변수 (primitive Type)
- 실제 값을 저장
- 실제 연산에 사용되는 변수

<br/>

| 자료형 | 기본형 변수                 |
|-----|------------------------|
| 정수형 | byte, short, int, long |
| 실수형 | float, double          |
| 문자형 | char                   |
| 논리형 | boolean                |

<br/><br/>



### 참조형 변수 (reference Type)
- 어떤 값이 저장되어 있는 주소(memort address)를 값으로 갖음
- 기본형 변수 외 모두 참조 변수
- 선언 시 변수의 타입으로 클래스의 이름을 사용하므로 클래스의 이름이 참조변수의 타입이 됨

<br/>

```java
// 클래스명 변수이름 = new 클래스명;
Date today = new Date();
```
<br/><br/><br/><br/><br/>





---
<br/>

## 상수 (constant)
- final 변수
- 값을 한번만 저장할 수 있는 메모리 공간
- 한 번 값을 저장하면 다른 값으로 변경할 수 없음
- 선언과 동시에 반드시 초기화해야 함 (JDK 1.6부터 선언과 동시에 초기화 하지 않아도 허용)
- 초기화 이후 상수의 값 변경 불가
- 상수명 생성 규칙
  - 모두 대문자로 지정
  - 여러 단어로 이루어진 경우 언더스코어(_)로 구분
- 장점
  - 리터럴에 의미있는 이름을 붙여서 코드의 이해와 수정을 쉽게 만듦

<br/>

```java
final int MAX_SCORE = 100; // final 타입 상수명 = 값;
```
<br/><br/><br/>




## 리터럴 (literal)
- 그 자체로 값을 의미하는 것

<br/>

```java
int age = 2; // 타입 변수명 = 값(리터럴);
final int MAX_SCORE = 100; // final 타입 상수명 = 값(리터럴);
```
<br/><br/><br/><br/><br/>






[참고 서적 - 자바의 정석](http://www.kyobobook.co.kr/product/detailViewKor.laf?ejkGb=KOR&mallGb=KOR&barcode=9788994492032&orderClick=LEa&Kc=)  
[참고 사이트 - TCP SCHOOL.com](http://www.tcpschool.com/java/java_datatype_variable)