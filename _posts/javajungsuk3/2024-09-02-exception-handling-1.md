---
layout: post
title: "Java의 정석 Chapter08. 예외처리 (1) - try-catch문"
subtitle: Java의 정석 3rd Edition | 남궁성
excerpt_image: /assets/images/excerpt-images/2024-09-02-exception-handling-1.png
categories: 
    - Java
tags: 
    - Java
    - Java의 정석
    - 예외처리
    - 컴파일 에러
    - 런타임 에러
    - 논리적 에러
    - 에러
    - 예외
    - RuntimeException
    - Exception
    - try-catch문
    - printStackTrace
    - getMessage()
    - System.out
    - System.err
    - 멀티 catch블럭
---
![banner](/assets/images/excerpt-images/2024-09-02-exception-handling-1.png)

## 예외처리(exception-handling)
### 프로그램 오류
- 프로그램이 실행 중 어떤 원인에 의해서 오작동을 하거나 비정상적으로 종료되는 경우가 있다. 이러한 결과를 초래하는 원인을 `프로그램 에러` 또는 `오류`라고 한다.
- 이를 발생시점에 따라 `컴파일 에러`와 `런타임 에러`로 나눌 수 있다.
    - `컴파일 에러(compile-time error)` 컴파일 시에 발생하는 에러
    - `런타임 에러(runtime error)` 프로그램의 실행도중에 발생하는 에러
    - `논리적 에러(logical error)` 컴파일도 잘되고 실행도 잘되지만, 의도와 다르게 동작하는 것 *(예를 들어, 창고의 재고가 음수가 된다던가, 게임 프로그램에서 비행기가 총알을 맞아도 죽지 않는 경우)*
- 소스코드를 컴파일 하면 컴파일러가 `소스코드(*.java)`에 대해 오타나 잘못된 구문, 자료형 체크 등의 기본적인 검사를 수행하여 오류가 있는지를 알려준다.
- 컴파일러가 알려준 에러들을 모두 수정해서 컴파일을 성공적으로 마치고 나면, `클래스 파일(*.class)`이 생성되고, 생성된 클래스 파일을 실행할 수 있게 되는 것이다.
- 하지만 컴파일을 에러없이 성공적으로 마쳤다고 해서 프로그램의 실행 시에도 에러가 발생하지 않는 것은 아니다. **컴파일러가 소스코드의 기본적인 사항은 컴파일 시에 모두 걸러줄 수는 있지만, 실행도중에 발생할 수 있는 잠재적인 오류까지 검사할 수 없기 때문에** 컴파일은 잘되었어도 실행 중에 에러에 의해서 잘못된 결과를 얻거나 프로그램이 비정상적으로 종료될 수 있다.
- 프로그램이 실행 중 동작을 멈춘 상태로 오랜 시간 지속되거나, 갑자기 프로그램이 실행을 멈추고 종료되는 경우 등이 이에 해당한다.
- `런타임 에러`를 방지하기 위해서는 **프로그램의 실행도중 발생할 수 있는 모든 경우의 수를 고려**하여 이에 대한 대비를 하는 것이 필요하다.
- 자바에서는 `실행 시(runtime)` 발생할 수 있는 프로그램 오류를 `에러`와 `예외` 두 가지로 구분하였다.
    - `에러(error)` 프로그램 코드에 의해서 수습될 수 없는 심각한 오류 *메모리 부족(OutOfMemoryError), 스택오버플로우(StackOverflowError)*
    - `예외(exception)` 발생하더라도 프로그래머가 이에 대한 적절한 코드를 미리 작성해 놓음으로써 프로그램의 비정상적인 종료를 막을 수 있는 다소 미약한 오류

### 예외 클래스의 계층구조
- **자바에서는 실행 시 발생할 수 있는 오류(Exception과 Error)를 클래스로 정의하였다.**
- 모든 클래스의 조상은 `Object클래스`이므로 `Exception`과 `Error`클래스 역시 Object클래스의 자손들이다.

![예외클래스 계층도](/assets/images/javajungsuk3/8-1.png)

- 모든 예외의 최고 조상은 `Exception클래스`이며, 상속계층도를 Exception클래스로부터 도식화하면 다음과 같다.

![Exception클래스와 RuntimeException클래스 중심의 상속계층도](/assets/images/javajungsuk3/8-2.png)

- 위 그림에서 볼 수 있듯이 예외클래스들은 다음과 같이 두 그룹으로 나눠질 수 있다.
- **RuntimeException클래스들**
    - RuntimeException클래스와 그 자손 클래스들
    - 주로 `프로그래머의 실수`에 의해서 발생할 수 있는 예외들
    - 배열의 범위를 벗어난다던가(ArrayIndexOutOfBoundsException), 값이 null인 참조변수의 멤버를 호출하려 했다던가(NullPointerException), 클래스간의 형변환을 잘못했다던가(ClassCastException), 정수를 0으로 나누려고 하는 경우(ArithmeticException)에 발생한다.
- **Exception클래스들**
    - RuntimeException클래스들을 제외한 나머지 클래스들
    - 주로 사용자의 실수와 같은 `외적인 요인`에 의해 발생하는 예외
    - 존재하지 않는 파일의 이름을 입력했다던가(FileNotFoundException), 실수로 클래스의 이름을 잘못 적었다던가(ClassNotFoundException), 또는 입력한 데이터 형식이 잘못된 경우(DataFormatException)에 발생한다.

### 예외처리하기 - try-catch문
- 프로그램의 실행도중에 발생하는 에러는 어쩔 수 없지만, `예외`는 프로그래머가 이에 대한 처리를 미리 해주어야 한다.
- `예외처리(exception handling)`란, 프로그램 실행 시 발생할 수 있는 예기치 못한 예외의 발생에 대비한 코드를 작성하는 것이다.
- 예외처리의 목적은 **예외의 발생으로 인해 실행 중인 프로그램의 갑작스런 비정상 종료를 막고, 정상적인 실행상태를 유지할 수 있도록 하는 것**이다.
- *에러와 예외는 모두 실행 시(runtime) 발생하는 오류이다.*
- 발생한 예외를 처리하지 못하면, 프로그램은 비정상적으로 종료되며, `처리되지 못한 예외(uncaught exception)`는 JVM의 `예외처리기(UncaughtExceptionHandler)`가 받아서 예외의 원인을 화면에 출력한다.
- 예외를 처리하기 위해서는 `try-catch문`을 사용하며, 그 구조는 다음과 같다.

```try-catch
try {
    // 예외가 발생할 가능성이 있는 문장들을 넣는다.
} catch(Exception1 e1) {
    // Exception1이 발생했을 경우, 이를 처리하기 위한 문장을 적는다.
} catch(Exception2 e2) {
    // Exception2이 발생했을 경우, 이를 처리하기 위한 문장을 적는다.
} catch(Exception3 e3) {
    // Exception3이 발생했을 경우, 이를 처리하기 위한 문장을 적는다.
}
```

- 하나의 `try블럭` 다음에는 여러 종류의 예외를 처리할 수 있도록 하나 이상의 `catch블럭`이 올 수 있으며, **이 중 발생한 예외의 종류와 일치하는 단 한 개의 catch블럭만 수행된다.**
- **발생한 예외의 종류와 일치하는 catch블럭이 없으면 예외는 처리되지 않는다.**
- *if문과 달리, try블럭이나 catch블럭 내에 포함된 문장이 하나뿐이어도 괄호{}를 생략할 수 없다.*

```ExceptionEx1.java
class ExceptionEx1 {
    public static void main(String[] args) {
        try {
            try {   } catch (Exception e) { }
        } catch (Exception e) {
//            try {   } catch (Exception e) { } // 에러. 변수 e가 중복 선언되었다.
        }

        try {

        } catch (Exception e) {

        }
    }
}
```

- 하나의 메서드 내에 여러 개의 `try-catch문`이 사용될 수 있다.
- `try블럭` 또는 `catch블럭`에 또 다른 `try-catch문`이 포함될 수 있다. *(catch블럭 내의 코드에서도 예외가 발생할 수 있기 때문)*
- **catch블럭의 괄호 내에 선언된 변수는 catch블럭 내에서만 유효하기 때문에, 위의 모든 catch블럭에 참조변수 'e' 하나만을 사용해도 된다.**
- catch블럭 내에 또 하나의 `try-catch문`이 포함된 경우, **같은 이름의 참조변수를 사용해서는 안 된다.** 각 catch블럭에 선언된 두 참조변수의 영역이 서로 겹치므로 다른 이름을 사용해야만 서로 구별되기 때문이다.
- 따라서 위의 예제에서 catch블럭 내의 try-catch문에 선언되어 있는 참조변수의 이름을 'e'가 아닌 다른 것으로 바꿔야 한다.

```ExceptionEx2.java
class ExceptionEx2 {
    public static void main(String[] args) {
        int number = 100;
        int result = 0;

        for(int i = 0; i < 10; i++) {
            result = number / (int) (Math.random() * 10); // 7번째 라인
            System.out.println(result);
        }
    }
}
```

```실행결과
14
12
12
25
Exception in thread "main" java.lang.ArithmeticException: / by zero
	at ExceptionEx2.main(ExceptionEx2.java:7)
```

- 위의 예제는 변수 number에 저장되어 있는 값 100을 0~9사이의 임의의 정수로 나눈 결과를 출력하는 일을 10번 반복한다.
- 0으로 나누려 했기 때문에 `ArithmeticException`이 발생했고, 발생한 위치는 `ExceptionEx2.main(ExceptionEx2.java:7)` main메서드(Exception2.java의 7번째 라인)
- `ArithmeticException`은 산술연산과정에서 오류가 있을 때 발생하는 예외이며, **정수는 0으로 나누는 것이 금지되어 있기 때문에 발생했다.** *(실수를 0으로 나누는 것은 금지되어 있지 않으므로 에러가 발생하지 않는다.)*

```ExceptionEx3.java
class ExceptionEx3 {
    public static void main(String[] args) {
        int number = 100;
        int result = 0;

        for(int i = 0; i < 10; i++) {
            try {
                result = number / (int) (Math.random() * 10);
                System.out.println(result);
            } catch(ArithmeticException e) {
                System.out.println("0"); // ArithmeticException이 발생하면 실행되는 코드
            }
        }
    }
}
```

```실행결과
50
20
12
0 // ArithmeticException이 발생하여 0이 출력되었다.
33
25
20
16
16
100
```

- `예외처리구문`을 추가해서 실행도중 예외가 발생하더라도 비정상적으로 종료되지 않도록 수정
- ArithmeticException이 발생했을 경우 0을 화면에 출력하도록 했다.
- 4번째에 0이 출력된 것은 for문의 4번째 반복에서 `ArithmeticException`이 발생했기 때문이다.
- `ArithmeticException`에 해당하는 `catch블럭`을 찾아서 그 catch블럭 내의 문장들을 실행한 다음 `try-catch문`을 벗어나 for문의 다음 반복을 계속 수행하는 작업을 모두 마치고 정상적으로 종료되었다.
- 만일 예외처리를 하지 않았다면, 3번째 줄까지만 출력되고 예외가 발생해서 프로그램이 비정상적으로 종료되었을 것이다.

### try-catch문에서의 흐름
- **try블럭 내에서 예외가 발생한 경우**
    - 발생한 예외와 일치하는 catch블럭이 있는지 확인한다.
    - 일치하는 catch블럭을 찾게 되면, 그 catch블럭 내의 문장들을 수행하고 전체 try-catch문을 빠져나가서 그 다음 문장을 계속해서 수행한다.
    - 만일 일치하는 catch블럭을 찾지 못하면, 예외는 처리되지 못한다.
- **try블럭 내에서 예외가 발생하지 않은 경우**
    - catch블럭을 거치지 않고 전체 try-catch문을 빠져나가서 수행을 계속한다.

```ExceptionEx4.java
class ExceptionEx4 {
    public static void main(String[] args) {
        System.out.println(1);
        System.out.println(2);
        try {
            System.out.println(3);
            System.out.println(4);
        } catch (Exception e) {
            System.out.println(5); // 실행되지 않는다.
        }
        System.out.println(6);
    }
}
```

```실행결과
1
2
3
4
6
```

- 예외가 발생하지 않았으므로 catch블럭의 문장이 실행되지 않았다.

```ExceptionEx5.java
class ExceptionEx5 {
    public static void main(String[] args) {
        System.out.println(1);
        System.out.println(2);
        try {
            System.out.println(3);
            System.out.println(0/0); // 0으로 나눠서 고의적으로 ArithmeticException을 발생시킨다.
            System.out.println(4); // 실행되지 않는다.
        } catch (ArithmeticException e) {
            System.out.println(5);
        }
        System.out.println(6);
    }
}
```

```실행결과
1
2
3
5
6
```

- 1, 2, 3을 출력한 다음 try블럭에서 예외가 발생했기 때문에 try블럭을 바로 벗어나 `System.out.println(4);`는 실행되지 않는다.
- 그리고는 발생한 예외에 해당하는 `catch블럭`으로 이동하여 문장들을 수행한다.
- 다음엔 `전체 try-catch문`을 벗어나서 그 다음 문장을 수행하여 6을 화면에 출력한다.
- **try블럭에서 예외가 발생하면, 예외가 발생한 위치 이후에 있는 try블럭의 문장들은 수행되지 않으므로,** try블럭에 포함시킬 코드의 범위를 잘 선택해야한다.

### 예외의 발생과 catch블럭
- catch블럭은 `괄호()`와 `블럭{}` 두 부분으로 나눠져 있다.
- 괄호()내에는 처리하고자 하는 예외와 같은 타입의 참조변수 하나를 선언해야 한다.
- 예외가 발생하면, **발생한 예외에 해당하는 클래스의 인스턴스가 만들어 진다.**
- 예제 8-5에서는 `ArithmeticException`이 발생했으므로 `ArithmeticException인스턴스`가 생성된다.
- 예외가 발생한 문장이 try블럭에 포함되어 있다면, 이 예외를 처리할 수 있는 catch블럭이 있는지 찾게 된다.
- 첫 번째 catch블럭부터 차례로 내려가면서 catch블럭의 괄호()내에 선언된 참조변수의 종류와 생성된 예외클래스의 인스턴스에 `instanceof연산자`를 이용해서 검사하게 되는데, 검사결과가 `true`인 catch블럭을 만날 때까지 검사는 계속된다.
- 검사결과가 true인 catch블럭을 찾게 되면 블럭에 있는 문장들을 모두 수행한 후에 try-catch문을 빠져나가고 예외는 처리된다.
- 검사결과가 true인 catch블럭이 하나도 없으면 예외는 처리되지 않는다.
- **모든 예외클래스는 Exception클래스의 자손이므로, catch블럭의 괄호()에 Exception클래스 타입의 참조변수를 선언해 놓으면 어떤 종류의 예외가 발생하더라도 이 catch블럭에 의해서 처리된다.**

```ExceptionEx6.java
class ExceptionEx6 {
    public static void main(String[] args) {
        System.out.println(1);
        System.out.println(2);
        try {
            System.out.println(3);
            System.out.println(0/0); // 0으로 나눠서 고의적으로 ArithmeticException을 발생시킨다.
            System.out.println(4); // 실행되지 않는다.
        } catch (Exception e) { // ArithmeticException대신 Exception을 사용.
            System.out.println(5);
        }
        System.out.println(6);
    }
}
```

```실행결과
1
2
3
5
6
```

- 예제 8-5를 변경한 것인데, 결과는 같다.
- catch블럭의 괄호()에 `ArithmeticException`타입의 참조변수 대신에 `Exception`클래스의 참조변수를 선언하였다.
- `ArithmeticException클래스`는 `Exception클래스`의 자손이므로 `ArithmeticException인스턴스`와 `Exception클래스`와의 `instanceof`연산결과가 `true`가 되어 Exception클래스 타입의 참조변수를 선언한 catch블럭의 문장들이 수행되고 예외는 처리되는 것이다.

```ExceptionEx7.java
class ExceptionEx7 {
    public static void main(String[] args) {
        System.out.println(1);
        System.out.println(2);
        try {
            System.out.println(3);
            System.out.println(0/0); // 0으로 나눠서 고의적으로 ArithmeticException을 발생시킨다.
            System.out.println(4); // 실행되지 않는다.
        } catch (ArithmeticException ae) {
            if(ae instanceof ArithmeticException)
                System.out.println("true");
            System.out.println("ArithmeticException");
        } catch (Exception e) { // ArithmeticException을 제외한 모든 예외가 처리된다.
            System.out.println("Exception");
        }
        System.out.println(6);
    }
}
```

```실행결과
1
2
3
true
ArithmeticException
6
```

- 위의 예제에서는 두 개의 catch블럭, `ArithmeticException`클래스 타입의 참조변수를 선언한 것과 `Exception`클래스 타입의 참조변수를 선언한 것을 사용하였다.
- try블럭에서 `ArithmeticException`이 발생하였으므로 catch블럭을 하나씩 차례로 검사하게 되는데, **첫 번째 검사에서 일치하는 catch블럭을 찾았기 때문에 두 번째 catch블럭은 검사하지 않게 된다.**
- 만일 try블럭 내에서 `ArithmeticException`이 아닌 다른 종류의 예외가 발생한 경우에는 두 번째 catch블럭인 Exception클래스 타입의 참조변수를 선언한 곳에서 처리되었을 것이다.
- 이처럼, **try-catch문의 마지막에 Exception클래스 타입의 참조변수를 선언한 catch블럭을 사용하면, 어떤 종류의 예외가 발생하더라도 이 catch블럭에 의해 처리되도록 할 수 있다.**

#### printStackTrace()와 getMessage()
- 예외가 발생했을 때 생성되는 예외클래스의 인스턴스에는 **발생한 예외에 대한 정보가 담겨있다.**
- `printStackTrace()`와 `getMessage()`를 통해서 이 정보들을 얻을 수 있다.
    - `printStackTrace()` 예외발생 당시의 `호출스택(Call Stack)`에 있었던 메서드의 정보와 예외 메시지를 화면에 출력한다.
    - `getMessage()` 발생한 예외클래스의 인스턴스에 저장된 메시지를 얻을 수 있다.
- catch블럭의 괄호()에 선언된 참조변수를 통해 이 인스턴스에 접근할 수 있다.
- 이 참조변수는 선언된 catch블럭 내에서만 사용 가능하다.

```ExceptionEx8.java
class ExceptionEx8 {
    public static void main(String[] args) {
        System.out.println(1);
        System.out.println(2);
        try {
            System.out.println(3);
            System.out.println(0/0); // 예외발생
            System.out.println(4); // 실행되지 않는다.
        } catch (ArithmeticException ae) {
            ae.printStackTrace();
            System.out.println("예외 메시지 : " + ae.getMessage());
        }
        System.out.println(6);
    }
}
```

```실행결과
1
2
3
예외 메시지 : / by zero
6
java.lang.ArithmeticException: / by zero
	at ExceptionEx8.main(ExceptionEx8.java:7)
```

- [Stack Overflow - is printStackTrace() of Throwable class an asynchronus](https://stackoverflow.com/questions/23738725/is-printstacktrace-of-throwable-class-an-asynchronus)
- `printStackTrace` 출력은 `표준 출력 스트림(System.out)`이 아닌 `System.err`에 출력되기 때문에 다른 순서로 표기될 수 있다.

```Throwable.java
public void printStackTrace() {
        printStackTrace(System.err);
    }
```

-  `ae.printStackTrace(System.out);`으로 변경하면 다음과 같은 순서로 표기된다.

```실행결과
1
2
3
java.lang.ArithmeticException: / by zero
	at ExceptionEx8.main(ExceptionEx8.java:7)
예외 메시지 : / by zero
6
```

- 예외가 발생해서 비정상적으로 종료되었을 때의 결과와 비슷하지만 예외는 try-catch문에 의해 처리되었으며 프로그램은 정상적으로 종료되었다.
- 그 대신 `ArithmeticException`인스턴스의 `printStackTrace()`를 사용해서, `호출 스택(call stack)`에 대한 정보와 예외 메시지를 출력하였다.
- 이처럼 try-catch문으로 예외처리를 하여 예외가 발생해도 비정상적으로 종료하지 않도록 해주는 동시에, `printStackTrace()` 또는 `getMessage()`와 같은 메서드를 통해서 **예외의 발생원인**을 알 수 있다.
- *printStackTrace(PrintStream s) 또는 printStackTrace(PrintWriter s)를 사용하면 발생한 예외에 대한 정보를 파일에 저장할 수도 있다. 파일 입출력에 대한 내용은 15장에서 배운다.*

#### 멀티 catch블럭
- `JDK1.7`부터 여러 catch블럭을 `|` 기호를 이용해서, 하나의 catch블럭으로 합칠 수 있게 되었으며, 이를 `멀티 catch블럭`이라 한다.
- `멀티 catch블럭`을 이용하면 중복된 코드를 줄일 수 있다.
- `|`기호로 연결할 수 있는 예외 클래스의 개수에는 제한이 없다.
- *멀티 catch블럭에 사용되는 '&#124;'는 논리 연산자가 아니라 기호이다.*

```try-catch
try {
    ...
} catch (ExceptionA e) {
    e.printStackTrace();
} catch (ExceptionB e2) {
    e2.printStackTrace();
}
```

```try-catch
try {
    ...
} catch (ExceptionA | ExceptionB e) {
    e.printStackTrace();
}
```

- 만일 멀티 catch블럭의 '&#124;'기호로 연결된 예외 클래스가 **조상과 자손의 관계에 있다면 컴파일 에러가 발생한다.**

```try-catch
try {
    ...
} catch (ParentException | ChildException e) { // 에러!
    e.printStackTrace();
}
```

- 두 예외 클래스가 조상과 자손의 관계에 있다면, 그냥 조상 클래스만 써주는 것과 똑같기 때문이다. 불필요한 코드는 제거하라는 의미에서 에러가 발생하는 것이다.

```try-catch
try {
    ...
} catch (ParentException e) {
    e.printStackTrace();
}
```

- 멀티 catch는 하나의 catch블럭으로 여러 예외를 처리하는 것이기 때문에, 발생한 예외를 멀티 catch블럭으로 처리하게 되었을 때, **멀티 catch블럭 내에서는 실제로 어떤 예외가 발생한 것인지 알 수 없다.**
- 그래서 참조변수 e로 멀티 catch블럭에서 '&#124;'기호로 연결된 예외 클래스들의 공통 분모인 **조상 예외 클래스**에 선언된 멤버만 사용할 수 있다.
- 필요하다면 `instanceof`로 어떤 예외가 발생한 것인지 확인하고 개별적으로 처리할 수 있다. 그러나 이렇게까지 해가면서 catch블럭을 합칠 일은 거의 없을 것이다.

```try-catch
try {
    ...
} catch (ExceptionA | ExceptionB e) {
    e.methodA(); // 에러. ExceptionA에 선언된 methodA()는 호출불가

    if(e instanceof ExceptionA) {
        ExceptionA e1 = (ExceptionA)e;
        e1.methodA(); // OK. ExceptionA에 선언된 메서드 호출가능
    } else { // if(e instanceof ExceptionB)
        ...
    }
    
    e.printStackTrace();
}
```

- **멀티 catch블럭에 선언된 참조변수 e는 상수이므로 값을 변경할 수 없다는 제약이 있다.**
- 이것은 여러 catch블럭이 하나의 참조변수를 공유하기 때문에 생기는 제약으로 실제로 참조변수의 값을 바꿀 일은 없을 것이다.
- 여러 catch블럭을 멀티 catch블럭으로 합치는 경우는 대부분 코드를 간단히 하는정도의 수준일 것이므로 이러한 제약에 대해 너무 고민하지 않아도 된다.

## 🚩 잊지않기
- 프로그램 오류
    - 프로그램이 실행 중 어떤 원인에 의해서 오작동을 하거나 비정상적으로 종료되는 경우, 이러한 결과를 초래하는 원인을 `프로그램 에러` 또는 `오류`라고 한다.
    - 발생시점에 따라 `컴파일 에러`와 `런타임 에러`로 나눌 수 있다.
        - 컴파일 에러(compile-time error) : 컴파일 시에 발생하는 에러
        - 런타임 에러(runtime error) : 프로그램의 실행도중에 발생하는 에러
            - 자바에서는 실행 시(runtime) 발생할 수 있는 프로그램 오류를 `에러`와 `예외` 두 가지로 구분
            - `에러(error)` : 프로그램 코드에 의해서 수습될 수 없는 심각한 오류 *메모리 부족(OutOfMemoryError), 스택오버플로우(StackOverflowError)*
            - `예외(exception)` : 발생하더라도 프로그래머가 이에 대한 적절한 코드를 미리 작성해 놓음으로써 프로그램의 비정상적인 종료를 막을 수 있는 다소 미약한 오류
        - 논리적 에러(logical error) : 컴파일도 잘되고 실행도 잘되지만, 의도와 다르게 동작하는 것 *(예) 창고의 재고가 음수가 된다던가, 게임 프로그램에서 비행기가 총알을 맞아도 죽지 않는 경우*
- 예외 클래스의 계층구조
    - 자바에서는 실행 시 발생할 수 있는 프로그램 오류(Exception, Error)를 클래스로 정의
    - 모든 예외의 최고 조상은 `Exception클래스`
    - Exception클래스들은 두 그룹으로 나눠질 수 있음
        - RuntimeException클래스들
            - RuntimeException클래스와 그 자손 클래스들
            - 주로 `프로그래머의 실수`에 의해서 발생할 수 있는 예외들
                - 배열의 범위를 벗어남(ArrayIndexOutOfBoundsException)
                - 값이 null인 참조변수의 멤버를 호출하려 함(NullPointerException)
                - 클래스간의 형변환을 잘못함(ClassCastException)
                - 정수를 0으로 나누려고 함(ArithmeticException)
        - Exception클래스들
            - RuntimeException클래스들을 제외한 나머지 클래스들
            - 주로 `사용자의 실수`와 같은 `외적인 요인`에 의해 발생하는 예외
                - 존재하지 않는 파일의 이름을 입력(FileNotFoundException)
                - 실수로 클래스의 이름을 잘못 적음(ClassNotFoundException)
                - 입력한 데이터 형식이 잘못됨(DataFormatException)
- 예외처리(exception-handling) - try-catch문
    - 프로그램 실행 시 발생할 수 있는 예기치 못한 예외의 발생에 대비한 코드를 작성하는 것이다.
    - 예외처리의 목적은 **예외의 발생으로 인해 실행 중인 프로그램의 갑작스런 비정상 종료를 막고, 정상적인 실행상태를 유지할 수 있도록 하는 것**이다.
    - 발생한 예외를 처리하지 못하면, 프로그램은 비정상적으로 종료되며, `처리되지 못한 예외(uncaught exception)`는 JVM의 `예외처리기(UncaughtExceptionHandler)`가 받아서 예외의 원인을 화면에 출력한다.
    - 예외를 처리하기 위해서는 `try-catch`문을 사용한다.
    - 하나의 try블럭 다음에는 여러 종류의 예외를 처리할 수 있도록 하나 이상의 catch블럭이 올 수 있다.
    - 발생한 예외의 종류와 일치하는 단 한 개의 catch블럭만 수행된다.
    - 일치하는 catch블럭이 없으면 예외는 처리되지 않는다.
    - **try블럭에서 예외가 발생하면, 예외가 발생한 위치 이후에 있는 try블럭의 문장들은 수행되지 않으므로, try블럭에 포함시킬 코드의 범위를 잘 선택해야한다.**

```try-catch
try {
    // 예외가 발생할 가능성이 있는 문장들을 넣는다.
} catch(Exception1 e1) {
    // Exception1이 발생했을 경우, 이를 처리하기 위한 문장을 적는다.
} catch(Exception2 e2) {
    // Exception2이 발생했을 경우, 이를 처리하기 위한 문장을 적는다.
} catch(Exception3 e3) {
    // Exception3이 발생했을 경우, 이를 처리하기 위한 문장을 적는다.
}
```

- 예외의 발생과 catch블럭
    - 예외가 발생하면, **발생한 예외에 해당하는 클래스의 인스턴스**가 만들어 진다.
    - 첫 번째 catch블럭부터 차례로 내려가면서 catch블럭의 괄호()내에 선언된 참조변수의 종류와 생성된 예외클래스의 인스턴스에 `instanceof연산자`를 이용해서 검사하게 되는데, 검사결과가 `true`인 catch블럭을 만날 때까지 검사는 계속된다.
    - **모든 예외클래스는 Exception클래스의 자손이므로, catch블럭의 괄호()에 Exception클래스 타입의 참조변수를 선언해 놓으면 어떤 종류의 예외가 발생하더라도 이 catch블럭에 의해서 처리된다.**
    - 일치하는 catch블럭을 찾으면 이후 catch블럭은 검사하지 않게 된다.
- printStackTrace()와 getMessage()
    - 예외가 발생했을 때 생성되는 예외클래스의 인스턴스에는 **발생한 예외에 대한 정보가 담겨있다.**
    - printStackTrace() : 예외발생 당시의 호출스택(Call Stack)에 있었던 메서드의 정보와 예외 메시지를 화면에 출력한다.
    - getMessage() : 발생한 예외클래스의 인스턴스에 저장된 메시지를 얻을 수 있다.
    - catch블럭의 괄호()에 선언된 참조변수를 통해 이 인스턴스에 접근할 수 있다.
- 멀티 catch블럭
    - `JDK1.7`부터 여러 catch블럭을 `|` 기호를 이용해서, 하나의 catch블럭으로 합칠 수 있게 됨
    - 만일 멀티 catch블럭의 '&#124;'기호로 연결된 예외 클래스가 **조상과 자손의 관계에 있다면 컴파일 에러가 발생한다.**
    
```try-catch
try {
    ...
} catch (ExceptionA | ExceptionB e) {
    e.printStackTrace();
}
```