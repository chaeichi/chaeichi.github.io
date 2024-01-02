---
layout: post
title: "Chapter01. 자바를 시작하기 전에"
subtitle: Java의 정석 3rd Edition | 남궁성
excerpt_image: /assets/images/excerpt-images/2024-01-02-getting-started-with-java.png
categories: 
    - Java
tags: 
    - Java
    - Java의 정석
---
![banner](/assets/images/excerpt-images/2024-01-02-getting-started-with-java.png)

## 자바(Java)
### 자바란?
- `자바(Java)`는 `썬 마이크로시스템즈(Sun Microsystems, Inc. 이하 썬)`의 `제임스 고슬링(James Gosling)`과 다른 연구원들이 개발하여 1995년 5월에 발표, 1996년 1월에 배포한 `객체지향 프로그래밍 언어`이다.

### 자바의 역사
- 1991년 썬의 엔니지어들에 의해서 고안된 `오크(Oak)`라는 언어에서부터 시작
- 원래 목표는 가전제품에 탑재될 소프트웨어를 만드는 것
- C/C++ 스타일의 언어와 가상 머신을 구현하는 것을 목표
- 여러 종류의 운영체제를 사용하는 컴퓨터들이 통신하는 인터넷이 등장하자 `운영체제에 독립적으로` 실행이 가능하도록 개발
- `한 번 쓰고 어느 곳에도 실행 "Write Once, Run Anywhere"`
- 사운드와 애니메이션 등의 멀티미디어적인 요소를 제공할 수 있는 `자바 애플릿(Java Applet)`이 당시 큰 인기를 얻음
- 서버쪽 프로그래밍을 위한 `서블릿(Servleet)`, `JSP(Java Server Pages)`

### 자바언어의 특징
#### 1. 운영체제에 독립적
- 기존의 언어는 한 운영체제에 맞게 개발된 프로그램을 다른 종류의 운영체제에 적용하기 위해서는 많은 노력이 필요
- 자바로 작성된 프로그램은 운영체제나 하드웨어가 아닌 `자바 가상 머신(Java Virtual Machine, JVM)`을 통해서만 통신
- JVM이 자바 응용프로그램으로부터 전달받은 명령을 해당 운영체제가 이해할 수 있도록 변환하여 전달하기 때문에 **운영체제에 독립적**

#### 2. 객체 지향 프로그래밍 언어(Object-Oriented Programming, OOP)
- 객체 지향 프로그래밍(Object-Oriented Programming, OOP) : 컴퓨터 프로그램을 명령어의 모음으로 보는 시각에서 벗어나 여러 개의 독립된 단위, 즉 `객체(object)`들의 모임으로 파악하고자 하는 것
- 자바는 객체 지향 프로그래밍 언어로 OOP의 4가지 특징인 `추상화`, `캡슐화`, `상속`, `다형성`이 잘 적용된 언어라는 평가를 받고 있음

#### 3. 비교적 배우기 쉬움
- 자바의 연산자와 기본 구문은 C++에서, 객체 지향 관련 구문은 스몰토크(Smalltalk)에서 가져옴
- 이들의 장점은 취하면서 복잡하고 불필요한 부분은 과감히 제거하여 단순화
- 간결하면서도 명료한 객체지향적 설계로 객체 지향 개념을 보다 쉽게 이해하고 활용할 수 있도록 함

#### 4. 자동 메모리 관리(Garbage Collection)
- `JVM`의 `가비지 컬렉터(Garbage Collector, GC)`가 **동적으로 할당한 메모리 영역 중 더 이상 쓰이지 않는 영역을 자동으로 찾아내어 해제**하기 때문에 프로그래머가 메모리를 따로 관리하지 않고 보다 프로그래밍에 집중할 수 있도록 도와준다.

#### 5. 네트워크와 분산처리 지원
- 풍부하고 다양한 네트워크 프로그래밍 라이브러리를 통해 비교적 짧은 시간에 네트워크 관련 프로그램을 쉽게 개발할 수 있도록 지원한다.

#### 6. 멀티 스레드(Multi Thread)
- **하나의 프로세스 내에서 둘 이상의 스레드가 동시에 작업을 수행**하는 `멀티 스레드(Multi Thread)`를 지원한다.
- 일반적으로 멀티 스레드의 지원은 사용되는 운영체제에 따라 구현방법과 처리방식이 상이
- 자바에서 개발되는 멀티 스레드 프로그램은 시스템과는 관계없이 구현이 가능
- 여러 스레드에 의한 `스케줄링(scheduling)`은 **자바 인터프리터**가 담당

#### 7. 동적 로딩(Dynamic Loading) 지원
- 보통 자바로 작성된 애플리케이션은 여러 개의 클래스로 구성되어 있음
- 자바는 `동적 로딩(Dynamic Loading)`을 지원하기 때문에 **실행 시 모든 클래스가 로딩되지 않고 필요한 시점에 클래스를 로딩하여 사용할 수 있음**
- 일부 클래스가 변경되어도 전체 애플리케이션을 다시 컴파일하지 않아도 됨

### JVM(Java Virtual Machine)
- JVM(Java Virtual Machine) : 자바를 실행하기 위한 가상 기계(컴퓨터)
    - 가상 기계(virtual machine) : 소프트웨어로 구현된 하드웨어를 뜻하는 넓은 의미의 용어
    - 가상 컴퓨터(virtual computer) : 실제 컴퓨터(하드웨어)가 아닌 소프트웨어로 구현된 컴퓨터 속의 컴퓨터
- 자바로 작성된 애플리케이션은 모두 JVM에서만 실행되기 때문에 자바 애플리케이션이 실행되기 위해서는 반드시 JVM이 필요

![그림1-1 Java애플리케이션과 일반 애플리케이션의 비교](/assets/images/javajungsuk3/1-1.png)
- **일반 애플리케이션**
    - 운영체제에 종속적
    - 다른 운영체제에서 실행시키기 위해서는 애플리케이션을 그 운영체제에 맞게 변경해야 함

- **Java 애플리케이션**
    - JVM하고만 상호작용 하기 때문에 운영체제와 하드웨어에는 독립적
    - 다른 운영체제에서도 프로그램의 변경없이 실행 가능
    *(단, JVM은 운영체제에 종속적이기 때문에 해당 운영체제에서 실행 가능한 JVM이 필요)*
    - 하드웨어에 맞게 완전히 컴파일된 상태가 아닌 실행 시에 해석(interpret)되기 때문에 속도가 느림
    - JVM이 `바이트코드(bytecode)`를 해당 운영체제의 기계어로 변환하여 운영체제로 전달
        - 바이트코드 : JVM이 이해할 수 있는 기계어
    - 바이트코드를 하드웨어의 기계어로 바로 변환해주는 `JIT컴파일러`로 속도의 격차가 많이 줄어듬

## 자바 개발 환경
### JDK와 JRE
- `JDK(Java Development Kit)` : 자바 개발 도구
- `JRE(Java Runtime Environment)` : 자바 실행 환경
- `JDK` = `JRE` + `개발에 필요한 실행파일(javac.exe 등)`
- `JRE` = `JVM` + `클래스 라이브러리(Java API)`

### JDK의 bin 디렉토리에 있는 주요 실행파일
- javac.exe : `자바 컴파일러`, 자바 소스 코드를 바이트코드로 컴파일
    - javac Hello.java
- java.exe : `자바 인터프리터`, 컴파일러가 생성한 바이트코드를 해석하고 실행
    - java Hello
- javap.exe : `역어셈블러`, 컴파일된 클래스파일을 원래의 소스로 변환
    - javap Hello > Hello.java
- javadoc.exe : `자동 문서 생성기`, 소스파일에 있는 주석을 이용하여 Java API 문서와 같은 형식의 문서를 자동으로 생성
    - javadoc Hello.java
- jar.exe : `압축프로그램`, 클래스 파일과 프로그램의 실행에 관련된 파일을 하나의 `jar파일(.jar)`로 압축하거나 압축해제한다.
    - 압축할 때 : jar cvf Hello.jar Hello1.class Hello2.class
    - 압축풀 때 : jar xvf Hello.jar

## 자바 프로그램 작성
```Hello.java
class Hello {
    public static void main(String[] args) {
        System.out.println("Hello, world."); // 화면에 글자를 출력한다.
    }
}
```
```출력결과
Hello, world.
```

### 클래스
- 자바에서 모든 코드는 반드시 클래스 안에 존재해야 함
- 서로 관련된 코드들은 그룹으로 나누어 별도의 클래스를 구성
- 클래스들이 모여 하나의 Java 애플리케이션을 이룬다.
- 키워드 `class` 다음에 클래스의 이름을 적고, 클래스의 시작과 끝을 의미하는 `괄호{}` 안에 원하는 코드를 넣는다.
```class
class 클래스이름 {
    /*
        주석을 제외한 모든 코드는 클래스의 블럭{} 내에 작성해야한다.
    */
}
```

### main 메서드
- `public static void main(String[] args)`
- 프로그램을 실행할 때 `java.exe`에 의해 호출될 수 있도록 미리 약속된 부분
- `[]`은 `배열`을 의미하는 기호로 `배열의 타입(type)` 또는 `배열의 이름`을 옆에 붙일 수 있음
- `String[] args`는 `String` 타입의 배열 `args`를 선언한 것
```main
class 클래스이름 {
    public static void main(String[] args) { // main 메서드의 선언부
        // 실행될 문장들을 적는다.
    }
}
```
- main 메서드의 선언부 다음에 나오는 괄호 `{}`는 메서드의 `시작`과 `끝`을 의미
    - 이 괄호 사이에 작업할 내용을 작성해 넣으면 된다.
- Java 애플리케이션은 **main 메서드의 호출**로 시작해서 **main 메서드의 첫 문장부터 마지막 문장까지 수행을 마치면 종료**
- **하나의 Java 애플리케이션에는 main 메서드를 포함한 클래스가 반드시 하나는 있어야 함**
    - main 메서드는 Java 애플리케이션의 시작점이므로 main 메서드 없이는 Java 애플리케이션이 실행될 수 없음
    - 모든 클래스가 main 메서드를 가지고 있어야 하는 것은 아님

### 하나의 소스파일에 둘 이상의 클래스 정의
- 하나의 소스파일에 하나의 클래스만을 정의하는 것이 보통이지만, 하나의 소스파일에 둘 이상의 클래스를 정의하는 것도 가능
- 소스파일의 이름은 `public class`의 이름과 일치해야 함
- public class가 없다면 소스파일 내의 어떤 클래스의 이름으로 해도 상관없음
- 소스파일(*.java)과 달리 `클래스파일(*.class)`은 **클래스마다 하나씩** 만들어짐

#### 올바른 작성 예
Hello2.java
```Hello2.java
public class Hello2 {}
class Hello3 {}
```
- 소스파일의 이름과 public class의 이름이 일치

Hello2.java
```Hello2.java
class Hello2 {}
class Hello3 {}
```
- public class가 없다면 소스파일 내의 어떤 클래스의 이름으로 해도 상관없음

#### 잘못된 작성 예

Hello2.java
```Hello2.java
public class Hello2 {}
public class Hello3 {}
```
- 하나의 소스파일에 둘 이상의 public class가 존재해서는 안 됨

Hello3.java
```Hello3.java
public class Hello2 {}
class Hello3 {}
```
- 소스파일의 이름과 public class의 이름이 일치하지 않음

hello2.java
```hello2.java
public class Hello2 {}
class Hello3 {}
```
- 대소문자를 구분하기 때문에 소스파일의 이름에서 'h'를 'H'로 바꿔야 함

### 자바 프로그램의 실행과정
1. 프로그램의 실행에 필요한 `클래스파일(*.class)` 로드
2. 클래스파일 검사(파일형식, 악성코드 체크)
3. 지정된 클래스에서 `main(String[] args)` 호출

### 주석(comment)
- 프로그래밍에 있어 내용을 메모하는 목적으로 쓰임
- 소스 코드를 더 쉽게 이해할 수 있게 만드는 것이 주 목적
- 프로그램의 작성자, 작성일시, 버전과 그에 따른 변경이력 등의 정보 제공 목적
- 컴파일러는 주석을 무시하고 건너뛰기 때문에 프로그램의 실행에 영향을 주지 않음

#### 주석의 종류
- 범위 주석 : `/*`와  `/*` 사이의 내용을 주석으로 간주
- 한 줄 주석 : `//`부터 라인 끝까지의 내용을 주석으로 간주

#### 올바른 작성 예
```comment
/*
Date : 2024. 01. 02
Source : Hello.java
*/

class Hello {
    public static void main(String[] args) { /* 프로그램의 시작 */
        System.out.println("Hello, world."); // Hello, wowrld를 출력한다.
    }
}
```

#### 잘못된 작성 예
`큰 따옴표(")` 안에 주석이 있을 때는 주석이 아닌 `문자열`로 인식
```comment
class Hello {
    public static void main(String[] args) {
        System.out.println("Hello, /* 이것은 주석 아님 */world.");
        System.out.println("Hello, world. // 이것도 주석 아님");
    }
}
```