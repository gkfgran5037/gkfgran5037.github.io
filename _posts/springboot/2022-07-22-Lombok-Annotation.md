---
title:  "Lombok Annotation"
categories:
  - springBoot
---

## 목차

- [목차](#목차)
- [@Getter @Setter](#getter-setter)
- [@RequiredArgsConstructor](#requiredargsconstructor)
- [@NoArgsConstructor](#noargsconstructor)
- [Builder](#builder)

<br/><br/><br/><br/><br/>






---
<br/>

## @Getter @Setter
- getter : 선언한 모든 필드의 get 메소드를 생성
- setter : 선언한 모든 필드의 set 메소드를 생성
  - JPA Entity 클래스에서는 Setter 메소드를 만들지 않고 목적과 의도를 나타낼 수 있는 메소드를 추가해야 함

<br/>
<br/>

## @RequiredArgsConstructor
선언된 모든 final 필드가 포함된 생성자를 생성

<br/>

```java
import lombok.Getter;
import lombok.Setter;

@Getter // 선언한 모든 필드의 get 메소드를 생성
@Setter // 선언한 모든 필드의 set 메소드를 생성
@RequiredArgsConstructor // 선언된 모든 final 필드가 포함된 생성자를 생성
public class HelloData {
	private String username;
	private int age;
}
```

<br/>
<br/>

## @NoArgsConstructor
기본 생성자 자동 추가
  - public Posts() {} 와 같은 효과

## Builder
- 해당 클래스의 빌더 패턴 클래스를 생성
- 해당 클래스 상단에 선언 시 생성자에 포함된 필드만 빌더에 포함

```java
@Getter
@NoArgsConstructor // 기본 생성자 자동 추가
public class Posts {
  private String title;
  private String content;
  
  @Builder // 해당 클래스의 빌더 패턴 클래스를 생성
  public Posts(String title, String content) {
      this.title = title;
      this.content = content;
  }
}
```
