---
layout: post
title: "Java의 정석 Chapter04. 조건문과 반복문 (2) - 반복문 for, while, do-while"
subtitle: Java의 정석 3rd Edition | 남궁성
excerpt_image: /assets/images/excerpt-images/2024-01-30-if-switch-for-while-statement-2.png
categories: 
    - Java
tags: 
    - Java
    - Java의 정석
    - 제어문
    - 반복문
    - for문
    - 중첩 for문
    - 별 찍기
    - 구구단
    - for each문
    - while문
    - do-while문
    - break문
    - continue문
    - 이름 붙은 반복문
---
![banner](/assets/images/excerpt-images/2024-01-30-if-switch-for-while-statement-2.png)


## 반복문 - for, while, do-while
- 어떤 작업이 `반복적으로 수행되도록 할 때` 사용
- `주어진 조건을 만족하는 동안` `주어진 문장들을 반복적으로 수행`하므로 `조건식`을 포함
- 반복문의 종류로는 `for문`, `while문`, while문의 변형인 `do-while문`이 있음
- for문이나 while문에 속한 문장은 조건에 따라 한 번도 수행되지 않을 수 있음
- **do-while문에 속한 문장은 무조건 최소한 한 번은 수행될 것이 보장됨**
- for문과 while문은 구조와 기능이 유사하여 어느 경우에나 서로 변환이 가능

### for문
- **주로 반복횟수를 알고 있을 때 사용**
- `초기화`, `조건식`, `증감식`, `블럭{}`으로 이루어져 있음
- 조건식이 `참`인 동안 `블럭{} 내의 문장들을 반복`하다 `거짓`이 되면 `반복문을 벗어남`

```for
for (초기화;조건식;증감식) {
    // 조건식이 참일 때 수행될 문장들을 적는다.
}
```

1. 초기화 수행
2. 조건식이 참이면 블럭 내 문장이 수행되고 증감식이 실행
3. 조건식이 거짓이 되면 for문 전체를 빠져나가게 됨

#### 초기화
- `반복문에 사용될 변수를 초기화`하는 부분
- 처음에 한 번만 수행됨
- 보통 변수 하나로 for문을 제어
- **둘 이상의 변수가 필요할 때는 콤마','를 구분자로 변수를 초기화하면 됨**
- **단, 두 변수의 타입이 같아야 함**

```for
for(int i=1; i<=10; i++) {...} // 변수 i의 값을 1로 초기화
for(int i=1, j=0; i<=10; i++) {...} // int 타입의 변수 i와 j를 선언하고 초기화
```

#### 조건식
- 조건식의 값이 `참(true)`이면 `반복`을 계속하고, `거짓(false)`이면 `반복을 중단하고 for문을 벗어남`

```for
for(int i=1; i<=10; i++) {...} // 'i<=10'가 참인 동안 블럭{}안의 문장들을 반복
```

#### 증감식
- `반복문을 제어하는 변수의 값`을 `증가` 또는 `감소`시키는 식
- 매 반복마다 변수의 값이 **증감식에 의해서** 점진적으로 변하다가 결국 **조건식이 거짓이 되어 for문을 벗어나게 됨**
- **콤마','를 이용해 두 문장 이상을 하나로 연결해서 쓸 수 있음**

```for
for(int i=1; i<=10; i++) {...} // 1부터 10까지 1씩 증가
for(int i=10; i>=1; i--) {...} // 10부터 1까지 1씩 감소
for(int i=1; i<=10; i+=2) {...} // 1부터 10까지 2씩 증가
for(int i=1; i<=10; i*=3) {...} // 1부터 10까지 3배씩 증가
for(int i=1, j=10; i<=10; i++, j--) {...} // i는 1부터 10까지 1씩 증가하고, j는 10부터 1까지 1씩 감소
```

#### for문의 구성요소 생략하기
- **for문의 세 가지 요소(초기화, 조건식, 증감식)는 필요하지 않으면 생략할 수 있으며, 심지어 모두 생략하는 것도 가능**
- 모두 생략할 경우 조건식은 항상 `참`으로 간주되어 무한 반복문이 됨
- 블럭{} 안에 if문을 넣어 특정 조건을 만족하면 for문을 빠져 나오게 해야 함

```for
for(;;) {...} // 초기화, 조건식, 증감식이 모두 생략. 조건식은 참이 된다
```

```FlowEx12.java
class FlowEx12 {
    public static void main(String[] args) {
        for(int i=1; i<=5; i++)
            System.out.println(i); // i의 값을 출력한다.

        for(int i=1; i<=5; i++)
            System.out.print(i); // print()를 쓰면 가로로 출력한다.

        System.out.println();
    }
}
```

```실행결과
1
2
3
4
5
12345
```

|i|i<=5|
|-|-|
|1|1<=5 → true|
|2|2<=5 → true|
|3|3<=5 → true|
|4|4<=5 → true|
|5|5<=5 → true|
|6|6<=5 → false|

- i의 값이 5일 때, 조건식 5<=5는 참이므로 문장이 수행되고 증감식이 실행되어 i의 값이 6으로 변함
- i의 값이 6일 때, 조건식 6<=5는 거짓이므로 for문을 벗어나기 때문에 6은 출력되지 않음

```FlowEx13.java
class FlowEx13 {
    public static void main(String[] args) {
        int sum = 0; // 합계를 저장하기 위한 변수.

        for(int i=1; i<=10; i++) {
            sum += i; // sum = sum + i;
            System.out.printf("1부터 %2d까지의 합: %2d%n", i, sum);
        }
    }
}
```

```실행결과
1부터  1까지의 합:  1
1부터  2까지의 합:  3
1부터  3까지의 합:  6
1부터  4까지의 합: 10
1부터  5까지의 합: 15
1부터  6까지의 합: 21
1부터  7까지의 합: 28
1부터  8까지의 합: 36
1부터  9까지의 합: 45
1부터 10까지의 합: 55
```

```FlowEx14.java
class FlowEx14 {
    public static void main(String[] args) {
        for(int i=1, j=10; i<=10; i++, j--) {
            System.out.printf("%d \t %d%n", i, j);
        }
    }
}
```

```실행결과
1 	 10
2 	 9
3 	 8
4 	 7
5 	 6
6 	 5
7 	 4
8 	 3
9 	 2
10 	 1
```

- i는 1부터 10까지 증가시키는 동시에, j는 10부터 1까지 감소시키면서 출력
- 실행결과에서 i와 j의 관계를 살펴보면, `i와 j를 더한 값`이 `11`로 일정
- j 대신 `11-i`라는 식을 사용할 수 있음
    - for(int i=1; i<=10; i++, 11-i)
- **for문에 사용되는 변수의 수가 적은 것이 더 효율적이고 간단하므로 변수들의 관계를 잘 파악하여 불필요한 변수의 사용을 줄이는 것이 좋음**

```FlowEx15.java
class FlowEx15 {
    public static void main(String[] args) {
        System.out.println("i \t 2*i \t 2*i-1 \t i*i \t 11-i \t i%3 \t i/3");
        System.out.println("--------------------------------------------------");

        for(int i=1; i<=10; i++)
            System.out.printf("%d \t %d \t %d \t %d \t %d \t %d \t %d%n",
                    i, 2*i, 2*i-1, i*i, 11-i, i%3, i/3);
    }
}
```

```실행결과
i 	 2*i 	 2*i-1 	 i*i 	 11-i 	 i%3 	 i/3
--------------------------------------------------
1 	 2 	 1 	 1 	 10 	 1 	 0
2 	 4 	 3 	 4 	 9 	 2 	 0
3 	 6 	 5 	 9 	 8 	 0 	 1
4 	 8 	 7 	 16 	 7 	 1 	 1
5 	 10 	 9 	 25 	 6 	 2 	 1
6 	 12 	 11 	 36 	 5 	 0 	 2
7 	 14 	 13 	 49 	 4 	 1 	 2
8 	 16 	 15 	 64 	 3 	 2 	 2
9 	 18 	 17 	 81 	 2 	 0 	 3
10 	 20 	 19 	 100 	 1 	 1 	 3
```

- `나머지 연산자 '%'`를 사용하면 `특정 범위의 값들이 순환되면서 반복`되게 할 수 있음
- `나누기 연산자 '/'`를 사용하면 `같은 값이 연속적으로 반복`되게 할 수 있음

### 중첩 for문
- for문 안에 또 다른 for문을 포함시키는 것도 가능
- **중첩의 횟수는 거의 제한이 없음**

#### 5행 10열의 별 찍기

```FlowEx16.java
class FlowEx16 {
    public static void main(String[] args) {
        for(int i=1; i<=5; i++) {
            for(int j=1; j<=10; j++) {
                System.out.print("*"); // *을 출력한다.
            }
            System.out.println(); // 줄바꿈을 한다.
        }
    }
}
```

```
**********
**********
**********
**********
**********
```

- 안쪽 for문을 통해 별을 10번 찍은 후, 바깥쪽 for문을 통해 줄바꿈
- 바깥쪽 for문이 5번 반복되면서 총 5행을 찍어냄

#### 삼각형 모양의 별 찍기

```FlowEx17.java
import java.util.Scanner;

class FlowEx17 {
    public static void main(String[] args) {
        int num = 0;
        System.out.print("*을 출력할 라인의 수를 입력하세요.>");

        Scanner scanner = new Scanner(System.in);
        String tmp = scanner.nextLine(); // 화면을 통해 입력받은 내용을 tmp에 저장
        num = Integer.parseInt(tmp); // 입력받은 문자열(tmp)을 숫자로 변환

        for(int i=0; i<num; i++) {
            for(int j=0; j<=i; j++) {
                System.out.print("*");
            }
            System.out.println();
        }
    }
}
```

```실행결과
*을 출력할 라인의 수를 입력하세요.>10
*
**
***
****
*****
******
*******
********
*********
**********
```

- 사용자로부터 라인의 수를 입력받아 별을 출력하는 예제
- 바깥쪽 for문을 통해 출력할 라인의 수를 지정
- 안쪽 for문을 통해 i의 값이 증가할 때마다(=줄이 바뀔 때마다) 찍는 별의 개수도 함께 증가하도록 함

#### 구구단

```FlowEx18.java
class FlowEx18 {
    public static void main(String[] args) {
        for(int i=2; i<=9; i++) {
            for(int j=1; j<=9; j++) {
                System.out.printf("%d x %d = %d%n", i, j, i*j);
            }
        }
    }
}
```

```실행결과
2 x 1 = 2
2 x 2 = 4
2 x 3 = 6
2 x 4 = 8
2 x 5 = 10
2 x 6 = 12
2 x 7 = 14
2 x 8 = 16
2 x 9 = 18
3 x 1 = 3
3 x 2 = 6
...
9 x 7 = 63
9 x 8 = 72
9 x 9 = 81
```

- 바깥쪽 for문을 통해 단을 지정함
    - for(int i=2; i<=9; i++) // 2단부터 9단까지 출력
- 안쪽 for문을 통해 하나의 단을 출력하도록 함
    - for(int j=1; j<=9; j++) // 1부터 9까지 하나의 단을 출력

#### 3개의 반복문 중첩

```FlowEx19.java
class FlowEx19 {
    public static void main(String[] args) {
        for(int i=1; i<=3; i++)
            for(int j=1; j<=3; j++)
                for(int k=1; k<=3; k++)
                    System.out.println("" + i + j + k);
    }
}
```

```실행결과
111
112
113
121
122
123
131
132
133
211
212
...
323
331
332
333
```

#### 2중 반복문

```FlowEx20.java
class FlowEx20 {
    public static void main(String[] args) {
        for(int i=1; i<=5; i++) {
            for(int j=1; j<=5; j++) {
                System.out.printf("[%d, %d]", i, j);
            }
            System.out.println();
        }
    }
}
```

```실행결과
[1, 1][1, 2][1, 3][1, 4][1, 5]
[2, 1][2, 2][2, 3][2, 4][2, 5]
[3, 1][3, 2][3, 3][3, 4][3, 5]
[4, 1][4, 2][4, 3][4, 4][4, 5]
[5, 1][5, 2][5, 3][5, 4][5, 5]
```

#### for문 안에 if문을 넣어 조건에 맞는 쌍만 출력

```FlowEx21.java
class FlowEx21 {
    public static void main(String[] args) {
        for(int i=1; i<=5; i++) {
            for(int j=1; j<=5; j++) {
                if(i==j) {
                    System.out.printf("[%d, %d]", i, j);
                } else {
                    System.out.printf("%5c", ' ');
                }
            }
            System.out.println();
        }
    }
}
```

```실행결과
[1, 1]                    
     [2, 2]               
          [3, 3]          
               [4, 4]     
                    [5, 5]
```

- 조건식 `i==j`가 참인 경우에만 i와 j의 값을 출력
- 조건식이 거짓일 경우에는 공백으로 출력

### for each문
- 배열과 컬렉션에 저장된 요소에 접근할 때 기존보다 편리한 방법으로 처리할 수 있도록 추가된 for문의 새로운 문법

```for-each
for (타입 변수명 : 배열 또는 컬렉션) {
    // 반복할 문장
}
```

- `타입`은 배열 또는 컬렉션의 `요소의 타입`
- **배열 또는 컬렉션에 저장된 값이 매 반복마다 하나씩 순서대로 읽혀서 변수에 저장됨**
- 일반적인 for문과 달리 **배열이나 컬렉션에 저장된 요소들을 읽어오는 용도로만 사용 가능**

```FlowEx22.java
class FlowEx22 {
    public static void main(String[] args) {
        int[] arr = {10, 20, 30, 40, 50};
        int sum = 0;

        // 일반적인 for문
        for(int i=0; i<arr.length; i++) {
            System.out.printf("%d ", arr[i]);
        }
        System.out.println();

        // 향상된 for문(for each문)
        for(int tmp : arr) {
            System.out.printf("%d ", tmp);
            sum += tmp;
        }
        System.out.println();
        System.out.println("sum=" + sum);
    }
}
```

```실행결과
10 20 30 40 50 
10 20 30 40 50 
sum=150
```

### while문
- `조건식`과 `블럭{}`으로 이루어져 있음

```while
while (조건식) {
    // 조건식의 연산결과가 참(true)인 동안, 반복될 문장들을 적는다.
}
```
- 먼저 조건식을 평가해서 조건식이 거짓이면 문장 전체를 벗어남
- 참이면 블럭{} 내의 문장을 수행하고 다시 조건식으로 돌아감

#### for문과 while문의 비교

```for
// 초기화, 조건식, 증감식
for(int i=1; i<=10; i++) {
    System.out.println(i);
}
```

```while
int i = 1; // 초기화

while(i<=10) {
    System.out.println(i);
    i++;
}
```

- 위의 두 코드는 완전히 동일
- for문은 초기화, 조건식, 증감식을 한 곳에 모아놓은 것으로 while문과 다르지 않음
- **for문과 while문은 항상 서로 변환이 가능**
- 초기화나 증감식이 필요하지 않은 경우라면 `while문`이 더 적합

#### while문의 조건식은 생략불가

```for
for(;;) { // 조건식이 항상 참
    ...
}
```

```while
while(ture) { // 조건식이 항상 참
    ...
}
```

- for문과 달리 while문은 조건식을 생략할 수 없음
- while문의 조건식이 항상 `참`이 되도록 하려면 반드시 `true`를 넣어야 함
- **무한 반복문은 반드시 블럭{} 안에 조건문을 넣어서 특정 조건을 만족하면 무한 반복문을 벗어나도록 해야함**

```FlowEx23.java
class FlowEx23 {
    public static void main(String[] args) {
        int i = 5;

        while(i-- != 0) {
            System.out.println(i + " - I can do it.");
        }
    }
}
```

```실행결과
4 - I can do it.
3 - I can do it.
2 - I can do it.
1 - I can do it.
0 - I can do it.
```

- `while(i-- != 0)`는 i의 값이 0이 아닌 동안만 참이 되고 매 반복마다 1씩 감소하다 0이 되면 조건식이 거짓이 되어 while문을 벗어남
- 후위형이므로 조건식이 평가된 후에 i의 값이 감소

```FlowEx24.java
class FlowEx24 {
    public static void main(String[] args) {
        int i = 11;
        System.out.println("카운트 다운을 시작합니다.");

        while(i-- != 0) {
            System.out.println(i);

            for(int j=0; j<2_000_000_000; j++) {
                ;
            }
        }
        System.out.println("GAME OVER");
    }
}
```

```실행결과
카운트 다운을 시작합니다.
10
9
8
7
6
5
4
3
2
1
0
GAME OVER
```

- 10부터 0까지 1씩 감소시켜가면서 출력을 하되, for문으로 매 출력마다 약간의 시간을 지연시키도록 함
- for문의 블럭{} 내에는 아무 일도 하지 않는 빈 문장 ';'만 있음(증감식을 2,000,000,000번 반복하면서 시간을 보냄)

```FlowEx25.java
import java.util.Scanner;

class FlowEx25 {
    public static void main(String[] args) {
        int num = 0, sum = 0;
        System.out.print("숫자를 입력하세요.(예:12345)>");

        Scanner scanner = new Scanner(System.in);
        String tmp = scanner.nextLine(); // 화면을 통해 입력받은 내용을 tmp에 저장
        num = Integer.parseInt(tmp); // 입력받은 문자열(tmp)을 숫자로 변환

        while(num != 0) {
            // num을 10으로 나눈 나머지를 sum에 더함
            sum += num % 10; // sum = sum + num % 10;
            System.out.printf("sum=%3d num=%d%n", sum, num);
            
            num /= 10; // num = num / 10; num을 10으로 나눈 값을 다시 num에 저장
        }
        System.out.println("각 자리수의 합:" + sum);
    }
}
```

```실행결과
숫자를 입력하세요.(예:12345)>12345
sum=  5 num=12345
sum=  9 num=1234
sum= 12 num=123
sum= 14 num=12
sum= 15 num=1
각 자리수의 합:15
```

- 어떤 수를 10으로 나머지 연산하면 마지막 자리를 얻을 수 있음
- 10으로 나누게 되면 마지막 한 자리는 제거됨
- 위 과정을 반복하여 num의 모든 자리를 얻을 수 있음
- `num/=10`의 의해 한 자리씩 줄어들다 0이 되면, while문의 조건식이 거짓이 되어 반복을 멈춤

```FlowEx26.java
class FlowEx26 {
    public static void main(String[] args) {
        int sum = 0;
        int i = 0;

        // i를 1씩 증가시켜서 sum에 계속 더해나간다.
        while ((sum += ++i) <= 100) {
            System.out.printf("%d - %d%n", i, sum);
        }
    }
}
```

```실행결과
1 - 1
2 - 3
3 - 6
4 - 10
5 - 15
6 - 21
7 - 28
8 - 36
9 - 45
10 - 55
11 - 66
12 - 78
13 - 91
```

- `while ((sum += ++i) <= 100)`는 i의 값을 1 증가시켜 sum에 더하고, sum의 값이 100보다 작거나 같을때까지만 반복함

```FlowEx27.java
import java.util.Scanner;

class FlowEx27 {
    public static void main(String[] args) {
        int num;
        int sum = 0;
        boolean flag = true; // while문의 조건식으로 사용될 변수
        Scanner scanner = new Scanner(System.in);

        System.out.println("합계를 구할 숫자를 입력하세요.(끝내려면 0을 입력)");

        while(flag) { // flag의 값이 true이므로 조건식은 참이 된다.
            System.out.print(">>");

            String tmp = scanner.nextLine();
            num = Integer.parseInt(tmp);

            if(num != 0) {
                sum += num; // num이 0이 아니면, sum에 더한다.
            } else {
                flag = false; // num이 0이면, flag의 값을 false로 변경.
            }
        }

        System.out.println("합계:"+sum);
    }
}
```

```실행결과
합계를 구할 숫자를 입력하세요.(끝내려면 0을 입력)
>>100
>>200
>>300
>>400
>>0
합계:1000
```

- 사용자로부터 반복해서 숫자를 입력받다 0을 입력하면 입력을 마치고 총 합을 출력하는 예제
- 처음엔 flag에 true를 저장해서 계속 반복하다 0을 입력받으면 flag를 false로 바꿔서 반복을 멈추게 함
- while문의 조건식이 상수는 아니지만, **변수가 고정된 값**을 유지하므로 무한반복문과 다름 없음
- **특정조건을 만족할 때 반복을 멈추게 하는 if문이 반복문 안에 꼭 필요함**

### do-while문

```do-while
do {
    // 수행될 문장들을 적는다.
} while (조건식); // 끝에 ';'을 잊지 않도록 주의.
```

- while문의 변형으로 조건식과 블럭{}의 순서를 바꿔놓은 것
- **블럭{}을 먼저 수행한 후 조건식을 평가**
- **do-while문은 최소한 한번은 수행될 것을 보장**
- **끝에 ';'을 잊지 않도록 주의**

```FlowEx28.java
import java.util.Scanner;

class FlowEx28 {
    public static void main(String[] args) {
        int input = 0, answer = 0;
    
        answer = (int) (Math.random() * 100) + 1; // 1 ~ 100사이의 임의의 수를 저장
        Scanner scanner = new Scanner(System.in);

        do {
            System.out.printf("1과 100사이의 정수를 입력하세요.>");
            input = scanner.nextInt();

            if(input > answer) {
                System.out.println("더 작은 수로 다시 시도해보세요.");
            } else if(input < answer) {
                System.out.println("더 큰 수로 다시 시도해보세요.");
            }
        } while(input != answer);

        System.out.println("정답입니다.");
    }
}
```

```실행결과
1과 100사이의 정수를 입력하세요.>50
더 작은 수로 다시 시도해보세요.
1과 100사이의 정수를 입력하세요.>40
더 큰 수로 다시 시도해보세요.
1과 100사이의 정수를 입력하세요.>45
더 작은 수로 다시 시도해보세요.
1과 100사이의 정수를 입력하세요.>42
더 큰 수로 다시 시도해보세요.
1과 100사이의 정수를 입력하세요.>43
정답입니다.
```

- `Math.random()`을 이용해 1과 100사이의 임의의 수를 answer에 저장
- 사용자로부터 정수를 입력받아 input과 answer의 값이 다른 동안 계속해서 반복
- 두 값이 같을 경우 반복을 벗어남

```FlowEx29.java
class FlowEx29 {
    public static void main(String[] args) {
        for(int i=1; i<=100; i++) {
            System.out.printf("i=%d ", i);

            int tmp = i;

            do {
                // tmp % 10이 3의 배수인지 확인(0 제외)
                if (tmp % 10 % 3 == 0 && tmp % 10 != 0)
                    System.out.print("짝");
                // tmp /= 10은 tmp = tmp / 10과 동일
            } while((tmp /= 10) != 0);
            System.out.println();
        }
    }
}
```

```실행결과
i=1 
i=2 
i=3 짝
i=4 
i=5 
i=6 짝
...
i=97 짝
i=98 짝
i=99 짝짝
i=100 
```

- 숫자 중에 3의 배수(3,6,9)가 포함되어 있으면 포함된 개수만큼 박수를 치는 게임
- 숫자의 각 자리를 확인해야하기 때문에 10으로 나누고 10으로 나머지 연산을 함
- `tmp % 10 % 3 == 0`은 tmp의 마지막 자리가 3의 배수인지 확인
- tmp%10의 값이 0일 때도 참이므로, 식 `tmp % 10 != 0`를 '&&'로 연결해서 tmp%10의 값이 0인 경우를 제외함

### break문
- **자신이 포함된 가장 가까운 반복문을 벗어남**

```FlowEx30.java
class FlowEx30 {
    public static void main(String[] args) {
        int sum = 0;
        int i = 0;

        while (true) {
            if(sum > 100)
                break;

            i++;
            sum += i;
        }

        System.out.println("i=" + i);
        System.out.println("sum=" + sum);
    }
}
```

```실행결과
i=14
sum=105
```

### continue문
- 반복이 진행되는 도중에 continue문을 만나면 **continue문 이후의 문장을 수행하지 않고 반복문의 끝으로 이동하여 다음 반복으로 넘어가서 계속 진행하도록 함**
- **반복문 전체를 벗어나지 않고 다음 반복을 계속 수행**
- `for문`의 경우 `증감식`으로 이동
- `while문`과 `do-while문`의 경우 `조건식`으로 이동

```FlowEx31.java
class FlowEx31 {
    public static void main(String[] args) {
        for(int i=0; i<=10; i++) {
            if (i%3 == 0)
                continue;
            System.out.println(i);
        }
    }
}
```

```실행결과
1
2
4
5
7
8
10
```

```FlowEx32.java
import java.util.Scanner;

class FlowEx32 {
    public static void main(String[] args) {
        int menu = 0;
        int num = 0;

        Scanner scanner = new Scanner(System.in);

        while(true) {
            System.out.println("(1) square");
            System.out.println("(2) square root");
            System.out.println("(3) log");
            System.out.print("원하는 메뉴(1~3)를 선택하세요.(종료:0)>");

            String tmp = scanner.nextLine();
            menu = Integer.parseInt(tmp);

            if(menu == 0) {
                System.out.println("프로그램을 종료합니다.");
                break;
            } else if (!(1 <= menu && 3 >= menu)) {
                System.out.println("잘못된 메뉴를 선택하셨습니다.(종료는 0)");
                continue;
            }
            System.out.println("선택하신 메뉴는 " + menu + "번입니다.");
        }
    }
}
```

```실행결과
(1) square
(2) square root
(3) log
원하는 메뉴(1~3)를 선택하세요.(종료:0)>4
잘못된 메뉴를 선택하셨습니다.(종료는 0)
(1) square
(2) square root
(3) log
원하는 메뉴(1~3)를 선택하세요.(종료:0)>1
선택하신 메뉴는 1번입니다.
(1) square
(2) square root
(3) log
원하는 메뉴(1~3)를 선택하세요.(종료:0)>0
프로그램을 종료합니다.
```

### 이름 붙은 반복문
- **중첩 반복문 앞에 이름을 붙이고 break문과 continue문에 이름을 지정해 줌으로써 하나 이상의 반복문을 벗어나거나 건너뛸 수 있음**

```FlowEx33.java
class FlowEx33 {
    public static void main(String[] args) {
        Loop1 : for(int i=2; i<=9; i++) {
            for(int j=1; j<=9; j++) {
                if(j==5)
                    break Loop1; // 실행결과1
//                    break; // 실행결과2
//                    continue Loop1; // 실행결과3
//                    continue; // 실행결과4
                System.out.println(i + "*" + j + "=" + i*j);
            } // end of for i
            System.out.println();
        } // end of Loop1
    }
}
```

- 바깥쪽 for문에 `Loop1`이라는 이름을 붙여주고 구구단을 출력

```실행결과1(break_Loop1)
2*1=2
2*2=4
2*3=6
2*4=8
```

- 바깥쪽 for문인 Loop1을 완전히 벗어남

```실행결과2(break)
2*1=2
2*2=4
2*3=6
2*4=8

3*1=3
3*2=6
3*3=9
3*4=12

...

9*1=9
9*2=18
9*3=27
9*4=36
```

- 안쪽 for문만 벗어남

```실행결과3(continue_Loop1)
2*1=2
2*2=4
2*3=6
2*4=8
3*1=3
3*2=6
3*3=9
3*4=12
...
9*1=9
9*2=18
9*3=27
9*4=36
```

- Loop1에 증감식으로 이동하여 계속 진행
- 줄바꿈이 일어나지 않은 것을 확인할 수 있음

```실행결과4(continue)
2*1=2
2*2=4
2*3=6
2*4=8
2*6=12
2*7=14
2*8=16
2*9=18

3*1=3
3*2=6
3*3=9
3*4=12
3*6=18
3*7=21
3*8=24
3*9=27

...

9*1=9
9*2=18
9*3=27
9*4=36
9*6=54
9*7=63
9*8=72
9*9=81
```

- 안쪽 for문에 증감식으로 이동하며 계속 진행
- i*5를 제외한 모든 구구단이 출력됨

```FlowEx34.java
import java.util.Scanner;

class FlowEx34 {
    public static void main(String[] args) {
        int menu = 0, num = 0;

        Scanner scanner = new Scanner(System.in);

        outer:
        while (true) {
            System.out.println("(1) square");
            System.out.println("(2) square root");
            System.out.println("(3) log");
            System.out.println("원하는 메뉴(1~3)를 선택하세요.(종료:0)>");

            String tmp = scanner.nextLine();
            menu = Integer.parseInt(tmp);

            if (menu == 0) {
                System.out.println("프로그램을 종료합니다.");
                break;
            } else if (!(1 <= menu && 3 >= menu)) {
                System.out.println("메뉴를 잘못 선택하셨습니다.(종료는 0)");
                continue;
            }

            for (;;) {
                System.out.println("계산할 값을 입력하세요.(계산 종료:0, 전체 종료:99)>");
                tmp = scanner.nextLine();
                num = Integer.parseInt(tmp);

                if (num == 0)
                    break; // 계산 종료. for문을 벗어난다.
                if (num == 99)
                    break outer; // 전체 종료. for문과 while문을 모두 벗어난다.

                switch(menu) {
                    case 1:
                        System.out.println("result=" + num*num);
                        break;
                    case 2:
                        System.out.println("result=" + Math.sqrt(num));
                        break;
                    case 3:
                        System.out.println("result=" + Math.log(num));
                        break;
                }
            }
        }
    }
}
```

```실행결과
(1) square
(2) square root
(3) log
원하는 메뉴(1~3)를 선택하세요.(종료:0)>
1
계산할 값을 입력하세요.(계산 종료:0, 전체 종료:99)>
2
result=4
계산할 값을 입력하세요.(계산 종료:0, 전체 종료:99)>
3
result=9
계산할 값을 입력하세요.(계산 종료:0, 전체 종료:99)>
0
(1) square
(2) square root
(3) log
원하는 메뉴(1~3)를 선택하세요.(종료:0)>
2
계산할 값을 입력하세요.(계산 종료:0, 전체 종료:99)>
4
result=2.0
계산할 값을 입력하세요.(계산 종료:0, 전체 종료:99)>
99
```

- 선택된 메뉴에서 0을 입력하면 `break`로 for문을 벗어나서 다시 while문으로 이동
- 선택된 메뉴에서 99를 입력하면 `break outer`로 while문을 벗어나 프로그램이 종료됨