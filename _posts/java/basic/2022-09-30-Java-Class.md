---
title:  "JAVA 객체 지향"
categories:
  - dataStructure
---


## 클래스
- 객체를 정의해 놓은 것
- 객체를 생성하는데 사용됨

<br/><br/><br/>



## 객체
- 클래스에 정의된 내용대로 메모리에 생성된 것
- 인스턴스
  - **인스턴스화** : 클래스로부터 객페를 만드는 과정
  - **인스턴스(instance)** : 클래스로부터 만들어진 객체를 그 클래스의 인스턴스라고 함
  - 클래스 -- 인스턴스화 --> 인스턴스
- 구성 요소
  - **속성(property)** : 멤버변수(memeber variable), 특성(attribute), 필드(field), 상태(state)
  - **기능(function)** : 메서드(method), 함수(finction), 행위(behavir)

<br/><br/><br/><br/><br/>



---
<br/>

## 지역변수 (local variable)
- 메서드 내에 선언
- 매서드 내에서만 사용 가능
- 메서드 종료 시 소멸되어 사용할 수 없음

<br/><br/><br/>


## 인스턴스변수 (instance variable)
- 클래스 영역에 선언
- 인스턴스마다 고유한 상태를 유지해야 하는 속성의 경우 사용
  - 인스턴스는 독립적인 저장공간을 가지므로 서로 다른 값을 가질 수 있음
- 클래스의 인스턴스를 생성할 때 만들어짐
  - 값을 읽어 오거나 저장하기 위해서는 먼저 인스턴스를 생성해야 함  

<br/><br/><br/>



## 클래스변수 (class variable)
- static 인스턴스변수
- 한 클래스의 모든 인스턴스들이 공통적인 값을 유지해야 하는 속성의 경우 사용
  - 모든 인스턴스가 공통된 저장공간(변수)을 공유하게 됨
  - 같은 저장공간을 참고하므로 항상 같은 값을 가짐
- 인스턴스를 생성하지 않고도 언제라도 바로 사용할 수 있음
  - 사용 방법 ) 클래스명.클래스변수
- 클래스가 메모리에 로딩될 때 생성되어 프로그램이 종료될 때 까지 유지됨
- public static 인스턴스 변수 
  - 같은 프로그램 내에서 어디서나 접근할 수 있는 전역변수(global variable)의 성격을 갖음

<br/>

```java
class Card {
  // 인스턴스 변수 - 자신만의 문의와 숫자를 유지하고 있어야 함
  String kind;
  int number;

  // 클래스 변수 - 모든 인스턴스기 공통적으로 같은 값을 유지해야 함
  //              한 카드의 withd 값 변경 해도 모든 카드의 width값이 변경됨
  static int width = 100;
  static int height =250;
}

class CardTest {
  public static void main(String args[]) {
    Card c1 = new Card();
    // c1 : Heart 7, 100 / 250
    c1.kind = "Heart"; 
    c1.number = 7;

    // c2 : Spade 4, 100 / 250
    Card c2 = new Card();
    c2.kind = "Spade"; 
    c2.number = 4;

    // c1 : Heart 7, 50 / 80
    // c2 : Spade 4, 50 / 80
    c1.width = 50;
    c1.height = 80;
  }
}
```


