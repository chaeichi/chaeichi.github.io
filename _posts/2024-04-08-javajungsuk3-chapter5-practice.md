---
layout: post
title: "Java의 정석 연습문제 - Chapter05. 배열"
subtitle: Java의 정석 3rd Edition | 남궁성
excerpt_image: /assets/images/excerpt-images/2024-04-08-javajungsuk3-chapter5-practice.png
categories: 
    - 문제풀이
tags: 
    - Java
    - Java의 정석
    - 연습문제
    - 문제풀이
    - 2차원 배열 90도 회전
---
![banner](/assets/images/excerpt-images/2024-04-08-javajungsuk3-chapter5-practice.png)

**[5-1]** 다음은 `배열을 선언하거나 초기화한 것`이다.잘못된 것을 고르고 그 이유를 설명하시오.<br/>
a. int[] arr[];<br/>
b. int[] arr = {1,2,3, }; // 마지막의 쉼표 ','는 있어도 상관 없음<br/>
c. int[] arr = new int[5];<br/>
**d. int[] arr = new int[5]{1,2,3,4,5};** // 배열의 크기가 괄호{} 안의 데이터 개수에 따라 자동적으로 결정되기 때문에 두 번째 대괄호 []에는 숫자를 넣으면 안됨<br/>
**e. int arr[5];** // 배열 선언 시에 배열 크기를 지정하는 것은 불가능<br/>
f. int[] arr[] = new int[3][];<br/>

**[정답]** d, e

**[5-2]** 다음과 같은 배열이 있을 때, `arr[3].length`의 값은 얼마인가?

```Exercise5_2.java
int[][] arr = {
    {5, 5, 5, 5, 5},
    {10, 10, 10},
    {20, 20, 20, 20},
    {30, 30}
}
```

**[정답]** 2<br/>

**[5-3]** `배열 arr에 담긴 모든 값을 더하는 프로그램`을 완성하시오.

```Exercise5_3.java
class Exercise5_3 {
    public static void main(String[] args) {
        int[] arr = {10, 20, 30, 40, 50};
        int sum = 0;

        /*
            (1) 알맞은 코드를 넣어 완성하시오.
         */

        System.out.println("sum=" + sum);
    }
}
```

```실행결과
sum=150
```

**[정답]**

```Exercise5_3.java
for(int i : arr)
    sum += i;
```

**[5-4]** `2차원 배열 arr에 담긴 모든 값의 총합과 평균을 구하는 프로그램`을 완성하시오.

```Exercise5_4.java
class Exercise5_4 {
    public static void main(String[] args) {
        int[][] arr = {
                {5, 5, 5, 5, 5},
                {10, 10, 10, 10, 10},
                {20, 20, 20, 20, 20},
                {30, 30, 30, 30, 30}
        };

        int total = 0;
        float average = 0;

        /*
            (1) 알맞은 코드를 넣어 완성하시오.
         */

        System.out.println("total=" + total);
        System.out.println("average=" + average);
    }
}
```

```실행결과
total=325
average=16.25
```

**[정답]**

```
for(int i = 0; i < arr.length; i++) {
    for(int j = 0; j < arr[0].length; j++) {
        total += arr[i][j];
    }
}

average = (float) total / (arr.length * arr[0].length);
```

**[해설]** **평균은 총합에서 배열의 모든 요소의 개수로 나누면 된다.** `arr.length * arr[0].length`는 `행 X 열`을 나타낸다. (모든 행의 요소 개수가 모두 동일하기 때문에 `arr[0].length`로 열을 계산하는 것이 가능하다.)

**[5-5]** 다음은 `1과 9 사이의 중복되지 않은 숫자로 이루어진 3자리 숫자를 만들어내는 프로그램`이다. (1) ~ (2)에 알맞은 코드를 넣어서 프로그램을 완성하시오.<br/>
**[참고]** `Math.random()`을 사용했기 때문에 실행결과와 다를 수 있다.

```Exercise5_5.java
class Exercise5_5 {
    public static void main(String[] args) {
        int[] ballArr = {1, 2, 3, 4, 5, 6, 7, 8, 9};
        int[] ball3 = new int[3];

        // 배열 ballArr의 임의의 요소를 골라서 위치를 바꾼다.
        for(int i = 0; i < ballArr.length; i++) {
            int j = (int) (Math.random() * ballArr.length);
            int tmp = 0;

            /*
                (1) 알맞은 코드를 넣어 완성하시오.
             */
        }

        // 배열 ballArr의 앞에서 3개의 수를 배열 ball3로 복사한다.
        /* (2) */

        for(int i = 0; i < ball3.length; i++) {
            System.out.print(ball3[i]);
        }
    }
}
```

```실행결과
486
```

**[정답]**

(1)

```Exercise5_5.java
tmp = ballArr[i];
ballArr[i] = ballArr[j];
ballArr[j] = tmp;
```

(2)

```Exercise5_5.java
System.arraycopy(ballArr, 0, ball3, 0, 3);
```

**[해설]**
- 변수 tmp를 이용하여 i번째 요소 `ballArr[i]`와 임의로 고른 j번째 요소 `ballArr[j]`의 값을 서로 바꾼다.

- `arraycopy()`를 이용하면 **지정된 범위의 값들을 한 번에 통째로 복사할 수 있다.**
    - `어느 배열의` `몇 번째 요소`에서 `어느 배열로` `몇 번째 요소로` `몇 개의 값을 복사`할 것인지 지정해줘야 함
    - System.arraycopy(num, 0, newNum, 0, num.length); // num[0]에서 newNum[0]으로 num.length개의 데이터를 복사
    - **각 요소들이 연속적으로 저장되어 있다는 배열의 특성 때문에 가능**

**[5-6]** 다음은 `거스름돈을 몇 개의 동전으로 지불할 수 있는지`를 계산하는 문제이다. `변수 money의 금액을 동전으로 바꾸었을 때 각각 몇 개의 동전이 필요한지 계산해서 출력하라.` 단, 가능한 한 적은 수의 동전으로 거슬러 주어야한다. (1)에 알맞은 코드를 넣어 프로그램을 완성하시오.<br/>
**[Hint]** 나눗셈 연산자와 나머지 연산자를 사용해야 한다.

```Exercise5_6.java
class Exercise5_6 {
    public static void main(String[] args) {
        // 큰 금액의 동전을 우선적으로 거슬러 줘야한다.
        int[] coinUnit = {500, 100, 50, 10};

        int money = 2680;
        System.out.println("money=" + money);

        for(int i = 0; i < coinUnit.length; i++) {
            /*
                (1) 알맞은 코드를 넣어 완성하시오.
             */
        }
    }
}
```

```실행결과
money=2680
500원: 5
100원: 1
50원: 1
10원: 3
```

**[정답]**

```Exercise5_6.java
System.out.println(coinUnit[i] + "원: " + money / coinUnit[i]);
money = money % coinUnit[i];
```

**[해설]** 변수 `money`를 `coninUnit[i]`로 나누어 거슬러 줄 동전의 개수를 구하고, 나머지 연산을 하여 `money`에 남은 금액을 저장한다. 

**[5-7]** 문제 5-6에 동전의 개수를 추가한 프로그램이다. `커맨드 라인으로부터 거슬러 줄 금액을 입력받아 계산`한다. `보유한 동전의 개수로 거스름돈을 지불할 수 없으면, '거스름돈이 부족합니다.'라고 출력하고 종료한다.` `지불할 돈이 충분히 있으면, 거스름돈을 지불한 만큼 가진 돈에서 빼고 남은 동전의 개수를 화면에 출력한다.` (1)에 알맞은 코드를 넣어서 프로그램을 완성하시오.

```Exercise5_7.java
class Exercise5_7 {
    public static void main(String[] args) {
        if(args.length!=1) {
            System.out.println("USAGE: java Exercise5_7 3120");
            System.exit(0);
        }

        // 문자열을 숫자로 변환한다. 입력한 값이 숫자가 아닐 경우 예외가 발생한다.
        int money = Integer.parseInt(args[0]);
        
        System.out.println("money=" + money);
        
        int[] coinUnit = {500, 100, 50, 10}; // 동전의 단위
        int[] coin = {5, 5, 5, 5}; // 단위별 동전의 개수

        for(int i = 0; i < coinUnit.length; i++) {
            int coinNum = 0;

            /* (1) 아래의 로직에 맞게 코드를 작성하시오.
                1. 금액(money)을 동전단위로 나눠서 필요한 동전의 개수(coinNum)를 구한다.
                2. 배열 coin에서 coinNum만큼의 동전을 뺀다.
                (만일 충분한 동전이 없다면 배열 coin에 있는 만큼만 뺀다.)
                3. 금액에서 동전의 개수(coinNum)와 동전단위를 곱한 값을 뺀다.
             */

            System.out.println(coinUnit[i] + "원: " + coinNum);
        }

        if(money > 0) {
            System.out.println("거스름돈이 부족합니다.");
            System.exit(0);
        }

        System.out.println("=남은 동전의 개수=");

        for(int i = 0; i < coinUnit.length; i++) {
            System.out.println(coinUnit[i] + "원: " + coin[i]);
        }
    }
}
```

```실행결과
// 커맨드 라인으로부터 입력받은 매개변수가 1개가 아닐 경우
USAGE: java Exercise5_7 3120

// java Exercise5_7 3170
money=3170
500원: 5
100원: 5
50원: 3
10원: 2
=남은 동전의 개수=
500원: 0
100원: 0
50원: 2
10원: 3

// java Exercise5_7 3510
money=3510
500원: 5
100원: 5
50원: 5
10원: 5
거스름돈이 부족합니다.
```

**[정답]**

```Exercise5_7.java
// 1. 금액(money)을 동전단위로 나눠서 필요한 동전의 개수(coinNum)를 구한다.
coinNum = money / coinUnit[i];

// 2. 배열 coin에서 coinNum만큼의 동전을 뺀다.
// (만일 충분한 동전이 없다면 배열 coin에 있는 만큼만 뺀다.)
if(coin[i] < coinNum) {
    coinNum = coin[i];
    coin[i] = 0;
} else {
    coin[i] = coin[i] - coinNum;
}

// 3. 금액에서 동전의 개수(coinNum)와 동전단위를 곱한 값을 뺀다.
money = money - (coinUnit[i] * coinNum);
```

**[5-8]** 다음은 `배열 answer에 담긴 데이터를 읽고 각 숫자의 개수를 세어서 개수만큼 '*'을 찍어서 그래프를 그리는 프로그램이다.` (1) ~ (2)에 알맞은 코드를 넣어서 완성하시오.

```Exercise5_8.java
class Exercise5_8 {
    public static void main(String[] args) {
        int[] answer = {1, 4, 4, 3, 1, 4, 4, 2, 1, 3, 2};
        int[] counter = new int[4];

        for(int i = 0; i < answer.length; i++) {
            /* (1) 알맞은 코드를 넣어 완성하시오. */
        }

        for(int i = 0; i < counter.length; i++) {
            /*
                (2) 알맞은 코드를 넣어 완성하시오
            */
            
            System.out.println();
        }
    }
}
```

```실행결과
```

**[정답]**

(1)

```Exercise5_8.java
counter[answer[i] - 1]++;
```

(2)

```Exercise5_8.java
System.out.print(counter[i]);

for(int j = 0; j < counter[i]; j++) {
    System.out.print("*");
}
```

**[해설]** 각 숫자의 개수를 셀 때, **배열의 인덱스는 0번부터 시작**이므로 `counter`의 `크기는 4`이지만 범위는 `0 ~ 3`이므로 `answer[i]에서 1을 빼준다.`

**[5-9]** `주어진 배열을 시계방향으로 90도 회전시켜서 출력하는 프로그램`을 완성하시오.

```Exercise5_9.java
class Exercise5_9 {
    public static void main(String[] args) {
        char[][] star = {
                {'*', '*', ' ', ' ', ' '},
                {'*', '*', ' ', ' ', ' '},
                {'*', '*', '*', '*', '*'},
                {'*', '*', '*', '*', '*'}
        };

        char[][] result = new char[star[0].length][star.length];

        for(int i = 0; i < star.length; i++) {
            for(int j = 0; j < star[i].length; j++){
                System.out.print(star[i][j]);
            }
            System.out.println();
        }
        System.out.println();

        for(int i = 0; i < star.length; i++) {
            for(int j = 0; j < star[i].length; j++) {
            /*
                (1) 알맞은 코드를 넣어 완성하시오.
             */
            }
        }

        for(int i = 0; i < result.length; i++) {
            for(int j = 0; j < result[i].length; j++) {
                System.out.print(result[i][j]);
            }
            System.out.println();
        }
    }
}
```

```실행결과
**   
**   
*****
*****

****
****
**  
**  
** 
```

**[정답]**

```Exercise5_9.java
int x = j;
int y = star.length - 1 - i;

result[x][y] = star[i][j];
```

**[해설]**

![2차원 배열 90도 회전](/assets/images/javajungsuk3/Exercise5_9.png)

- `N X M`의 2차원 배열을 90도 회전시키면 `M X N`의 2차원 배열이 된다.
    - **회전시킨 행 번호 = 회전하기 전 열 번호**
    - **회전시킨 열 번호 = N - 1 - 회전 전의 행 번호**

**[5-10]** 다음은 `알파벳과 숫자를 아래에 주어진 암호표로 암호화하는 프로그램`이다. (1)에 알맞은 코드를 넣어서 완성하시오.

![암호표](/assets/images/javajungsuk3/Exercise5_10.png)

```Exercise5_10.java
class Exercise5_10 {
    public static void main(String[] args) {
        char[] abcCode = {
                '`', '~', '!', '@', '#', '$', '%', '^', '&', '*',
                '(',')', '-', '_', '+', '=', '|', '[', ']', '{',
                '}', ';', ':', ',', '.', '/'};

        char[] numCode = {'q', 'w', 'e', 'r', 't', 'y', 'u', 'i', 'o', 'p'};

        String src = "abc123";
        String result = "";

        // 문자열 src의 문자를 charAt()으로 하나씩 읽어서 변환 후 result에 저장
        for(int i = 0; i < src.length(); i++) {
            char ch = src.charAt(i);
            /*
                (1) 알맞은 코드를 넣어 완성하시오.
             */
        }

        System.out.println("src:" + src);
        System.out.println("result:" + result);
    }
}
```

```실행결과
src:abc123
result:`~!wer
```

**[정답]**

```Exercise5_10.java
// ch가 영어 소문자인 경우
if(ch >= 'a' && ch <= 'z') {
    result += abcCode[ch - 'a'];
}

// ch가 숫자인 경우
if(ch >= '0' && ch <= '9') {
    result += numCode[ch - '0'];
}
```

**[5-11]** `주어진 2차원 배열의 데이터보다 가로와 세로로 1이 더 큰 배열을 생성해서 배열의 행과 열의 마지막 요소에 각 열과 행의 총합을 저장하고 출력하는 프로그램`이다. (1)에 알맞은 코드를 넣어서 완성하시오.

```Exercise5_11.java
class Exercise5_11 {
    public static void main(String[] args) {
        int[][] score = {
                {100, 100, 100},
                {20, 20, 20},
                {30, 30, 30},
                {40, 40, 40},
                {50, 50, 50}
        };

        int[][] result = new int[score.length + 1][score[0].length + 1];

        for(int i = 0; i < score.length; i++) {
            for(int j = 0; j < score[i].length; j++) {
                /*
                    (1) 알맞은 코드를 넣어 완성하시오.
                 */
            }
        }

        for(int i = 0; i < result.length; i++) {
            for(int j = 0; j < result[i].length; j++) {
                System.out.printf("%4d", result[i][j]);
            }
            System.out.println();
        }
    }
}
```

```실행결과
 100 100 100 300
  20  20  20  60
  30  30  30  90
  40  40  40 120
  50  50  50 150
 240 240 240 720
```

**[정답]**

```Exercise5_11.java
// 배열 score에 담긴 값들을 배열 result에 동일한 위치에 복사
result[i][j] = score[i][j];

// 배열 result에 마지막 열에 동일한 행들의 값들을 더해줌
result[i][score[0].length] += score[i][j];

// 배열 result에 마지막 행에 동일한 열들의 값들을 더해줌
result[score.length][j] += score[i][j];

// 배열 result에 마지막 행, 마지막 열에 배열 score에 담긴 값들을 모두 더해줌
result[score.length][score[0].length] += score[i][j];
```

**[5-12]** `예제 5-23`을 변경하여, 아래와 같은 결과가 나오도록 하시오.

```실행결과
Q1. chair의 뜻은? dmlwk
틀렸습니다. 정답은 의자입니다.

Q2. computer의 뜻은? 컴퓨터
정답입니다.

Q3. integer의 뜻은? 정수
정답입니다.

전체 3문제 중 2문제 맞추셨습니다.
```

**[정답]**

```Exercise5_12.java
import java.util.Scanner;

class Exercise5_12 {
    public static void main(String[] args) {
        String[][] words = {
                {"chair", "의자"}, // words[0][0], words[0][1]
                {"computer", "컴퓨터"}, // words[1][0], words[1][1]
                {"integer", "정수"} // words[2][0], words[2][1]
        };

        int answer = 0; // 문제를 맞춘 개수를 저장하기 위한 변수
        Scanner scanner = new Scanner(System.in);

        for(int i = 0; i < words.length; i++) {
            System.out.printf("Q%d. %s의 뜻은? ", i+1, words[i][0]);

            String tmp = scanner.nextLine();

            if(tmp.equals(words[i][1])) {
                System.out.printf("정답입니다.%n%n");
                answer++;
            } else {
                System.out.printf("틀렸습니다. 정답은 %s입니다.%n%n", words[i][1]);
            }
        }
        System.out.println("전체 " + words.length + "문제 중 " + answer + "문제 맞추셨습니다.");
    }
}
```

**[5-13]** `단어의 글자위치를 섞어서 보여주고 원래의 단어를 맞추는 예제`이다. 실행결과와 같이 동작하도록 예제의 빈 곳을 채우시오.

```Exercise5_13.java
import java.util.Scanner;

class Exercise5_13 {
    public static void main(String[] args) {
        String[] words = {"television", "computer", "mouse", "phone"};

        Scanner scanner = new Scanner(System.in);

        for(int i = 0; i < words.length; i++) {
            char[] question = words[i].toCharArray(); // String을 char[]로 변환

            /*
                (1) 알맞은 코드를 넣어 완성하시오.
                char 배열 question에 담긴 문자의 위치를 임의로 바꾼다.
             */

            System.out.printf("Q%d. %s의 정답을 입력하세요.>", i+1, new String(question));

            String answer = scanner.nextLine();

            // trim()으로 answer의 좌우 공백을 제거한 후, equals로 word[i]와 비교
            if(words[i].equals(answer.trim()))
                System.out.printf("맞았습니다.%n%n");
            else
                System.out.printf("틀렸습니다.%n%n");
        }
    }
}
```

```실행결과
Q1. iontiveels의 정답을 입력하세요.>television
맞았습니다.

Q2. tueomprc의 정답을 입력하세요.>computer
맞았습니다.

Q3. semuo의 정답을 입력하세요.>asdf
틀렸습니다.

Q4. neoph의 정답을 입력하세요.>phone
맞았습니다.
```

**[정답]**

```Exercise5_13.java
for(int j = 0; j < question.length; j++) {
    int idx = (int) (Math.random() * question.length);
    char tmp = question[j];
    question[j] = question[idx];
    question[idx] = tmp;
}
```

## 🚩 잊지않기
- [arraycopy()는 지정된 범위의 값들을 한 번에 통째로 복사할 수 있다.](https://chaeichi.github.io/java/2024/03/20/array-2.html#h-systemarraycopy%EB%A5%BC-%EC%9D%B4%EC%9A%A9%ED%95%9C-%EB%B0%B0%EC%97%B4%EC%9D%98-%EB%B3%B5%EC%82%AC)
    - System.arraycopy(num, 0, newNum, 0, num.length); // num[0]에서 newNum[0]으로 num.length개의 데이터를 복사
- **⭐2차원 배열 회전시키는 문제 공부하기⭐**
    - `N X M`의 2차원 배열을 90도 회전시키면 `M X N`의 2차원 배열이 된다.
        - 회전시킨 행 번호 = 회전하기 전 열 번호
        - 회전시킨 열 번호 = N - 1 - 회전 전의 행 번호