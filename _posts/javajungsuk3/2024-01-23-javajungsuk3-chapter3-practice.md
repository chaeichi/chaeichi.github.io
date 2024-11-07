---
layout: post
title: "Java의 정석 연습문제 - Chapter03. 연산자"
subtitle: Java의 정석 3rd Edition | 남궁성
excerpt_image: /assets/images/excerpt-images/javajungsuk3/2024-01-23-javajungsuk3-chapter3-practice.png
categories: 
    - 문제풀이
tags: 
    - Java
    - Java의 정석
    - 연습문제
    - 문제풀이
---
![banner](/assets/images/excerpt-images/javajungsuk3/2024-01-23-javajungsuk3-chapter3-practice.png)

## ✏️ 연습문제
**3-1.** 다음 연산의 결과를 적으시오.

```Exercise3_1.java
class Exercise3_1 {
    public static void main(String[] args) {
        int x = 2;
        int y = 5;
        char c = 'A'; // 'A'의 문자코드는 65
        
        System.out.println(1 + x << 33);
        System.out.println(y >= 5 || x < 0 && x > 2);
        System.out.println(y += 10 - x++);
        System.out.println(x += 2);
        System.out.println(!('A' <= c && c <= 'Z'));
        System.out.println('C' - c);
        System.out.println('5' - '0');
        System.out.println(c + 1);
        System.out.println(++c);
        System.out.println(c++);
        System.out.println(c);
    }
}
```

**[정답]**

```실행결과
6
true
13
5
false
2
5
66
B
B
C
```

**[해설]**<br/>
**System.out.println(1 + x << 33); // 6**<br/>
x의 값이 2이므로 1 + x는 3이 되고, 3 << 33이 된다. **자료형의 비트 수보다 큰 값으로 시프트 연산을 하면 자료형의 비트 수로 나눈 나머지만큼만 이동한다.** int는 32비트이므로 33만큼 시프트 연산을 하면 나머지인 1만큼만 이동한다. 3을 1만큼 왼쪽 이동을 하면 2의 1 제곱인 2를 곱하는 것과 같으므로 연산 결과는 6이 된다.<br/>

**System.out.println(y >= 5 \|\| x < 0 && x > 2); // true** <br/>
y의 값이 5이므로 5 >= 5는 true가 된다. OR 연산자는 한 쪽만 참이여도 연산결과가 참이므로 연산 결과는 true가 된다.<br/>

**System.out.println(y += 10 - x++); // 13**<br/>
y += 10 - x++는 y = y + (10 - x++)와 같다. 10 - x++에서 x++은 후위형이므로 값이 참조된 뒤에 값이 증가한다. 따라서 10 - x가 먼저 실행되어 8이 되고, x의 값은 1 증가한 3이 된다. y의 값은 5이므로 8 + 5의 결과인 13이 y에 저장된다.<br/>

**System.out.println(x += 2); // 5**<br/>
x += 2는 x = x + 2와 같다. x의 값은 3이므로 3 + 2의 결과인 5가 x에 저장된다.<br/>

**System.out.println(!('A' <= c && c <= 'Z')); // false**<br/>
('A' <= c && c <= 'Z')는 문자 c가 대문자인지 확인하는 조건식이다. c의 값은 대문자 A이므로 'A' < = 'A' && 'A' <= 'Z'가 되고 양쪽 조건식 모두 true이므로 해당 조건식은 true가 된다. 그리고 이 조건식 앞에 논리부정을 수행하기 때문에 true에 반대인 false가 최종결과가 된다.<br/>

**System.out.println('C' - c); // 2**<br/>
이항연산자의 피연산자가 int형보다 작은 타입인 경우 int형으로 변환한 다음 연산이 수행된다. 유니코드 영문자 A ~ Z는 순서대로 배치되어 있고 'A'는 문자코드로 65이므로 'C'는 67이 된다. 따라서 67 - 65의 결과인 2가 출력된다.<br/>

**System.out.println('5' - '0'); // 5**<br/>
유니코드 숫자 0 ~ 9는 순서대로 배치되어 있으므로 0을 빼주면 숫자로 변환하게 된다.<br/>

**System.out.println(c + 1); // 66**<br/>
유니코드 A는 65이므로 65 + 1의 결과인 66이 출력된다.<br/>

**System.out.println(++c); // B**<br/>
**증감연산자는 자동 형변환이 발생하지 않는다.** ++c는 전위형이므로 값이 먼저 증가하고 참조되므로 B가 출력된다.<br/>

**System.out.println(c++); // B**<br/>
c++은 후위형이므로 값이 먼저 참조되므로 B가 먼저 참조되고 값이 증가한다.<br/>

**System.out.println(c); // C**<br/>
c에 저장된 C가 출력된다.<br/>

**3-2.** 아래의 코드는 `사과를 담는데 필요한 바구니(바켓)의 수를 구하는 코드`이다. 만일 사과의 수가 123개이고 하나의 바구니에는 10개의 사과를 담을 수 있다면, 13개의 바구니가 필요할 것이다. (1)에 알맞은 코드를 넣으시오.

```Exercise3_2.java
class Exercise3_2 {
    public static void main(String[] args) {
        int numOfApples = 123; // 사과의 개수
        int sizeOfBucket = 10; // 바구니의 크기(바구니에 담을 수 있는 사과의 개수)
        int numOfBucket = (/* (1) */); // 모든 사과를 담는데 필요한 바구니의 수

        System.out.println("필요한 바구니의 수 : " + numOfBucket);
    }
}
```

```실행결과
필요한 바구니의 수 : 13
```

**[정답]** numOfApples/sizeOfBucket + (numOfApples%sizeOfBucket == 0 ? 0 : 1)<br/>
**[해설]** 사과의 개수(numOfApples)를 바구니의 크기(sizeOfBucket)로 나누면 필요한 바구니의 수(numOfBucket)를 얻을 수 있다. **이 때, int/int는 소수부를 반올림하지 않고 버린다.** 사과의 개수를 바구니의 크기로 나눈 나머지가 0이 아닐 경우 하나의 바구니가 더 필요하므로 나머지 연산자를 이용해 나머지가 있을 경우 바구니를 하나 더 추가하도록 작성해준다.<br/>

**3-3.** 아래는 변수 num의 값에 따라 `'양수', '음수', '0'을 출력하는 코드`이다. `삼항 연산자`를 이용해서 (1)에 알맞은 코드를 넣으시오.<br/>
**[Hint]** 삼항 연산자를 두 번 사용하라.

```Exercise3_3.java
class Exercise3_3 {
    public static void main(String[] args) {
        int num = 10;
        System.out.println(/* (1) */);
    }
}
```

```실행결과
양수
```

**[정답]** num > 0 ? "양수" : (num < 0 ? "음수" : "0")<br/>
**[해설]** 삼항 연산자를 이용해 num이 0보다 큰 경우 '양수', 작거나 같은 경우 괄호 안의 삼항 연산자를 수행해 num이 0보다 작은 경우 '음수', 아닐 경우(num의 값이 0일 경우) '0'이 출력되도록 한다.<br/>

**3-4.** 아래는 변수 num의 값 중에서 `백의 자리 이하를 버리는 코드`이다. 만일 변수 num의 값이 '456'이라면 '400'이 되고, '111'이라면 '100'이 된다. (1)에 알맞은 코드를 넣으시오.

```Exercise3_4.java
class Exercise3_4 {
    public static void main(String[] args) {
        int num = 456;
        System.out.println(/* (1) */);
    }
}
```

```실행결과
400
```

**[정답]** (num / 100) * 100<br/>
**[해설]** int/int는 소수부를 반올림하지 않고 버린다는 점을 이용하여 num을 100으로 나눠 백의 자리만 남기고 100을 다시 곱해준다.<br/>

**3-5.** 아래는 변수 num의 값 중에서 `일의 자리를 1로 바꾸는 코드`이다. 만일 변수 num의 값이 '333'이라면 '331'이 되고, '777'이라면 '771'이 된다. (1)에 알맞은 코드를 넣으시오.

```Exercise3_5.java
class Exercise3_5 {
    public static void main(String[] args) {
        int num = 333;
        System.out.println(/* (1) */);
    }
}
```

```실행결과
331
```

**[정답]** (num / 10) * 10 + 1<br/>
**[해설]** 앞의 문제와 마찬가지로 int/int는 소수부를 반올림하지 않고 버린다는 점을 이용하여 num을 10으로 나눠 십의 자리까지만 남기고 10을 다시 곱해준다. 그 후 1을 더해주면 일의 자리를 1로 바꿀 수 있게 된다.<br/>

**3-6.** 아래는 `변수 num의 값보다 크면서도 가장 가까운 10의 배수에서 변수 num의 값을 뺀 나머지를 구하는 코드`이다. 예를 들어, 24의 크면서도 가장 가까운 10의 배수는 30이다. 19의 경우 20이고, 81의 경우 90이 된다. 30에서 24를 뺀 나머지는 6이기 때문에 변수 num의 값이 24라면 6을 결과로 얻어야 한다. (1)에 알맞은 코드를 넣으시오.<br/>
**[Hint]** 나머지 연산자를 사용하라.

```Exercise3_6.java
class Exercise3_6 {
    public static void main(String[] args) {
        int num = 24;
        System.out.println(/* (1) */);
    }
}
```

```실행결과
6
```

**[정답]** 10 - (num%10)<br/>
**[해설]** num의 값보다 크면서 가장 가까운 10의 배수이면 num보다 십의 자리가 1 큰 수이다. 변수 num의 나머지를 구해 10에서 빼주는 것으로 쉽게 구할 수 있다.<br/>

**3-7.** 아래는 `화씨(Fahrenheit)를 섭씨(Celcius)로 변환하는 코드`이다. 변환공식이 `C = 5 / 9 x (F - 32)`라고 할 때, (1)에 알맞은 코드를 넣으시오. 단, `변환 결과값은 소수점 셋째자리에서 반올림`해야한다. **(Math.round()를 사용하지 않고 처리할 것)**

```Exercise3_7.java
class Exercise3_7 {
    public static void main(String[] args) {
        int fahrenheit = 100;
        float celcius = (/* (1) */);

        System.out.println("Fahrenheit : " + fahrenheit);
        System.out.println("Celcius : " + celcius);

    }
}
```

```실행결과
Fahrenheit : 100
Celcius : 37.78
```

**[정답]** (int)(5 / 9f * (fahrenheit - 32) * 100 + 0.5) / 100f<br/>
**[해설]**
- 화씨를 섭씨로 변환하는 코드를 작성
    - 5 / 9f * (fahrenheit - 32) = 37.77778
    - 5/9의 결과는 0이므로 실수형태의 결과를 얻으려면 어느 한 쪽을 실수형으로 만들어야하기 때문에 9f를 사용했다.
- 소수점 셋째자리를 반올림하기 위해 소수점 둘째자리까지 정수부로 만들어주고 소수점 셋째자리에 해당하는 부분은 소수점 첫째자리가 됨
    - 5 / 9f * (fahrenheit - 32) * 100 = 3777.7778
- 0.5를 더해 소수점 셋째자리에 해당하는 부분을 반올림해줌
    - 3777.7778 + 0.5 = 3778.2778
- int형으로 변환하여 나머지 부분을 버려줌
    - (int)3778.2778 = 3778
- 위의 결과를 100f로 다시 나눠 소수점 둘째자리까지 표현
    - 3778 / 100f = 37.78

**3-8.** 아래 코드의 문제점을 수정해서 실행결과와 같은 결과를 얻도록 하시오.

```Exercise3_8.java
class Exercise3_8 {
    public static void main(String[] args) {
        byte a = 10;
        byte b = 20;
        byte c = a + b;

        char ch = 'A';
        ch = ch + 2;

        float f = 3 / 2;
        long l = 3000 * 3000 * 3000;

        float f2 = 0.1f;
        double d = 0.1;

        boolean result = d == f2;

        System.out.println("c="+c);
        System.out.println("ch="+ch);
        System.out.println("f="+f);
        System.out.println("l="+l);
        System.out.println("result="+result);
    }
}
```

```실행결과
c=30
ch=C
f=1.5
l=27000000000
result=true
```

**byte c = a + b → (byte)(a + b)**<br/>
**ch = ch + 2 → (char)(ch + 2)**<br/>
int형보다 작은 타입의 자료형(byte, char)은 int형으로 자동 형변환되어 연산되므로 byte와 char에 저장하기 위해서는 명시적 형변환이 필요하다.<br/>

**float f = 3 / 2 → 3f / 2**<br/>
**long l = 3000 * 3000 * 3000 → 3000L * 3000 * 3000**<br/>
int와 int의 연산결과는 int이므로 피연산자중 하나라도 float/long으로 만들어야 연산결과로 float/long을 얻을 수 있으므로 접미사를 붙여 리터럴을 해당 타입으로 만들어줘야한다.<br/>

**boolean result = d == f2 → (float)d == f2**<br/>
**비교연산자도 이항연산자이므로 연산 시 두 피연산자의 타입을 맞추기 위해 형변환이 발생하는데 정수형과 달리 실수형은 근사값으로 저장되므로 오차가 발생할 수 있다.** float형을 double형으로 형변환하는 것보단 double 값을 유효자리수가 적은 float로 형변환해서 비교하는 것이 정확한 결과를 얻을 수 있다.<br/>

**3-9.** 다음은 문자형 변수 ch가 `영문자(대문자 또는 소문자)이거나 숫자일 때만` 변수 b의 값이 `true`가 되도록 하는 코드이다. (1)에 알맞은 코드를 넣으시오.

```Exercise3_9.java
class Exercise3_9 {
    public static void main(String[] args) {
        char ch = 'z';
        boolean b = (/* (1) */);

        System.out.println(b);
    }
}
```

```실행결과
true
```

**[정답]** (ch >= 'A' && ch <= 'Z') \|\| (ch >= 'a' && ch <= 'z') \|\| (ch >= '0' && ch <= '9')<br/>
**[해설]** 유니코드 0 ~ 9, A ~ Z , a ~ z가 연속적으로 배치되어 있는 점을 이용해 각각 숫자, 대문자, 소문자 범위 내에 해당하는 값인지 비교하는 조건식들을 작성하고 OR 연산자로 연결해준다.


**3-10.** 다음은 `대문자를 소문자로 변경하는 코드`인데, 문자 ch에 저장된 문자가 `대문자인 경우에만 소문자로 변경`한다. 문자코드는 `소문자가 대문자보다 32만큼 더 크다.` 예를 들어 'A'의 코드는 65이고, 'a'의 코드는 97이다. (1)~(2)에 알맞은 코드를 넣으시오.

```Exercise3_10.java
class Exercise3_10 {
    public static void main(String[] args) {
        char ch = 'A';

        char lowerCase = (/* (1) */) ? (/* (2) */) : ch;

        System.out.println("ch:" + ch);
        System.out.println("ch to lowerCase:" + lowerCase);
    }
}
```

```실행결과
ch:A
ch to lowerCase:a
```

**[정답]** **(1)** ch >= 'A' && ch <= 'Z' **(2)** (char)(ch + 32)<br/>
**[해설]** 문자 ch에 저장된 문자가 대문자인지 비교하여 참이면 ch + 32를 저장하고 거짓이면 ch를 저장한다. 이 때, ch + 32의 연산결과가 int 타입이므로 char 타입으로의 형변환이 필요하다.

## 🚩 잊지않기
- [증감 연산자는 일반 산술 변환에 의한 자동 형변환이 발생하지 않는다.](https://chaeichi.github.io/java/2024/01/13/operator-2.html#h-%EC%A6%9D%EA%B0%90-%EC%97%B0%EC%82%B0%EC%9E%90--)
- [정수형의 표현 형식으로 소수점 이하의 값은 표현할 수 없기 때문에 int/int는 반올림이 발생하지 않는다.](https://chaeichi.github.io/java/2024/01/08/variable-5.html#h-%EC%8B%A4%EC%88%98%ED%98%95%EC%9D%84-%EC%A0%95%EC%88%98%ED%98%95%EC%9C%BC%EB%A1%9C-%EB%B3%80%ED%99%98)
- **⭐접미사 잊지 않기(연산할 때, float나 long에 값 저장할 때)⭐**
- [비교연산자도 이항 연산자이므로 비교하는 피연산자의 타입이 서로 다를 경우 자동 형변환하여 피연산자의 타입을 일치시킨 후에 비교한다.](https://chaeichi.github.io/java/2024/01/19/operator-4.html#h-%EB%B9%84%EA%B5%90-%EC%97%B0%EC%82%B0%EC%9E%90)
