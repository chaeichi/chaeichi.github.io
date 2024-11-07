---
layout: post
title: "Java의 정석 Chapter10. 날짜와 시간 & 형식화 (2) - 형식화 클래스"
subtitle: Java의 정석 3rd Edition | 남궁성
excerpt_image: /assets/images/excerpt-images/javajungsuk3/2024-11-06-date-time-and-formatting-2.png
categories: 
    - Java
tags: 
    - Java
    - Java의 정석
    - 형식화 클래스
    - DecimalFormat
    - SimpleDateFormat
    - ChoiceFormat
    - MessageFormat
---
![banner](/assets/images/excerpt-images/javajungsuk3/2024-11-06-date-time-and-formatting-2.png)

## 형식화 클래스
- `java.text패키지`에 포함
- 숫자, 날짜, 텍스트 데이터를 일정한 형식에 맞게 표현할 수 있는 방법을 객체지향적으로 설계하여 표준화
- 형식화에 사용될 패턴을 정의
- 데이터를 정의된 패턴에 맞춰 형식화할 수 있을 뿐만 아니라 역으로 형식화된 데이터에서 원래의 데이터를 얻어낼 수도 있다.
- 형식화된 데이터의 패턴만 정의해주면 복잡한 문자열에서도 `substring()`을 사용하지 않고도 쉽게 원하는 값을 얻어낼 수 있다.

### DecimalFormat
- 숫자를 형식화 하는데 사용
- 숫자 데이터를 정수, 부동소수점, 금액 등의 다양한 형식으로 표현할 수 있음
- 반대로 일정한 형식의 텍스트 데이터를 숫자로 쉽게 변환하는 것도 가능

|기호|의미|패턴|결과(1234567.89)|
|-|-|-|-|
|0|10진수(값이 없을 때는 0)|0<br>0.0<br/>000000000.0000|1234568<br/>1234567.9<br>001234567.8900|
|#|10진수|#<br/>#.#<br/>##########.####|1234568<br/>1234567.9<br/>123456.89|
|.|소수점|#.#|1234567.9|
|-|음수부호|#.#-<br/>-#.#|1234567.9-<br/>-1234567.9|
|,|단위 구분자|#,###.##<br/>#,####.##|1,234,567.89<br/>123,4567.89|
|E|지수기호|#E0<br/>0E0<br/>##E0<br/>00E0<br/>####E0<br/>0000E0<br/>#.#E0<br/>0.0E0<br/>0.000000000E0<br/>00.00000000E0<br/>000.0000000E0<br/>#.#########E0<br/>##.########E0<br/>###.#######E0|.1E7<br/>1E6<br/>1.2E6<br/>12E5<br/>123.5E4<br/>1235E3<br/>1.2E6<br/>1.2E6<br/>1.234567890E6<br/>12.34567890E5<br/>123.4567890E4<br/>1.23456789E6<br/>1.23456789E6<br/>1.23456789E6|
|;|패턴구분자|#,###.##+;<br/>#,###.##-|1.234.567.89+(양수일 때)<br/>1.234.567.89-(음수일 때)|
|%|퍼센트|#.#%|123456789%|
|\u2030|퍼밀(퍼센트 x 10)|#.#\u2030|1234567890%|
|\u00A4|통화|\u00A4 #,###|₩ 1,234,568|
|'|escape문자|'#'#,####<br/>''#,###|#1,234,568<br/>'1,234,568|

#### DecimalFormat을 사용하는 방법

```DecimalFormat
double number = 1234567.89;
DecimalFormat df = new DecimalFormat("#.#E0");
String result = df.format(number);
```

- 원하는 출력형식의 패턴을 작성하여 DecimalFormat인스턴스를 생성한다.
- 출력하고자 하는 문자열로 format메서드를 호출하면 원하는 패턴에 맞게 변환된 문자열을 얻게 된다.

```DecimalFormatEx1.java
import java.text.DecimalFormat;

class DecimalFormatEx1 {
    public static void main(String[] args) throws Exception {
        double number = 1234567.89;
        String[] pattern = {
          "0",
          "#",
          "0.0",
          "#.#",
          "0000000000.0000",
          "##########.####",
          "#.#-",
          "-#.#",
          "#,###.##",
          "#,####.##",
          "#E0",
          "0E0",
          "##E0",
          "00E0",
          "####E0",
          "0000E0",
          "#.#E0",
          "0.0E0",
          "0.000000000E0",
          "00.00000000E0",
          "000.0000000E0",
          "#.#########E0",
          "##.########E0",
          "###.#######E0",
          "#,###.##+;#,###.##-",
          "#.#%",
          "#.#\u2030",
          "\u00A4 #,###",
          "'#'#,###",
          "''#,###",
        };

        for(int i = 0; i < pattern.length; i++) {
            DecimalFormat df = new DecimalFormat(pattern[i]);
            System.out.printf("%19s : %s\n", pattern[i], df.format(number));
        }
    }
}
```

```실행결과
                  0 : 1234568
                  # : 1234568
                0.0 : 1234567.9
                #.# : 1234567.9
    0000000000.0000 : 0001234567.8900
    ##########.#### : 1234567.89
               #.#- : 1234567.9-
               -#.# : -1234567.9
           #,###.## : 1,234,567.89
          #,####.## : 123,4567.89
                #E0 : .1E7
                0E0 : 1E6
               ##E0 : 1.2E6
               00E0 : 12E5
             ####E0 : 123.5E4
             0000E0 : 1235E3
              #.#E0 : 1.2E6
              0.0E0 : 1.2E6
      0.000000000E0 : 1.234567890E6
      00.00000000E0 : 12.34567890E5
      000.0000000E0 : 123.4567890E4
      #.#########E0 : 1.23456789E6
      ##.########E0 : 1.23456789E6
      ###.#######E0 : 1.23456789E6
#,###.##+;#,###.##- : 1,234,567.89+
               #.#% : 123456789%
               #.#‰ : 1234567890‰
            ¤ #,### : ₩ 1,234,568
           '#'#,### : #1,234,568
            ''#,### : '1,234,568
```

```DecimalFormatEx2.java
import java.text.DecimalFormat;

class DecimalFormatEx2 {
    public static void main(String[] args) {
        DecimalFormat df = new DecimalFormat("#,###.##");
        DecimalFormat df2 = new DecimalFormat("#.###E0");

        try {
            Number num = df.parse("1,234,567.89");
            System.out.print("1,234,567.89" + " -> ");

            double d = num.doubleValue();
            System.out.print(d + " -> ");
            System.out.println(df2.format(num));
        } catch(Exception e) {}
    }
}
```

```실행결과
1,234,567.89 -> 1234567.89 -> 1.235E6
```

- 패턴을 이용해서 숫자를 다르게 변환하는 예제
- `parse메서드`를 이용하면 기호와 문자가 포함된 문자열을 숫자로 쉽게 변환할 수 있다.
- `parse(String source)`는 DecimalFormat의 조상인 NumberFormat에 정의된 메서드이다.
- Number클래스는 Integer, Double과 같은 숫자를 저장하는 래퍼 클래스의 조상이며, `doubleValue()`는 Number에 저장된 값을 double형의 값으로 변환하여 반환한다.
- 이 외에도 `intValue()`, `floatValue()` 등의 메서드가 Number클래스에 정의되어 있다.

### SimpleDateFormat
- 앞서 배운 Date와 Calendar만으로는 날짜 데이터를 원하는 형태로 다양하게 출력하는 것은 불편하고 복잡하다.
- SimpleDateFormat을 사용하면 문제들이 간단히 해결된다.
- *DateFormat은 추상클래스로 SimpleDateFormat의 조상이다. DateFormat은 추상클래스이므로 인스턴스를 생성하기 위해서는 getDateInstance()와 같은 static메서드를 이용해야 한다. getDateInstance()에 의해서 반환되는 것은 DateFormat을 상속받아 완전하게 구현한 SimpleDateFormat인스턴스이다.*

|기호|의미|보기|
|-|-|-|
|G|연대(BC, AD)|AD|
|y|년도|2006|
|M|월(1~12 또는 1월~12월)|10 또는 10월, OCT|
|w|년의 몇 번째 주(1~53)|50|
|W|월의 몇 번째 주(1~5)|4|
|D|년의 몇 번째 일(1~366)|100|
|d|월의 몇 번째 일(1~31)|15|
|F|월의 몇 번째 요일(1~5)|1|
|E|요일|월|
|a|오전/오후(AM, PM)|PM|
|H|시간(0~23)|20|
|k|시간(1~24)|13|
|K|시간(0~11)|10|
|h|시간(1~12)|11|
|m|분(0~59)|35|
|s|초(0~59)|55|
|S|천분의 일초(0~999)|253|
|z|Time zone(General time zone)|GMT+9:00|
|Z|Time zone(RFC 822 time zone)|+0900|
|'|escape문자(특수문자를 표현하는데 사용)|없음|

#### SimpleDateFormat을 사용하는 방법

```SimpleDateFormat
Date today = new Date();
SimpleDateFormat df = new SimpleDateFormat("yyyy-MM-dd");
String result = df.format(today);
```

- 원하는 출력형식의 패턴을 작성하여 SimpleDateFormat인스턴스를 생성
- 출력하고자 하는 Date인스턴스를 가지고 format(Date d)를 호출하면 지정한 출력형식에 맞게 변환된 문자열을 얻게 된다.

```DateFormatEx1.java
import java.text.SimpleDateFormat;
import java.util.Date;

class DateFormatEx1 {
    public static void main(String[] args) {
        Date today = new Date();

        SimpleDateFormat sdf1, sdf2, sdf3, sdf4;
        SimpleDateFormat sdf5, sdf6, sdf7, sdf8, sdf9;

        sdf1 = new SimpleDateFormat("yyyy-MM-dd");
        sdf2 = new SimpleDateFormat("''yy년 MMM dd일 E요일");
        sdf3 = new SimpleDateFormat("yyyy-MM-dd HH:mm:ss.SSS");
        sdf4 = new SimpleDateFormat("yyyy-MM-dd hh:mm:ss a");

        sdf5 = new SimpleDateFormat("오늘은 올 해의 D번째 날입니다.");
        sdf6 = new SimpleDateFormat("오늘은 이 달의 d번째 날입니다.");
        sdf7 = new SimpleDateFormat("오늘은 올 해의 w번째 주입니다.");
        sdf8 = new SimpleDateFormat("오늘은 이 달의 W번째 주입니다.");
        sdf9 = new SimpleDateFormat("오늘은 이 달의 F번째 E요일입니다.");

        System.out.println(sdf1.format(today));
        System.out.println(sdf2.format(today));
        System.out.println(sdf3.format(today));
        System.out.println(sdf4.format(today));
        System.out.println();
        System.out.println(sdf5.format(today));
        System.out.println(sdf6.format(today));
        System.out.println(sdf7.format(today));
        System.out.println(sdf8.format(today));
        System.out.println(sdf9.format(today));
    }
}
```

```실행결과
2024-11-06
'24년 11월 06일 수요일
2024-11-06 18:22:41.116
2024-11-06 06:22:41 오후

오늘은 올 해의 311번째 날입니다.
오늘은 이 달의 6번째 날입니다.
오늘은 올 해의 45번째 주입니다.
오늘은 이 달의 2번째 주입니다.
오늘은 이 달의 1번째 수요일입니다.
```

```DateFormatEx2.java
import java.text.SimpleDateFormat;
import java.util.Calendar;
import java.util.Date;

class DateFormatEx2 {
    public static void main(String[] args) {
        Calendar cal = Calendar.getInstance();
        cal.set(2005, 9, 3); // 2005년 10월 3일 - Month는 0~11의 범위를 갖는다.

        Date day = cal.getTime(); // Calendar를 Date로 변환

        SimpleDateFormat sdf1, sdf2, sdf3, sdf4;
        sdf1 = new SimpleDateFormat("yyyy-MM-dd");
        sdf2 = new SimpleDateFormat("yy-MM-dd E요일");
        sdf3 = new SimpleDateFormat("yyyy-MM-dd HH:mm:ss.SSS");
        sdf4 = new SimpleDateFormat("yyyy-MM-dd hh:mm:ss a");

        System.out.println(sdf1.format(day));
        System.out.println(sdf2.format(day));
        System.out.println(sdf3.format(day));
        System.out.println(sdf4.format(day));
    }
}
```

```실행결과
2005-10-03
05-10-03 월요일
2005-10-03 18:27:35.761
2005-10-03 06:27:35 오후
```

- Date인스턴스만 format메서드에 사용될 수 있기 때문에 `getTime()`을 사용하여 Calendar인스턴스를 Date인스턴스로 변환해야 한다. 
- 반대로 Date인스턴스를 Calendar인스턴스로 변환할 때는 `setTime()`을 사용하면 된다.

```DateFormatEx3.java
import java.text.DateFormat;
import java.text.SimpleDateFormat;
import java.util.Date;

class DateFormatEx3 {
    public static void main(String[] args) {
        DateFormat df = new SimpleDateFormat("yyyy년 MM월 dd일");
        DateFormat df2 = new SimpleDateFormat("yyyy/MM/dd");

        try {
            Date d = df.parse("2015년 11월 23일");
            System.out.println(df2.format(d));
        } catch (Exception e) {}
    }
}
```

```실행결과
2015/11/23
```

- SimpleDateFormat의 `parse(String source)`는 문자열source를 날짜Date인스턴스로 변환해준다.
- 사용자로부터 날짜 데이터를 문자열로 입력받을 때, 입력받은 문자열을 날짜로 인식하기 위해 substring메서드를 이용해 년, 월, 일을 뽑아내야 하는데 `parse(String source)`는 이러한 수고를 덜어준다.
- *parse(String source)는 SimpleDateFormat의 조상인 DateFormat에 정의되어 있다.*
- *저장된 형식과 입력된 형식이 일치하지 않는 경우에는 예외가 발생하므로 적절한 예외처리가 필요하다.*

```DateFormatEx4.java
import java.text.DateFormat;
import java.text.SimpleDateFormat;
import java.util.Calendar;
import java.util.Date;
import java.util.Scanner;

class DateFormatEx4 {
    public static void main(String[] args) {
        String pattern = "yyyy/MM/dd";
        DateFormat df = new SimpleDateFormat(pattern);
        Scanner s = new Scanner(System.in);

        Date inDate = null;

        System.out.println("날짜를 " + pattern + "의 형태로 입력해주세요. (입력 예 : 2015/12/31)");

        while(s.hasNextLine()) {
            try {
                inDate = df.parse(s.nextLine());
                break;
            } catch (Exception e) {
                System.out.println("날짜를 " + pattern + "의 형태로 다시 입력해주세요. (입력 예 : 2015/12/31)");
            }
        }

        Calendar cal = Calendar.getInstance();
        cal.setTime(inDate);
        Calendar today = Calendar.getInstance();
        long day = (cal.getTimeInMillis() - today.getTimeInMillis() / (60*60*1000));
        System.out.println("입력하신 날짜는 현재와 " + day + "시간 차이가 있습니다.");
    }
}
```

```실행결과
날짜를 yyyy/MM/dd의 형태로 입력해주세요. (입력 예 : 2015/12/31)
asdfasdf
날짜를 yyyy/MM/dd의 형태로 다시 입력해주세요. (입력 예 : 2015/12/31)
20241231
날짜를 yyyy/MM/dd의 형태로 다시 입력해주세요. (입력 예 : 2015/12/31)
2024/12/31
입력하신 날짜는 현재와 1301시간 차이가 있습니다.
```

- 화면으로부터 날짜를 입력받아서 계산결과를 출력하는 예제
- while과 try-catch문을 이용해서 사용자가 올바른 형식으로 날짜를 입력할 때까지 반복해서 입력받도록 하였다.
- 지정된 패턴으로 입력되지 않은 경우, parse메서드를 호출하는 부분에서 예외(ParseException)가 발생하기 때문에 while문을 벗어나지 못한다.

### ChoiceFormat
- 특정 범위에 속하는 값을 문자열로 변환해준다.
- 연속적 또는 불연속적인 범위의 값들을 처리하는데 있어서 if문이나 switch문은 적절하지 못한 경우가 많다.
- 이럴 때 ChoiceFormat을 잘 사용하면 복잡하게 처리될 수 밖에 없었던 코드를 간단하고 직관적으로 만들 수 있다.

```ChoiceFormatEx1.java
import java.text.ChoiceFormat;

class ChoiceFormatEx1 {
    public static void main(String[] args) {
        double[] limits = {60, 70, 80, 90}; // 낮은 값부터 큰 값의 순서로 적어야한다.
        // limits, grades간의 순서와 개수를 맞추어야 한다.
        String[] grades = {"D", "C", "B", "A"};

        int[] scores = {100, 95, 88, 70, 52, 60, 70};

        ChoiceFormat form = new ChoiceFormat(limits, grades);

        for(int i = 0; i < scores.length; i++) {
            System.out.println(scores[i] + ":" + form.format(scores[i]));
        }
    }
}
```

```실행결과
100:A
95:A
88:B
70:C
52:D
60:D
70:C
```

- limits는 범위의 경계값을 저장하는데 사용
- grades는 범위에 포함된 값을 치환할 문자열을 저장하는데 사용
- 경계값은 double형으로 반드시 모두 `오름차순`으로 정렬되어 있어야 한다.
- 치환될 문자열의 개수는 경계값에 의해 정의된 범위의 개수와 일치해야 한다.
- 그렇지 않으면 `Illegalargumentexception`이 발생한다.
- 예외에서는 4개의 경계값에 의해 '60~69', '70~79', '80~89', '90~'의 범위가 정의되어 있다.

```ChoiceFormatEx2.java
import java.text.ChoiceFormat;

class ChoiceFormatEx2 {
    public static void main(String[] args) {
        String pattern = "60#D|70#C|80<B|90#A";
        int[] scores = {91, 90, 80, 88, 70, 52, 60};

        ChoiceFormat form = new ChoiceFormat(pattern);

        for(int i = 0; i < scores.length; i++) {
            System.out.println(scores[i] + ":" + form.format(scores[i]));
        }
    }
}
```

```실행결과
91:A
90:A
80:C
88:B
70:C
52:D
60:D
```

- 이전 예제를 패턴을 사용하도록 변경한 것이다.
- 패턴은 구분자로 `#`과 `<` 두 가지를 제공한다.
    - `#`은 경계값을 범위에 포함시킨다.
    - `<`는 경계값을 범위에 포함시키지 않는다.
    - 위의 결과에서 90은 A지만, 80은 B가 아닌 C
- `limit#value`의 형태로 사용한다.

### MessageFormat
- 데이터를 정해진 양식에 맞게 출력할 수 있도록 도와준다.
- 데이터가 들어갈 자리를 마련해놓은 양식을 미리 작성하고 프로그램을 이용해서 다수의 데이터를 같은 양식으로 출력할 때 사용하면 좋다.
- SimpleDateFormat의 parse처럼 MessageFormat의 parse를 이용하면 지정된 양식에서 필요한 데이터만을 손쉽게 추출해 낼 수도 있다.

```MessageFormatEx1.java
import java.text.MessageFormat;

class MessageFormatEx1 {
    public static void main(String[] args) {
        String msg = "Name: {0} \nTel: {1} \nAge: {2} \nBirthDay: {3}";

        Object[] arguments = {
                "이자바", "02-123-1234", "27", "07-09"
        };

        String result =
                MessageFormat.format(msg, arguments);
        System.out.println(result);
    }
}
```

```실행결과
Name: 이자바 
Tel: 02-123-1234 
Age: 27 
Birthday: 07-09
```

- MessageFormat에 사용할 양식인 문자열 msg를 작성할 때 `{숫자}`로 표시된 부분이 데이터가 출력될 자리이다.
- 이 자리는 순차적일 필요는 없고 여러 번 반복해서 사용할 수도 있다.
- 여기에 사용되는 숫자는 배열처럼 인덱스가 0부터 시작하며 양식에 들어갈 데이터는 객체배열인 arguments에 지정되어 있음을 알 수 있다.
- 객체배열이기 때문에 String이외에도 다른 객체들이 지정될 수 있으며, 이 경우보다 세부적인 옵션들이 사용될 수 있다.

```MessageFormatEx2.java
import java.text.MessageFormat;

class MessageFormatEx2 {
    public static void main(String[] args) {
        String tableName = "CUST_INFO";
        String msg = "INSERT INTO " + tableName + " VALUES (''{0}'',''{1}'',''{2}'',''{3}'');";

        Object[][] arguments = {
                {"이자바", "02-123-1234", "27", "07-09"},
                {"김프로", "032-333-1234", "33", "10-07"},
        };

        for(int i = 0; i < arguments.length; i++) {
            String result = MessageFormat.format(msg, arguments[i]);
            System.out.println(result);
        }
    }
}
```

```실행결과
INSERT INTO CUST_INFO VALUES ('이자바','02-123-1234','27','07-09');
INSERT INTO CUST_INFO VALUES ('김프로','032-333-1234','33','10-07');
```

- 반복문으로 하나 이상의 데이터 집합을 출력하는 예제
- 다수의 데이터를 Database에 저장하기 위한 Insert문으로 변환하는 경우 등에 사용하면 좋을 것이다.

```MessageFormatEx3.java
import java.text.MessageFormat;

class MessageFormatEx3 {
    public static void main(String[] args) throws Exception {
        String[] data = {
                "INSERT INTO CUST_INFO VALUES ('이자바','02-123-1234','27','07-09');",
                "INSERT INTO CUST_INFO VALUES ('김프로','032-333-1234','33','10-07');"
        };

        String pattern = "INSERT INTO CUST_INFO VALUES ({0},{1},{2},{3});";
        MessageFormat mf = new MessageFormat(pattern);

        for(int i = 0; i < data.length; i++) {
            Object[] objs = mf.parse(data[i]);
            for(int j = 0; j < objs.length; j++) {
                System.out.print(objs[j] + ",");
            }
            System.out.println();
        }
    }
}
```

```실행결과
'이자바','02-123-1234','27','07-09',
'김프로','032-333-1234','33','10-07',
```

- `parse(String source)`를 이용해서 출력된 데이터로부터 필요한 데이터만을 뽑아내는 방법

```MessageFormatEx4.java
import java.io.File;
import java.text.MessageFormat;
import java.util.Scanner;

class MessageFormatEx4 {
    public static void main(String[] args) throws Exception {
        String tableName = "CUST_INFO";
        String fileName = "data4.txt";
        String msg = "INSERT INTO " + tableName + " VALUES ({0},{1},{2},{3});";

        Scanner s = new Scanner(new File(fileName));

        String pattern = "{0},{1},{2},{3}";
        MessageFormat mf = new MessageFormat(pattern);

        while(s.hasNextLine()) {
            String line = s.nextLine();
            Object[] objs = mf.parse(line);
            System.out.println(MessageFormat.format(msg, objs));
        }

        s.close();
    }
}
```

```실행결과
INSERT INTO CUST_INFO VALUES ('이자바','02-123-1234','27','07-09');
INSERT INTO CUST_INFO VALUES ('김프로','032-333-1234','33','10-07');
```

- Scanner를 통해 파일로부터 데이터를 라인별로 읽어서 처리하도록 변경
- 이렇게 파일로부터 데이터를 제공받으면 데이터가 변경되어도 다시 컴파일을 하지 않아도 된다.
- 실행 시에 입력받을 데이터가 저장된 파일명도 지정하도록 예제를 변경하면, 파일의 이름이 바뀌어도 다시 컴파일하지 않아도 되므로 더 편리할 것이다.