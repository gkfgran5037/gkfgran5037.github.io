---
title:  "JAVA String, char"
categories:
  - dataStructure
---

## String
- java.lang.String
- 문자열과 관련된 작업을 할 때 유용하게 사용할 수 있는 다양한 메소드가 포함
- 불변 객체(immutable object) : String 인스턴스는 한 번 생성되면 그 값을 읽기만 할 수 있고, 변경할 수는 없음
  - 덧셈(+) 연산자를 이용하여 문자열 결합을 수행하면, 기존 문자열의 내용이 변경되는 것이 아니라 내용이 합쳐진 새로운 String 인스턴스가 생성되는 것

</br>
</br>

[참고 사이트 - TCPSCHOOL.com](http://www.tcpschool.com/java/java_api_string)

</br>
</br>
</br>
</br>
</br>

### substring( , )
- subString(beginIndex) : beginIndex부터 문자열의 마지막인덱스까지 추출
- subString(beginIndex, endIndex) : beginIndex부터 끝 endIndex 전까지 추출
  - String 재생성 시 subLen이 endIndex - beginIndex로 세팅되기 때문

<br/>

```java
// 내부 로직
public String substring(int beginIndex, int endIndex) {
    int length = length();
    checkBoundsBeginEnd(beginIndex, endIndex, length);
    int subLen = endIndex - beginIndex;
    if (beginIndex == 0 && endIndex == length) {
        return this;
    }
    return isLatin1() ? StringLatin1.newString(value, beginIndex, subLen)
                        : StringUTF16.newString(value, beginIndex, subLen);
}
```

<br/>
<br/>



<br/>
<br/>
<br/>

### isBlank()
- 문자열이 비어있거나 빈공백으로만 이루어진 경우 True 반환
- null 일 경우 NullPointerException 발생
   
<br/>

```java
// 내부 로직
public boolean isBlank() {
    return indexOfNonWhitespace() == length();
}
```

<br/>

```java
// 사용 방법
boolean result;
if ("".isBlank()) {
    result = true;
}
if (" ".isBlank()) {
    result = true;
}
// NullPointerException 발생
String str = null;
if (str3.isBlank()) {
    result = false;
}
```

<br/>
<br/>
<br/>

### isEmpty()



### repeat()
- 문자 반복

```java
String str = "abc";
str.repeat(3); // abcabcabc
```







---

## StringUtils
- org.apache.commons.lang 
- 기본제공 X -> jar 다운 받아서 사용
- 파라미터 값으로 null을 넘겨도 NullPointerException 발생 안 함

### isBlank()
- 문자열이 비어있거나 빈공백으로만 이루어진 경우 True 반환
- null 이어도 NullPointerException 발생하지 않음

```java
boolean result;
if(StringUtils.isBlank("")) {
  result = true; 
}
if(StringUtils.isEmpty(" ")) {
  result = true; 
}
if(StringUtils.isEmpty(null)) {
  result = true; 
}
```

<br/>
<br/>
<br/>

### isEmpty()
- 

```java
boolean result;
if(StringUtils.isEmpty("")) {
  result = true; 
}
if(StringUtils.isEmpty(" ")) {
  result = false; 
}
if(StringUtils.isEmpty(null)) {
  result = true; 
}
```