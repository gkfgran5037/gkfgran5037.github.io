---
title:  "SpringBoot 03 테스트 코드 JUnit vs assertJ"
categories:
  - Spring Boot
---

## JUnit
<br/>

| assertTrue(boolean condition)                           | 진실 여부 확인        |
|---------------------------------------------------------|-----------------|
| assertFalse(boolean condition)                          | 거짓 여부 확인        |
| assertEqueals(Object expected, Object actual)           | 객체의 값 동일 여부 확인  |
| assertNotEqueals(Object expected, Object actual)        | 객체의 값 불일치 여부 확인 |
| assertArrayEquals(Object[] expecteds, Object[] actuals) | 배열 일치 여부 확인     |
| assertSame(Object expected, Object actual)              | 같은 객체인지 여부 확인   |
| assertNotSame(Object expected, Object actual)           | 다른 객체인지 여부 확인   |

<br/>
<br/>
<br/>
<br/>
<br/>

---
<br/>

## assertj     
- CoreMathers와 달리 추가적으로 라이브러리가 필요하지 않음
  - JUnit의 seertThat을 쓰게 되면 is()와 같이 CoreMatchers 라이브러리가 필요함
- 자동완성이 좀 더 확실하게 지원됨
  - IDE에서는 CoreMathcers와 같은 Matcher 라이브러리의 자동완성 지원이 약함

<br/>

| assertThat(Predicate<T> actual) | 검증하고 싶은 대상을 메소드 인자로 받음<br/>메소드 체이닝이 지원되어 isEqualTo와 같이 메소드를 이어서 사용 가능 |
|---------------------------------|-----------------------------------------------------------------------|
| isEqualTo                       | 동일 여부 확인<br/>assertThat에 있는 값과 isEqualTo의 값을 비교해서 같을 때만 성공            |

