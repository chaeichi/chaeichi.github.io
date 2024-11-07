---
layout: post
title: "Java의 정석 연습문제 - Chapter02. 변수"
subtitle: Java의 정석 3rd Edition | 남궁성
excerpt_image: /assets/images/excerpt-images/2024-01-10-javajungsuk3-chapter2-practice.png
categories: 
    - 문제풀이
tags: 
    - Java
    - Java의 정석
    - 연습문제
    - 문제풀이
---
![banner](/assets/images/excerpt-images/2024-01-10-javajungsuk3-chapter2-practice.png)

## ✏️ 연습문제
**2-1.** 다음 표의 빈 칸에 `8개의 기본형(primitive type)`을 알맞은 자리에 넣으시오.

| |1byte|2byte|4byte|8byte|
|---|---|---|---|---|
|**논리형**|boolean| | | |
|**문자형**| |char| | |
|**정수형**|byte|short|int|long|
|**실수형**| | |float|double|

**2-2.** 주민등록번호를 숫자로 저장하고자 한다. 이 값을 저장하기 위해서는 어떤 자료형(data type)을 선택해야 할까? `regNo`라는 이름의 변수를 선언하고 자신의 주민등록번호로 초기화 하는 한 줄의 코드를 적으시오.<br/>
**[정답]** long regNo = (주민등록번호 13자리)L;<br/>
**[해설]** 주민등록번호는 13자리로, 약 20억까지 표현할 수 있는 int형으로는 주민등록번호를 저장하기에 부족하다. 그러므로 long형을 사용해야 한다.

**2-3.** 다음의 문장에서 `리터럴, 변수, 상수, 키워드`를 적으시오.<br/>
int i = 100;<br/>
long l = 100L;<br/>
final float PI = 3.14f;<br/>

**[정답]**<br/>
리터럴 : 100, 100L, 3.14f<br/>
변수 : i, l<br/>
상수 : PI<br/>
키워드 : int, long, final, float

**2-4.** 다음 중 `기본형(primitive type)`이 아닌 것은?<br/>
a. int<br/>
**b. Byte**<br/>
c. double<br/>
d. boolean

**[정답]** b<br/>
**[해설]** 8개의 기본형은 boolean, char, byte, short, int, long, float, double로 그 외 타입은 모두 참조형(reference type)이다. java.lang.Byte 클래스와 기본형 변수 byte는 다르다.

**2-5.** 다음 문장들의 출력결과를 적으시오. 오류가 있는 문장의 경우, 괄호 안에 `오류`라고 적으시오.<br/>
System.out.println("1" + "2"); → (12) // 큰 따옴표(")로 감싸면 문자열이 된다.<br/>
System.out.println(true + ""); → (true) // 문자열 + any type → 문자열<br/>
System.out.println('A' + 'B'); → (131) // char형은 연산 시 int형으로 변환되여 연산되므로 'A'(65) + 'B'(66) = 131<br/>
System.out.println('1' + 2); → (51) // '1'(49) + 2 = 51<br/>
System.out.println('1' + '2'); → (99) // '1'(49) + '2'(50) = 99<br/>
System.out.println('J' + "ava"); → (Java) // 문자열 + any type → 문자열<br/>
System.out.println(true + null); → (오류)

**2-6.** 다음 중 `키워드`가 아닌 것은? (모두 고르시오.)<br/>
a. if<br/>
**b. True**<br/>
**c. NULL**<br/>
**d. Class**<br/>
**e. System**

**[정답]** b, c, d, e<br/>
**[해설]** Java에서는 대소문자를 구분하기 때문에 true, null, class는 예약어지만 True, NULL, Class는 예약어가 아니다. System은 예약어가 아닌 표준 입출력 클래스이다.

**2-7.** 다음 중 `변수의 이름`으로 사용할 수 있는 것은? (모두 고르시오.)<br/>
**a. $ystem**<br/>
b. channel#5 // 특수문자 '#'은 사용할 수 없다.<br/>
c. 7eleven // 숫자로 시작할 수 없다.<br/>
**d. If**<br/>
**e. 자바**<br/>
f. new // 예약어는 사용할 수 없다.<br/>
**g. $MAX_NUM**<br/>
h. hello@com // 특수문자 '@'는 사용할 수 없다.<br/>

**[정답]** a, d, e, h<br/>
**[해설]**
1. 대소문자가 구분되며 길이에 제한이 없다.
    - `True`와 `true`는 다른 것으로 간주
2. `예약어`를 사용해서는 안 된다.
    - `예약어(Reserved word)` 또는 `키워드(keyword)` : 프로그래밍 언어의 구문에 사용되는 단어
    - 예약어는 **클래스나 변수, 메서드의 이름으로 사용할 수 없음**
    - `true`는 예약어라서 사용할 수 없지만, `True`는 가능하다.
3. 숫자로 시작해서는 안 된다.
    - `top10`은 허용하지만, `7up`은 허용하지 않는다.
4. 특수문자는 `_`와 `$`만을 허용한다.
    - `$harp`은 허용되지만, `S#arp`은 허용되지 않는다.

**2-8.** `참조형 변수(reference type)`와 같은 크기의 기본형(primitive type)은? (모두 고르시오.)<br/>
**a. int(4byte)**<br/>
b. long(8byte)<br/>
c. short(2byte)<br/>
**d. float(4byte)**<br/>
e. double(8byte)

**[정답]** a, d<br/>
**[해설]** 모든 참조형 변수는 메모리 주소를 값으로 가지므로 4byte이다.

**2-9.** 다음 중 `형변환`을 생략할 수 있는 것은? (모두 고르시오.)<br/>
byte b = 10;<br/>
char ch = 'A';<br/>
int i = 100;<br/>
long l = 1000L;

a. b = (byte)i;<br/>
b. ch = (char)b;<br/>
c. short s = (short)ch;<br/>
**d. float f = (float)l;**<br/>
**e. l = (int)ch**;

**[정답]** d, e<br/>
**[해설]** 형변환은 표현 범위가 좁은 타입에서 넓은 타입으로 변환하는 경우 값 손실이 없어 두 개의 값 중 표현 범위가 넓은 쪽으로 형변환이 된다. short형과 char형은 2byte로 크기는 같지만 표현범위는 다르다. char형은 부호를 표현하지 않기 때문에 byte형과 short형을 char형으로 변환하게 될 경우 값 손실이 발생할 수 있기 때문에 명시적으로 형변환이 필요하다.

**2-10.** `char` 타입의 변수에 저장될 수 있는 정수 값의 범위는? (10진수로 적으시오.)<br/>
**[정답]** 0 ~ 65535<br/>
**[해설]** char는 2byte(=16bit)로 2<sup>16</sup>개의 값을 표현할 수 있으며, 표현범위는 0부터 2<sup>16</sup>-1이다.

**2-11.** 다음 중 변수를 잘못 초기화한 것은? (모두 고르시오.)<br/>
**a. byte b = 256;** // byte의 범위를 넘어선다.<br/>
**b. char c = '';** // char는 아무런 문자를 넣지 않는 빈 문자를 허용하지 않는다.(공백문자와 혼동하지 않기)<br/>
**c. char answer = 'no';** // char는 단 하나의 문자만을 저장할 수 있다.<br/>
**d. float f = 3.14;** // float는 접미사 f를 붙여줘야 한다. (double은 기본형이므로 접미사 d 생략 가능)<br/>
e. double d = 1.4e3f; // double형이 float형보다 크기가 크므로 가능하다.<br/>

**[정답]** a, b, c, d

**2-12.** 다음 중 `main 메서드`의 선언부로 알맞은 것은? (모두 고르시오.)<br/>
**a. public static void main(String[] args)**<br/>
**b. public static void main(String args[])** // 배열을 의미하는 기호인 '[]'는 타입 뒤에 붙여도 되고 변수명 뒤에 붙여도 된다.<br/>
**c. public static void main(String[] arv)** // 매개변수 args에 이름은 달라도 된다.<br/>
d. public void static main(String[] args) // void는 반드시 main 앞에 와야한다.<br/>
**e. static public void main(String[] args)** // public과 static은 위치가 바뀌어도 된다.<br/>

**[정답]** a, b, c, e<br/>


**2-13.** 다음 중 타입과 기본값이 잘못 연결된 것은? (모두 고르시오.)<br/>
a. boolean - false<br/>
b. char - '\n0000'<br/>
**c. float - 0.0** // float는 0.0f가 기본값<br/>
d. int - 0<br/>
**e. long - 0** // long은 0.0L이 기본값<br/>
**f. String - ""** // String은 참조형 타입. 모든 참조형 타입의 기본값은 null

## 🚩 잊지않기
- System은 예약어가 아니라 표준 입출력 클래스이다.
- [참조형 변수는 메모리 주소를 값으로 가지므로 모두 4byte의 크기를 가진다. 또한 참조형 타입의 기본값은 모두 null이다.](https://chaeichi.github.io/java/2024/01/04/variable-1.html#h-%EC%B0%B8%EC%A1%B0%ED%98%95-%EB%B3%80%EC%88%98reference-type)
- [char형은 부호를 표현하지 않기 때문에 byte형과 short형을 char형으로 변환하게 될 경우 값 손실이 발생할 수 있기 때문에 자동 형변환이 수행되지 않는다.](https://chaeichi.github.io/java/2024/01/08/variable-5.html#h-%EC%9E%90%EB%8F%99-%ED%98%95%EB%B3%80%ED%99%98%EC%9D%98-%EA%B7%9C%EC%B9%99)
- [char형은 안에 아무런 문자를 넣지 않는 빈 문자를 허용하지 않는다. (공백문자와 빈 문자는 다르다.)](https://chaeichi.github.io/java/2024/01/04/variable-1.html#h-%EB%AC%B8%EC%9E%90-%EB%A6%AC%ED%84%B0%EB%9F%B4%EA%B3%BC-%EB%AC%B8%EC%9E%90%EC%97%B4-%EB%A6%AC%ED%84%B0%EB%9F%B4)
- [main 메서드의 선언부에서 배열을 의미하는 기호인 '[]'는 타입 뒤에 붙여도 되고 변수명 뒤에 붙여도 된다. 매개변수 args에 이름은 달라도 된다.](https://chaeichi.github.io/java/2024/01/02/getting-started-with-java.html#h-main-%EB%A9%94%EC%84%9C%EB%93%9C)
- char형의 기본값은 '\n0000'이다.