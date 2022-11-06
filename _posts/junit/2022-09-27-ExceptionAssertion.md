---
title:  "AssertJ Exception Assertions"
categories:
  - junit
---

## Exception Assertions
- 예외가 발생했다고 주장하거나 예상한 예외인지 확인하기 위해 사용
- 사용 방식
  - JUnit5 : assertThrows(Exception.class, () -> {})
  - JUnit4 : @Test(expected = Exception.class)

<br/>
<br/>
<br/>

---
<br/>

## assertThrows()
- org.junit.jupiter.api.Assertions
- Assertions.assertThrows(Exception.class, () -> 실행내용)
<br/>

```java
// 테스트 성공
@Test
void 숫자외의값() {
    Assertions.assertThrows(NumberFormatException.class, () -> splitAndSum("ab,c"));

}
```
<br/>

```java
// 메시지까지 체크
@Test
void 숫자외의값() {
    String data = "abc";
    Exception exception = Assertions.assertThrows(NumberFormatException.class, () -> splitAndSum(data));
    Assertions.assertEquals("For input string: \"" + data + "\"", exception.getMessage());
}
```

<br/>
<br/>
<br/>

---
<br/>

## assertThatThrownBy()
- org.assertj.core.api.Assertions
- 인자로 받은 람다에서 익셉션이 발생하는지 검증
   
### isInstanceOf()
- 익셉션의 타입을 추가로 검증

<br/>

```java
// 테스트 성공
@Test
void 숫자외의값() {
    assertThatThrownBy(() -> splitAndSum("abc")).isInstanceOf(NumberFormatException.class);
}
```
<br/>

```java
// 메시지까지 체크
@Test
void 숫자외의값() {
    assertThatThrownBy(() -> splitAndSum("abc"))
        .isInstanceOf(NumberFormatException.class)
        .hasMessageStartingWith("For input string: ");
}
```

<br/>
<br/>
<br/>

---
<br/>

## assertThatExceptionOfType()
- 발생할 익셉션의 타입을 지정하고 isThrownBy() 메서드를 이용해서 익셉션이 발생할 코드 블록을 지정

<br/>

```java
@Test
void 숫자외의값() {
    String data = "abc";
    assertThatExceptionOfType(NumberFormatException.class)
        .isThrownBy(() -> splitAndSum(data))
        .withMessageMatching("For input string: \"" + data + "\"");
}
```


[공식 문서 - AssertJ Exception Assertions](https://joel-costigliola.github.io/assertj/assertj-core-features-highlight.html#exception-assertion)  
[참고 사이트 - digitalocean.com](https://www.digitalocean.com/community/tutorials/junit-assert-exception-expected)   
[참고 블로그 - covenant.tistory](https://covenant.tistory.com/256)
[참고 블로그 - incheol-jung.gitbook](https://incheol-jung.gitbook.io/docs/study/undefined-3/d.-assertj)
