---
layout: post
title: "Java의 정석 Chapter08. 예외처리 (3) - 자동 자원 반환 / 사용자 정의 예외 / 예외 되던지기 / 연결된 예외"
subtitle: Java의 정석 3rd Edition | 남궁성
excerpt_image: /assets/images/excerpt-images/javajungsuk3/2024-09-06-exception-handling-3.png
categories: 
    - Java
tags: 
    - Java
    - Java의 정석
    - 예외처리
    - try-with-resources문
    - Suppressed
    - 사용자 정의 예외
    - checked예외
    - unchecked예외
    - re-throwing
    - chained exception
    - Caused by
---
![banner](/assets/images/excerpt-images/javajungsuk3/2024-09-06-exception-handling-3.png)

## 자동 자원 반환 - try-with-resources문
- JDK1.7부터 `try-with-resources문`이라는 `try-catch문`의 변형이 새로 추가되었다.
- 주로 입출력에 사용되는 클래스 중에서는 **사용한 후에 꼭 닫아줘야 하는 것들이 있다.** 그래서 사용했던 자원(resources)이 반환되기 때문이다.

```try-catch
try {
    fis = new FileInputStream("score.dat");
    dis = new DataInputStream(fis);
    ...
} catch (IOException ie) {
    ie.printStackTrace();
} finally {
    dis.close(); // 작업 중에 예외가 발생하더라도, dis가 닫히도록 finally블럭에 넣음
}
```

- 위의 코드는 `DataInputStream`을 사용해서 파일로부터 데이터를 읽는 코드이다.
- 데이터를 읽는 도중에 예외가 발생하더라도 `DataInputStream`이 닫히도록 `finally블럭`안에 `close()`를 넣었다.
- 문제는 `close()`가 예외를 발생시킬 수 있다.

```try-catch
try {
    fis = new FileInputStream("score.dat");
    dis = new DataInputStream(fis);
    ...
} catch (IOException ie) {
    ie.printStackTrace();
} finally {
    try {
        if(dis != null)
            dis.close();
    } catch (IOException ie) {
        ie.printStackTrace();
    }
}
```

- `finally블럭` 안에 `try-catch문`을 추가해서 close()에서 발생할 수 있는 예외를 처리하도록 변경하였다.
- 그러나 코드가 복잡해져서 별로 보기에 좋지 않다.
- 더 나쁜 것은 **try블럭과 finally블럭에서 모두 예외가 발생하면, try블럭의 예외는 무시된다는 것이다.**
- 이러한 점을 개선하기 위해서 `try-with-resources문`이 추가된 것이다.

```try-with-resources
// 괄호() 안에 두 문장 이상 넣을 경우 ';'로 구분한다.
try (FileInputStream fis = new FileInputStream("score.dat");
    DataInputStream dis = new DataInputStream(fis)) {
    
    while(true) {
        score = dis.readInt();
        System.out.println(score);
        System.out.println(score);
        sum += score;
    }
} catch (EOFException e) {
    System.out.println("정수의 총합은 " : sum + "입니다.");
} catch (IOException ie) {
    ie.printStackTrace();
}
```

- `try-with-resources문`의 `괄호()` 안에 객체를 생성하는 문장을 넣으면, **이 객체는 close()를 호출하지 않아도 try블럭을 벗어나는 순간 자동적으로 close()가 호출된다.** 그 다음에는 catch블럭 또는 finally블럭이 수행된다.
- *try블럭의 괄호() 안에 변수를 선언하는 것도 가능하며, 선언된 변수는 try블럭 내에서만 사용할 수 있다.*
- 이처럼 `try-with-resources문`에 의해 자동으로 객체의 `close()`가 호출될 수 있으려면, 클래스가 `AutoCloseable`이라는 인터페이스를 구현한 것이어야만 한다.

```AutoCloseable
public interface AutoCloseable {
    void close() throws Exception
}
```

- 위의 인터페이스는 각 클래스에서 적절히 자원 반환작업을 하도록 구현되어 있다.
- 그런데 위의 코드를 보면 Exception을 발생시킬 수 있다.
- 만일 자동 호출된 close()에서 예외가 발생하면 어떻게 될지 예제를 먼저 실행해보자.

```TryWithResourceEx.java
class TryWithResourceEx {
    public static void main(String[] args) {
        try (CloseableResource cr = new CloseableResource()) {
            cr.exceptionWork(false); // 예외가 발생하지 않는다.
        } catch (WorkException e) {
            e.printStackTrace();
        } catch (CloseException e) {
            e.printStackTrace();
        }
        System.out.println();

        try (CloseableResource cr = new CloseableResource()) {
            cr.exceptionWork(true); // 예외가 발생한다.
        } catch (WorkException e) {
            e.printStackTrace();
        } catch (CloseException e) {
            e.printStackTrace();
        }
    }
}

class CloseableResource implements AutoCloseable {
    public void exceptionWork(boolean exception) throws WorkException {
        System.out.println("exceptionWork(" + exception + ")가 호출됨");

        if(exception)
            throw new WorkException("WorkException 발생!!!");
    }

    public void close() throws CloseException {
        System.out.println("close()가 호출됨");
        throw new CloseException("CloseException 발생!!!");
    }
}

class WorkException extends Exception {
    WorkException(String msg) { super(msg); }
}

class CloseException extends Exception {
    CloseException(String msg) { super(msg); }
}
```

```실행결과
exceptionWork(false)가 호출됨
close()가 호출됨
CloseException: CloseException 발생!!!
	at CloseableResource.close(TryWithResourceEx.java:32)
	at TryWithResourceEx.main(TryWithResourceEx.java:5)

exceptionWork(true)가 호출됨
close()가 호출됨
WorkException: WorkException 발생!!!
	at CloseableResource.exceptionWork(TryWithResourceEx.java:27)
	at TryWithResourceEx.main(TryWithResourceEx.java:13)
	Suppressed: CloseException: CloseException 발생!!!
		at CloseableResource.close(TryWithResourceEx.java:32)
		at TryWithResourceEx.main(TryWithResourceEx.java:12)
```

- main메서드에 두 개의 try-catch문이 있다.
    - 첫 번째 것은 `close()`에서만 예외를 발생시킨다.
    - 두 번째 것은 `exceptionWork()`와 `close()`에서 모두 예외를 발생시킨다.
- 첫 번째는 일반적인 예외가 발생했을 때와 같은 형태로 출력되었다.
- 두 번째는 출력형태가 다르다. 먼저 `exceptionWork()`에서 발생한 예외에 대한 내용이 출력되고, `close()`에서 발생한 예외는 **억제된(Suppressed)**이라는 의미의 머리말과 함께 출력되었다.
- 두 예외가 동시에 발생할 수 없기 때문에, 실제 발생한 예외를 `WorkException`으로 하고, `CloseException`은 **억제된 예외**로 다룬다.
- 억제된 예외에 대한 정보는 실제로 발생한 `WorkException`에 저장된다.
- `Throwable`에는 억제된 예외와 관련된 다음과 같은 메서드가 정의되어 있다.
    - void addSuppressed(Throwable exception) // 억제된 예외를 추가
    - Throwable[] getSuppressed() // 억제된 예외(배열)를 반환
- 만일 기존의 `try-catch문`을 사용했다면, 먼저 발생한 `WorkException`은 무시되고, 마지막으로 발생한 `CloseException`에 대한 내용만 출력되었을 것이다.

## 사용자 정의 예외 만들기
- 기존의 정의된 예외 클래스 외에 필요에 따라 프로그래머가 새로운 예외 클래스를 정의하여 사용할 수 있다.
- 보통 `Exception클래스` 또는 `RuntimeException클래스`로부터 상속받아 클래스를 만들지만, 필요에 따라서 알맞은 예외 클래스를 선택할 수 있다.
- *가능하면 새로운 예외 클래스를 만들기보다 기존의 예외클래스를 활용하자.*

```custom-exception
class MyException extends Exception {
    MyException(String msg) { // 문자열을 매개변수로 받는 생성자
        super(msg); // 조상인 Exception클래스의 생성자를 호출한다.
    }
}
```

- Exception클래스로부터 상속받아서 MyException클래스를 만들었다.
- Exception클래스는 생성 시에 `String` 값을 받아서 `메시지`로 저장할 수 있다.
- 필요하다면, 멤버변수나 메서드를 추가할 수 있다.

```custom-exception
class MyException extends Exception {
    // 에러 코드 값을 저장하기 위한 필드를 추가 했다.
    private final int ERR_CORD; // 생성자를 통해 초기화 한다.

    MyException(String msg, int errCode) { // 생성자
        super(msg);
        ERR_CORD = errCode;
    }

    MyException(String msg) { // 생성자
        this(msg, 100); // ERR_CORD를 100(기본값)으로 초기화한다.
    }

    public int getErrCord() { // 에러 코드를 얻을 수 있는 메서드를 추가했다.
        return ERR_CORD; // 이 메서드는 주로 getMessage()와 함께 사용될 것이다.
    }
}
```

- 이전의 코드를 좀 더 개선하여 메시지 뿐만 아니라 에러코드 값도 저장할 수 있도록 ERR_CORD와 getErrCord()를 MyException클래스의 멤버로 추가했다.
- 이렇게 함으로써 MyException이 발생했을 때, catch블럭에서 getMessage()와 getErrCord()를 사용해서 에러코드와 메시지를 모두 얻을 수 있을 것이다.

- 기존의 예외 클래스는 주로 `Exception`을 상속받아서 `checked예외`로 작성하는 경우가 많았다.
- 요즘에는 예외처리를 선택적으로 할 수 있는 `RuntimeException`을 상속받아서 작성하는 쪽으로 바뀌어가고 있다.
- 프로그래밍 환경이 달라지면서 필수적으로 처리해야만 할 것 같았던 예외들이 선택적으로 처리해도 되는 상황으로 바뀌는 경우가 종종 발생하고 있다.
- 그래서 필요에 따라 예외처리의 여부를 선택할 수 있는 `unchecked예외`가 강제적인 `checked예외`보다 더 환영받고 있다.
- `checked예외`는 반드시 예외처리를 해주어야 하기 때문에 예외처리가 불필요한 경우에도 `try-catch문`을 넣어서 코드가 복잡해지기 때문이다.

```NewExceptionTest.java
class NewExceptionTest {
    public static void main(String[] args) {
        try {
            startInstall(); // 프로그램 설치에 필요할 준비를 한다.
            copyFiles(); // 파일들을 복사한다.
        } catch (SpaceException e) {
            System.out.println("에러 메시지 : " + e.getMessage());
            e.printStackTrace();
            System.out.println("공간을 확보한 후에 다시 설치하시기 바랍니다.");
        } catch (MemoryException me) {
            System.out.println("에러 메시지 : " + me.getMessage());
            me.printStackTrace();
            System.gc(); // Garbage Collection을 수행하여 메모리를 늘려준다.
            System.out.println("다시 설치를 시도하세요.");
        } finally {
            deleteTempFiles(); // 프로그램 설치에 사용된 임시파일들을 삭제한다.
        }
    }

    static void startInstall() throws SpaceException, MemoryException {
        if(!enoughSpace()) // 충분한 설치 공간이 없을 경우
            throw new SpaceException("설치할 공간이 부족합니다.");
        if(!enoughMemory()) // 충분한 메모리가 없을 경우
            throw new MemoryException("메모리가 부족합니다.");
    }

    static void copyFiles() { /* 파일들을 복사하는 코드를 적는다. */ }
    static void deleteTempFiles() { /* 임시파일들을 삭제하는 코드를 적는다. */ }

    static boolean enoughSpace() {
        // 설치하는데 필요한 공간이 있는지 확인하는 코드를 적는다.
        return false;
    }

    static boolean enoughMemory() {
        // 설치하는데 필요한 메모리공간이 있는지 확인하는 코드를 적는다.
        return true;
    }
}

class SpaceException extends Exception {
    SpaceException(String msg) {
        super(msg);
    }
}

class MemoryException extends Exception {
    MemoryException(String msg) {
        super(msg);
    }
}
```

```실행결과
에러 메시지 : 설치할 공간이 부족합니다.
공간을 확보한 후에 다시 설치하시기 바랍니다.
SpaceException: 설치할 공간이 부족합니다.
	at NewExceptionTest.startInstall(NewExceptionTest.java:22)
	at NewExceptionTest.main(NewExceptionTest.java:4)
```

- 사용자 정의 예외 클래스 SpaceException과 MemoryException을 만들어서 사용했다.
    - SpaceException은 프로그램을 설치하려는 곳에 충분한 공간이 없을 경우에 발생
    - MemoryException은 설치작업을 수행하는데 메모리가 충분히 확보되지 않았을 경우에 발생
    - 이 두 예외는 startInstall()을 수행하는 동안에 발생할 수 있으며, enoughSpace()와 enoughMemory()의 실행결과에 따라서 발생하는 예외의 종류가 달라지도록 했다.

## 예외 되던지기(exception re-throwing)
- 한 메서드에서 발생할 수 있는 예외가 여럿일 경우, 몇 개는 try-catch문을 통해서 메서드 내에서 자체적으로 처리하고, 그 나머지는 선언부에 지정하여 호출한 메서드에서 처리하도록 함으로써, 양쪽으로 나눠서 처리되도록 할 수 있다.
- 그리고 심지어는 **단 하나의 예외에 대해서도 예외가 발생한 메서드와 호출한 메서드, 양쪽에서 처리하도록 할 수 있다.**
- 이것은 예외를 처리한 후에 **인위적으로 다시 발생시키는 방법**을 통해서 가능한데, 이것을 `예외 되던지기(exception re-throwing)`라고 한다.
- 먼저 예외가 발생할 가능성이 있는 메서드에서 `try-catch문`을 사용해서 예외를 처리해주고 `catch문`에서 필요한 작업을 행한 후에 `throw문`을 사용해서 예외를 다시 발생시킨다.
- 다시 발생한 예외는 이 메서드를 `호출한 메서드`에게 전달되고 호출한 메서드의 `try-catch문`에서 예외를 또 다시 처리한다.
- 이 방법은 **하나의 예외에 대해서 예외가 발생한 메서드와 이를 호출한 메서드 양쪽 모두에서 처리해줘야 할 작업이 있을 때** 사용된다.
- 이 때 주의할 점은 예외가 발생한 메서드에서는 `try-catch문`을 사용해서 예외처리를 해줌과 동시에 `메서드의 선언부`에 발생한 예외를 `throws`에 지정해줘야 한다는 것이다.

```ExceptionEx17.java
class ExceptionEx17 {
    public static void main(String[] args) {
        try {
            method1();
        } catch (Exception e) {
            System.out.println("main메서드에서 예외가 처리되었습니다.");
        }
    }

    static void method1() throws Exception {
        try {
            throw new Exception();
        } catch (Exception e) {
            System.out.println("method1메서드에서 예외가 처리되었습니다.");
            throw e; // 다시 예외를 발생시킨다.
        }
    }
}
```

```실행결과
method1메서드에서 예외가 처리되었습니다.
main메서드에서 예외가 처리되었습니다.
```

- `method1()`과 `main메서드` 양쪽의 catch블럭이 모두 수행되었음을 알 수 있다.
- `method1()`의 catch블럭에서 예외를 처리하고도 `throw문`을 통해 다시 예외를 발생시켰다. 그리고 이 예외를 `main메서드`에서 한 번 더 처리하였다.

```return
static int method1() {
    try {
        System.out.println("method1()이 호출되었습니다.);
        return 0; // 현재 실행 중인 메서드를 종료한다.
    } catch (Exception e) {
        e.printStackTrace();
        return 1; // catch블럭 내에서도 return문이 필요하다.
    } finally {
        System.out.println("method1()의 finally블럭이 실행되었습니다.");
    }
}
```

- **반환값이 있는 return문의 경우, catch블럭에도 return문이 있어야 한다. 예외가 발생했을 경우에도 값을 반환해야하기 때문이다.**

```return
static int method1() throws Exception { // 예외를 선언해야 함
    try {
        System.out.println("method1()이 호출되었습니다.);
        return 0; // 현재 실행 중인 메서드를 종료한다.
    } catch (Exception e) {
        e.printStackTrace();
        // return 1; // catch블럭 내에서도 return문이 필요하다.
        throw new Exception(); // return문 대신 예외를 호출한 메서드로 전달
    } finally {
        System.out.println("method1()의 finally블럭이 실행되었습니다.");
    }
}
```

- 또는 **catch블럭에서 예외 되던지기를 해서 호출한 메서드로 예외를 전달하면, return문이 없어도 된다.** 그래서 검증에서도 assert문 대신 AssertError를 생성해서 던진다.
- finally블럭 내에서도 return문을 사용할 수 있으며, try블럭이나 catch블럭의 return문 다음에 수행된다. **최종적으로 finally블럭 내의 return문이 반환된다.**

## 연결된 예외(chained exception)
- 한 에러가 다른 예외를 발생시킬 수도 있다. 예를 들어 예외 A가 예외 B를 발생시켰다면, A를 B의 `원인 예외(cause exception)`라고 한다.

```chained-exception
try {
    startInstall(); // SpaceException 발생
    copyFiles();
} catch (SpaceException e) {
    InstallException ie = new InstallException("설치중 예외발생"); // 예외 생성
    ie.initCause(e); // InstallException의 원인 예외를 SpaceException으로 지정
    throw ie; // InstallException을 발생시킨다.
} catch (MemoryException me) {
    ...
}
```

- SpaceException을 `원인 예외`로 하는 InstallException을 발생시키는 방법
- 먼저 InstallException을 생성한 후, `initCause()`로 SpaceException을 InstallException의 `원인 예외`로 등록한다. 그리고 `throw`로 이 예외를 던진다.
- `initCause()`는 Exception클래스의 조상인 `Throwable클래스`에 정의되어 있기 때문에 모든 예외에서 사용가능하다.
    - Throwable initCause(Throwable cause) // 지정한 예외를 원인 예외로 등록
    - Throwable getCause() // 원인 예외를 반환
- 여러 가지 예외를 `하나의 큰 분류의 예외`로 묶어서 다루기 위해 원인 예외로 등록해서 다시 예외를 발생시킨다.

```try-catch
try {
    startInstall(); // SpaceException발생
    copyFiles();
} catch (InstallException e) { // InstallException은
    e.printStackTrace(); // SpaceException과 MemoryException의 조상
}
```

- InstallException을 SpaceException과 MemoryException의 `조상`으로 해서 catch블럭을 작성하면, **실제로 발생한 예외가 어떤 것인지 알 수 없다는 문제가 생긴다.**
- 그리고 SpaceException과 MemoryException의 `상속관계`를 변경해야 한다는 것도 부담이다.
- 그래서 생각한 것이 예외가 `원인 예외`를 포함할 수 있게 한 것이다. 이렇게 하면, 두 예외는 상속관계가 아니어도 상관없다.

```chained-exception
public class Throwable implements Serializable {
    ...
    private Throwable cause = this; // 객체 자신(this)을 원인 예외로 등록
}
```

- 또 다른 이유는 `checked예외`를 `unchecked예외`로 바꿀 수 있도록 하기 위해서이다.
- `checked예외`를 `unchecked예외`로 바꾸면 예외처리가 `선택적`이 되므로 억지로 예외처리를 하지 않아도 된다.

```checked-exception
static void startInstall() throws SpaceException, MemoryException {
    if(!enoughSpace()) // 충분한 설치 공간이 없을 경우
        throw new SpaceException("설치할 공간이 부족합니다.");
    if(!enoughMemory()) // 충분한 메모리가 없을 경우
        throw new MemoryException("메모리가 부족합니다.");
}
```

```unchecked-exception
static void startInstall() throws SpaceException, MemoryException {
    if(!enoughSpace()) // 충분한 설치 공간이 없을 경우
        throw new SpaceException("설치할 공간이 부족합니다.");
    if(!enoughMemory()) // 충분한 메모리가 없을 경우
        throw new RuntimeException(new MemoryException("메모리가 부족합니다."));
}
```

- MemoryException은 `Exception`의 자손이므로 반드시 예외를 처리해야 하는데, 이 예외를 `RuntimeException`으로 감싸버렸기 때문에 `unchecked예외`가 되었다.
- 그래서 더 이상 startInstall()의 선언부에 MemoryException을 선언하지 않아도 된다.
- 참고로 위의 코드에서는 `initCause()`대신 `RuntimeException의 생성자`를 사용했다.

```RuntimeException
RuntimeException(Throwable cause) // 원인 예외로 등록하는 생성자
```

```ChainedExceptionEx.java
class ChainedExceptionEx {
    public static void main(String[] args) {
        try {
            install();
        } catch (InstallException e) {
            e.printStackTrace();
        } catch (Exception e) {
            e.printStackTrace();
        }
    }

    static void install() throws InstallException {
        try {
            startInstall(); // 프로그램 설치에 필요한 준비를 한다.
            copyFiles(); // 파일들을 복사한다.
        } catch (SpaceException se) {
            InstallException ie = new InstallException("설치 중 예외발생");
            ie.initCause(se);
            throw ie;
        } catch (MemoryException me) {
            InstallException ie = new InstallException("설치 중 예외발생");
            ie.initCause(me);
            throw ie;
        } finally {
            deleteTempFiles(); // 프로그램 설치에 사용된 임시파일들을 삭제한다.
        }
    }

    static void startInstall() throws SpaceException, MemoryException {
        if(!enoughSpace()) // 충분한 설치 공간이 없을 경우
            throw new SpaceException("설치할 공간이 부족합니다.");
        if(!enoughMemory()) // 충분한 메모리가 없을 경우
            throw new MemoryException("메모리가 부족합니다.");
//            throw new RuntimeException(new MemoryException("메모리가 부족합니다."));
    }

    static void copyFiles() { /* 파일들을 복사하는 코드를 적는다. */ }
    static void deleteTempFiles() { /* 임시파일들을 삭제하는 코드를 적는다. */ }

    static boolean enoughSpace() {
        // 설치하는데 필요한 공간이 있는지 확인하는 코드를 적는다.
        return false;
    }

    static boolean enoughMemory() {
        // 설치하는데 필요한 메모리공간이 있는지 확인하는 코드를 적는다.
        return true;
    }
}

class InstallException extends Exception {
    InstallException(String msg) {
        super(msg);
    }
}

class SpaceException extends Exception {
    SpaceException(String msg) {
        super(msg);
    }
}

class MemoryException extends Exception {
    MemoryException(String msg) {
        super(msg);
    }
}
```

```실행결과
InstallException: 설치 중 예외발생
	at ChainedExceptionEx.install(ChainedExceptionEx.java:17)
	at ChainedExceptionEx.main(ChainedExceptionEx.java:4)
Caused by: SpaceException: 설치할 공간이 부족합니다.
	at ChainedExceptionEx.startInstall(ChainedExceptionEx.java:31)
	at ChainedExceptionEx.install(ChainedExceptionEx.java:14)
	... 1 more
```

- `원인 예외`로 등록한 SpaceException이 `Caused by`라는 머리말과 함께 출력되었다.

## 🚩 잊지않기
- 자동 자원 반환 - try-with-resources문
    - 주로 입출력에 사용되는 클래스 중에서는 **사용한 후에 꼭 닫아줘야 하는 것들이 있다.** 그래서 사용했던 자원(resources)이 반환되기 때문이다.
    - try-with-resources문의 `괄호()` 안에 객체를 생성하는 문장을 넣으면, **이 객체는 close()를 호출하지 않아도 try블럭을 벗어나는 순간 자동적으로 close()가 호출된다.**
    - *try블럭의 괄호() 안에 변수를 선언하는 것도 가능하며, 선언된 변수는 try블럭 내에서만 사용할 수 있다.*

    ```try-with-resources
    // 괄호() 안에 두 문장 이상 넣을 경우 ';'로 구분한다.
    try (FileInputStream fis = new FileInputStream("score.dat");
        DataInputStream dis = new DataInputStream(fis)) {
        ...
    }
    ```

    - 이처럼 try-with-resources문에 의해 자동으로 객체의 close()가 호출될 수 있으려면, 클래스가 `AutoCloseable`이라는 인터페이스를 구현한 것이어야만 한다.

    ```AutoCloseable
    public interface AutoCloseable {
        void close() throws Exception
    }
    ```

- 억제된 예외
    - 두 예외는 동시에 발생할 수 없다.
    - `억제된(Suppressed)`이라는 의미와 머리말과 함께 출력된다.
    - 두 예외가 동시에 발생할 수 없기 때문에, 두 번째로 발생한 예외는 억제된 예외로 다룬다.
    - 억제된 예외에 대한 정보는 실제로 발생한 예외에 저장된다.
    - `Throwable`에는 억제된 예외와 관련된 다음과 같은 메서드가 정의되어 있다.
        - void addSuppressed(Throwable exception) // 억제된 예외를 추가
        - Throwable[] getSuppressed() // 억제된 예외(배열)를 반환
    - 만일 try-with-resources문이 아닌 try-catch문을 사용했다면, 먼저 발생한 예외는 무시되고 마지막으로 발생한 예외에 대한 내용만 출력되었을 것이다.

- 사용자 정의 예외 만들기
    - 기존의 정의된 예외 클래스 외에 필요에 따라 프로그래머가 새로운 예외 클래스를 정의하여 사용할 수 있다.
    - 보통 `Exception클래스` 또는 `RuntimeException클래스`로부터 상속받아 클래스를 만들지만, 필요에 따라서 알맞은 예외 클래스를 선택할 수 있다.

    ```custom-exception
    class MyException extends Exception {
    MyException(String msg) { // 문자열을 매개변수로 받는 생성자
        super(msg); // 조상인 Exception클래스의 생성자를 호출한다.
    }
    }
    ```
    - Exception클래스는 생성 시에 String 값을 받아서 메시지로 저장할 수 있다.
    - 필요하다면, 멤버변수나 메서드를 추가할 수 있다.

- 예외 되던지기(exception re-throwing)
    - 한 메서드에서 발생할 수 있는 예외가 여럿일 경우, 몇 개는 try-catch문을 통해서 메서드 내에서 자체적으로 처리하고, 그 나머지는 선언부에 지정하여 호출한 메서드에서 처리하도록 함으로써, **양쪽으로 나눠서 처리되도록 할 수 있다.**
    - 그리고 심지어는 **단 하나의 예외에 대해서도 예외가 발생한 메서드와 호출한 메서드, 양쪽에서 처리하도록 할 수 있다.**
    - 예외를 처리한 후에 **인위적으로 다시 발생시키는 방법**을 통해서 가능한데, 이것을 `예외 되던지기(exception re-throwing)`라고 한다.
    - 먼저 예외가 발생할 가능성이 있는 메서드에서 `try-catch문`을 사용해서 예외를 처리해주고 catch문에서 필요한 작업을 행한 후에 `throw문`을 사용해서 예외를 다시 발생시킨다.
    - 다시 발생한 예외는 이 메서드를 `호출한 메서드`에게 전달되고 호출한 메서드의 `try-catch문`에서 예외를 또 다시 처리한다.
    - **이 때 주의할 점은 예외가 발생한 메서드에서는 try-catch문을 사용해서 예외처리를 해줌과 동시에 메서드의 선언부에 발생한 예외를 throws에 지정해줘야 한다는 것이다.**
    - **반환값이 있는 return문의 경우, catch블럭에도 return문이 있어야 한다. 예외가 발생했을 경우에도 값을 반환해야하기 때문이다.**
    - 또는 **catch블럭에서 예외 되던지기를 해서 호출한 메서드로 예외를 전달하면, return문이 없어도 된다.** 그래서 검증에서도 assert문 대신 AssertError를 생성해서 던진다.
    - finally블럭 내에서도 return문을 사용할 수 있으며, try블럭이나 catch블럭의 return문 다음에 수행된다. **최종적으로 finally블럭 내의 return문이 반환된다.**
- 연결된 예외(chained exception)
    - 한 에러가 다른 예외를 발생시킬 수도 있다. 예를 들어 예외 A가 예외 B를 발생시켰다면, A를 B의 `원인 예외(cause exception)`라고 한다.
    - B를 생성한 후, `initCause()`로 A를 B의 `원인 에외`로 등록한다. 그리고 `throw`로 이 예외를 던진다.
    - `initCause()`는 Exception클래스의 조상인 `Throwable클래스`에 정의되어 있기 때문에 모든 예외에서 사용가능하다.
        - Throwable initCause(Throwable cause) // 지정한 예외를 원인 예외로 등록
        - Throwable getCause() // 원인 예외를 반환
    - 여러 가지 예외를 `하나의 큰 분류의 예외`로 묶어서 다루기 위해 원인 예외로 등록해서 다시 예외를 발생시킨다.
    - 이를 이용하면 checked예외를 unchecked예외로 바꿀 수 있다.
    - 원인 예외로 등록한 Exception은 `Caused by`라는 머리말과 함께 출력된다.