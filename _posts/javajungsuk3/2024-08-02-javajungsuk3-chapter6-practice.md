---
layout: post
title: "Java의 정석 연습문제 - Chapter06. 객체지향 프로그래밍 I"
subtitle: Java의 정석 3rd Edition | 남궁성
excerpt_image: /assets/images/excerpt-images/javajungsuk3/2024-08-02-javajungsuk3-chapter6-practice.png
categories: 
    - 문제풀이
tags: 
    - Java
    - Java의 정석
    - 연습문제
    - 문제풀이
    - 두 점 사이의 거리 구하기
    - Math.sqrt()
    - Math.pow()
---
![banner](/assets/images/excerpt-images/javajungsuk3/2024-08-02-javajungsuk3-chapter6-practice.png)

**[6-1]** 다음과 같은 멤버변수를 갖는 `SutdaCard클래스`를 정의하시오.<br/>

|타입|변수명|설명|
|-|-|-|
|int|num|카드의 숫자(1 ~ 10사이의 정수)|
|boolean|isKwang|광(光)이면 true, 아니면 false|

```SutdaCard.java
class SutdaCard {
    int num; // 카드의 숫자(1 ~ 10사이의 정수)
    boolean isKwang; // 광(光)이면 true, 아니면 false
}
```

**[6-2]** 문제 6-1에서 정의한 SutdaCard클래스에 `두 개의 생성자`와 `info()`를 추가해서 실행결과와 같은 결과를 얻도록 하시오.<br/>

```Exercise6_2.java
class Exercise6_2 {
    public static void main(String[] args) {
        SutdaCard card1 = new SutdaCard(3, false);
        SutdaCard card2 = new SutdaCard();

        System.out.println(card1.info());
        System.out.println(card2.info());
    }
}

class SutdaCard {
    /*
        (1) 알맞은 코드를 넣어 완성하시오.
    */
}
```

```실행결과
3
1K
```

**[정답]**

```SutdaCard.java
class SutdaCard {
    int num; // 카드의 숫자(1 ~ 10사이의 정수)
    boolean isKwang; // 광(光)이면 true, 아니면 false

    SutdaCard() {
        this(1, true);
    }

    SutdaCard(int num, boolean isKwang) {
        this.num = num;
        this.isKwang = isKwang;
    }

    String info() {
        return num + (isKwang ? "K" : "");
    }
}
```

**[해설]**

```Exercise6_2.java
SutdaCard card1 = new SutdaCard(3, false);
SutdaCard card2 = new SutdaCard();
```

`num`과 `isKwang`을 매개변수로 받는 생성자와 매개변수가 없는 기본 생성자 `두 개의 생성자`가 존재한다는 것을 알 수 있음

```실행결과
3 // num = 3, isKwang = false
1K
```

`num = 3, isKwang = false`인 card1의 출력결과가 `3`, card2의 출력결과가 `1K`인 것으로 기본 생성자가 값을 `num = 1, isKwang = true`로 지정해줬다는 것을 유추할 수 있음

**[6-3]** 다음과 같은 멤버변수를 갖는 `Student클래스`를 정의하시오.<br/>

|타입|변수명|설명|
|-|-|-|
|String|name|학생이름|
|int|ban|반|
|int|no|번호|
|int|kor|국어점수|
|int|eng|영어점수|
|int|math|수학점수|

```Student.java
class Student {
    String name; // 학생이름
    int ban; // 반
    int no; // 번호
    int kor; // 국어점수
    int eng; // 영어점수
    int math; // 수학점수
}
```

**[6-4]** 문제 6-3에서 정의한 Student클래스에 다음과 같이 정의된 두 개의 메서드 `getTotal()`과 `getAverage()`를 추가하시오.<br/>

(1) 메서드명 : getTotal<br/>
기능 : 국어(kor), 영어(eng), 수학(math)의 점수를 모두 더해서 반환한다.<br/>
반환타입 : int<br/>
매개변수 : 없음<br/>

(2) 메서드명 : getAverage<br/>
기능 : 총점(국어점수 + 영어점수 + 수학점수)을 과목수로 나눈 평균을 구한다. 소수점 둘째자리에서 반올림할 것<br/>
반환타입 : float<br/>
매개변수 : 없음<br/>

```Exercise6_4.java
class Exercise6_4 {
    public static void main(String[] args) {
        Student s = new Student();
        s.name = "홍길동";
        s.ban = 1;
        s.no = 1;
        s.kor = 100;
        s.eng = 60;
        s.math = 76;
        
        System.out.println("이름 : " + s.name);
        System.out.println("총점 : " + s.getTotal());
        System.out.println("평균 : " + s.getAverage());
    }
}

class Student {
    /*
        (1) 알맞은 코드를 넣어 완성하시오.
    */
}
```

```실행결과
이름 : 홍길동
총점 : 236
평균 : 78.7
```

**[정답]**

```Student.java
class Student {
    String name; // 학생이름
    int ban; // 반
    int no; // 번호
    int kor; // 국어점수
    int eng; // 영어점수
    int math; // 수학점수

    int getTotal() {
        return kor + eng + math;
    }

    float getAverage() {
        return Math.round((getTotal() / 3.0f) * 10) / 10.0f;
    }
}
```

**[해설]**

```getAverage()
getTotal() / 3.0f; // 78.666664
(getTotal() / 3.0f) * 10 ; // 786.6666
Math.round((getTotal() / 3.0f) * 10); // 787.0
Math.round((getTotal() / 3.0f) * 10) / 10.0f; // 78.7
```

**[6-5]** 다음과 같은 실행결과를 얻도록 Student클래스에 `생성자`와 `info()`를 추가하시오.

```Exercise6_5.java
class Exercise6_5 {
    public static void main(String[] args) {
        Student s= new Student("홍길동", 1, 1, 100, 60, 76);
        System.out.println(s.info());
    }
}

class Student {
    /*
        (1) 알맞은 코드를 넣어 완성하시오.
    */
}
```

```실행결과
홍길동, 1, 1, 100, 60, 76, 236, 78.7
```

**[정답]**

```Student.java
class Student {
    String name; // 학생이름
    int ban; // 반
    int no; // 번호
    int kor; // 국어점수
    int eng; // 영어점수
    int math; // 수학점수

    Student(String name, int ban, int no, int kor, int eng, int math) {
        this.name = name;
        this.ban = ban;
        this.no = no;
        this.kor = kor;
        this.eng = eng;
        this.math = math;
    }

    String info() {
        return name
                + ", " + ban
                + ", " + no
                + ", " + kor
                + ", " + eng
                + ", " + math
                + ", " + getTotal()
                + ", " + getAverage();
    }

    int getTotal() {
        return kor + eng + math;
    }

    float getAverage() {
        return Math.round((getTotal() / 3.0f) * 10) / 10.0f;
    }
}
```

**[6-6]** `두 점의 거리를 계산`하는 `getDistance()`를 완성하시오.<br/>
**[Hint]** 제곱근 계산은 `Math.sqrt(double a)`를 사용하면 된다.

```Exercise6_6.java
class Exercise6_6 {
    // 두 점 (x,y)와 (x1,y1)간의 거리를 구한다.
    static double getDistance(int x, int y, int x1, int y1) {
        /*
            (1) 알맞은 코드를 넣어 완성하시오.
        */
    }

    public static void main(String[] args) {
        System.out.println(getDistance(1, 1, 2, 2));
    }
}
```

```실행결과
1.4142135623730951
```

**[정답]**

```Exercise6_6.java
return Math.sqrt(Math.pow((x1 - x), 2) + Math.pow((y1 - y), 2));
```

**[해설]** 두 점 사이의 거리 공식<br/>

![Exercise6_6](/assets/images/javajungsuk3/Exercise6_6.png)

제곱근 계산 `Math.sqrt(double a)`<br/>
제곱 `Math.pow(double a, double b)`<br/>

2제곱이므로 메서드를 호출하지 않고 곱셈연산을 사용할 수도 있다. *(메서드를 호출하는 것은 곱셈연산보다 비용이 많이 드는 작업이다.)*

```Exercise6_6.java
return Math.sqrt((x1 - x) * (x1 - x) + (y1 - y) * (y1 - y));
```

**[6-7]** 문제 6-6에서 작성한 클래스메서드 `getDistance()`를 MyPoint클래스의 `인스턴스메서드`로 정의하시오.

```Exercise6_7.java
class MyPoint {
    int x;
    int y;

    MyPoint(int x, int y) {
        this.x = x;
        this.y = y;
    }

    /*
        (1) 인스턴스메서드 getDistance를 작성하시오.
     */
}

class Exercise6_7 {
    public static void main(String[] args) {
        MyPoint p = new MyPoint(1,1);

        // p와 (2,2)의 거리를 구한다.
        System.out.println(p.getDistance(2,2));
    }
}
```

```실행결과
1.4142135623730951
```

**[정답]**

```Exercise6_7.java
double getDistance(int x1, int y1) {
    return Math.sqrt((x1 - x) * (x1 - x) + (y1 - y) * (y1 - y));
}
```

**[6-8]** 다음의 코드에 정의된 변수들을 종류별로 구분해서 적으시오.

```PlayingCard
class PlayingCard {
    int kind;
    int num;

    static int width;
    static int height;

    PlayingCard(int k, int n) {
        kind = k;
        num = n;
    }

    public static void main(String[] args) {
        PlayingCard card = new PlayingCard(1,1);
    }
}
```

- **클래스변수(static 변수)** : width, height<br/>
- **인스턴스변수** : kind, num<br/>
- **지역변수** : k, n, args, card<br/>

**[6-9]** 다음은 컴퓨터 게임의 `병사(marine)`를 클래스로 정의한 것이다. 이 클래스의 멤버 중에 `static`을 붙여야 하는 것은 어떤 것들이고 그 이유는 무엇인가? `(단, 모든 병사의 공격력과 방어력은 같아야 한다.)`

```Marine
class Marine {
    int x = 0; y = 0; // Marine의 위치 좌표(x, y)
    int hp = 60; // 현재 체력
    int weapon = 6; // 공격력
    int armor = 0; // 방어력

    void weaponUp() {
        weapon++;
    }

    void armorUp() {
        armor++;
    }

    void move(int x, int y) {
        this.x = x;
        this.y = y;
    }
}
```

**[정답]** weapon, armor, weaponUp(), armorUp()<br/>
**[해설]** 인스턴스가 공통적으로 가져야 하는 값은 클래스변수로 정의해야한다. 모든 병사의 공격력과 방어력이 같아야 하므로 `weapon`과 `armor`는 클래스변수가 되어야한다. `weaponUp()`과 `armorUp()`은 클래스변수를 이용하여 작업하기 때문에 클래스메서드로 정의하는 것이 맞다. **인스턴스변수를 가지고 작업을 하면 인스턴스메서드로, 클래스변수를 가지고 작업을 하면 클래스메서드로 정의하도록 한다.**<br/>

**[6-10]** 다음 중 `생성자`에 대한 설명으로 옳지 않은 것은? (모두 고르시오)<br/>
a. 모든 생성자의 이름은 클래스의 이름과 동일해야한다.<br/>
**b. 생성자는 객체를 생성하기 위한 것이다.** // 생성자는 객체를 초기화 할 목적으로 사용된다. 객체를 생성하는 것은 `new`연산자이다.<br/>
c. 클래스에는 생성자가 반드시 하나 이상 있어야 한다.<br/>
d. 생성자가 없는 클래스는 컴파일러가 기본 생성자를 추가한다.<br/>
**e. 생성자는 오버로딩 할 수 없다.** // 생성자도 메서드이기 때문에 오버로딩 할 수 있다.<br/>

**[정답]** b, e<br/>

**[6-11]** 다음 중 `this`에 대한 설명으로 맞지 않은 것은? (모두 고르시오)<br/>
a. 객체 자신을 가리키는 참조변수이다.<br/>
**b. 클래스 내에서라면 어디서든 사용할 수 있다.** // 인스턴스메서드 내에서만 사용이 가능하다.<br/>
c. 지역변수와 인스턴스변수를 구별할 때 사용한다.<br/>
d. 클래스 메서드 내에서는 사용할 수 없다.<br/>

**[정답]** b<br/>

**[6-12]** 다음 중 `오버로딩`이 성립하기 위한 조건이 아닌 것은? (모두 고르시오)<br/>
a. 메서드의 이름이 같아야 한다.<br/>
b. 매개변수의 개수나 타입이 달라야 한다.<br/>
**c. 리턴타입이 달라야 한다.**<br/>
**d. 매개변수의 이름이 달라야 한다.**<br/>

**[정답]** c, d<br/>
**[해설]** 오버로딩이 성립하기 위해서는 메서드의 이름이 같고 매개변수의 개수나 타입이 달라야 한다. 리턴타입과 매개변수 이름은 오버로딩에 아무런 영향을 주지 못한다.<br/>

**[6-13]** 다음 중 아래의 `add메서드`를 올바르게 `오버로딩` 한 것은? (모두 고르시오)

```add()
long add(int a, int b) { return a + b; }
```

a. long add(int x, int y) { return x + y; } // 매개변수 이름은 오버로딩에 아무런 영향을 주지 못한다.<br/>
**b. long add(long a, long b) { return a + b; }** // 매개변수 타입이 다르므로 오버로딩이 성립한다.<br/>
**c. int add(byte a, byte b) { return a + b; }** // 매개변수 타입이 다르므로 오버로딩이 성립한다.<br/>
**d. int add(long a, int b) { return (int)(a + b); }** // 매개변수 타입이 다르므로 오버로딩이 성립한다.<br/>

**[정답]** b, c, d<br/>

**[6-14]** 다음 중 `초기화`에 대한 설명으로 옳지 않은 것은? (모두 고르시오)<br/>
a. 멤버변수는 자동 초기화되므로 초기화하지 않고도 값을    참조할 수 있다. // 기본값으로 자동 초기화<br/>
b. 지역변수는 사용하기 전에 반드시 초기화해야 한다.<br/>
**c. 초기화 블럭보다 생성자가 먼저 수행된다.** // 기본값 → 명시적 초기화 → 초기화 블럭 → 생성자 순으로 수행된다.<br/>
d. 명시적 초기화를 제일 우선적으로 고려해야 한다.<br/>
**e.클래스변수보다 인스턴스변수가 먼저 초기화된다.** // 클래스변수가 인스턴스변수보다 먼저 초기화된다.<br/>

**[정답]** c, e<br/>

**[6-15]** 다음 중 `인스턴스변수의 초기화 순서`가 올바른 것은?<br/>
**a. 기본값-명시적초기화-초기화블럭-생성자**<br/>
b. 기본값-명시적초기화-생성자-초기화블럭<br/>
c. 기본값-초기화블럭-명시적초기화-생성자<br/>
d. 기본값-초기화블럭-생성자-명시적초기화<br/>

**[정답]** a<br/>

**[6-16]** 다음 중 `지역변수`에 대한 설명으로 옳지 않은 것은? (모두 고르시오)<br/>
**a. 자동 초기화되므로 별도의 초기화가 필요없다.** // 지역변수는 자동 초기화가 되지 않으므로 별도의 초기화가 필요하다.<br/>
b. 지역변수가 선언된 메서드가 종료되면 지역변수도 함께    소멸된다.<br/>
c. 매서드의 매개변수로 선언된 변수도 지역변수이다.<br/>
d. 클래스변수나 인스턴스변수보다 메모리 부담이 적다.<br/>
**e. 힙(heap)영역에 생성되며 가비지 컬렉터에 의해 소멸된다.** // 힙 영역에는 인스턴스가 생성된다. 지역변수는 스택 영역에 생성된다.<br/>

**[정답]** a, e<br/>

**[6-17]** `호출스택`이 다음과 같은 상황일 때 옳지 않은 설명은? (모두 고르시오)<br/>

![Exercise6_17](/assets/images/javajungsuk3/Exercise6_17.png)

a. 제일 먼저 호출스택에 저장된 것은 main메서드이다.<br/>
**b. println메서드를 제외한 나머지 메서드들은 모두 종료된 상태이다.** // 호출스택 제일 위에 있는 것이 현재 실행중인 메서드, 나머지는 대기상태이다.<br/>
c. method2메서드를 호출한 것은 main메서드이다.<br/>
d. println메서드가 종료되면 method1메서드가 수행을 재개한다.<br/>
e. main-method2-method1-println의 순서로 호출되었다.<br/>
f. 현재 실행중인 메서드는 println 뿐이다.<br/>

**[정답]** b<br>

**[6-18]** 다음의 코드를 컴파일하면 에러가 발생한다. 컴파일    에러가 발생하는 라인과 그 이유를 설명하시오.

```MemberCall
class MemberCall {
    int iv = 10;
    static int cv = 20;

    int iv2 = cv;
    static int cv2 = iv; // 라인 A

    static void staticMethod1() {
        System.out.println(cv);
        System.out.println(iv); // 라인 B
    }

    void instanceMethod1() { 
        System.out.println(cv);
        System.out.println(iv); // 라인 C 
    }
    
    static void staticMethod2() { 
        staticMethod1();
        instanceMethod1(); // 라인 D
    }

    void instanceMethod2() {
        staticMethod1(); // 라인 E
        instanceMethod1();
    }
}
```

**[정답]** 라인 A, 라인B, 라인 D<br/>
**[해설]** 클래스멤버는 인스턴스멤버를 참조하거나 호출할 수 없다. 인스턴스 멤버가 존재하는 시점에 클래스 멤버는 항상 존재하지만, 클래스 멤버가 존재하는 시점에 인스턴스 멤버가 존재하지 않을 수도 있기 때문이다.<br/>

**[6-19]** 다음 코드의 실행 결과를 예측하여 적으시오.

```Exercise6_19.java
class Exercise6_19 {
    public static void change(String str) {
        str += "456";
    }

    public static void main(String[] args) {
        String str = "ABC123";
        System.out.println(str);
        change(str);
        System.out.println("After change: " + str);
    }
}
```

**[정답]**

```실행결과
ABC123
After change: ABC123
```

**[해설]** `main메서드`에 str과 `change메서드`에 str은 각각 메서드 내에서 사용되는 지역변수이다. `change(str)`은 str에 저장된 값 `ABC123`을 복사하여 넘겨준 것이며 반환받는 값이 없기 때문에 str 값에는 변화가 없다.<br>

**[6-20]** 다음과 같은 정의된 메서드를 작성하고 테스트하시오.<br/>
**[주의]** `Math.random()`을 사용하는 경우 실행결과와 다를 수 있음.<br/>

메서드명 : shuffle<br/>
기능 : 주어진 배열에 담긴 값의 위치를 바꾸는 작업을 반복하여 뒤섞이게 한다. 처리한 배열을 반환한다.<br/>
반환타입 : int[]<br/>
매개변수 : int[] arr - 정수값이 담긴 배열<br/>

```Exercise6_20.java
class Exercise6_20 {
    /*
        (1) shuffle메서드를 작성하시오.
     */

    public static void main(String[] args) {
        int[] original = {1,2,3,4,5,6,7,8,9};
        System.out.println(java.util.Arrays.toString(original));

        int[] result = shuffle(original);
        System.out.println(java.util.Arrays.toString(result));
    }
}
```

```실행결과
[1, 2, 3, 4, 5, 6, 7, 8, 9]
[8, 7, 9, 3, 1, 5, 2, 4, 6]
```

**[정답]**

```shuffle()
static int[] shuffle(int[] arr) {
    // 매개변수 유효성 검사
    if(arr == null || arr.length == 0)
        return arr;
    
    for(int i = 0; i < arr.length; i++) {
        int j = (int) (Math.random() * 9) + 1;
        int tmp = arr[i];
        arr[i] = arr[j];
        arr[j] = tmp;
    }
    return arr;
}
```

**[6-21]** `Tv클래스`를 주어진 로직대로 완성하시오. 완성한 후에 실행해서 주어진 실행결과와 일치하는지 확인하라.<br/>
**[참고]** 코드를 단순히 하기 위해서 유효성검사는 로직에서 제외했다.<br/>

```Exercise6_21.java
class MyTv {
    boolean isPowerOn;
    int channel;
    int volume;

    final int MAX_VOLUME = 100;
    final int MIN_VOLUME = 0;
    final int MAX_CHANNEL = 100;
    final int MIN_CHANNEL = 1;

    void turnOnOff() {
        // (1) isPowerOn의 값이 true면 false로, false면 true로 바꾼다.
    }

    void volumeUp() {
        // (2) volume의 값이 MAX_VOLUME보다 작을 때만 값을 1 증가시킨다.
    }

    void volumeDown() {
        // (3) volume의 값이 MIN_VOLUME보다 클 때만 값을 1 감소시킨다.
    }

    void channelUp() {
        // (4) channel의 값을 1증가시킨다.
        // 만일 channel이 MAX_CHANNEL이면, channel의 값을 MIN_CHANNEL로 바꾼다.
    }

    void channelDown() {
        // (5) channel의 값을 1 감소시킨다.
        // 만일 channel이 MIN_CHANNEL이면, channel의 값을 MAX_CHANNEL로 바꾼다.
    }
}

public class Exercise6_21 {
    public static void main(String[] args) {
        MyTv t = new MyTv();

        t.channel = 100;
        t.volume = 0;
        System.out.println("CH: " + t.channel + ", VOL: " + t.volume);

        t.channelDown();
        t.volumeDown();
        System.out.println("CH: " + t.channel + ", VOL: " + t.volume);

        t.volume = 100;
        t.channelUp();
        t.volumeUp();
        System.out.println("CH: " + t.channel + ", VOL: " + t.volume);
    }
}
```

```실행결과
CH: 100, VOL: 0
CH: 99, VOL: 0
CH: 100, VOL: 100
```

**[정답]**

(1)

```turnOnOff()
void turnOnOff() {
    // (1) isPowerOn의 값이 true면 false로, false면 true로 바꾼다.
    isPowerOn = !isPowerOn;
}
```

(2)

```volumeUp()
void volumeUp() {
    // (2) volume의 값이 MAX_VOLUME보다 작을 때만 값을 1 증가시킨다.
    if(volume < MAX_VOLUME) volume++;
}
```

(3)

```volumeDown()
void volumeDown() {
    // (3) volume의 값이 MIN_VOLUME보다 클 때만 값을 1 감소시킨다.
    if(volume > MIN_VOLUME) volume--;
}
```

(4)

```channelUp()
void channelUp() {
    // (4) channel의 값을 1증가시킨다.
    // 만일 channel이 MAX_CHANNEL이면, channel의 값을 MIN_CHANNEL로 바꾼다.
    if(channel == MAX_CHANNEL) { channel = MIN_CHANNEL; }
    else { channel++; }
}
```

(5)

```channelDown()
void channelDown() {
    // (5) channel의 값을 1 감소시킨다.
    // 만일 channel이 MIN_CHANNEL이면, channel의 값을 MAX_CHANNEL로 바꾼다.
    if(channel == MIN_CHANNEL) { channel = MAX_CHANNEL; }
    else { channel--; }
}
```

**[6-22]** 다음과 같이 정의된 메서드를 작성하고 테스트하시오.<br/>

메서드명 : isNumber<br/>
기능 : 주어진 문자열이 모두 숫자로만 이루어져있는지 확인한다.<br/>
모두 숫자로만 이루어져 있으면 true를 반환하고,<br/>
그렇지 않으면 false를 반환한다.<br/>
만일 주어진 문자열이 null이거나 빈문자열 ""이라면 false를 반환한다.<br/>
반환타입 : boolean<br/>
매개변수 : String str - 검사할 문자열<br/>

**[Hint]** String클래스의 charAt(int i)메서드를 사용하면 문자열의 i번째 위치한 문자를 얻을 수 있다.

```Exercise6_22.java
class Exercise6_22 {
    /*
        (1) isNumber메서드를 작성하시오.
     */

    public static void main(String[] args) {
        String str = "123";
        System.out.println(str + "는 숫자입니까? " + isNumber(str));
        
        str = "1234o";
        System.out.println(str + "는 숫자입니까? " + isNumber(str));
    }
}
```

```실행결과
123는 숫자입니까? true
1234o는 숫자입니까? false
```

**[정답]**

```isNumber()
static boolean isNumber(String str) {
    if(str == null || str.isEmpty()) return false;
    for(int i = 0; i < str.length(); i++) {
        char ch = str.charAt(i);
        if(ch < '0' || ch > '9') {
            return false;
        }
    }
    return true;
}
```

**[6-23]** 다음과 같이 정의된 메서드를 작성하고 테스트하시오.<br/>

메서드명 : max<br/>
기능 : 주어진 int형 배열의 값 중에서 제일 큰 값을 반환한다.<br/>
만일 주어진 배열이 null이거나 크기가 0인 경우, -999999를 반환한다.<br/>
반환타입 : int<br/>
매개변수 : int[] arr - 최대값을 구할 배열<br/>

```Exercise6_23.java
class Exercise6_23 {
    /*
        (1) max메서드를 작성하시오.
     */

    public static void main(String[] args) {
        int[] data = {3,2,9,4,7};
        System.out.println(java.util.Arrays.toString(data));
        System.out.println("최대값: " + max(data));
        System.out.println("최대값: " + max(null));
        System.out.println("최대값: " + max(new int[] {})); // 크기가 0인 배열
    }
}
```

```실행결과
[3, 2, 9, 4, 7]
최대값: 9
최대값: -999999
최대값: -999999
```

**[정답]**

```max()
static int max(int[] arr) {
    if(arr == null || arr.length == 0) return -999999;
    int maxNum = arr[0]; // 배열의 첫 번째 요소를 대입
    for(int i = 1; i < arr.length; i++) { // 배열의 두 번째 값부터 비교
        if(maxNum < arr[i]) maxNum = arr[i];
    }
    return maxNum;
}
```

**[6-24]** 다음과 같이 정의된 메서드를 작성하고 테스트하시오.<br/>

메서드명 : abs<br/>
기능 : 주어진 값을 절대값으로 반환한다.<br/>
반환타입 : int<br/>
매개변수 : int value<br/>

```Exercise6_24.java
class Exercise6_24 {
    /*
        (1) abs메서드를 작성하시오.
     */
    
    public static void main(String[] args) {
        int value = 5;
        System.out.println(value + "의 절대값: " + abs(value));
        value = 10;
        System.out.println(value + "의 절대값: " + abs(value));
    }
}
```

```실행결과
```

**[정답]**

```abs()
static int abs(int value) {
    if(value >= 0) { return value; }
    else { return -value; }
}
```

## 🚩 잊지않기
- **간만에 문제 풀려니까 머리 엄청 안돌아간다😂**
- ⭐두 점 사이의 거리 공식⭐<br/>
![Exercise6_6](/assets/images/javajungsuk3/Exercise6_6.png)
- [매개변수의 유효성 검사](https://chaeichi.github.io/java/2024/07/24/oop1-4.html#h-%EB%A7%A4%EA%B0%9C%EB%B3%80%EC%88%98%EC%9D%98-%EC%9C%A0%ED%9A%A8%EC%84%B1-%EA%B2%80%EC%82%AC)
    - 매개변수의 타입이 맞으면 어떤 값도 매개변수를 통해 넘어올 수 있기 때문에, 가능한 모든 경우의 수에 대해 고민하고 그에 대비한 코드를 작성해야 한다.
- [JVM의 메모리 구조](https://chaeichi.github.io/java/2024/07/28/oop1-5.html)
    - `메서드 영역`은 클래스가 사용될 때 JVM이 클래스파일(*.class)을 읽어서 분석하여 클래스에 대한 정보(클래스 데이터)를 저장하는 곳으로 클래스멤버가 생성되는 공간이다.
    - `힙 영역` 인스턴스가 생성되는 공간이다.
    - `스택영역` 메서드의 작업에 필요한 메모리 공간을 제공하며, 지역변수와 연산의 중간결과가 저장되는 공간이다.
- [멤버변수의 초기화 시기와 순서](https://chaeichi.github.io/java/2024/08/01/oop1-10.html#h-%EB%A9%A4%EB%B2%84%EB%B3%80%EC%88%98%EC%9D%98-%EC%B4%88%EA%B8%B0%ED%99%94-%EC%8B%9C%EA%B8%B0%EC%99%80-%EC%88%9C%EC%84%9C)
    - 클래스변수의 초기화 순서는 기본값 → 명시적 초기화 → 클래스 초기화 블럭 순이다.
    - 인스턴스변수의 초기화 순서는 기본값 → 명시적 초기화 → 인스턴스 초기화 블럭 → 생성자 순이다.