---
layout: post
title: "Java의 정석 연습문제 - Chapter09. java.lang패키지와 유용한 클래스"
subtitle: Java의 정석 3rd Edition | 남궁성
excerpt_image: /assets/images/excerpt-images/2024-10-10-javajungsuk3-chapter9-practice.png
categories: 
    - 문제풀이
tags: 
    - Java
    - Java의 정석
    - 연습문제
    - 문제풀이
---
![banner](/assets/images/excerpt-images/2024-10-10-javajungsuk3-chapter9-practice.png)

**[9-1]** 다음과 같은 실행결과를 얻도록 SutdaCard클래스의 `equals()`를 멤버변수인 num, isKwang의 값을 비교하도록 `오버라이딩`하고 테스트 하시오.

```Exercise9_1.java
class Exercise9_1 {
    public static void main(String[] args) {
        SutdaCard c1 = new SutdaCard(3, true);
        SutdaCard c2 = new SutdaCard(3, true);

        System.out.println("c1 = " + c1);
        System.out.println("c2 = " + c2);
        System.out.println("c1.equals(c2) : " + c1.equals(c2));
    }
}

class SutdaCard {
    int num;
    boolean isKwang;

    SutdaCard() {
        this(1, true);
    }

    SutdaCard(int num, boolean isKwang) {
        this.num = num;
        this.isKwang = isKwang;
    }

    public boolean equals(Object obj) {
        /*
            (1) 매개변수로 넘겨진 객체의 num, isKwang과
            멤버변수 num, isKwang을 비교하도록 오버라이딩 하시오.
         */
    }

    public String toString() {
        return num + ( isKwang ? "K" : "");
    }
}
```

```실행결과
c1 = 3K
c2 = 3K
c1.equals(c2) : true
```

**[정답]**

```Exercise9_1.java
public boolean equals(Object obj) {
    if(obj instanceof SutdaCard) {
        SutdaCard c = (SutdaCard)obj;
        return num == c.num && isKwang == c.isKwang;
    }
    return false;
}
```

**[해설]** 매개변수가 `Object`타입이므로 `instanceof`로 확인한 후에 형변환해서 멤버변수 num과 isKwang을 비교하도록 한다.

**[9-2]** 다음과 같은 실행결과를 얻도록 Point3D클래스의 `equals()`를 멤버변수인 x, y, z의 값을 비교하도록 `오버라이딩`하고, `toString()`은 실행결과를 참고해서 적절히 `오버라이딩` 하시오.

```Exercise9_2.java
class Exercise9_2 {
    public static void main(String[] args) {
        Point3D p1 = new Point3D(1, 2, 3);
        Point3D p2 = new Point3D(1, 2, 3);

        System.out.println(p1);
        System.out.println(p2);
        System.out.println("p1 == p2 ? " + (p1 == p2));
        System.out.println("p1.equals(p2) ? " + p1.equals(p2));

    }
}

class Point3D {
    int x, y, z;

    Point3D(int x, int y, int z) {
        this.x = x;
        this.y = y;
        this.z = z;
    }

    Point3D() {
        this(0, 0, 0);
    }

    public boolean equals(Object obj) {
        /*
            (1) 인스턴스변수 x, y, z를 비교하도록 오버라이딩하시오.
         */
    }

    public String toString() {
       /*
            (2) 인스턴스변수 x, y, z의 내용을 출력하도록 오버라이딩하시오.
        */
    }
}
```

```실행결과
[1,2,3]
[1,2,3]
p1==p2?false
p1.equals(p2)?true
```

**[정답]**

**(1)**

```Exercise9_2.java
public boolean equals(Object obj) {
    if (obj instanceof Point3D) {
        Point3D p = (Point3D)obj;
        return x == p.x && y == p.y && z == p.z;
    }
    return false;
}
```

**(2)**

```Exercise9_2.java
public String toString() {
   return "[" + x + "," + y + "," + z + "]";
}
```

**[9-3]** 다음과 같은 실행결과가 나오도록 코드를 완성하시오.

```Exercise9_3.java
class Exercise9_3 {
    public static void main(String[] args) {
        String fullPath = "c:\\jdk1.8\\work\\PathseparateTest.java";
        String path = "";
        String fileName = "";

        /*
            (1) 알맞은 코드를 넣어 완성하시오.
         */

        System.out.println("fullPath:" + fullPath);
        System.out.println("path:" + path);
        System.out.println("fileName:" + fileName);
    }
}
```

```실행결과
fullPath:c:\jdk1.8\work\PathSeparateTest.java
path:c:\jdk1.8\work
fileName:PathSeparateTest.java
```

**[정답]**

```Exercise9_3.java
int pos = fullPath.lastIndexOf("\\");
if(pos != -1) {
    path = fullPath.substring(0, pos);
    fileName = fullPath.substring(pos + 1);
}
```

**[해설]** `lastIndexOf`를 이용해 경로 구분이 끝나는 부분을 찾은 뒤 `substring`을 사용해서 path와 fileName을 나누어 저장한다.

**[9-4]** 다음과 같이 정의된 메서드를 작성하고 테스트하시오.<br/>
메서드명 : printGraph<br/>
기능 : 주어진 배열에 담긴 값만큼 주어진 문자를 가로로 출력한 후, 값을 출력한다.<br/>
반환타입 : 없음<br/>
매개변수 : int[] dataArr - 출력할 그래프의 데이터<br/>
char ch - 그래프로 출력할 문자

```Exercise9_4.java
class Exercise9_4 {
    static void printGraph(int[] dataArr, char ch) {
        /*
            (1) printGraph메서드를 작성하시오.
         */
    }

    public static void main(String[] args) {
        printGraph(new int[]{3,7,1,4}, '"');
    }
}
```

```실행결과
***3
*******7
*1
****4
```

**[정답]**

```Exercise9_4.java
static void printGraph(int[] dataArr, char ch) {
    for (int i = 0; i < dataArr.length; i++) {
        for (int j = 0; j < dataArr[i]; j++) {
            System.out.print(ch);
        }
        System.out.println(dataArr[i]);
    }
}
```

**[해설]** 반복문으로 배열에 저장된 숫자를 읽어서 그 숫자만큼 문자를 출력하도록 한다.

**[9-5]** 다음과 같이 정의된 메서드를 작성하고 테스트하시오.<br/>
메서드명 : count<br/>
기능 : 주어진 문자열(src)에 찾으려는 문자열(target)이 몇 번 나오는지 세어서 반환한다.<br/>
반환타입 : int<br/>
매개변수 : String src<br/>
String target<br/>

**[Hint]** String클래스의 `indexOf(String str, int fromIndex)`를 사용할 것

```Exercise9_5.java
class Exercise9_5 {
    public static int count(String src, String target) {
        int count = 0; // 찾은 횟수
        int pos = 0; // 찾기 시작한 위치

        /*
            (1) 반복문을 사용해서 아래의 과정을 반복한다.
            1. src에서 target을 pos의 위치부터 찾는다.
            2. 찾으면 count의 값을 1 증가 시키고,
                pos의 값을 target.length만큼 증가시킨다.
            3. indexOf의 결과가 -1이면 반복문을 빠져나가서 count를 반환한다.
         */
    }
    
    public static void main(String[] args) {
        System.out.println(count("12345AB12AB345AB", "AB"));
        System.out.println(count("12345", "AB"));
    }
}
```

```실행결과
3
0
```

**[정답]**

```Exercise9_5.java
public static int count(String src, String target) {
    int count = 0; // 찾은 횟수
    int pos = 0; // 찾기 시작한 위치

    while(true) {
        pos = src.indexOf(target, pos);
        
        if (pos != -1) {
           count++;
           pos += target.length();
        } else {
            break;
        }
    }

    return count;
}
```

**[해설]** `indexOf`는 주어진 문자열이 존재하는지 확인하여 해당 문자열의 시작 위치를 알려준다. 찾지 못할 경우 -1을 반환한다.<br/>

문자열을 찾으면 count의 값을 1 증가 시키고, 찾은 문자열의 이후부터 탐색할 수 있도록 pos의 값을 target.length만큼 증가시킨다.

**[9-6]** 다음과 같이 정의된 메서드를 작성하고 테스트하시오.<br/>
메서드명 : fillZero<br/>
기능 : 주어진 문자열(숫자)로 주어진 길이의 문자열로 만들고, 왼쪽 빈 공간은 '0'으로 채운다. 만일 주어진 문자열이 null이거나 문자열의 길이가 length의 값과 같으면 그대로 반환한다. 만일 주어진 length의 값이 0보다 같거나 작은 값이면, 빈 문자열("")을 반환한다.<br/>
반환타입 : String<br/>
매개변수 : String src - 변환할 문자열<br/>
int length - 변환한 문자열의 길이<br/>

```Exercise9_6.java
class Exercise9_6 {
    public static String fillZero(String src, int length) {
        /*
            (1) fillZero메서드를 작성하시오.
            1. src가 널이거나 src.length()가 length와 같으면 src를 그대로 반환한다.
            2. length의 값이 0보다 같거나 작으면 빈 문자열("")을 반환한다.
            3. src의 길이가 length의 값보다 크면 src를 length만큼 잘라서 반환한다.
            4. 길이가 length인 char배열을 생성한다.
            5. 4에서 생성한 char배열을 '0'으로 채운다.
            6. src에서 문자배열을 뽑아내서 4에서 생성한 배열에 복사한다.
            7. 4에서 생성한 배열로 String을 생성해서 반환한다.
         */
    }

    public static void main(String[] args) {
        String src = "12345";
        System.out.println(fillZero(src, 10));
        System.out.println(fillZero(src, -1));
        System.out.println(fillZero(src, 3));
    }
}
```

```실행결과
0000012345

123
```

**[정답]**

```Exercise9_6.java
public static String fillZero(String src, int length) {
    if (src == null || src.length() == length)
        return src;

    if (length <= 0)
        return "";

    if (src.length() > length) {
        return src.substring(0, length);
    }

    char[] newStr = new char[length];
    for (int i = 0; i < newStr.length; i++) {
        newStr[i] = '0';
    }
    System.arraycopy(src.toCharArray(), 0, newStr, length - src.length(), src.length());
    return String.valueOf(newStr);
}
```

**[9-7]** 다음과 같이 정의된 메서드를 작성하고 테스트하시오.<br/>
메서드명 : contains<br/>
기능 : 첫 번째 문자열(src)에 두 번째 문자열(target)이 포함되어 있는지 확인한다.<br/>
포함되어 있으면 true, 그렇지 않으면 false를 반환한다.<br/>
반환타입 : boolean<br/>
매개변수 : String src<br/>
String target<br/>

**[Hint]** String클래스의 `indexOf()`를 사용할 것

```Exercise9_7.java
class Exercise9_7 {
    /*
        (1) contains메서드를 작성하시오.
     */
    public static void main(String[] args) {
        System.out.println(contains("12345", "23"));
        System.out.println(contains("12345", "67"));
    }
}
```

```실행결과
true
false
```

**[정답]**

```Exercise9_7.java
public static boolean contains(String src, String target) {
    return src.indexOf(target) != -1;
}
```

**[9-8]** 다음과 같이 정의된 메서드를 작성하고 테스트하시오.<br/>
메서드명 : round<br/>
기능 : 주어진 값을 반올림하여, 소수점 이하 n자리의 값을 반환한다.<br/>
예를 들어 n의 값이 3이면, 소수점 4째 자리에서 반올림하여 소수점 이하 3자리의 수를 반환한다.<br/>
반환타입 : double<br/>
매개변수 : double d - 변환할 값<br/>
int n - 반올림한 결과의 소수점 자리<br/>

**[Hint]** `Math.round()`와 `Math.pow()`를 이용하라.

```Exercise9_8.java
class Exercise9_8 {
    /*
        (1) round메서드를 작성하시오.
     */
    
    public static void main(String[] args) {
        System.out.println(round(3.1415, 1));
        System.out.println(round(3.1415, 2));
        System.out.println(round(3.1415, 3));
        System.out.println(round(3.1415, 4));
        System.out.println(round(3.1415, 5));
    }
}
```

```실행결과
3.1
3.14
3.142
3.1415
3.1415
```

**[정답]**

```Exercise9_8.java
static double round(double d, int n) {
    return Math.round(d * Math.pow(10, n)) / Math.pow(10, n);
}
```

**[해설]** `Math.round()`는 소수점 첫째자리에서 반올림해서 반환한다.<br/>
`Math.pow(double a, double b)`는 a의 b제곱을 반환한다.<br/>

`Math.pow(10, n)`를 이용해 소수점 n째자리까지를 정수부로 만들고, `Math.round()`로 반올림한 후 다시 `Math.pow(10, n)`로 나눠준다.

**[9-9]** 다음과 같이 정의된 메서드를 작성하고 테스트하시오.<br/>
메서드명 : delChar<br/>
기능 : 주어진 문자열에서 금지된 문자들을 제거하여 반환한다.<br/>
반환타입 : String<br/>
매개변수 : String src - 변환할 문자열<br/>
String delCh - 제거할 문자들로 구성된 문자열<br/>

**[Hint]** `StringBuffer`와 String클래스의 `charAt(int i)`과 `indexOf(int ch)`를 사용하라.

```Exercise9_9.java
class Exercise9_9 {
    /*
        (1) delChar메서드를 작성하시오.
     */

    public static void main(String[] args) {
        System.out.println("(1!2@3^4~5)" + " -> " + delChar("(1!2@3^4~5)", "~!@#$%^&*()"));
        System.out.println("(1  2       3   4\t5)" + " -> " + delChar("(1  2       3   4\t5)", " \t"));
    }
}
```

```실행결과
(1!2@3^4~5) -> 12345
(1  2       3   4	5) -> (12345)
```

**[정답]**

```
public static String delChar(String src, String delCh) {
    StringBuffer sb = new StringBuffer(src.length());
    for(int i = 0; i < src.length(); i++) {
        char ch = src.charAt(i);

        if(delCh.indexOf(ch) == -1) {
            sb.append(ch);
        }
    }
    return sb.toString();
}
```

**[해설]** 반복문을 이용해서 변환할 문자열 src를 순서대로 가져와서 금지된 문자 delCh에 포함되어있는지 확인한다. 포함되어 있지 않을 경우에만 `StringBuffer`에 추가한다.

**[9-10]** 다음과 같이 정의된 메서드를 작성하고 테스트하시오.<br/>
메서드명 : format<br/>
기능 : 주어진 문자열을 지정된 크기의 문자열로 변환한다. 나머지 공간은 공백으로 채운다.<br/>
매개변수 : String str - 변환할 문자열<br/>
int length - 변환된 문자열의 길이<br/>
int alignment - 변환된 문자열의 정렬조건<br/>
(0 : 왼쪽 정렬, 1 : 가운데 정렬, 2 : 오른쪽 정렬)<br/>

```Exercise9_10.java
class Exercise9_10 {
    /*
        (1) formath 메서드를 작성하시오.
        1. length의 값이 str의 길이보다 작으면 length만큼만 잘라서 반환한다.
        2. 1의 경우가 아니면, length크기의 char배열을 생성하고 공백으로 채운다.
        3. 정렬조건(alignment)의 값에 따라 문자열(str)을 복사할 위치를 결정한다.
        (System.arraycopy() 사용)
        4. 2에서 생성한 char배열을 문자열로 만들어서 반환한다.
     */
    
    public static void main(String[] args) {
        String str = "가나다";

        System.out.println(format(str, 7, 0)); // 왼쪽 정렬
        System.out.println(format(str, 7, 1)); // 가운데 정렬
        System.out.println(format(str, 7, 2)); // 오른쪽 정렬
    }
}
```

```실행결과
가나다    
  가나다  
    가나다
```

**[정답]**

```Exercise9_10.java
static String format(String str, int length, int alignment) {
    int diff = length - str.length();
    if (diff < 0) {
        return str.substring(0, length);
    }

    char[] ch = new char[length];
    for (int i = 0; i < ch.length; i++) {
        ch[i] = ' ';
    }

    switch (alignment) {
        case 0:
            System.arraycopy(str.toCharArray(), 0, ch, 0, str.length());
            break;
        case 1:
            System.arraycopy(str.toCharArray(), 0, ch, diff/2, str.length());
            break;
        case 2:
            System.arraycopy(str.toCharArray(), 0, ch, diff, str.length());
            break;
    }
    return new String(ch);
}
```

**[9-11]** 커맨드라인으로 2~9사이의 두 개의 숫자를 받아서 두 숫자사이의 구구단을 작성하는 프로그램을 작성하시오.<br/>
예를 들어 3과 5를 입력하면 3단부터 5단까지 출력한다.

```실행결과
> java Exercise9_11 2
시작 단과 끝 단, 두 개의 정수를 입력해주세요.
USAGE : GugudanTest 3 5

> java Exercise9_11 1 5
단위 범위는 2와 9사이의 값이어야 합니다.
USAGE : GugudanTest 3 5

> java Exercise9_11 3 5
3*1=3
3*2=6
3*3=9
3*4=12
3*5=15
3*6=18
3*7=21
3*8=24
3*9=27

4*1=4
4*2=8
4*3=12
4*4=16
4*5=20
4*6=24
4*7=28
4*8=32
4*9=36

5*1=5
5*2=10
5*3=15
5*4=20
5*5=25
5*6=30
5*7=35
5*8=40
5*9=45
```

**[정답]**

```Exercise9_11.java
import java.util.Scanner;

class Exercise9_11 {
    public static void main(String[] args) {
        try {
            if (args.length != 2) {
                throw new Exception("시작 단과 끝 단, 두 개의 정수를 입력해주세요.");
            }

            int start = Integer.parseInt(args[0]);
            int end = Integer.parseInt(args[1]);

            if (!(2 <= start && 9 >= start && 2 <= end && 9 >= end)) {
                throw new Exception("단위 범위는 2와 9사이의 값이어야 합니다.");
            }

            if (start > end) {
                int tmp = end;
                end = start;
                start = tmp;
            }

            for(int i = start; i <= end; i++) {
                for(int j = 1; j <= 9; j++) {
                    System.out.printf("%d*%d=%d%n", i, j, i*j);
                }
                System.out.println();
            }

        } catch(Exception e) {
            System.out.println(e.getMessage());
            System.out.println("USAGE : GugudanTest 3 5");
            System.exit(0);
        }
    }
}
```

**[9-12]** 다음과 같이 정의된 메서드를 작성하고 테스트하시오.<br/>
**[주의]** Math.random()을 사용하는 경우 실행결과와 다를 수 있음.<br/>

메서드명 : getRand<br/>
기능 : 주어진 범위(from-to)에 속한 임의의 정수값을 반환한다.<br/>
(양쪽 경계값 모두 범위에 포함)<br/>
from의 값이 to의 값보다 클 경우도 처리되어야 한다.<br/>
반환타입 : int<br/>
매개변수 : int from - 범위의 시작값<br/>
int to - 범위의 끝값<br/>

**[Hint]** `Math.random()`과 절대값을 반환하는 `Math.abs(int a)`, 그리고 둘 중에 작은 값을 반환하는 `Math.min(int a, int b)`를 사용하라.

```Exercise9_12.java
class Exercise9_12 {
    /*
        (1) getRand메서드를 작성하시오.
     */

    public static void main(String[] args) {
        for (int i = 0; i < 20; i++)
            System.out.print(getRand(1, -3) + ",");
    }
}
```

```실행결과
1,0,-2,-3,-3,1,-3,1,-3,1,0,-2,1,-3,-2,0,1,-2,-3,-3,
```

**[정답]**

```Exercise9_12.java
static int getRand(int from, int to) {
    return (int) (Math.random() * Math.abs((to - from) + 1)) + Math.min(from, to);
}
```

**[해설]** `Math.random()`에 곱해주는 값은 범위에 포함된 정수의 개수이다.<br/>
범위에 포함된 정수의 개수는 `(끝값 - 시작값) + 1`로 구할 수 있다.<br/>
`Math.abs()`를 이용해 끝값 - 시작값이 음수가 되지 않도록 처리해줘야 한다.<br/>
`Math.random()`에 더해주는 값은 범위의 시작값이다. `Math.min(int a, int b)`을 이용해서 작은 값이 더해지도록 처리해야 한다.

**[9-13]** 다음은 하나의 긴 문자열(source) 중에서 특정 문자열과 일치하는 문자열의 개수를 구하는 예제이다. 빈 곳을 채워 예제를 완성하시오.

```Exercise9_13.java
class Exercise9_13 {
    public static void main(String[] args) {
        String src= "aabbccAABBCCaa";
        System.out.println(src);
        System.out.println("aa를 " + stringCount(src, "aa") + "개 찾았습니다.");
    }

    static int stringCount(String src, String key) {
        return stringCount(src, key, 0);
    }

    static int stringCount(String src, String key, int pos) {
        int count = 0;
        int index = 0;

        if (key == null || key.length() == 0)
            return 0;

        /*
            (1) 알맞은 코드를 넣어 완성하시오.
         */
        return count;
    }
}
```

```실행결과
aabbccAABBCCaa
aa를 2개 찾았습니다.
```

**[정답]**

```Exercise9_13.java
while((index = src.indexOf(key, pos)) != -1) {
    count++;
    pos = index + key.length();
}
```

**[9-14]** 다음은 화면으로부터 전화번호의 일부를 입력받아 일치하는 전화번호를 주어진 문자열 배열에서 찾아서 출력하는 프로그램이다. 알맞은 코드를 넣어 프로그램을 완성하시오.<br/>
**[Hint]** `Pattern`, `Matcher`클래스를 사용할 것

```Exercise9_14.java
import java.util.ArrayList;
import java.util.Scanner;

class Exercise9_14 {
    public static void main(String[] args) {
        String[] phoneNumArr = {
                "012-3456-7890",
                "099-2456-7980",
                "088-2346-9870",
                "013-3456-7890"
        };

        ArrayList list = new ArrayList();
        Scanner s = new Scanner(System.in);

        while(true) {
            System.out.print(">>");
            String input = s.nextLine().trim();

            if(input.equals("")) {
                continue;
            } else if(input.equalsIgnoreCase("Q")) {
                System.exit(0);
            }

            /*
                (1) 알맞은 코드를 넣어 완성하시오.
             */

            if(list.size() > 0) {
                System.out.println(list);
                list.clear();
            } else {
                System.out.println("일치하는 번호가 없습니다.");
            }
        }
    }
}
```

```실행결과
>>
>>
>>asdf
일치하는 번호가 없습니다.
>>0
[012-3456-7890, 099-2456-7980, 088-2346-9870, 013-3456-7890]
>>234
[012-3456-7890, 088-2346-9870]
>>7890
[012-3456-7890, 013-3456-7890]
>>q
```

**[정답]**

```Exercise9_14.java
Pattern p = Pattern.compile(".*" + input + ".*");
for (int i = 0; i < phoneNumArr.length; i++) {
    String tmp = phoneNumArr[i].replace("-", "");
    Matcher m = p.matcher(tmp);
    if (m.matches()) {
        list.add(phoneNumArr[i]);
    }
}
```

**[해설]** `".*" + input + ".*"` 입력받은 문자열을 포함하는 모든 문자열을 의미하는 패턴<br/>
`phoneNumArr[i].replace("-", "")` 사용자가 234를 입력했을 때, 01**2-34**56-7890, 088-**234**6-9870가 모두 검색될 수 있도록 전화번호에서 '-'를 제거한 후 패턴과 일치하는지 비교해야 한다.

## 🚩 잊지않기
- **java.lang패키지와 유용한 클래스로 이런 것도 있구나 정도로만 알아두면 좋을 듯 :)**
- [String클래스의 생성자와 메서드](https://chaeichi.github.io/java/2024/09/14/java-lang-2.html#h-string%ED%81%B4%EB%9E%98%EC%8A%A4%EC%9D%98-%EC%83%9D%EC%84%B1%EC%9E%90%EC%99%80-%EB%A9%94%EC%84%9C%EB%93%9C)
- [StringBuffer클래스와 StringBuilder클래스](https://chaeichi.github.io/java/2024/09/17/java-lang-3.html)
- [Math클래스](https://chaeichi.github.io/java/2024/09/18/java-lang-4.html)
- [정규식(Regular Expression) - java.util.regex패키지](https://chaeichi.github.io/java/2024/10/03/java-lang-6.html#h-%EC%A0%95%EA%B7%9C%EC%8B%9Dregular-expression---javautilregex%ED%8C%A8%ED%82%A4%EC%A7%80)