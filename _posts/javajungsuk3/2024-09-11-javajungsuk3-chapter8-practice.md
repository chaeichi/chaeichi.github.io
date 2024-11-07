---
layout: post
title: "Java의 정석 연습문제 - Chapter08. 예외처리"
subtitle: Java의 정석 3rd Edition | 남궁성
excerpt_image: /assets/images/excerpt-images/2024-09-11-javajungsuk3-chapter8-practice.png
categories: 
    - 문제풀이
tags: 
    - Java
    - Java의 정석
    - 연습문제
    - 문제풀이
---
![banner](/assets/images/excerpt-images/2024-09-11-javajungsuk3-chapter8-practice.png)

**[8-1]** `예외처리의 정의`와 `목적`에 대해서 설명하시오.<br/>
**[정답]** 정의 : 프로그램 실행 시 발생할 수 있는 예외의 발생에 대비한 프로그램 코드를 작성하는 것<br/>
목적 : 프로그램의 비정상 종료를 막고, 정상적인 실행상태를 유지하는 것<br/>
**[해설]** 에러(error) - 프로그램 코드에 의해서 수습될 수 없는 심각한 오류<br/>
예외(exception) - 프로그램 코드에 의해서 수습될 수 있는 다소 미약한 오류<br/>

**[8-2]** 다음은 실행도중 예외가 발생하여 화면에 출력된 내용이다. 이에 대한 설명 중 옳지 않은 것은?

```Exercise8_2.java
java.lang.ArithmeticException : / by zero
    at ExceptionEx18.method2 (ExceptionEx18.java:12)
    at ExceptionEx18.method1 (ExceptionEx18.java:8)
    at ExceptionEx18.main (ExceptionEx18.java:4)
```

a. 위의 내용으로 예외가 발생했을 당시 호출스택에 존재했던 메서드를 알 수 있다.<br/>
b. 예외가 발생한 위치는 method2메서드이며, Exception18.java파일의 12번째 줄이다.<br/>
c. 발생한 예외는 ArithmeticException이며, 0으로 나누어서 예외가 발생했다.<br/>
**d. method2메서드가 method1메서드를 호출하였고 그 위치는 Exception18.java파일의 8번째 줄이다.**<br/>

**[정답]** d<br/>
**[해설]** method1메서드가 method2메서드를 호출하였다.<br/>
main메서드 → method1메서드 → method2메서드 순으로 호출<br/>

**[8-3]** 다음 중 오버라이딩이 잘못된 것은? (모두 고르시오)

```Exercise8_3.java
void add(int a, int b) throws InvalidNumberException, NotANumberException {}

class NumberException extends Exception {}
class InvalidNumberException extends NumberException {}
class NotANumberException extends NumberException {}
```

a. void add(int a, int b) throws InvalidNumberException, NotANumberException {}<br/>
b. void add(int a, int b) throws InvalidNumberException {}<br/>
c. void add(int a, int b) throws NotANumberException {}<br/>
**d. void add(int a, int b) throws Exception {}**<br/>
**e. void add(int a, int b) throws NumberException {}**<br/>

**[정답]** d, e<br/>
**[해설]** 조상 클래스의 메서드보다 많은 수의 예외를 선언할 수 없다.<br/>
여기서 주의해야할 점은 단순히 선언된 예외의 개수가 문제가 아니라는 것이다.<br/>
Exception은 모든 예외의 최고 조상이므로 가장 많은 개수의 예외를 던질 수 있도록 선언한 것이다.<br/>

**[8-4]** 다음과 같은 메서드가 있을 때, 예외를 잘못 처리한 것은? (모두 고르시오)

```Exercise8_4.java
void method() throws InvalidNumberException, NotANumberException {}

class NumberException extends RuntimeException {}
class InvalidNumberException extends NumberException {}
class NotANumberException extends NumberException {}
```

a. try {method();} catch(Exception e) {}<br/>
b. try {method();} catch(NumberException e) {} catch(Exception e) {}<br/>
**c. try {method();} catch(Exception e) {} catch(NumberException e)**<br/>
d. try {method();} catch(InvalidNumberException e) catch(NotANumberException e)<br/>
e. try {method();} catch(NumberException e) {}<br/>
f. try {method();} catch(RuntimeException e) {}<br/>

**[정답]** c<br/>
**[해설]** try블럭 내에서 예외가 발생하면, catch블럭 중에서 예외를 처리할 수 있는 것을 차례대로 찾아 내려간다.<br/>
예외의 최고조상인 Exception이 선언된 catch블럭은 모든 예외를 다 처리할 수 있다.<br/>
Exception을 처리하는 catch블럭은 모든 catch블럭 중 제일 마지막에 있어야 한다.<br/>

**[8-5]** 아래의 코드가 수행되었을 때의 실행결과를 적으시오.

```Exercise8_5.java
class Exercise8_5 {
    static void method(boolean b) {
        try {
            System.out.println(1);
            if(b) throw new ArithmeticException();
            System.out.println(2);
        } catch(RuntimeException r) {
            System.out.println(3);
            return;
        } catch(Exception e) {
            System.out.println(4);
            return;
        } finally {
            System.out.println(5);
        }
        System.out.println(6);
    }

    public static void main(String[] args) {
        method(true);
        method(false);
    }
}
```

**[정답]**

```실행결과
1
3
5
1
2
5
6
```

**[해설]** **method(true)일 경우, 1,3,5가 출력된다**<br/>
ArithmeticException은 RuntimeException의 자손이므로 RuntimeException이 정의된 catch블럭이 실행된다.<br/>
catch블럭 내에 return문이 있으므로 메서드를 종료하고 빠져나가게 된다.<br/>
try블럭에서 return문이 실행되는 경우에도 finally블럭의 문장들이 먼저 실행된 후에, 현재 실행 중인 메서드를 종료한다.<br/>

**method(false)일 경우, 1,2,5,6이 출력된다.**<br/>
Exception이 발생하지 않으며 finally블럭은 예외의 발생여부에 상관없이 실행된다.

**[8-6]** 아래의 코드가 수행되었을 때의 실행결과를 적으시오.

```Exercise8_6.java
class Exercise8_6 {
    public static void main(String[] args) {
        try {
            method1();
        } catch(Exception e) {
            System.out.println(5);
        }
    }

    static void method1() {
        try {
            method2();
            System.out.println(1);
        } catch(ArithmeticException e) {
            System.out.println(2);
        } finally {
            System.out.println(3);
        }
        System.out.println(4);
    }

    static void method2() {
        throw new NullPointerException();
    }
}
```

**[정답]**

```실행결과
3
5
```

**[해설]** main메서드 → method1메서드 → method2메서드 순으로 호출<br/>
① method2()에서 NullPointerException이 발생하였으나, 예외를 처리할 수 있는 try-catch블럭이 없으므로 이를 호출한 method1()로 되돌아간다.<br/>
② 그러나 method1()에도 NullPointerException을 처리할 수 있는 catch블럭이 없으므로 이를 호출한 main()으로 되돌아간다.<br/>
이 때, method1()에 finally블럭이 수행되어 '3'을 출력한다.<br/>
③ method1()을 호출한 main()에서는 모든 예외의 최고 조상인 Exception이 선언된 catch블럭이 존재하므로 예외가 처리되고 '5'를 출력한다.

**[8-7]** 아래의 코드가 수행되었을 때의 실행결과를 적으시오.

```Exercise8_7.java
class Exercise8_7 {
    static void method(boolean b) {
        try {
            System.out.println(1);
            if(b) System.exit(0);
            System.out.println(2);
        } catch (RuntimeException r) {
            System.out.println(3);
            return;
        } catch (Exception e) {
            System.out.println(4);
            return;
        } finally {
            System.out.println(5);
        }
        System.out.println(6);
    }

    public static void main(String[] args) {
        method(true);
        method(false);
    }
}
```

**[정답]**

```실행결과
1
```

**[해설]** method(ture)가 수행될 때, 1 출력 후 if문의 값이 true이므로 `System.exit(0);` 문장이 수행되어 프로그램이 즉시 종료된다.<br/>
프로그램이 종료된 것이기 때문에 finally블럭은 수행되지 않는다.

**[8-8]** 다음은 1~100사이의 숫자를 맞추는 게임을 실행하던 도중에 `숫자가 아닌 영문자`를 넣어서 발생한 예외이다. 예외처리를 해서 숫자가 아닌 값을 입력했을 때는 다시 입력을 받도록 보완하라.

```실행결과
1과 100사이의 값을 입력하세요 :50
더 작은 수를 입력하세요.
1과 100사이의 값을 입력하세요: asdf
Exception in thread "main" java.util.InputMismatchException
    at java.util.Scanner.throwFor(Scanner.java:819)
    at java.util.Scanner.next(Scanner.java:1431)
    at java.util.Scanner.nextInt(Scanner.java:2040)
    at java.util.Scanner.nextInt(Scanner.java:2000)
    at Exercise8_8.main(Exercise8_8.java:16)
```

```Exercise8_8.java
import java.util.Scanner;

class Exercise8_8 {
    public static void main(String[] args) {
        // 1~100사이의 임의의 값을 얻어서 answer에 저장한다.
        int answer = (int) (Math.random() * 100) + 1;
        int input = 0; // 사용자입력을 저장할 공간
        int count = 0; // 시도횟수를 세기 위한 변수

        do {
            count++;
            System.out.print("1과 100사이의 값을 입력하세요 :");

            input = new Scanner(System.in).nextInt();

            if(answer > input) {
                System.out.println("더 큰 수를 입력하세요.");
            } else if(answer < input) {
                System.out.println("더 작은 수를 입력하세요.");
            } else {
                System.out.println("맞췄습니다.");
                System.out.println("시도횟수는 " + count + "번입니다.");
                break;
            }
        } while (true); // 무한반복문
    }
}
```

**[정답]**

```Exercise8_8.java
do {
    count++;
    System.out.print("1과 100사이의 값을 입력하세요 :")

    try {
        input = new Scanner(System.in).nextInt();
    } catch(InputMismatchException e) {
        System.out.println("유효하지 않은 값입니다. 다시 값을 입력해주세요.");
        continue;
    }

    ...

} while (true);
```

```실행결과
1과 100사이의 값을 입력하세요 :50
더 큰 수를 입력하세요.
1과 100사이의 값을 입력하세요 :asdf
유효하지 않은 값입니다. 다시 값을 입력해주세요.
1과 100사이의 값을 입력하세요 :80
더 큰 수를 입력하세요.
1과 100사이의 값을 입력하세요 :90
더 작은 수를 입력하세요.
1과 100사이의 값을 입력하세요 :85
더 작은 수를 입력하세요.
1과 100사이의 값을 입력하세요 :83
맞췄습니다.
시도횟수는 6번입니다.
```

**[해설]** 사용자로부터 값을 입력받는 부분을 try-catch구문으로 예외처리를 해주면 된다.<br/>
이후의 문장을 수행하지 않고 값을 다시 입력받아야 하므로 `continue`를 사용하여 다음 반복으로 넘어가서 진행하도록 한다.<br/>
반복문에서 `return` 사용 시 반복문을 포함하는 메서드 자체를 종료시킨다.

**[8-9]** 다음과 같은 조건의 예외클래스를 작성하고 테스트하시오.<br/>
**[참고]** 생성자는 실행결과를 보고 알맞게 작성해야한다.<br/>
클래스명 : UnsupportedFuctionException<br/>
조상클래스명 : RuntimeException<br/>

멤버변수 : <br/>
이름 : ERR_CODE<br/>
저장값 : 에러코드<br/>
타입 : int<br/>
기본값 : 100<br/>
제어자 : final private<br/>

메서드 : <br/>
[1] 메서드명 : getErrorCode<br/>
기능 : 에러코드(ERR_CODE)를 반환한다.<br/>
반환타입 : int<br/>
매개변수 : 없음<br/>
제어자 : public<br/>

[2] 메서드명 : getMessage<br/>
기능 : 메세지의 내용을 반환한다. (Exception클래스의 getMessage()를 오버라이딩)<br/>
반환타입 : String<br/>
매개변수 : 없음<br/>
제어자 : public<br/>

```Exercise8_9.java
class Exercise8_9 {
    public static void main(String[] args) throws Exception {
        throw new UnsupportedFuctionException("지원하지 않는 기능입니다.", 100);
    }
}
```

```실행결과
Exception in thread "main" UnsupportedFuctionException: [100]지원하지 않는 기능입니다.
	at Exercise8_9.main(Exercise8_9.java:3)
```

**[정답]**

```UnsupportedFuctionException
class UnsupportedFuctionException extends RuntimeException {
    final private int ERR_CODE; // 에러코드

    UnsupportedFuctionException(String message, int errCode) {
        super(message);
        ERR_CODE = errCode;
    }

    UnsupportedFuctionException(String message) {
        this(message, 100); // ERR_CODE의 기본값을 100으로 초기화
    }

    public int getErrorCode() { // 에러코드(ERR_CODE)를 반환한다.
        return ERR_CODE;
    }

    public String getMessage() { // Exception클래스의 getMessage()를 오버라이딩
        return "[" + getErrorCode() + "]" +  super.getMessage();
    }
}
```

**[8-10]** 아래의 코드가 수행되었을 때의 실행결과를 적으시오.

```Exercise8_10.java
class Exercise8_10 {
    public static void main(String[] args) {
        try {
            method1();
            System.out.println(6);
        } catch(Exception e) {
            System.out.println(7);
        }
    }

    static void method1() throws Exception {
        try {
            method2();
            System.out.println(1);
        } catch (NullPointerException e) {
            System.out.println(2);
            throw e;
        } catch(Exception e) {
            System.out.println(3);
        } finally {
            System.out.println(4);
        }
        System.out.println(5);
    }

    static void method2() {
        throw new NullPointerException();
    }
}
```

**[정답]**

```실행결과
2
4
7
```

**[해설]** main메서드 → method1메서드 → method2메서드 순으로 호출<br/>
① method2()에서 NullPointerException()이 발생하였으나, 예외를 처리할 수 있는 try-catch블럭이 없으므로 method2()를 호출한 method1()로 되돌아간다.<br/>
② method1()에서 NullPointerException이 정의된 catch블럭이 수행되고 2를 출력한다. 그리고 `throw e;`문장을 통해 예외를 다시 발생시킨다.<br/>
③ catch블럭 내에 예외를 처리할 문장이 없으므로 method1()을 호출한 main()으로 되돌아간다.<br/>
이 때, finally블럭이 수행되어 4가 출력된다.<br/>
④ main()에서 모든 예외의 조상인 Exception이 정의된 catch블럭이 실행되고 7을 출력한다.


## 🚩 잊지않기
- [에러와 예외](https://chaeichi.github.io/java/2024/09/02/exception-handling-1.html#h-%ED%94%84%EB%A1%9C%EA%B7%B8%EB%9E%A8-%EC%98%A4%EB%A5%98)
    - 에러(error) : 프로그램 코드에 의해서 수습될 수 없는 심각한 오류
    - 예외(exception) : 프로그램 코드에 의해서 수습될 수 있는 다소 미약한 오류
- [Exception은 모든 예외의 최고 조상이다.](https://chaeichi.github.io/java/2024/09/02/exception-handling-1.html#h-%EC%97%90%EC%99%B8-%ED%81%B4%EB%9E%98%EC%8A%A4%EC%9D%98-%EA%B3%84%EC%B8%B5%EA%B5%AC%EC%A1%B0)
    - try블럭 내에서 예외가 발생하면, catch블럭 중에서 예외를 처리할 수 있는 것을 차례대로 찾아 내려간다.
    - **Exception이 선언된 catch블럭은 모든 예외를 다 처리할 수 있으므로 모든 catch블럭 중 제일 마지막에 있어야 한다.**
- [finally블럭](https://chaeichi.github.io/java/2024/09/03/exception-handling-2.html#h-finally%EB%B8%94%EB%9F%AD)
    - finally블럭은 예외의 발생여부에 상관없이 실행되어야할 코드를 포함시킬 목적으로 사용된다.
    - try블럭에서 return문이 실행되는 경우에도 finally블럭의 문장들이 먼저 실행된 후에, 현재 실행 중인 메서드를 종료한다.
- [반복이 진행되는 도중에 continue문을 만나면 continue문 이후의 문장을 수행하지 않고 반복문의 끝으로 이동하여 다음 반복으로 넘어가서 계속 진행하도록 한다.(반복문 전체를 벗어나지 않고 다음 반복을 계속 수행)](https://chaeichi.github.io/java/2024/01/30/if-switch-for-while-statement-2.html#h-continue%EB%AC%B8)
- 반복문 안에서 return을 사용할 경우 반복문이 종료될 수 있다.