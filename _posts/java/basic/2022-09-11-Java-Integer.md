---
title:  "JAVA Integer, int"
categories:
  - dataStructure
---

## 정수형 데이터 타입
<br/>

| 타입  | bit | byte | 저장 가능한 값의 범위                      |
|-------|-----|------|-------------------------------------------|
| byte  | 8   | 1    |  -128 ~ 127                               |
| short | 16  | 2    | -32768 ~ 32767                            |
| int   | 32  | 4    | -2147483648 ~ 2147483647                  |
| long  | 64  | 8    | -9223372036854775808 ~ 922372036854775807 |

<br/>
<br/>

## 실수형 데이터 타입
- 정밀도 : 'a x 10ⁿ (1 ≤ a < 10)'의 형태로 표현된 7자리의 10진수를 오차없이 저장할 수 있음
  - double 타입은 15자리의 정밀도를 가지므로 flaot 타입 보다 훨씬 더 정밀하게 값 표현 가능 
<br/>

| 타입   | 정밀도  | bit | byte | 저장 가능한 값의 범위             | 사용 목적                 |
|--------|------|-----|------|--------------------------|-----------------------|
| float  | 7자리  | 32  | 4    |  -3.4x10^38 ~ 3.4x10^38  | 연산속도의 향상이나 메모리 절약     |
| double | 15자리 | 64  | 8    | -1.7x10^308 ~ 1.7x10^308 | 더 큰값의 범위 또는 높은 정밀도 필요 |

<br/>
<br/>

## 형변환 
<br/>

### casting (타입) 피연산자
  - 기본형에서 boolean을 제외한 나머지 타입들은 서로 형변환 가능
    - ex) double = int / (double) int
  - 기본형과 참조형(String 등)간의 형변환은 불가능

<br/>

### 문자 -> 숫자
- String -> int
  1. Integer.parseInt(문자) 
    - 리턴타입 : int
  2. Integer.valueOf(문자)
    - 리턴타입 : Integer
- "-"를 음수로 전환  
- ex) Integer.parseInt("-123") -> -123

<br/>

```java
// parseInt() 리턴타입 : int
public static int parseInt(String s) throws NumberFormatException {
    return parseInt(s,10);
}
```

```java
// valueOf() 리턴타입 : Integer
public static Integer valueOf(String s) throws NumberFormatException {
    return Integer.valueOf(parseInt(s, 10));
}
```

```java
// Integer.parseInt() 음수반환 부분

char firstChar = s.charAt(0);
if (firstChar < '0') { // Possible leading "+" or "-"
    if (firstChar == '-') {
        negative = true;
        limit = Integer.MIN_VALUE;
    } else if (firstChar != '+') {
        throw NumberFormatException.forInputString(s, radix);
    }

    if (len == 1) { // Cannot have lone "+" or "-"
        throw NumberFormatException.forInputString(s, radix);
    }
    i++;
}
```
<br/>
<br/>

### 숫자 -> 문자
- int -> String
  1. String.valueOf(숫자)
  2. Integer.toString(숫자)

<br/>
<br/>

### char배열 -> 문자
- char[] -> String
  1. String.valueOf(cahr배열)


<br/>
<br/>
<br/>
<br/>
<br/>

---
<br/>

## Math
| 메소드명   | 설명                                           | 메소드                            |
|--------|----------------------------------------------|--------------------------------|
| abs    | 주어진 값의 절대값 반환                                | int abs(int a)                 |
|        |                                              | long abs(long a)               |
|        |                                              | float abs(float a)             |
|        |                                              | double abs(double a)           |
| ceil   | 주어진 값을 올림하여 반환                               | double ceil(double a)          |
| floor  | 주어진 값을 버림하여 반환                               | double floor(double a)         |
| max    | 주어진 두 값을 비교하여 큰 쪽을 반환                        | int max(int a, int b)          |
|        |                                              | long max(long a, long b)       |
|        |                                              | float max(float a, float b)    |
|        |                                              | double max(double a, double b) |
| min    | 주어진 두 값을 비교하여 작은 쪽을 반환                       | int min(int a, int b)          |
|        |                                              | long min(long a, long b)       |
|        |                                              | float min(float a, float b)    |
|        |                                              | double min(double a, double b) |
| random | 0.0 ~ 1.0 범위의 double값을 반환 (1.0은 범위에 포함되지 않음) | double random()                |
| sqrt   | 주어진 값의 제곱근을 반환                               | double sqrt(double a)          |
| pow    | 주어진 값 a의 b승을 반환                              | double pow(double a, double b) |

<br/>
<br/>
<br/>
<br/>
<br/>

[참고 서적 : 남궁성 - Java의 정석]()  
[참고 블로그 : 코딩팩토리](https://coding-factory.tistory.com/130)