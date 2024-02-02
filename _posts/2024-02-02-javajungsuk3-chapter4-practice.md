---
layout: post
title: "Java의 정석 연습문제 - Chapter04. 조건문과 반복문"
subtitle: Java의 정석 3rd Edition | 남궁성
excerpt_image: /assets/images/excerpt-images/2024-02-02-javajungsuk3-chapter4-practice.png
categories: 
    - 문제풀이
tags: 
    - Java
    - Java의 정석
    - 연습문제
    - 문제풀이
    - 피보나치 수열
---
![banner](/assets/images/excerpt-images/2024-02-02-javajungsuk3-chapter4-practice.png)

## ✏️ 연습문제

**4-1.** 다음의 문장들을 `조건식`으로 표현하라.
1. int형 변수 x가 10보다 크고 20보다 작을 때 true인 조건식
    - x > 10 && x < 20
2. char형 변수 ch가 공백이나 탭이 아닐 때 true인 조건식
    - !(ch == ' ' \|\| ch == '\t')
3. char형 변수 ch가 'x' 또는 'X'일 때 true인 조건식
    - ch == 'x' \|\| ch == 'X'
4. char형 변수 ch가 숫자('0' ~ '9')일 때 true인 조건식
    - ch >= '0' && ch <= '9'
5. char형 변수 ch가 영문자(대문자 또는 소문자)일 때 true인 조건식
    - (ch >= 'A' && ch <= 'Z') \|\| (ch >= 'a' && ch <= 'z')
6. int형 변수 year이 400으로 나눠떨어지거나 또는 4로 나눠떨어지고 100으로 나눠떨어지지 않을 때 true인 조건식
    - (year % 400 == 0) \|\| (year % 4 == 0 && year % 100 != 0)
7. boolean형 변수 powerOn이 false일 때 true인 조건식
    - !powerOn
8. 문자열 참조변수 str이 "yes"일 때 true인 조건식
    - str.equals("yes")

**4-2.** `1부터 20까지의 정수` 중에서 `2와 3의 배수가 아닌 수의 총합`을 구하시오.<br/>
**[정답]** 73<br/>
**[해설]**

```Exercise4_2.java
class Exercise4_2 {
    public static void main(String[] args) {
        int sum = 0;

        for(int i=1; i<=20; i++) {
            if(i%2!=0 && i%3!=0) {
                sum += i;
            }
        }
        System.out.println(sum);
    }
}
```

**4-3.** `1+(1+2)+(1+2+3)+(1+2+3+4)+...+(1+2+3+...+10)`의 결과를 계산하시오.<br/>
**[정답]** 220<br/>
**[해설]**

```Exercise4_3.java
class Exercise4_3 {
    public static void main(String[] args) {
        int sum = 0;
        int totalSum = 0;

        for(int i=1; i<=10; i++) {
            sum += i;
            totalSum += sum;
        }
        System.out.println(totalSum);
    }
}
```

|sum|totalSum|
|-|-|
|1|1|
|3 = 1+2|4 = 1+(1+2)|
|6 = 1+2+3|10 = 1+(1+2)+(1+2+3)|
|...|...|
|55 = 1+2+3+...+10|220 = 1+(1+2)+(1+2+3)+(1+2+3+4)+...+(1+2+3+...+10)|


**4-4.** `1+(-2)+3+(-4)+...`과 같은 식으로 계속 더해나갔을 때, 몇까지 더해야 `총합이 100이상`이 되는지 구하시오.<br/>
**[정답]** 199<br/>
**[해설]**

```Exercise4_4.java
class Exercise4_4 {
    public static void main(String[] args) {
        int sum = 0;
        int num = 0;

        // 총합이 100보다 작은 동안 반복
        while (sum < 100) { 
            ++num;

            // num의 값이 홀수이면 sum에 더하고, 짝수이면 sum에서 빼줌
            if (num % 2 != 0) {
                sum += num;
            } else {
                sum -= num;
            }
        }

        System.out.println("sum:" + sum);
        System.out.println("num:" + num);
    }
}
```

```실행결과
sum:100
num:199
```

**4-5.** 다음의 `for문`을 `while문으로 변경`하시오.

```Exercise4_5.java
class Exercise4_5 {
    public static void main(String[] args) {
        
        for(int i=0; i<=10; i++) {
         for(int j=0; j<=i; j++) {
             System.out.print("*");
         }
         System.out.println();
        }
    }
}
```

**[정답]**

```Exercise4_5.java
class Exercise4_5 {
    public static void main(String[] args) {
        int i = 0;

        while(i<=10) {
            int j = 0;
            while (j<=i) {
                System.out.printf("*");
                j++;
            }
            System.out.println();
            i++;
        }
    }
}
```

- 중첩 for문을 while문으로 변경 시 내부 for문 초기화식 위치 실수하지 않기
    - 내부 for문 반복 횟수 = 외부 for문 증가 횟수 * 내부 for문 증가 횟수

**4-6.** 두 개의 주사위를 던졌을 때, `눈의 합이 6이 되는 모든 경우의 수`를 출력하는 프로그램을 작성하시오.<br/>

```Exercise4_6.java
class Exercise4_6 {
    public static void main(String[] args) {
        for(int i=1; i<=6; i++) {
            for(int j=1; j<=6; j++) {
                if(i+j == 6)
                    System.out.printf("[%d, %d]", i, j);
            }
        }
    }
}
```

```실행결과
[1, 5][2, 4][3, 3][4, 2][5, 1]
```

**4-7.** `Math.random()`을 이용해서 `1부터 6 사이의 임의의 정수`를 변수 value에 저장하는 코드를 완성하라. (1)에 알맞은 코드를 넣으시오.<br/>

```Exercise4_7.java
class Exercise4_7 {
    public static void main(String[] args) {
        int value = ( /* (1) */ );
        System.out.println("value:" + value);
    }
}
```

**[정답]** (int)(Math.random() * 6) + 1<br/>
**[해설]**
- `Math.random()`
    - `난수(임의의 수)`를 얻기 위해 사용
    - `0.0과 1.0사이`의 범위에 속하는 `하나의 double 값`을 반환
    - **0.0은 포함하고 1.0은 포함되지 않음**
    - Math.random() `* 6`
        - 0.0 ~ 5.999...
    - `(int)`(Math.random() * 6)
        - 소수점 부분이 필요하지 않을 경우 `정수 부분만 사용`
        - 0 ~ 5
    - (int)(Math.random() * 6) `+ 1`
        - `1부터 범위 시작`
        - 1 ~ 6

**4-8.** 방정식 `2x+4y=10`의 모든 해를 구하시오. 단, x와 y는 정수이고 각각의 범위는 `0<=x<=10`, `0<=y<=10`이다.<br/>

```Exercise4_8.java
class Exercise4_8 {
    public static void main(String[] args) {
        for(int x=0; x<=10; x++) {
            for(int y=0; y<=10; y++) {
                if((2*x) + (4*y) == 10) {
                    System.out.printf("x=%d, y=%d%n", x, y);
                }
            }
        }
    }
}
```

```실행결과
x=1, y=2
x=3, y=1
x=5, y=0
```

**4-9.** 숫자로 이루어진 `문자열 str`이 있을 때, `각 자리의 합을 더한 결과를 출력하는 코드`를 완성하라. 만일 문자열이 "12345"라면, '1+2+3+4+5'의 결과인 15가 출력되어야 한다. (1)에 알맞은 코드를 넣으시오.<br/>
**[Hint]** String클래스의 `charAt(int i)`를 사용<br/>

```Exercise4_9.java
class Exercise4_9 {
    public static void main(String[] args) {
        String str = "12345";
        int sum = 0;

        for(int i=0; i<str.length(); i++) {
            /*
                (1) 알맞은 코드를 넣어 완성하시오.
            */
        }
        System.out.println(sum);
    }
}
```

```실행결과
15
```

**[정답]** sum += str.charAt(i) - '0';

**[해설]**
- `문자열.charAt(index)`
    - **index는 0부터 시작**
    - 반복문을 이용하여 각 문자열의 문자를 하나씩 읽어올 수 있음
    - 문자열 "12345"가 있을 때, charAt(0) = '1'

- 숫자 0 ~ 9는 문자코드가 연속적으로 할당되어 있으므로 문자 '0'을 빼주면 숫자로 변환할 수 있다.

|문자|문자코드|
|-|-|
|...|...|
|'0'|48|
|'1'|49|
|'2'|50|
|'3'|51|
|...|...|

**4-10.** `int 타입의 변수 num`이 있을 때, `각 자리의 합을 더한 결과를 출력하는 코드`를 완성하라. 만일 변수 num의 값이 12345라면, '1+2+3+4+5'의 결과인 15가 출력되어야 한다. (1)에 알맞은 코드를 넣으시오.<br/>
**[주의]** 문자열로 변환하지 말고 숫자로만 처리해야 한다.

```Exercise4_10.java
class Exercise4_10 {
    public static void main(String[] args) {
        int num = 12345;
        int sum = 0;

        /*
            (1) 알맞은 코드를 넣어 완성하시오.
        */

        System.out.println("sum="+sum);
    }
}
```

```실행결과
sum=15
```

**[정답]**

```Exercise4_10.java
while(num > 0) {
    sum += num % 10;
    num /= 10;
}
```

**[해설]** 숫자를 10으로 나머지 연산하면 마지막 자리를 뽑아낼 수 있고, 10으로 나누면 마지막 자리를 제거할 수 있다. num이 0이 될 때까지 해당 과정을 반복하면 num의 모든 자리를 얻을 수 있다.

|num|num%10|num/10|
|-|-|-|
|12345|5|1234|
|1234|4|123|
|123|3|12|
|12|2|1|
|1|1|0|

**4-11.** `피보나치(Fibonnaci) 수열`은 앞의 두 수를 더해서 다음 수를 만들어 나가는 수열이다. 예를 들어 앞의 두 수가 1과 1이라면 그 다음 수는 2가 되고 그 다음 수는 1과 2를 더해서 3이 되어서 1,1,2,3,5,8,13,21,...과 같은 식으로 진행된다. `1과 1부터 시작하는 피보나치 수열의 10번째 수`는 무엇인지 계산하는 프로그램을 완성하시오.

```Exercise4_11.java
class Exercise4_11 {
    public static void main(String[] args) {
        // Fibonnaci 수열의 시작의 첫 두 숫자를 1, 1로 한다.
        int num1 = 1;
        int num2 = 1;
        int num3 = 0; // 세 번째 값
        System.out.print(num1 + ", " + num2);

        for(int i=0; i<8; i++) {
            /*
                (1) 알맞은 코드를 넣어 완성하시오.
            */
        }
    }
}
```

```실행결과
1, 1, 2, 3, 5, 8, 13, 21, 34, 55
```

**[정답]**

```Exercise4_11.java
num3 = num1 + num2; // 세 번째 값을 첫 번째와 두 번째 값을 더해서 구한다.
System.out.print(", " + num3); // 세 번째 값 출력
num1 = num2; // 두 번째 값을 첫 번째 값에 넣는다.
num2 = num3; // 세 번째 값을 두 번째 값에 넣는다.
```

**4-12.** `구구단의 일부분`을 다음과 같이 출력하시오.

```실행결과
2*1=2	3*1=3	4*1=4	
2*2=4	3*2=6	4*2=8	
2*3=6	3*3=9	4*3=12	

5*1=5	6*1=6	7*1=7	
5*2=10	6*2=12	7*2=14	
5*3=15	6*3=18	7*3=21	

8*1=8	9*1=9	
8*2=16	9*2=18	
8*3=24	9*3=27	
```

**[정답]**

```Exercise4_12.java
class Exercise4_12 {
    public static void main(String[] args) {
       for(int k = 2; k <= 8; k+=3) { // 구구단 세 단씩 단을 나눠줌
           for (int i = 1; i <= 3; i++) { // 곱하는 수
               for (int j = k; j <= k+2; j++) { // k부터 K+2단까지 출력
                   if(j==10) { // 10단은 출력하지 않음
                       break;
                   }
                   System.out.printf("%d*%d=%d\t", j, i, j * i);
               }
               System.out.println();
           }
           System.out.println();
       }
    }
}
```

**4-13.** 다음은 주어진 `문자열(value)이 숫자인지를 판별하는 프로그램`이다. (1)에 알맞은 코드를 넣어서 프로그램을 완성하시오.

```Exercise4_13.java
class Exercise4_13 {
    public static void main(String[] args) {
        String value = "12o34";
        char ch = ' ';
        boolean isNumber = true;

        // 반복문과 charAt(int i)를 이용해서 문자열의 문자를
        // 하나씩 읽어서 검사한다.
        for (int i = 0; i < value.length(); i++) {
            /*
                (1) 알맞은 코드를 넣어 완성하시오.
            */
        }
        
        if (isNumber) {
            System.out.println(value + "는 숫자입니다.");
        } else {
            System.out.println(value + "는 숫자가 아닙니다.");
        }
    }
}
```

```실행결과
12o34는 숫자가 아닙니다.
```

**[정답]**

```Exercise4_13.java
ch = value.charAt(i);
if(!(ch >= '0' && ch <= '9')) {
    isNumber = false;
    break;
}
```

**4-14.** 다음은 `숫자맞추기 게임`을 작성한 것이다. `1과 100사이의 값`을 반복적으로 입력해서 컴퓨터가 생각한 값을 맞추면 게임이 끝난다. 사용자가 값을 입력하면, `컴퓨터는 자신이 생각한 값과 비교해서 결과를 알려준다.` 사용자가 컴퓨터가 생각한 숫자를 맞추면 게임이 끝나고 `몇 번만에 숫자를 맞췄는지 알려준다.` (1) ~ (2)에 알맞은 코드를 넣어 프로그램을 완성하시오.

```Exercise4_14.java
import java.util.Scanner;

class Exercise4_14 {
    public static void main(String[] args) {
        // 1~100사이의 임의의 값을 얻어서 answer에 저장한다.
        int answer = /* (1) */;
        int input = 0; // 사용자 입력을 저장할 공간
        int count = 0; // 시도횟수를 세기위한 변수

        // 화면으로부터 사용자 입력을 받기 위해서 Scanner 클래스 사용
        Scanner scanner = new Scanner(System.in);

        do {
            count++;
            System.out.print("1과 100사이의 값을 입력하세요 : ");
            input = scanner.nextInt();

            /*
                (2) 알맞은 코드를 넣어 완성하시오.
            */
        } while(true); // 무한 반복문
    }
}
```

```실행결과
1과 100사이의 값을 입력하세요 : 50
더 작은 수를 입력하세요
1과 100사이의 값을 입력하세요 : 40
더 작은 수를 입력하세요
1과 100사이의 값을 입력하세요 : 30
더 작은 수를 입력하세요
1과 100사이의 값을 입력하세요 : 20
더 큰 수를 입력하세요
1과 100사이의 값을 입력하세요 : 25
더 큰 수를 입력하세요
1과 100사이의 값을 입력하세요 : 27
더 작은 수를 입력하세요
1과 100사이의 값을 입력하세요 : 26
맞췄습니다.
시도횟수는 7번입니다.
```

**[정답]**

(1)

```Exercise4_14.java
(int)(Math.random() * 100) + 1
```

(2)

```Exercise4_14.java
if(input > answer) {
    System.out.println("더 작은 수를 입력하세요");
} else if(input < answer) {
    System.out.println("더 큰 수를 입력하세요");
} else {
    System.out.println("맞췄습니다.");
    System.out.println("시도횟수는 " + count + "번입니다.");
    break;
}
```

**4-15.** 다음은 `회문수`를 구하는 프로그램이다. 회문수(palindrome)란, `숫자를 거꾸로 읽어도 앞으로 읽는 것과 같은 수`를 말한다. 예를 들면 '12321'이나 '13531' 같은 수를 말한다. (1)에 알맞은 코드를 넣어서 프로그램을 완성하시오.

```Exercise4_15.java
class Exercise4_15 {
    public static void main(String[] args) {
        int number = 12321;
        int tmp = number;

        int result = 0; // 변수 number를 거꾸로 변환해서 담을 변수

        while(tmp != 0) {
            /*
                (1) 알맞은 코드를 넣어 완성하시오.
            */
        }
        
        if (number == result) {
           System.out.println(number + "는 회문수 입니다.");
        } else {
            System.out.println(number + "는 회문수가 아닙니다.");
        }
    }
}
```

```실행결과
12321는 회문수 입니다.
```

**[정답]**

```Exercise4_15.java
result = (result * 10) + (tmp % 10);
tmp /= 10;
```

**[해설]** 
tmp % 10과 tmp /= 10은 마지막 자리를 뽑아내고 제거하는데 사용했었다. 여기서는 뽑아낸 수를 그냥 더하는 것이 아니라 기존의 저장된 값에 10을 곱해 자릿값을 한 자리씩 앞으로 밀어내며 더하는 방식이다.

|result|result*10|tmp|tmp%10|tmp/=10|
|-|-|-|-|-|
|0|0|12321|1|1232|
|1|10|1232|2|123|
|12|120|123|3|12
|123|1230|12|2|1|
|1232|12320|1|1|0|
|12321|-|0|-|-|

## 🚩 잊지않기
- **⭐반복문은 조건식이 참(ture)일 때 수행된다는 사실 기억하고 작성 시 헷갈리지 않기⭐**
- 변수 초기화 잊지 않기
- `문제 4-5번`과 같이 중첩 for문을 while문으로 변경 시 내부 for문 초기화식 위치 실수하지 않기
    - `내부 for문 반복 횟수 = 외부 for문 증가 횟수 * 내부 for문 증가 횟수`라는 것을 기억하자.
- `문제 4-12번` 구구단 일부 출력 문제를 풀 때, 구구단을 가로로 출력하는 부분까지는 쉽게 해결했으나 세 단씩 단이 나뉘는 부분에서 꽤 헤맸다😢 (결국 답지랑 다른 방식으로 풀긴 했지만 제대로 동작하는 코드를 작성해내긴 했다...)
- **❌❌❌새로운 유형에 문제를 만나도 멘붕 금지❌❌❌**