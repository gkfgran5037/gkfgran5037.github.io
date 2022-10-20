---
title:  "JAVA 정규 표현식"
categories:
  - dataStructure
---

<p style="color:gray; font-size:10px; text-align:center;"> 자세한 내용은 [코딩팩토리](https://coding-factory.tistory.com/529)에서 확인 바랍니다. </p>

<br/><br/><br/>




## 정규표현식 (Regular Expression)
- 특정한 규칙을 가진 문자열의 집합을 표현하기 위해 쓰이는 형식언어
- 입력값을 정해진 형식에 맞는지 검증 시 사용
- java.util.regex 패키지 사용
  - Pattern클래스, Matcher클래스
<br/>

| 정규 표현식        | 설명        |
|---------------|-----------|
| ^[0-9]*$      | 숫자        |
| ^[+-]?[0-9]*$ | (부호) + 숫자 |
| ^[a-zA-Z]*$   | 영문자       |
| ^[가-힣]*$      | 한글        |

<br/>

```java
public class CheckUtil {
  private static final Pattern NUMBER_PATTERN = Pattern.compile("^[+-]?[0-9]*$");
  private static final Pattern POSITIVE_PATTERN = Pattern.compile("^[0-9]*$");

  public boolean checkNumber(String number) {
    Matcher matcher = NUMBER_PATTERN.matcher(number);
    return matcher.find();
  }

  public boolean checkPositive(String number) {
    Matcher matcher = POSITIVE_PATTERN.matcher(number);
    return matcher.find();
  }

  public List<Boolean> result() {
    List<Boolean> list = new ArrayList<>();
    list.add(checkNumber("3")); // true
    list.add(checkNumber("+3")); // true
    list.add(checkNumber("-3")); // true

    list.add(checkPositive("3")); // true
    list.add(checkPositive("-3")); // false
    list.add(checkPositive("+3")); // false
    return list;
  }
}
```

<br/><br/><br/>





## Pattern
- 대상 문자열을 검증하는 기능
- Pttern.mathes(정규표현식, 검증대상 문자열)
  - 정규식 일치 여부 반환 (boolean)
<br/>

| 메서드                         | 설명                                      |
|-----------------------------|-----------------------------------------|
| compile(String regex)       | 주어진 정규표현식으로부터 패턴을 만듭니다.                 |
| matcher(CharSequence input) | 대상 문자열이 패턴과 일치할 경우 true를 반환합니다.         |
| asPredicate()               | 문자열을 일치시키는 데 사용할 수있는 술어를 작성합니다.         |
| pattern()                   | 컴파일된 정규표현식을 String 형태로 반환합니다.           |
| split(CharSequence input)   | 문자열을 주어진 인자값 CharSequence 패턴에 따라 분리합니다. |

<br/><br/><br/>




## Matcher
- 대상 문자열의 패턴을 해석하고 주어진 패턴과 일치하는지 판별할 때 주로 사용
- CharSequence : 다양한 형태의 입력 데이터로부터 문자 단위의 매칭 기능 지원
<br/>

| 메서드              | 설명                                           |
|------------------|----------------------------------------------|
| matches()        | 대상 문자열과 패턴이 일치할 경우 true 반환               |
| find()           | 대상 문자열과 패턴이 일치하는 경우 true를 반환하고, 그 위치로 이동 |
| find(int start)  | start위치 이후부터 매칭검색을 수행                    |
| start()          | 매칭되는 문자열 시작위치 반환                         |
| start(int group) | 지정된 그룹이 매칭되는 시작위치 반환                     |
| end()            | 매칭되는  문자열 끝 다음 문자위치 반환                   |
| end(int group)   | 지정되 그룹이 매칭되는 끝 다음 문자위치 반환                |
| group()          | 매칭된 부분을 반환                               |
| group(int group) | 매칭된 부분중 group번 그룹핑 매칭부분 반환             |
| groupCount()     | 패턴내 그룹핑한(괄호지정) 전체 갯수를 반환                 |

<br/><br/><br/>





| 표현식  | 설명                                                                                      |
|------|-----------------------------------------------------------------------------------------|
| ^    | 문자열 시작                                                                                  |
| $    | 문자열 종료                                                                                  |
| .    | 임의의 한 문자(단 \은 넣을 수 없음)                                                                  |
| *    | 앞 문자가 없을 수도 무한정 많을 수도 있음                                                                |
| +    | 앞 문자가 하나 이상                                                                             |
| ?    | 앞 문자가 없거나 하나 있음                                                                         |
| [ ]  | 문자의 집합이나 범위를 나타내며 두 문자 사이는 - 기호로 범위를 나타냄. <br/>[] 내에서 ^ 가 선행하여 존재하면 not을 나타냄        |
| { }  | 횟수 또는 범위를 나타냄                                                                        |
| ( )  | 소괄호 안의 문자를 하나의 문자로 인식                                                                   |
| \\|    | 패턴 안에서 or 연산을 수행할 때 사용                                                                  |
| \    | 정규 표현식 역슬래시(\)는 확장문자 <br/>- 역슬래시 다음에 일반 문자가 오면 특수문자로 취급하고 역슬래시 다음에 특수문자가 오면 그 문자 자체를 의미 |
| \s   | 공백 문자                                                                                   |
| \S   | 공백 문자가 아닌 나머지 문자                                                                        |
| \w   | 알파벳이나 숫자                                                                                |
| \W   | 알파벳이나 숫자를 제외한 문자                                                                        |
| \d   | 숫자 [0-9]와 동일                                                                            |
| \D   | 숫자를 제외한 모든 문자                                                                           |
| (?i) | 앞 부분에 (?!)라는 옵션을 넣어주게 되면 대소문자는 구분하지 안함.                                               |

<br/><br/><br/>




```java
String text = "//;\n1;2;3";

// 형식 : "//" + 문자1 + "\n" + 문자2
Matcher matcher = Pattern.compile("//(.)\n(.*)").matcher(text);
// "//;\n1;2;3" : true / "1;2;3" : false
if (matcher.find()) {
    String cutomDelimiter = matcher.group(1); // group(1) : 문자1
    String[] tokens = matcher.group(2).split(cutomDelimiter); // group(2) : 문자 
}
```




[공식](https://docs.oracle.com/en/java/javase/11/docs/api/java.base/java/util/regex/Matcher.html)
[참고 블로그 - 코딩팩토리](https://coding-factory.tistory.com/529)