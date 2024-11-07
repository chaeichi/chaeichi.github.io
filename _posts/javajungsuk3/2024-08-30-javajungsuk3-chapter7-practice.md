---
layout: post
title: "Java의 정석 연습문제 - Chapter07. 객체지향 프로그래밍 II"
subtitle: Java의 정석 3rd Edition | 남궁성
excerpt_image: /assets/images/excerpt-images/2024-08-30-javajungsuk3-chapter7-practice.png
categories: 
    - 문제풀이
tags: 
    - Java
    - Java의 정석
    - 연습문제
    - 문제풀이
---
![banner](/assets/images/excerpt-images/2024-08-30-javajungsuk3-chapter7-practice.png)

**[7-1]** 섯다카드 20장을 포함하는 `섯다카드 한 벌(SutdaDeck 클래스)`을 정의한 것이다. 섯다카드 20장을 담는 `SutdaCard배열`을 초기화하시오. **단, 섯다카드는 1부터 10까지의 숫자가 적힌 카드가 한 장씩 있고, 숫자가 1, 3, 8인 경우에는 둘 중의 한 장은 광(kwang)이어야 한다.** 즉, SutdaCard의 인스턴스변수 isKwang의 값이 true이어야 한다.

```Exercise7_1.java
class SutdaDeck {
    final int CARD_NUM = 20;
    SutdaCard[] cards = new SutdaCard[CARD_NUM];

    SutdaDeck() {
        /*
            (1) 배열 SutdaCard를 적절히 초기화 하시오.
         */
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

    // info() 대신 Object클래스의 toString을 오버라이딩했다.
    public String toString() {
        return num + (isKwang ? "K" : "");
    }
}

class Exercise7_1 {
    public static void main(String[] args) {
        SutdaDeck deck = new SutdaDeck();

        for(int i = 0; i < deck.cards.length; i++)
            System.out.print(deck.cards[i] + ",");
    }
}
```

```실행결과
1K,2,3K,4,5,6,7,8K,9,10,1,2,3,4,5,6,7,8,9,10,
```

**[정답]**

```Exercise7_1.java
for(int i = 0; i < cards.length; i++) {
            int num = i % 10 + 1;
            boolean isKwang = (i < 10) && (num == 1 || num == 3 || num == 8);

            cards[i] = new SutdaCard(num, isKwang);
        }
```

**[해설]** 섯다카드는 1부터 10까지 있고, i의 값이 0부터 19까지 변하는 동안 원하는 num의 값을 얻기 위해서는 `i % 10 + 1`과 같은 계산식을 사용하면 된다.<br/>

|i|i % 10|i % 10 + 1|
|-|-|-|
|0|0|1|
|1|1|2|
|...|...|...|
|9|9|10|
|10|0|1|
|11|1|2|

그리고 num의 값이 1, 3, 8일 때, 한 쌍의 카드 중 하나는 `광(kwang)`이어야 한다.<br/>
`boolean isKwang = (i < 10) && (num == 1 || num == 3 || num == 8);`<br/>

한 쌍의 카드 중 하나만 광(kwang)이어야하는데 실행결과 1부터 10까지의 첫 번째 반복문에 출력되는 1, 3, 8이 광이므로 `i < 10`<br/>
숫자가 1, 3, 8일 때만이므로 `num == 1 || num == 3 || num == 8`<br/>
`AND(&&)`가 `OR(||)`보다 우선순위가 높기 때문에 괄호를 꼭 사용해야 한다.<br/>

**[7-2]** 문제7-1의 `SutdaDeck클래스`에 다음에 정의된 새로운 메서드를 추가하고 테스트 하시오.<br/>
**[주의]** `Math.random()`을 사용하는 경우 실행결과가 다를 수 있음.<br/>

[1] 메서드명 : shuffle<br/>
기능 : 배열 cards에 담긴 카드의 위치를 뒤섞는다. (Math.random() 사용)<br/>
반환타입 : 없음<br/>
매개변수 : 없음<br/>

[2] 메서드명 : pick<br/>
기능 : 배열 cards에서 지정된 위치의 SutdaCard를 반환한다.<br/>
반환타입 : SutdaCard<br/>
매개변수 : int index - 위치<br/>

[3] 메서드명 : pick<br/>
기능 : 배열 cards에서 임의의 위치의 SutdaCard를 반환한다. (Math.random() 사용)<br/>
반환타입 : SutdaCard<br/>
매개변수 : 없음<br/>

```Exercise7_2.java
class SutdaDeck {
    final int CARD_NUM = 20;
    SutdaCard[] cards = new SutdaCard[CARD_NUM];

    SutdaDeck() {
        for(int i = 0; i < cards.length; i++) {
            int num = i % 10 + 1;
            boolean isKwang = (i < 10) && (num == 1 || num == 3 || num == 8);

            cards[i] = new SutdaCard(num, isKwang);
        }
    }

    /*
        (1) 위에 정의된 세 개의 메서드를 작성하시오.
     */
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

    public String toString() {
        return num + (isKwang ? "K" : "");
    }
}

class Exercise7_2 {
    public static void main(String[] args) {
        SutdaDeck deck = new SutdaDeck();

        System.out.println(deck.pick(0));
        System.out.println(deck.pick());
        deck.shuffle();

        for(int i = 0; i < deck.cards.length; i++)
            System.out.print(deck.cards[i] + ",");

        System.out.println();
        System.out.println(deck.pick(0));
    }
}
```

```실행결과
1K
2
8K,10,8,2,10,1,4,7,2,5,9,1K,6,3K,4,7,9,3,5,6,
8K
```

**[정답]**

```Exercise7_2.java
void shuffle() {
        for(int i = 0; i < cards.length; i++) {
            int random = (int) (Math.random() * cards.length);
            SutdaCard tmp = cards[i];
            cards[i] = cards[random];
            cards[random] = tmp;
        }
    }

SutdaCard pick(int index) {
    if(index < 0 || index >= CARD_NUM) // index의 유효성을 검사한다.
        return null;
    return cards[index];
}

SutdaCard pick() {
    int index = (int) (Math.random() * cards.length);
    return pick(index);
}
```

**[해설]** pick(index)메서드는 매개변수 index에 대한 유효성검사가 필요하다. 그렇지 않으면 배열의 index의 범위를 넘어서 `arrayindexoutofboundsexception`이 발생할 수 있다. 매개변수가 있는 메서드는 반드시 작업 전에 `유효성검사`를 해야한다는 것을 기억하자.<br/>

pick()메서드의 경우 재사용성을 높이기 위해 index 값을 얻은 후 다시 pick(int index)메서드를 호출한다. *(연습문제 풀이에서는 비효율적이지만 코드의 중복을 제거하고 재사용성을 높이기 위해 이처럼 작성하였다고 한다. 상황에 맞는 적절한 코드를 작성하면 되므로 객체지향적인 측면에 얽매여서 코드를 짤 필요는 없다고 한다.)*<br/>

**[7-3]** `오버라이딩`의 정의와 필요성에 대해서 설명하시오.<br/>
**[정답]** 오버라이딩은 부모 클래스로부터 상속받은 메서드를 자손 클래스에 맞게 재정의하는 것을 의미한다. 부모 클래스로부터 상속받은 메서드를 자손 클래스에서 그대로 사용하기도 하지만, 자손 클래스 자신에 맞게 변경해야하는 경우 오버라이딩이 필요하다.<br/>

**[7-4]** 다음 중 `오버라이딩`의 조건으로 옳지 않은 것은? (모두 고르시오)<br/>
a. 조상의 메서드의 이름이 같아야 한다.<br/>
b. 매개변수의 수와 타입이 모두 같아야 한다.<br/>
**c. 접근 제어자는 조상의 메서드보다 좁은 범위로만 변경할 수 있다.**<br/>
**d. 조상의 메서드보다 더 많은 수의 예외를 선언할 수 있다.**<br/>

**[정답]** c, d<br/>
**[해설]** `오버라이딩`은 메서드의 내용만 새로 작성하는 것이므로 **선언부는 조상의 것과 완전히 일치해야한다.**<br/>
조상 클래스의 메서드와 **이름이 같아야 한다.**<br/>
조상 클래스의 메서드와 **매개변수가 같아야 한다.**<br/>
조상 클래스의 메서드와 **반환타입이 같아야 한다.**<br/>
**접근 제어자를 조상 클래스의 메서드보다 좁은 범위로 변경할 수 없다.**<br/>
**조상 클래스의 메서드보다 많은 수의 예외를 선언할 수 없다.**<br/>
인스턴스메서드를 static메서드로 또는 그 반대로 변경할 수 없다.<br/>

**[7-5]** 다음의 코드는 컴파일하면 에러가 발생한다. 그 이유를 설명하고 에러를 수정하기 위해서는 코드를 어떻게 바꾸어야 하는가?

```Exercise7_5.java
class Product {
    int price; // 제품의 가격
    int bounsPoint; // 제품구매 시 제공하는 보너스 점수

    Product(int price) {
        this.price = price;
        bounsPoint = (int) (price/10.0);
    }
}

class Tv extends Product {
    Tv() { }

    public String toString() {
        return "Tv";
    }
}

class Exercise7_5 {
    public static void main(String[] args) {
        Tv t = new Tv();
    }
}
```

**[정답]** Product클래스에 기본 생성자 `Product()`를 추가해주어야 한다.<br/>
**[해설]** **자손 클래스의 생성자 첫 줄에 조상 클래스 생성자를 호출해야 한다.** *(실제 코드에서는 `super()`를 호출하는 곳이 없지만 컴파일러가 자동적으로 추가해준다.)* 조상 클래스인 Product클래스에는 `Product(int price)`가 정의되어 있기 때문에 기본 생성자인 `Product()`를 컴파일러가 자동적으로 추가해주지 않는다. 결국 정의되지 않은 생성자를 호출하게 되는 것이므로 컴파일 에러가 발생한다. 직접 Product클래스에 `Product() {}`를 넣어주면 문제가 해결된다.

**[7-6]** `자손 클래스의 생성자`에서 `조상 클래스의 생성자`를 호출해야 하는 이유는 무엇인가?<br/>
**[정답]** 조상 클래스에 정의된 인스턴스 변수들이 초기화되도록 하기 위해서<br/>
**[해설]** **자손 클래스의 인스턴스를 생성하면 자손의 멤버와 조상의 멤버가 하나로 합쳐진 인스턴스가 생성된다.** 이 때 `조상 클래스 멤버의 초기화 작업`이 수행되어야 하기 때문에 `자손 클래스의 생성자`에서 `조상 클래스의 생성자`가 호출되어야 한다. 자손의 생성자에서 직접 초기화하기보다는 조상의 생성자를 호출함으로써 초기화되도록 하는 것이 바람직하다.

**[7-7]** 다음 코드를 실행했을 때 호출되는 생성자의 순서와 실행결과를 적으시오.

```Exercise7_7.java
class Parent {
    int x = 100;

    Parent() {
        this(200);
    }

    Parent(int x) {
        this.x = x;
    }

    int getX() {
        return x;
    }
}

class Child extends Parent {
    int x = 3000;

    Child() {
        this(1000);
    }

    Child(int x) {
        this.x = x;
    }
}

class Exercise7_7 {
    public static void main(String[] args) {
        Child c = new Child();
        System.out.println("x = " + c.getX());
    }
}
```

**[정답]** Child() → Child(int x) → Parent() → Parent(int x) → Object()

```실행결과
x = 200
```

**[해설]** Child클래스의 인스턴스변수 x는 1000, Parent클래스의 인스턴스변수 x는 200이 된다. `getX()`는 조상인 `Parent클래스`에 정의된 것이므로 `getX()`에서 `x`는 `Parent클래스`의 인스턴스변수 x를 의미한다. 그래서 `x = 200`이 출력된 것이다.

**[7-8]** 다음 중 `접근제어자`를 접근범위가 넓은 것에서 좁은 것의 순으로 바르게 나열한 것은?<br/>
**a. public - protected - (default) - private**<br/>
b. public - (default) - protected - private<br/>
c. (default) - public - protected - private<br/>
d. private - protected - (default) - public<br/>

**[정답]** a<br/>
**[해설]** `public` 접근 제한이 전혀 없다.<br/>
`protected` 같은 패키지 내에서, 그리고 다른 패키지의 자손 클래스에서 접근이 가능하다.<br/>
`default` 같은 패키지 내에서만 접근이 가능하다.<br/>
`private` 같은 클래스 내에서만 접근이 가능하다.

**[7-9]** 다음 중 제어자 `final`을 붙일 수 있는 대상과 붙였을 때 그 의미를 적은 것이다. 옳지 않은 것은? (모두 고르시오)<br/>
a. 지역변수 - 값을 변경할 수 없다.<br/>
b. 클래스 - 상속을 통해 클래스에 새로운 멤버를 추가할 수 없다.<br/>
c. **메서드 - 오버로딩을 할 수 없다.**<br/>
d. 멤버변수 - 값을 변경할 수 없다.<br/>

**[정답]** c<br/>
**[해설]** final로 지정된 메서드는 오버라이딩을 통해 재정의 될 수 없다.<br/>
제어자 final은 `마지막의` 또는 `변경될 수 없는`의 의미를 가지고 있으며 거의 모든 대상에 사용될 수 있다.<br/>

|대상|의미|
|-|-|
|클래스|변경될 수 없는 클래스, 확장될 수 없는 클래스가 된다.<br/>final로 지정된 클래스는 다른 클래스의 조상이 될 수 없다.|
|메서드|변경될 수 없는 메서드<br/>final로 지정된 메서드는 오버라이딩을 통해 재정의 될 수 없다.|
|멤버변수<br/>지역변수|변수 앞에 final이 붙으면 값을 변경할 수 없는 상수가 된다.|

**[7-10]** MyTv2클래스의 멤버변수 isPowerOn, channel, volume을 **클래스 외부에서 접근할 수 없도록** 제어자를 붙이고 대신 이 멤버변수들의 값을 어디서나 읽고 변경할 수 있도록 `getter`와 `setter` 메서드를 추가하라.

```Exercise7_10.java
class MyTv2 {
    boolean isPowerOn;
    int channel;
    int volume;

    final int MAX_VOLUME = 100;
    final int MIN_VOLUME = 0;
    final int MAX_CHANNEL = 100;
    final int MIN_CHANNEL = 1;

    /*
        (1) 알맞은 코드를 넣어 완성하시오.
     */
}

class Exercise7_10 {
    public static void main(String[] args) {
        MyTv2 t = new MyTv2();

        t.setChannel(10);
        System.out.println("CH: " + t.getChannel());
        t.setVolume(20);
        System.out.println("VOL: " + t.getVolume());
    }
}
```

```실행결과
CH: 10
VOL: 20
```

**[정답]**

```Exercise7_10.java
class MyTv2 {
    private boolean isPowerOn;
    private int channel;
    private int volume;

    ...

    public int getChannel() {
        return channel;
    }

    public void setChannel(int channel) {
        if(channel > MAX_CHANNEL || channel < MIN_CHANNEL)
            return;
        this.channel = channel;
    }

    public int getVolume() {
        return volume;
    }

    public void setVolume(int volume) {
        if(volume > MAX_VOLUME || channel < MIN_VOLUME)
            return;
        this.volume = volume;
    }
}
```

**[7-11]** 문제 7-10에서 작성한 MyTv2클래스에 `이전 채널(previous channel)`로 이동하는 메서드를 추가해서 실행결과와 같은 결과를 얻도록 하시오.<br/>
**[Hint]** 이전 채널의 값을 저장할 멤버변수를 정의하라.

메서드명 : gotoPrevChannel<br/>
기능 : 현재 채널을 이전 채널로 변경한다.<br/>
반환타입 : 없음<br/>
매개변수 : 없음

```Exercise7_11.java
class Exercise7_11 {
    public static void main(String[] args) {
        MyTv2 t = new MyTv2();

        t.setChannel(10);
        System.out.println("CH: " + t.getChannel());
        t.setChannel(20);
        System.out.println("CH: " + t.getChannel());
        t.gotoPrevChannel();
        System.out.println("CH: " + t.getChannel());
        t.gotoPrevChannel();
        System.out.println("CH: " + t.getChannel());
    }
}
```

```실행결과
CH: 10
CH: 20
CH: 10
CH: 20
```

**[정답]**

```Exercise7_11.java
class MyTv2 {
    private boolean isPowerOn;
    private int channel;
    private int prevChannel; // 이전 채널의 값을 저장할 변수
    private int volume;

    ...

    public void setChannel(int channel) {
        if(channel > MAX_CHANNEL || channel < MIN_CHANNEL)
            return;
        prevChannel = this.channel; // 현재 채널의 값을 이전 채널에 저장
        this.channel = channel;
    }   

    public void gotoPrevChannel() {
       setChannel(prevChannel); // 현재 채널을 이전 채널로 변경
    }
}
```

**[7-12]** 다음 중 `접근 제어자`에 대한 설명으로 옳지 않은 것은? (모두 고르시오)<br/>

a. public은 접근제한이 전혀 없는 접근 제어자이다.<br/>
b. (default)가 붙으면 같은 패키지 내에서만 접근이 가능하다.<br/>
**c. 지역변수에도 접근 제어자를 사용할 수 있다.**<br/>
d. protected가 붙으면, 같은 패키지 내에서도 접근이 가능하다.<br/>
e. protected가 붙으면, 다른 패키지의 자손 클래스에서 접근이 가능하다.<br/>

**[정답]** c<br/>
**[해설]** 접근 제어자가 사용될 수 있는 곳 - 클래스, 멤버변수, 메서드, 생성자

**[7-13]** Math클래스의 생성자는 접근 제어자가 `private`이다. 그 이유는 무엇인가?<br/>
**[정답]** Math클래스는 모든 메서드가 `static메서드`이고 인스턴스변수가 존재하지 않기 때문에 **객체를 생성할 필요가 없기 때문이다.**

**[7-14]** 문제 7-1에 나오는 섯다카드의 `숫자`와 `종류(isKwang)`는 사실 **한 번 값이 지정되면 변경되어서는 안 되는 값이다.** 카드의 숫자가 한 번 잘못 바뀌면 똑같은 카드가 두 장이 될 수도 있기 때문이다. 이러한 문제점이 발생하지 않도록 아래의 SutdaCard를 수정하시오.

```Exercise7_14.java
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

    public String toString() {
        return num + (isKwang ? "K" : "");
    }
}

class Exercise7_14 {
    public static void main(String[] args) {
        SutdaCard card = new SutdaCard(1, true);
    }
}
```

**[정답]** 멤버변수 num, isKwang 앞에 제어자 `final`을 붙여준다.

```Exercise7_14.java
class SutdaCard {
    final int NUM;
    final boolean IS_KWANG;

    SutdaCard() {
        this(1, true);
    }

    SutdaCard(int num, boolean isKwang) {
        this.NUM = num;
        this.IS_KWANG = isKwang;
    }

    public String toString() {
        return NUM + (IS_KWANG ? "K" : "");
    }
}
```

**[해설]** **변수 앞에 final이 붙으면 값을 변경할 수 없는 상수가 된다.** 인스턴스변수를 `final`로 선언한 뒤 생성자를 통해 초기화를 하면 이후 값이 변경될 수 없다.

**[7-15]** 클래스가 다음과 같이 정의되어 있을 때, `형변환`을 올바르게 하지 않은 것은? (모두 고르시오.)<br/>

```Exercise7_15
class Unit{}
class AirUnit extends Unit{}
class GroundUnit extends Unit{}
class Tank extends GroundUnit{}
class AirCraft extends AirUnit{}

Unit u = new GroundUnit();
Tank t = new Tank();
AirCraft ac = new AirCraft();
```

a. u = (Unit)ac;<br/>
b. u = ac;<br/>
c. GroundUnit gu = (GroundUnit)u;<br/>
d. AirUnit au = ac;<br/>
**e. t = (Tank)u;**<br/>
f. GroundUnit gu = t;

**[정답]** e<br/>
**[해설]** **조상타입의 인스턴스를 자손타입으로 형변환 할 수 없다.** 조상타입 인스턴스인 GroundUnit을 자손타입인 Tank로 형변환할 수 없다.

**[7-16]** 다음 중 연산결과가 `true`가 아닌 것은? (모두 고르시오)

```Exercise7_16
class Car{}
class FireEngine extends Car implements Movable{}
class Ambulance extends Car{}

FireEngine fe = new FireEngine();
```

a. fe instanceof FireEngine<br/>
b. fe instanceof Movable<br/>
c. fe instanceof Object<br/>
d. fe instanceof Car<br/>
**e. fe instanceof Ambulance**

**[정답]** e<br/>
**[해설]** `instanceof연산자`는 실제 인스턴스의 모든 조상이나 구현한 인터페이스에 대해 `true`를 반환한다. `Ambulance`는 `FireEngine`과 아무런 관계가 없으므로 `false`를 반환한다.

**[7-17]** 아래 세 개의 클래스로부터 공통부분을 뽑아서 `Unit`이라는 클래스를 만들고, 이 클래스를 `상속`받도록 코드를 변경하시오.

```Exercise7_17
class Marine { // 보병
    int x, y; // 현재위치
    void move(int x, int y) { /* 지정된 위치로 이동 */ }
    void stop() { /* 현재 위치에 정지 */ }
    void stimPack() { /* 스팀팩을 사용한다. */ }
}

class Tank { // 탱크
    int x, y; // 현재위치
    void move(int x, int y) { /* 지정된 위치로 이동 */ }
    void stop() { /* 현재 위치에 정지 */ }
    void changeMode() { /* 공격모드로 변환한다. */ }
}

class Dropship { // 수송선
    int x, y; // 현재위치
    void move(int x, int y) { /* 지정된 위치로 이동 */ }
    void stop() { /* 현재 위치에 정지 */ }
    void load() { /* 선택된 대상을 태운다. */ }
    void unload() { /* 선택된 대상을 내린다. */ }
}
```

**[정답]**

```Exercise7_17
class Unit {
    int x, y; // 현재위치
    void move(int x, int y) { /* 지정된 위치로 이동 */ }
    void stop() { /* 현재 위치에 정지 */ }
}

class Marine extends Unit { // 보병
    void stimPack() { /* 스팀팩을 사용한다. */ }
}

class Tank extends Unit { // 탱크
    void changeMode() { /* 공격모드로 변환한다. */ }
}

class Dropship extends Unit { // 수송선
    void load() { /* 선택된 대상을 태운다. */ }
    void unload() { /* 선택된 대상을 내린다. */ }
}
```

**[7-18]** 다음과 같은 실행결과를 얻도록 코드를 완성하시오.<br/>
**[Hint]** `instanceof연산자`를 사용해서 형변환한다.<br/>

메서드명 : action<br/>
기능 : 주어진 객체의 메서드를 호출한다.<br/>
DanceRobot인 경우, dance()를 호출하고,<br/>
SingRobot인 경우, sing()을 호출하고,<br/>
DrawRobot인 경우, draw()를 호출한다.<br/>
반환타입 : 없음<br/>
매개변수 : Robot r - Robot인스턴스 또는 Robot의 자손 인스턴스

```Exercise7_18.java
class Exercise7_18 {
    /*
        (1) action메서드를 작성하시오.
     */

    public static void main(String[] args) {
        Robot[] arr = { new DanceRobot(), new SingRobot(), new DrawRobot()};
        for(int i = 0; i < arr.length; i++)
            action(arr[i]);
    }
}

class Robot { }

class DanceRobot extends Robot {
    void dance() {
        System.out.println("춤을 춥니다.");
    }
}

class SingRobot extends Robot {
    void sing() {
        System.out.println("노래를 합니다.");
    }
}

class DrawRobot extends Robot {
    void draw() {
        System.out.println("그림을 그립니다.");
    }
}
```

```실행결과
춤을 춥니다.
노래를 합니다.
그림을 그립니다.
```

**[정답]**

```Exercise7_18.java
static void action(Robot r) {
    if(r instanceof DanceRobot) {
        ((DanceRobot) r).dance();
    }
    if(r instanceof SingRobot) {
        ((SingRobot) r).sing();
    }
    if(r instanceof DrawRobot) {
        ((DrawRobot) r).draw();
    }
}
```

**[해설]** action메서드 내에서는 매개변수로 들어온 값이 `Robot인스턴스` 또는 `Robot의 자손 인스턴스`라는 것만 알 수 있다. 따라서 `instanceof연산자`를 사용해서 실제 인스턴스 타입을 확인하고 해당 타입으로 형변환을 해야 정의된 메서드를 호출할 수 있다.

**[7-19]** 다음은 물건을 구입하는 사람을 정의한 `Buyer`클래스이다. 이 클래스는 멤버변수로 `돈(money)`과 `장바구니(cart)`를 가지고 있다. 제품을 구입하는 기능의 `buy메서드`와 장바구니에 구입한 물건을 추가하는 `add메서드`, 구입한 물건의 목록과 사용금액, 그리고 남은 금액을 출력하는 `summary메서드`를 완성하시오.<br/>

[1] 메서드명 : buy<br/>
기능 : 지정된 물건을 구입한다.<br/>
가진 돈(money)에서 물건의 가격을 빼고, 장바구니(cart)에 담는다.<br/>
만일 가진 돈이 물건의 가격보다 적다면 바로 종료한다.<br/>
반환타입 : 없음<br/>
매개변수 : Product p - 구입할 물건<br/>

[2] 메서드명 : add<br/>
기능 : 지정된 물건을 장바구니에 담는다.<br/>
만일 장바구니에 담을 공간이 없으면, 장바구니의 크기를 2배로 늘린 다음에 담는다.<br/>
반환타입 : 없음<br/>
매개변수 : Product p - 구입할 물건<br/>

[3] 메서드명 : summary<br/>
기능 : 구입할 물건의 목록과 사용금액, 남은 금액을 출력한다.<br/>
반환타입 : 없음<br/>
매개변수 : 없음<br/>

```Exercise7_19.java
class Exercise7_19 {
    public static void main(String[] args) {
        Buyer b = new Buyer();
        b.buy(new Tv());
        b.buy(new Computer());
        b.buy(new Tv());
        b.buy(new Audio());
        b.buy(new Computer());
        b.buy(new Computer());
        b.buy(new Computer());

        b.summary();
    }
}

class Buyer {
    int money = 1000;
    Product[] cart = new Product[3]; // 구입한 제품을 저장하기 위한 배열
    int i = 0; // Product배열 cart에 사용될 index

    void buy(Product p) {
        /*
            (1) 아래의 로직에 맞게 코드를 작성하시오.
            1.1 가진 돈과 물건의 가격을 비교해서 가진 돈이 적으면 메서드를 종료한다.
            1.2 가진 돈이 충분하면, 제품의 가격을 가진 돈에서 빼고
            1.3 장바구니에 구입한 물건을 담는다. (add메서드 호출)
         */
    }

    void add(Product p) {
        /*
            (2) 아래의 로직에 맞게 코드를 작성하시오.
            2.1 i의 값이 장바구니의 크기보다 같거나 크면
                2.1.1 기존의 장바구니보다 2배 큰 새로운 배열을 생성한다.
                2.1.2 기존의 장바구니의 내용을 새로운 배열에 복사한다.
                2.1.3 새로운 장바구니와 기존의 장바구니를 바꾼다.
            2.2 물건을 장바구니(cart)에 저장한다. 그리고 i의 값을 1 증가시킨다.
         */
    }

    void summary() {
        /*
            (3) 아래의 로직에 맞게 코드를 작성하시오.
            3.1 장바구니에 담긴 물건들의 목록을 만들어 출력한다.
            3.2 장바구니에 담긴 물건들의 가격을 모두 더해서 출력한다.
            3.3 물건을 사고 남은 금액(money)을 출력한다.
         */
    }
}

class Product {
    int price; // 제품의 가격

    Product(int price) {
        this.price = price;
    }
}

class Tv extends Product {
    Tv() { super(100); }
    public String toString() { return "Tv"; }
}

class Computer extends Product {
    Computer() { super(200); }
    public String toString() { return "Computer"; }
}

class Audio extends Product {
    Audio() { super(50); }
    public String toString() { return "Audio"; }
}
```

```실행결과
잔액이 부족하여 Computer을/를 살 수 없습니다.
구입한 물건 : Tv, Computer, Tv, Audio, Computer, Computer, 
사용한 금액 : 850
남은 금액 : 150
```

**[정답]**

```Exercise7_19.java
void buy(Product p) {
    if(money < p.price) {
        System.out.println("잔액이 부족하여 " + p + "을/를 살 수 없습니다.");
        return;
    }
    money -= p.price;
    add(p);
}

void add(Product p) {
    if(i >= cart.length) {
        Product[] newCart = new Product[cart.length * 2];
        System.arraycopy(cart, 0, newCart, 0, cart.length);
        cart = newCart;
    }
    cart[i++] = p;
}

void summary() {
    String itemList = "";
    int total = 0;

    for (Product product : cart) {
        if (product == null) break;
        itemList += product + ", ";
        total += product.price;
    }
    
    System.out.println("구입한 물건 : " + itemList);
    System.out.println("사용한 금액 : " + total);
    System.out.println("남은 금액 : " + money);
}
```

**[7-20]** 다음의 코드를 실행한 결과를 적으시오.

```Exercise7_20.java
class Exercise7_20 {
    public static void main(String[] args) {
        Parent p = new Child();
        Child c = new Child();

        System.out.println("p.x = " + p.x);
        p.method();
        
        System.out.println("c.x = " + c.x);
        c.method();
    }
}

class Parent {
    int x = 100;

    void method() {
        System.out.println("Parent Method");
    }
}

class Child extends Parent {
    int x = 200;

    void method() {
        System.out.println("Child Method");
    }
}
```

**[정답]**

```Exercise7_20.java
p.x = 100
Child Method
c.x = 200
Child Method
```

**[해설]** 조상타입의 참조변수로 자손타입의 인스턴스를 참조하는 경우 메서드는 `실제 인스턴스의 메서드(오버라이딩된 메서드)`가 호출되는 반면, 멤버변수는 `참조변수의 타입`에 영향을 받는다. 조상타입의 참조변수 p로 자손타입의 인스턴스 Child를 참조하는 경우, 인스턴스변수 x는 Parent에 정의된 x를 호출하게 된다.

**[7-21]** 다음과 같이 `attack메서드`가 정의되어 있을 때, 이 메서드의 매개변수로 가능한 것 두 가지를 적으시오.

```Exercise7_21
interface Movable {
    void move(int x, int y);
}

void attack(Movable f) {
    / * 내용 생략 */
}
```

**[정답]** null, Movable을 구현한 클래스 또는 그 자손의 인스턴스

**[7-22]** 아래는 도형을 정의한 `Shape클래스`이다. 이 클래스를 조상으로 하는 `Circle클래스`와 `Rectangle클래스`를 작성하시오. 이 때, 생성자도 각 클래스에 맞게 적절히 추가해야한다.

[1] 클래스<br/>
클래스명 : Circle<br/>
조상클래스 : Shape<br/>
멤버변수 : double r - 반지름<br/>

[2] 클래스<br/>
클래스명 : Rectangle<br/>
조상클래스 : Shape<br/>
멤버변수 : double width - 폭<br/>
double height - 높이<br/>

메서드<br/>
메서드명 : isSquare<br/>
기능 : 정사각형인지 아닌지를 알려준다.<br/>
반환타입 : boolean<br/>
매개변수 : 없음<br/>

```Exercise7_22.java
abstract class Shape {
    Point p;

    Shape() {
        this(new Point(0,0));
    }

    Shape(Point p) {
        this.p = p;
    }

    abstract double calcArea(); // 도형의 면적을 계산해서 반환하는 메서드

    Point getPosition() {
        return p;
    }

    void setPosition(Point p) {
        this.p = p;
    }
}

class Point {
    int x;
    int y;

    Point() {
        this(0,0);
    }

    Point(int x, int y) {
        this.x = x;
        this.y = y;
    }

    public String toString() {
        return "{" + x + ", " + y + "}";
    }
}
```

**[정답]**

```Exercise7_22.java
class Circle extends Shape {
    double r; // 반지름

    Circle(double r) {
        this(new Point(0,0), r);
    }

    Circle(Point p, double r) {
        super(p);
        this.r = r;
    }

    double calcArea() {
        return r * r * Math.PI;
    }
}

class Rectangle extends Shape {
    double width; // 폭
    double height; // 높이

    Rectangle(double width, double height) {
        this(new Point(0,0), width, height);
    }

    Rectangle(Point p, double width, double height) {
        super(p);
        this.width = width;
        this.height = height;
    }

    boolean isSquare() {
        return width * height != 0 || width == height;
    }

    double calcArea() {
        return width * height;
    }
}
```

**[7-23]** 문제 7-22에서 정의한 클래스들의 `면적을 구하는 메서드`를 작성하고 테스트 하시오.

메서드명 : sumArea<br/>
기능 : 주어진 배열에 담긴 도형들의 넓이를 모두 더해서 반환한다.<br/>
반환타입 : double<br/>
매개변수 : Shape[] arr<br/>

```Exercise7_23.java
class Exercise7_23 {
    /*
        (1) sumArea메서드를 작성하시오.
     */

    public static void main(String[] args) {
        Shape[] arr = {new Circle(5.0), new Rectangle(3, 4), new Circle(1)};
        System.out.println("면적의 합 : " + sumArea(arr));
    }
}
```

```실행결과
면적의 합 : 93.68140899333463
```

**[정답]**

```Exercise7_23.java
static double sumArea(Shape[] arr) {
    double sum = 0;
    for (Shape shape : arr) {
        sum += shape.calcArea();
    }
    return sum;
}
```

**[7-24]** 다음 중 `인터페이스`의 장점이 아닌 것은?<br/>
a. 표준화를 가능하게 해준다.<br/>
b. 서로 관계없는 클래스들에게 관계를 맺어줄 수 있다.<br/>
c. 독립적인 프로그래밍이 가능하다.<br/>
d. 다중상속을 가능하게 해준다.<br/>
**e. 패키지간의 연결을 도와준다.**<br/>

**[정답]** e<br/>
**[해설]** 인터페이스는 클래스와는 달리 `다중상속`, 즉 여러 개의 인터페이스로부터 상속받는 것이 가능하다.<br/>

**인터페이스의 장점**<br/>
개발시간을 단축시킬 수 있다.<br/>
표준화가 가능하다.<br/>
서로 관계없는 클래스들에게 관계를 맺어줄 수 있다.<br/>
독립적인 프로그래밍이 가능하다.

**[7-25]** `Outer클래스`의 내부 클래스 `Inner`의 멤버변수 `iv`의 값을 출력하시오.

```Exercise7_25.java
class Outer {
    class Inner {
        int iv = 100;
    }
}
class Exercise7_25 {
    public static void main(String[] args) {
        /*
            (1) 알맞은 코드를 넣어 완성하시오.
         */
    }
}
```

```실행결과
100
```

**[정답]**

```Exercise7_25.java
public static void main(String[] args) {
    Outer ou = new Outer();
    Outer.Inner in = ou.new Inner();
    System.out.println(in.iv);
}
```

**[해설]** 내부 클래스의 인스턴스를 생성하려면 먼저 외부 클래스의 인스턴스를 생성해야 한다.

**[7-26]** `Outer클래스`의 내부 클래스 `Inner`의 멤버변수 `iv`의 값을 출력하시오.

```Exercise7_26.java
class Outer {
    static class Inner {
        int iv = 200;
    }
}
class Exercise7_26 {
    public static void main(String[] args) {
        /*
            (1) 알맞은 코드를 넣어 완성하시오.
         */
    }
}
```

```실행결과
200
```

**[정답]**

```Exercise7_26
public static void main(String[] args) {
    Outer.Inner oi = new Outer.Inner();
    System.out.println(oi.iv);
}
```

**[해설]** `static클래스`는 인스턴스 클래스와 달리 외부 클래스를 생성하지 않고도 사용할 수 있다.

**[7-27]** 다음과 같은 실행결괄를 얻도록 (1) ~ (4)의 코드를 완성하시오.

```Exercise7_27.java
class Outer {
    int value = 10;

    class Inner {
        int value = 20;
        void method1() {
            int value = 30;
            
            System.out.println(/* (1) */);
            System.out.println(/* (2) */);
            System.out.println(/* (3) */);
        }
    }
}

class Exercise7_27 {
    public static void main(String[] args) {
        /*
            (4) 알맞은 코드를 넣어 완성하시오.
         */
        
        inner.method1();
    }
}
```

```실행결과
30
20
10
```

**[정답]**

```Exercise7_27.java
class Outer {
    int value = 10;

    class Inner {
        int value = 20;
        void method1() {
            int value = 30;

            System.out.println(value);
            System.out.println(this.value);
            System.out.println(Outer.this.value);
        }
    }
}

class Exercise7_27 {
    public static void main(String[] args) {
        Outer outer = new Outer();
        Outer.Inner inner = outer.new Inner();
        inner.method1();
    }
}
```

**[해설]** 외부 클래스의 인스턴스 변수는 내부 클래스에서 `외부클래스이름.this.변수이름`으로 접근할 수 있다.<br/>`method1()`에서 `value`는 메서드 내에 선언된 지역변수를 가리킨다.<br/>
`this.value`는 `Inner클래스`의 선언된 멤버변수를 가리킨다.<br/>
`Outer.this.value`는 `Outer클래스`의 선언된 멤버변수를 가리킨다.

**[7-28]** 아래의 `EventHandler`를 `익명 클래스(anonymous class)`로 변경하시오.

```Exercise7_28.java
import java.awt.*;
import java.awt.event.WindowAdapter;
import java.awt.event.WindowEvent;

class Exercise7_28 {
    public static void main(String[] args) {
        Frame f = new Frame();
        f.addWindowListener(new EventHandler());
    }
}

class EventHandler extends WindowAdapter {
    public void windowClosing(WindowEvent e) {
        e.getWindow().setVisible(false);
        e.getWindow().dispose();
        System.exit(0);
    }
}
```

**[정답]**

```Exercise7_28.java
import java.awt.*;
import java.awt.event.WindowAdapter;
import java.awt.event.WindowEvent;

class Exercise7_28 {
    public static void main(String[] args) {
        Frame f = new Frame();
        f.addWindowListener(new WindowAdapter() {
            public void windowClosing(WindowEvent e) {
                e.getWindow().setVisible(false);
                e.getWindow().dispose();
                System.exit(0);
            }
        });
    }
}
```

**[7-29]** 지역 클래스에서 외부 클래스의 `인스턴스 멤버`와 `static멤버`에 모두 접근할 수 있지만, 지역변수는 `final이 붙은 상수`만 접근할 수 있는 이유는 무엇인가?<br/>

**[정답]** 메서드가 수행을 마쳐서 지역변수가 소멸된 시점에도, 지역 클래스의 인스턴스가 소멸된 지역변수를 참조하려는 경우가 발생할 수 있기 때문이다.

## 🚩 잊지않기
- **객체지향 프로그래밍 단원 문제를 풀면서 자잘한 실수를 많이 했던거 같다.**
- `문제 7-1` 푸는데 생각을 제대로 못했다.
    - 섯다카드가 1부터 10까지 있고, i의 값이 0부터 19까지 변하는 동안 num의 값이 1 ~ 10까지 변하는 과정이 두 번 반복되는데, `i % 10 + 1` 계산식을 바로 생각해내지 못했다.
    - 한 쌍의 카드 중 하나만 `광(kwang)`이어야 하는데 실행결과 1부터 10까지의 첫 번째 반복문에 출력되는 것에 광이 있으니까 조건으로 `i < 10`을 생각해내면 됐는데, 이것도 바로 생각해내지 못했다.
    - 어려운 문제는 아니였던거 같은데 왜 문제 푸는 당시엔 생각해내지 못했는지😭
- `문제 7-7`에서 조금 헷갈렸던 부분이 있다.
    - 자손클래스에서 오버라이딩 된 메서드가 아니였는데 조상클래스의 인스턴스변수가 출력되는걸 보고 헷갈렸었다.
    - `getX()`는 조상인 `Parent클래스`에 정의된 것이므로 `getX()`에서 `x`는 `Parent클래스`의 인스턴스변수 x를 의미한다.
- 문제를 풀면서 `코드의 재사용성`에 대해 생각해보게 되었다.
    - `문제 7-2`에서 pick()메서드의 경우 재사용성을 높이기 위해 index 값을 얻은 후 다시 pick(int index)메서드를 호출한다.*(연습문제 풀이에서는 비효율적이지만 코드의 중복을 제거하고 재사용성을 높이기 위해 이처럼 작성하였다고 한다.)*
    - `문제 7-10`에서 현재 채널을 이전 채널로 변경하는 `gotoPrevChannel메서드`를 작성할 때 채널을 변경하는 `setChannel메서드`를 호출할 생각을 못하고 채널을 변경하는 문장을 중복으로 작성했었다
    - `setChannel메서드`를 호출하는 코드로 변경함으로써 불필요한 중복 코드를 제거할 수 있었다.
    - 상황에 맞는 적절한 코드를 작성하는 것이 중요하다! *(객체지향적인 측면에 얽매여서 코드를 짤 필요는 없다고 한다.)*<br/>
- [final이 사용될 수 있는 곳 - 클래스, 메서드, 멤버변수, 지역변수](https://chaeichi.github.io/java/2024/08/17/oop2-4.html#h-final---%EB%A7%88%EC%A7%80%EB%A7%89%EC%9D%98-%EB%B3%80%EA%B2%BD%EB%90%A0-%EC%88%98-%EC%97%86%EB%8A%94)
    - 변수에 사용되면 값을 변경할 수 없는 `상수`가 됨
    - 메서드에 사용되면 `오버라이딩`을 할 수 없게 됨
    - 클래스에 사용되면 자신을 확장하는 `자손클래스`를 정의하지 못하게 됨