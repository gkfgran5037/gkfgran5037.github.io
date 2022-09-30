---
title:  "JAVA 제어자"
categories:
  - dataStructure
---

## 제어자
- 클래스, 변수 또는 메서드의 선언부에 함께 사용되어 부가적인 의미 부여
- 클래스나 멤버변수와 메서드에 주로 사용 
- 하나의 대상에 대해서 여러 제어자를 조합하여 사용하는 것 가능
  - 예외) 접근 제어자 : 4개 중 하나만 선택 가능

<br/><br/>



## static - 클래스의, 공통적인
- static이 사용될 수 있는 곳
  - 멤버변수, 메서드, 초기화 블럭
  - static이 붙은면 클래스에 관계된 것이므로 인스턴스를 생성하지 않고도 사용 가능 
- **클래스변수(static 변수)** 
  - static 멤버변수
  - 인스턴스에 관계없이 같은 값을 갖음
  - 모든 인스턴스에 공통적으로 사용되는 클래스 변수
  - 인스턴스 생성하지 않고도 사용 가능
  - 클래스가 메모리에 로드될 때 생성됨
- **메서드**
  - static 메서드
  - 인스턴스를 생성하지 않고도 호출이 가능한 static 메서드
  - static 메서드 내에서는 인스턴스멤버들을 직접 사용할 수 없음
  - 인스턴스 멤버를 사용하지 않는 메서드는 static 메서드로 선언 고려
    - 인스턴스를 생성하지 않고도 호출이 가능해서 더 편리하고 속도도 더 빠름

<br/><br/><br/>





## final - 마지막의, 변경될 수 없는
- **final 클래스**
  - 변경될 수 없는 클래스, 확장될 수 없는 클래스
  - 다른 클래스의 조상이 될 수 없음
- **final 메서드**
  - 변경될 수 없는 메서드
  - 오버라딩을 통해 재정의 될 수 없음
- **final 멤버변수/지역변수**
  - 값을 변경할 수 없는 상수
  - 일반적으로 선언과 초기화를 동시에 함
  - 인스턴스 변수 : 생성자에서 초기화 가능
    - 각 인스턴스마다 final이 붙은 멤버변수가 다른 값을 갖도록 하는 것 가능

<br/>

```java
class Card {
    final int NUMBER;
    final String KIND;
    static int width = 100;
    static int height = 250;

    Card(String kind, int num) {
        KIND = kind;
        NUMBER = num;
    }
}

class FinalCardTest {
    public static void main(String args[]) {
        Card c = new Card("Heart", 10); // 객체 생성 시 초기화
        //c.NUMBER = 5; // 에러 발생 - 상수 변경 불가
        c.KIND; // 가능
        c.NUMBER; // 가능
    }
}
```

<br/><br/><br/>




## abstract - 추상의, 미완성의
- 메서드의 선언부만 작성하고 실제 수행내용은 구현하지 않은 추상 메서드를 선언하는데 사용
- 클래스에 사용되어 클래스 내에 추상메서드가 존재한다는 것을 쉽게 알 수 있게 함
- **abstract 클래스**
  - 클래스 내에 추상 메서드가 선언되어 있음
- ** abstract 메서드**
  - 선언부만 작성하고 구현부는 작성하지 않은 추상 메서드  






<br/><br/><br/><br/><br/>

---
## 접근 제어자 (access modifier)
- 외부에서 접근하지 못하도록 제한하는 역할

| 접근 제어자    | 설명                                                       |
|-----------|----------------------------------------------------------|
| private   | 같은 클레스 내에서만 접근이 가능                                       |
| default   | 같은 패키지 내에서만 접근이 가능<br/>- 생성자에 접근 제어자가 지정되어 있지 않을 시 기본 적용 |
| protected | 같은 패키지 내에서, 그리고 다른 패키지의 자손클래스에서 접근 가능                    |
| public    | 접근 제한이 전혀 없음                                             |



[참고 서적 - 자바의 정석](http://www.kyobobook.co.kr/product/detailViewKor.laf?ejkGb=KOR&mallGb=KOR&barcode=9788994492032&orderClick=LEa&Kc=)  