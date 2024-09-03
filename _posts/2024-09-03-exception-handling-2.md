---
layout: post
title: "Java의 정석 Chapter08. 예외처리 (2) - throw, throws, finally"
subtitle: Java의 정석 3rd Edition | 남궁성
excerpt_image: /assets/images/excerpt-images/2024-09-03-exception-handling-2.png
categories: 
    - Java
tags: 
    - Java
    - Java의 정석
    - 예외처리
    - throw
    - unchecked예외
    - checked예외
    - throws
    - finally
---
![banner](/assets/images/excerpt-images/2024-09-03-exception-handling-2.png)

## 예외 발생시키기
- 키워드 `throw`를 사용해서 **프로그래머가 고의로 예외를 발생시킬 수 있다.**
    - 먼저, 연산자 `new`를 이용해서 발생시키려는 예외 클래스의 객체를 만든다.
        - `Exception e = new Exception("고의로 발생시켰음");`
    - 키워드 throw를 이용해서 예외를 발생시킨다.
        - `throw e;`

```ExceptionEx9.java
class ExceptionEx9 {
    public static void main(String[] args) {
        try {
            Exception e = new Exception("고의로 발생시켰음");
            throw e; // 예외를 발생시킴
//            throw new Exception("고의로 발생시켰음."); // 위의 두 줄을 한 줄로 줄여 쓸 수 있다.
        } catch (Exception e) {
            System.out.println("에러 메시지 : " + e.getMessage());
            e.printStackTrace();
        }
        System.out.println("프로그램이 정상 종료되었음.");
    }
}
```

```실행결과
에러 메시지 : 고의로 발생시켰음
프로그램이 정상 종료되었음.
java.lang.Exception: 고의로 발생시켰음
	at ExceptionEx9.main(ExceptionEx9.java:4)
```

- Exception인스턴스를 생성할 때, 생성자에 `String`을 넣어주면, 이 String이 `Exception인스턴스에 메시지`로 저장된다.
- 이 메시지는 `getMessage()`를 이용해서 얻을 수 있다.

```ExceptionEx10.java
class ExceptionEx10 {
    public static void main(String[] args) {
        throw new Exception(); // Exception을 고의로 발생시킨다.
    }
}
```

```컴파일 결과
unreported exception java.lang.Exception; must be caught or declared to be thrown
```

- 예외처리가 되어야 하는 부분에 예외처리가 되어 있지 않다는 `컴파일 에러` 발생
- `Exception클래스들`이 발생할 가능성이 있는 문장들에 대해 예외처리를 해주지 않으면 컴파일조차 되지 않는다.

```ExceptionEx11.java
class ExceptionEx11 {
    public static void main(String[] args) {
        throw new RuntimeException(); // RuntimeException을 고의로 발생시킨다.
    }
}
```

```실행결과
Exception in thread "main" java.lang.RuntimeException
	at ExceptionEx11.main(ExceptionEx11.java:3)
```

- 예외처리를 하지 않았음에도 불구하고 이전의 예제와는 달리 `성공적으로 컴파일`될 것이다.
- 그러나 실행하면, 위의 실행결과처럼 `RuntimeException`이 발생하여 비정상적으로 종료될 것이다.
- `RuntimeException클래스들`에 해당하는 예외는 **프로그래머에 의해 실수로 발생하는 것들이기 때문에 예외처리를 강제하지 않는다.**
- 만일 RuntimeException클래스들에 속하는 예외가 발생할 가능성이 있는 코드에도 예외처리를 필수로 해야 한다면, 아래와 같이 참조변수와 배열이 사용되는 모든 곳에 예외처리를 해주어야 할 것이다.

```throw
try {
    int[] arr = new int[10];
    System.out.println(arr[0]);
} catch (ArrayIndexOutOfBoundsException ae) {
    ...
} catch (NullPointerException ae) {
    ...
}
```

- 컴파일러가 예외처리를 확인하지 않는 `RuntimeException클래스들`은 `unchecked예외`라고 부른다.
- 컴파일러가 예외처리를 확인하는 `Exception클래스들`은 `checked예외`라고 부른다.

## 메서드에 예외 선언하기
- 예외를 처리하는 방법에는 지금까지 배워온 `try-catch문`을 사용하는 것 외에, `예외를 메서드에 선언`하는 방법이 있다.
- 메서드에 예외를 선언하려면, 메서드의 선언부에 키워드 `throws`를 사용해서 메서드 내에서 발생할 수 있는 예외를 적어주기만 하면 된다.
- 예외가 여러 개일 경우에는 `쉼표(,)`로 구분한다.

```throws
void method() throws Exception1, Exception2, ..., ExceptionN {
    // 메서드의 내용
}
```
- *예외를 발생시키는 키워드 `throw`와 예외를 메서드에 선언할 때 쓰이는 `throws`를 잘 구별하자.*
- 만일 아래와 같이 모든 예외의 최고조상인 `Exception클래스`를 메서드에 선언하면, 이 메서드는 **모든 종류의 예외**가 발생할 가능성이 있다는 뜻이다.

```throws
void method() throws Exception {
    // 메서드의 내용
}
```

- 이렇게 예외를 선언하면, 이 예외뿐만 아니라 그 `자손타입의 예외`까지도 발생할 수 있다는 점에 주의해야 한다.
- 앞서 오버라이딩에서 살펴본 것과 같이, 오버라이딩할 때는 **단순히 선언된 예외의 개수가 아니라 상속관계까지 고려해야 한다.**
- 메서드의 선언부에 예외를 선언함으로써 메서드를 사용하려는 사람이 메서드의 선언부를 보았을 때, **이 메서드를 사용하기 위해서는 어떠한 예외들이 처리되어져야 하는지** 쉽게 알 수 있다.
- 기존의 많은 언어들에서는 메서드에 예외선언을 하지 않기 때문에, 어떤 상황에 어떤 종류의 예외가 발생할 가능성이 있는지 충분히 예측하기 힘들기 때문에 그에 대한 대비를 하는 것이 어려웠다.
- 그러나 자바에서는 메서드를 작성할 때 메서드 내에서 발생할 가능성이 있는 예외를 메서드의 선언부에 명시하여 **이 메서드를 사용하는 쪽에서는 이에 대한 처리를 하도록 강요**하기 때문에 견고한 프로그램 코드를 작성할 수 있도록 도와준다.

![Java API에서 찾아본 Object클래스의 wait메서드](/assets/images/javajungsuk3/8-3.png)

- Java API문서에서 찾아본 `java.lang.Object클래스`의 `wait메서드`에 대한 설명
- 메서드의 선언부에 `InterruptedException`이 키워드 `throws`와 함께 적혀있는 것을 볼 수 있다.
- 이 메서드에서는 `InterruptedException`이 발생할 수 있으니, 이 메서드를 호출하고자 하는 메서드에서는 `InterruptedException`을 처리해주어야 한다는 것이다.

![Java API에서 찾아본 InterruptedException](/assets/images/javajungsuk3/8-4.png)

- Java API문서에서 찾아본 `InterruptedException`
- `InterruptedException`은 `Exception클래스`의 자손이다.
- 따라서 InterruptedException은 반드시 처리해주어야 하는 예외임으로 wait메서드의 선언부에 키워드 throws와 함께 선언되어져 있는 것이다.

- wait메서드 설명의 아래쪽에 있는 `Throws:`를 보면, wait메서드에서 발생할 수 있는 예외의 리스트와 언제 발생하는지에 대한 설명이 덧붙여져 있다.
- 메서드에 선언되어 있는 `InterruptedException` 외에도 `IllegalArgumentException`와 `IllegalMonitorStateException`이 있다.

![Java API에서 찾아본 IllegalArgumentException](/assets/images/javajungsuk3/8-5-1.png)

![Java API에서 찾아본 IllegalMonitorStateException](/assets/images/javajungsuk3/8-5-2.png)

- 이들은 `RuntimeException클래스`의 자손이므로 예외처리를 해주지 않아도 된다. 그래서 wait메서드의 선언부에 적지 않은 것이다.

- **메서드에 예외를 선언할 때 일반적으로 RuntimeException클래스들은 적지 않는다.**
- 이들을 메서드의 선언부의 `throws`에 선언한다고 해서 문제가 되지는 않지만, 보통 반드시 처리해주어야 하는 예외들만 선언한다.
- 이처럼 Java API문서를 통해 사용하고자 하는 메서드의 선언부와 'Throws:'를 보고, **이 메서드에서는 어떤 예외가 발생할 수 있으며 반드시 처리해주어야 하는 예외는 어떤 것들이 있는지 확인하는 것이 좋다.**
- 사실 예외를 메서드의 `throws`에 명시하는 것은 **예외를 처리하는 것이 아니라, 자신(예외가 발생할 가능성이 있는 메서드)을 호출한 메서드에게 예외를 전달하여 예외처리를 떠맡기는 것이다.**
- 예외를 전달받은 메서드가 또 다시 자신을 호출한 메서드에게 전달할 수 있으며, 이런 식으로 계속 `호출스택`에 있는 메서드들을 따라 전달되다가 제일 마지막에 있는 `main메서드`에서도 예외가 처리되지 않으면, main메서드마저 종료되어 프로그램이 전체가 종료된다.

```ExceptionEx12.java
class ExceptionEx12 {
    public static void main(String[] args) throws Exception {
        method1(); // 같은 클래스 내의 static멤버이므로 객체생성없이 직접 호출가능.
    }

    static void method1() throws Exception {
        method2();
    }

    static void method2() throws Exception {
        throw new Exception();
    }
}
```

```실행결과
Exception in thread "main" java.lang.Exception
	at ExceptionEx12.method2(ExceptionEx12.java:11)
	at ExceptionEx12.method1(ExceptionEx12.java:7)
	at ExceptionEx12.main(ExceptionEx12.java:3)
```

- 프로그램의 실행도중 `java.lang.Exception`이 발생하여 비정상적으로 종료했다는 것과 예외가 발생했을 때 `호출스택(call stack)`의 내용을 알 수 있다.
    - 예외가 발생했을 때, 모두 3개의 메서드(main, method1, method2)가 호출스택에 있었다.
    - 예외가 발생한 곳은 제일 윗줄에 있는 method2()다.
    - main메서드가 method1()을, 그리고 method1()은 method2()를 호출했다는 것을 알 수 있다.
- 위의 예제를 보면 `throw new Exception();` 문장에 의해 예외가 강제적으로 발생했으나 `try-catch문`으로 예외처리를 해주지 않았다.
- `method2()`는 종료되면서 예외를 자신을 호출한 `method1()`에게 넘겨준다.
- `method1()`에서도 역시 예외처리를 해주지 않았으므로 종료되면서 `main메서드`에게 예외를 넘겨준다.
- 그러나 `main메서드`에서 조차 예외처리를 해주지 않았으므로 main메서드가 종료되어 프로그램이 예외로 인해 **비정상적으로 종료**되는 것이다.
- 이처럼 예외가 발생한 메서드에서 예외처리를 하지 않고 자신을 호출한 메서드에게 예외를 넘겨줄 수는 있지만, 이것으로 예외가 처리가 된 것은 아니고 예외를 단순히 전달만 하는 것이다.
- 결국 어느 한 곳에서 반드시 `try-catch문`으로 예외처리를 해주어야 한다.

```ExceptionEx13.java
class ExceptionEx13 {
    public static void main(String[] args) {
        method1(); // 같은 클래스 내의 static멤버이므로 객체생성없이 직접 호출가능.
    }

    static void method1() {
        try {
            throw new Exception();
        } catch (Exception e) {
            System.out.println("method1메서드에서 예외가 처리되었습니다.");
            e.printStackTrace();
        }
    }
}
```

```실행결과
method1메서드에서 예외가 처리되었습니다.
java.lang.Exception
	at ExceptionEx13.method1(ExceptionEx13.java:8)
	at ExceptionEx13.main(ExceptionEx13.java:3)
```

- 예외는 처리되었지만, `printStackTrace()`를 통해 예외에 대한 정보를 화면에 출력하였다.
- 예외가 `method1()`에서 발생했으며, `main메서드`가 `method1()`을 호출했음을 알 수 있다.

```ExceptionEx14.java
class ExceptionEx14 {
    public static void main(String[] args) {
        try {
            method1();
        } catch (Exception e) {
            System.out.println("main메서드에서 에외가 처리되었습니다.");
            e.printStackTrace();
        }
    }

    static void method1() throws Exception {
        throw new Exception();
    }
}
```

```실행결과
main메서드에서 에외가 처리되었습니다.
java.lang.Exception
	at ExceptionEx14.method1(ExceptionEx14.java:12)
	at ExceptionEx14.main(ExceptionEx14.java:4)
```

- 예제 8-13은 `method1()`에서 예외처리를 했다.
    - `예외가 발생한 메서드(method1())` 내에서 처리되어지면, 호출한 메서드(main메서드)에서는 **예외가 발생했다는 사실조차 모르게 된다.**
- 예제 8-14는 `method1()`에서 예외를 선언하여 `자신을 호출하는 메서드(main메서드)`에 예외를 전달했으며, `호출한 메서드(main메서드)`에서는 `try-catch문`으로 예외처리를 했다.
    - 예외가 발생한 메서드에서 예외를 처리하지 않고 `호출한 메서드`로 넘겨주면, 호출한 메서드에서는 **`method1()`을 호출한 라인에서 예외가 발생한 것으로 간주되어 이에 대한 처리를 하게된다.**
- 이처럼 예외가 발생한 메서드 `method1()`에서 처리할 수도 있고, 예외가 발생한 메서드를 호출한 `main메서드`에서 처리할 수도 있다. 또는 두 메서드가 예외처리를 분담할 수도 있다.

```ExceptionEx15.java
import java.io.File;

class ExceptionEx15 {
    public static void main(String[] args) {
        // command line에서 입력받은 값을 이름으로 갖는 파일을 생성한다.
        File f = createFile(args[0]);
        System.out.println(f.getName() + " 파일이 성공적으로 생성되었습니다.");
    }

    static File createFile(String fileName) {
        try {
            if (fileName == null || fileName.equals("")) {
                throw new Exception("파일이름이 유효하지 않습니다.");
            }
        } catch(Exception e) {
            // fileName이 부적절한 경우, 파일 이름을 '제목없음.txt'로 한다.
            fileName = "제목없음.txt";
        } finally {
            File f = new File(fileName); // File클래스의 객체를 만든다.
            createNewFile(f); // 생성된 객체를 이용해서 파일을 생성한다.
            return f;
        }
    }

    static void createNewFile(File f) {
        try {
            f.createNewFile(); // 파일을 생성한다
        } catch (Exception e) { }
    }
}
```

```실행결과
> java ExceptionEx15 "test.txt"
test.txt 파일이 성공적으로 생성되었습니다.

> java ExceptionEx15 ""
제목없음.txt 파일이 성공적으로 생성되었습니다.

> dir *.txt
디렉터리: C:\chaeichi\javajungsuk3\ch08

Mode                 LastWriteTime         Length Name
----                 -------------         ------ ----
-a----      2024-09-03  오후 10:07              0 test.txt
-a----      2024-09-03  오후 10:08              0 제목없음.txt
```

- *실행 시 커맨드라인에 파일이름을 입력하지 않으면, args[0]
이 유효하지 않으므로 `File f = createFile(args[0]);`에서 `ArrayIndexOutOfBoundsException`이 발생한다.*
- 이 예제는 예외가 발생한 메서드에서 직접 예외를 처리하도록 되어 있다.
- createFile메서드를 호출한 main메서드에서는 예외가 발생한 사실을 알지 못한다.
- 적절하지 못한 파일이름(fileName)이 입력되면 예외를 발생시키고, catch블럭에서 파일이름을 '제목없음.txt'로 설정해서 파일을 생성한다.
- 그리고 `finally블럭`에서는 **예외의 발생여부에 관계없이 파일을 생성하는 일을 한다.**
- *File클래스의 createNewFile()은 예외가 선언된 메서드이므로 finally블럭 내에 또 다시 try-catch문으로 처리해야하므로 좀 복잡해진다. 이해를 돕기 위해 예제의 기본 흐름을 되도록 간단히 하려고 내부적으로 예외처리를 한 createNewFile(File f)메서드를 만들어서 사용했다.*

```ExceptionEx16.java
import java.io.File;

class ExceptionEx16 {
    public static void main(String[] args) {
        try {
            File f = createFile(args[0]);
            System.out.println(f.getName() + "파일이 성공적으로 생성되었습니다.");
        } catch (Exception e) {
            System.out.println(e.getMessage() + " 다시 입력해 주시기 바랍니다.");
        }
    }

    static File createFile(String fileName) throws Exception {
        if(fileName == null || fileName.equals(""))
            throw new Exception("파일이름이 유효하지 않습니다.");
        File f = new File(fileName); // File클래스의 객체를 만든다.
        // File객체의 createNewFile메서드를 이용해서 실제 파일을 생성한다.
        f.createNewFile();
        return f; //생성된 객체의 참조를 반환한다.
    }
}
```

```실행결과
> java ExceptionEx16 test2.txt
test2.txt파일이 성공적으로 생성되었습니다.

> java ExceptionEx16 ""
파일이름이 유효하지 않습니다. 다시 입력해 주시기 바랍니다.
```

- 이 예제에서는 예외가 발생한 createFile메서드에서 잘못 입력된 파일이름을 임의로 처리하지 않고, 호출한 main메서드에게 예외가 발생했음을 알려서 파일이름을 다시 입력 받도록 하였다.
- 예제 8-15와는 달리 **createFile메서드에 예외를 선언**했기 때문에, File클래스의 createNewFile()에 대한 예외처리를 하지 않아도 되므로 createNewFile(File f)메서드를 굳이 따로 만들지 않았다.
- 두 예제 모두 커맨드라인으로부터 파일이름을 입력 받아서 파일을 생성하는 일을 하며, 파일 이름을 잘못 입력했을 때(null 또는 빈 문자열일 때) 예외가 발생하도록 되어 있다.
- 예제 8-15는 예외가 발생한 `createFile메서드` 자체 내에서 처리한다.
    - 이처럼 예외가 발생한 메서드 내에서 **자체적으로 처리해도 되는 것은 메서드 내에서 `try-catch`문을 사용해서 처리한다.**
- 예제 8-16은 createFile메서드를 `호출한 메서드(main메서드)`에서 처리한다.
    - **메서드에 호출 시 넘겨받아야 할 값(fileName)을 다시 받아야 하는 경우(메서드 내에서 자체적으로 해결이 안되는 경우)에는 예외를 메서드에 선언해서, 호출한 메서드에서 처리해야한다.**

## finally블럭
- `finally블럭`은 **예외의 발생여부에 상관없이 실행되어야할 코드를 포함시킬 목적**으로 사용된다.
- `try-catch문`의 끝에 선택적으로 덧붙여 사용할 수 있으며, **try-catch-finally**의 순서로 구성된다.

```finally
try {
    // 예외가 발생할 가능성이 있는 문장들을 넣는다.
} catch (Exception1 e1) {
    // 예외처리를 위한 문장을 적는다.
} finally {
    // 예외의 발생여부에 관계없이 항상 수행되어야하는 문장들을 넣는다.
    // finally블럭은 try-catch문의 맨 마지막에 위치해야한다.
}
```

- 예외가 발생한 경우에는 **try → catch → finally**의 순으로 실행된다.
- 예외가 발생하지 않은 경우에는 **try → finally**의 순으로 실행된다.

```FinallyTest.java
class FinallyTest {
    public static void main(String[] args) {
        try {
            startInstall(); // 프로그램 설치에 필요한 준비를 한다.
            copyFiles(); // 파일들을 복사한다.
            deleteTempFiles(); // 프로그램 설치에 사용된 임시파일들을 삭제한다.
        } catch (Exception e) {
            e.printStackTrace();
            deleteTempFiles(); // 프로그램 설치에 사용된 임시파일들을 삭제한다.
        }
    }

    static void startInstall() {
        /* 프로그램 설치에 필요한 준비를 하는 코드를 적는다. */
    }
    static void copyFiles() { /* 파일을 복사하는 코드를 적는다. */ }
    static void deleteTempFiles() { /* 임시파일들을 삭제하는 코드를 적는다. */ }
}
```

- *startInstall(), copyFiles(), deleteTempFiles()에 주석문 이외에는 아무런 문장이 없지만, 각 메서드의 의미에 해당하는 작업을 수행하는 코드들이 작성되어 있다고 가정하자.*
- 이 예제가 하는 일은 프로그램 설치를 위한 준비를 하고 파일들을 복사하고 설치가 완료되면, 프로그램을 설치하는데 사용된 임시파일들을 삭제하는 순서로 진행된다.
- 프로그램의 설치과정 중에 예외가 발생하더라도, 설치에 사용된 임시파일들이 삭제되도록 catch블럭에 deleteTempFiles()메서드를 넣었다.
- 결국 `try블럭`의 문장을 수행하는 동안에(프로그램을 설치하는 과정에), **예외의 발생여부에 관계없이 `deleteTempFiles()메서드`는 실행되어야 하는 것이다.** 이럴 때 `finally블럭`을 사용하면 좋다.

```FinallyTest2.java
class FinallyTest2 {
    public static void main(String[] args) {
        try {
            startInstall(); // 프로그램 설치에 필요한 준비를 한다.
            copyFiles(); // 파일들을 복사한다.
        } catch (Exception e) {
            e.printStackTrace();
        } finally {
            deleteTempFiles(); // 프로그램 설치에 사용된 임시파일들을 삭제한다.
        }
    }

    static void startInstall() {
        /* 프로그램 설치에 필요한 준비를 하는 코드를 적는다. */
    }
    static void copyFiles() { /* 파일을 복사하는 코드를 적는다. */ }
    static void deleteTempFiles() { /* 임시파일들을 삭제하는 코드를 적는다. */ }
}
```

```FinallyTest3.java
class FinallyTest3 {
    public static void main(String[] args) {
        // method1()은 static메서드이므로 인스턴스 생성없이 직접 호출이 가능하다.
        FinallyTest3.method1();
        System.out.println("method1()의 수행을 마치고 main메서드로 돌아왔습니다.");
    }

    static void method1() {
        try {
            System.out.println("method1()이 호출되었습니다.");
            return; // 현재 실행 중인 메서드를 종료한다.
        } catch (Exception e) {
            e.printStackTrace();
        } finally {
            System.out.println("method1()의 finally블럭이 실행되었습니다.");
        }
    }
}
```

```실행결과
method1()이 호출되었습니다.
method1()의 finally블럭이 실행되었습니다.
method1()의 수행을 마치고 main메서드로 돌아왔습니다.
```

- try블럭에서 `return문`이 실행되는 경우에도 `finally블럭`의 문장들이 먼저 실행된 후에, 현재 실행 중인 메서드를 종료한다.
- 이와 마찬가지로 catch블럭의 문장 수행 중에 return문을 만나도 `finally블럭`의 문장들은 수행된다.