---
layout: post
title: "Java의 정석 Chapter04. 조건문과 반복문 (1) - 조건문 if, switch"
subtitle: Java의 정석 3rd Edition | 남궁성
excerpt_image: /assets/images/excerpt-images/2024-01-26-if-switch-for-while-statement-1.png
categories: 
    - Java
tags: 
    - Java
    - Java의 정석
    - 제어문
    - 조건문
    - if문
    - if-else문
    - if-else if문
    - 중첩 if문
    - switch문
    - Math.random()
    - charAt()
---
![banner](/assets/images/excerpt-images/2024-01-26-if-switch-for-while-statement-1.png)

## 제어문(control statement)
- **프로그램의 흐름(flow)을 바꾸는 역할을 하는 문장들**
- 제어문에는 `조건문`과 `반복문`이 있음
- `조건문` : `조건에 따라` 다른 문장이 수행되도록 함
- `반복문` : `특정 문장들을 반복`해서 수행함

## 조건문 - if, switch
- `조건식`과 `문장을 포함하는 블럭{}`으로 구성
- 조건식의 연산결과에 따라 실행할 문장이 달라짐
- 조건문에는 `if문`과 `switch문`, 두 가지가 있음
- 주로 `if문`이 많이 사용됨
- 처리할 경우의 수가 많을 때는 `switch문`이 효율적
- **switch문은 if문보다 제약이 많음**

### if문
- `조건식`과 `괄호{}`로 이루어져 있음
- **만일(if) 조건식이 참(true)이면 괄호{} 안의 문장들을 수행하라**는 의미

```if
if (조건식) {
    // 조건식이 참(true)일 때 수행될 문장들을 적는다.
}
```

#### 조건식
- if문에 사용되는 조건식은 일반적으로 `비교연산자`와 `논리연산자`로 구성
- **등가비교 연산자 '==' 대신 대입 연산자 '='을 사용하지 않도록 주의**
- 조건식의 결과는 반드시 `true` 또는 `false`이어야 함

```FlowEx1.java
class FlowEx1 {
    public static void main(String[] args) {
        int x = 0;
        System.out.printf("x=%d 일 때, 참인 것은 %n", x);

        if(x==0) System.out.println("x==0");
        if(x!=0) System.out.println("x!=0");
        if(!(x==0)) System.out.println("!(x==0)");
        if(!(x!=0)) System.out.println("!(x!=0)");

        x = 1;
        System.out.printf("x=%d 일 때, 참인 것은 %n", x);

        if(x==0) System.out.println("x==0");
        if(x!=0) System.out.println("x!=0");
        if(!(x==0)) System.out.println("!(x==0)");
        if(!(x!=0)) System.out.println("!(x!=0)");
    }
}
```

```실행결과
x=0 일 때, 참인 것은 
x==0
!(x!=0)
x=1 일 때, 참인 것은 
x!=0
!(x==0)
```

#### 블럭{}
- `블럭(block)` : `괄호{}`를 이용해 `여러 문장을 하나의 단위로 묶는 것`
- **'}' 다음에 문장의 끝을 의미하는 ';'을 붙이지 않음**
- 블럭 내의 문장들은 탭(tab)으로 들여쓰기(indentation) 해서 블럭 안에 속한 문장이라는 것을 알기 쉽게 해주는 것이 좋음
- 블럭 안에 한 문장만 넣거나 아무런 문장도 넣지 않을 수 있음
- **한 문장만 넣을 경우 괄호{}를 생략할 수 있음**
    
```if
if (score > 60) System.out.println("합격입니다.");
```
- 나중에 새로운 문장들이 추가되면 괄호{}로 문장들을 감싸주어야 하는데, 이 때 괄호{}를 추가하는 것을 잊기 쉬우므로 **가능하면 괄호{}를 생략하지 않고 사용하는 것이 바람직**

```if
if (score > 60)
    System.out.println("합격입니다."); // 문장1. if문에 속한 문장
    System.out.println("축하드립니다."); // 문장2. if문에 속한 문장이 아님
```

```FlowEx2.java
import java.util.Scanner;

class FlowEx2 {
    public static void main(String[] args) {
        int input;

        System.out.printf("숫자를 하나 입력하세요.>");
        Scanner scanner = new Scanner(System.in);
        String tmp = scanner.nextLine(); // 화면을 통해 입력받은 내용을 tmp에 저장
        input = Integer.parseInt(tmp); // 입력받은 문자열(tmp)을 숫자로 변환

        if(input==0) {
            System.out.println("입력하신 숫자는 0입니다.");
        }

        if(input!=0) {
            System.out.println("입력하신 숫자는 0이 아닙니다.");
            System.out.printf("입력하신 숫자는 %d입니다.", input);
        }
    }
}
```

```실행결과1
숫자를 하나 입력하세요.>0
입력하신 숫자는 0입니다.
```

```실행결과2
숫자를 하나 입력하세요.>3
입력하신 숫자는 0이 아닙니다.
입력하신 숫자는 3입니다.
```

### if-else문
- 조건식의 결과가 `참이 아닐 때`, 즉 `거짓`일 때 else 블럭의 문장을 수행
- 조건식의 결과에 따라 두 개의 블럭{} 중 어느 한 블럭{}의 내용이 수행
- **두 블럭{}의 내용이 모두 수행되거나, 모두 수행되지 않는 경우는 없음**

```if-else
if (조건식) {
    // 조건식이 참(true)일 때 수행될 문장들을 적는다.
} else {
    // 조건식이 거짓(false)일 때 수행될 문장들을 적는다.
}
```

```FlowEx3.java
import java.util.Scanner;

class FlowEx3 {
    public static void main(String[] args) {
        System.out.printf("숫자를 하나 입력하세요.>");
        Scanner scanner = new Scanner(System.in);
        int input = scanner.nextInt(); // 화면을 통해 입력받은 숫자를 input에 저장

        if(input==0) {
            System.out.println("입력하신 숫자는 0입니다.");
        } else {
            System.out.println("입력하신 숫자는 0이 아닙니다.");
        }
    }
}
```

```실행결과1
숫자를 하나 입력하세요.>5
입력하신 숫자는 0이 아닙니다.
```

```실행결과2
숫자를 하나 입력하세요.>0
입력하신 숫자는 0입니다.
```

### if-else if문
- `if-else문`은 `두 가지 경우 중 하나`가 수행되는 구조
- `여러 개의 조건식`을 써야하는 경우 `if-else if문` 사용

```if-else_if
if (조건식1) {
    // 조건식1의 연산결과가 참일 때 수행될 문장들을 적는다.
} else if (조건식2) {
    // 조건식2의 연산결과가 참일 때 수행될 문장들을 적는다.
} else if (조건식3) {
    // 조건식3의 연산결과가 참일 때 수행될 문장들을 적는다.
} else { // 마지막에는 보통 else 블럭으로 끝나며, else 블럭은 생략 가능
    // 위의 어느 조건식도 만족하지 않을 때 수행될 문장들을 적는다.
}
```

- 결과가 참인 조건식을 만날 때까지 첫 번째 조건식부터 순서대로 평가 **(첫 번째 조건식이 거짓이면, 두 번째 조건식으로 넘어감)**
- 참인 조건식을 만나면, 해당 블럭{}의 문장들을 수행하고 if-else if문 전체를 빠져나옴
- 결과가 참인 조건식이 하나도 없으면서 else 블럭이 생략되었을 경우, 어떤 블럭도 수행되지 않을 수 있음

```FlowEx4.java
import java.util.Scanner;

class FlowEx4 {
    public static void main(String[] args) {
        int score = 0; // 점수를 저장하기 위한 변수
        char grade = ' '; // 학점을 저장하기 위한 변수. 공백으로 초기화한다.
        
        System.out.printf("점수를 입력하세요.>");
        Scanner scanner = new Scanner(System.in);
        score = scanner.nextInt(); // 화면을 통해 입력받은 숫자를 score에 저장

        if(score >= 90) { // score가 90점보다 같거나 크면 A학점
            grade = 'A';
        } else if(score >= 80) { // score가 80점보다 같거나 크면 B학점
            grade = 'B';
        } else if(score >= 70) { // score가 70점보다 같거나 크면 C학점
            grade = 'C';
        } else { // 나머지는 D학점
            grade = 'D';
        }

        System.out.println("당신의 학점은 " + grade + "입니다.");
    }
}
```

```실행결과1
점수를 입력하세요.>70
당신의 학점은 C입니다.
```

```실행결과2
점수를 입력하세요.>63
당신의 학점은 D입니다.
```

### 중첩 if문
- `if문의 블럭 내`에 `또 다른 if문`을 포함시키는 것
- 중첩의 횟수에는 거의 제한이 없음
- 내부의 if문은 외부의 if문보다 안쪽으로 들여쓰기를 해서 두 if문의 범위가 명확히 구분될 수 있도록 작성해야 함
- if문과 else블럭의 관계가 의도한 바와 다르게 형성될 수 있기 때문에 괄호{}의 생략에 더욱 조심해야 함

```중첩if
if (num >= 0)
    if (num != 0)
        sign = '+';
else
    sign = '-';
```

- **else 블럭은 가까운 if문에 속한 것으로 간주되기 때문에 안쪽 if문의 else 블럭이 되어버림**

```중첩if
if (num >= 0){
    if (num != 0)
        sign = '+';
} else{
    sign = '-';
}
```
- 위와 같이 괄호{}를 넣어서 if블럭과 else블럭의 관계를 확실히 해주는 것이 좋음

```FlowEx5.java
import java.util.Scanner;

class FlowEx5 {
    public static void main(String[] args) {
        int score = 0;
        char grade = ' ', opt = '0';

        System.out.printf("점수를 입력해주세요.>");

        Scanner scanner = new Scanner(System.in);
        score = scanner.nextInt();

        System.out.printf("당신의 점수는 %d입니다.%n", score);

        if(score >= 90) { // score가 90점 보다 같거나 크면 A학점(grade)
            grade = 'A';
            if(score >= 98) { // 90점 이상 중에서도 98점 이상은 A+
                opt = '+';
            } else if(score < 94) { // 90점 이상 94점 미만은 A-
                opt = '-';
            }
        } else if(score >= 80) { // score가 80점 보다 같거나 크면 B학점(grade)
            grade = 'B';
            if(score >= 88) {
                opt = '+';
            } else if(score < 84) {
                opt = '-';
            }
        } else { // 나머지는 C학점(grade)
            grade = 'C';
        }
        System.out.printf("당신의 학점은 %c%c입니다.%n", grade, opt);
    }
}
```

```실행결과1
점수를 입력해주세요.>100
당신의 점수는 100입니다.
당신의 학점은 A+입니다.
```

```실행결과2
점수를 입력해주세요.>81
당신의 점수는 81입니다.
당신의 학점은 B-입니다.
```

```실행결과3
점수를 입력해주세요.>85
당신의 점수는 85입니다.
당신의 학점은 B0입니다.
```

### switch문
- `하나의 조건식`으로 `많은 경우의 수`를 처리할 수 있음
- 표현이 간결하므로 알아보기 쉬움
- 제약조건이 있기 때문에 어쩔 수 없이 if문으로 작성해야 하는 경우도 있음

```switch
switch (조건식) {
    case 값1:
        // 조건식의 결과과 값1과 같을 경우 수행될 문장들
        break; // switch문을 벗어난다.
    case 값2:
        // 조건식의 결과과 값2와 같을 경우 수행될 문장들
        break; // switch문을 벗어난다.
    ...
    default:
        // 조건식의 결과와 일치하는 case문이 없을 때 수행될 문장들
}
```

1. 조건식을 계산
2. 조건식의 결과와 일치하는 case문으로 이동
3. 이후의 문장들을 수행
4. break문이나 switch문의 끝을 만나면 switch문 전체를 빠져나감

- 조건식의 결과와 일치하는 case문이 하나도 없는 경우 default문으로 이동
- default문의 위치는 어디라도 상관없으나 보통 마지막에 놓기 때문에 break문을 쓰지 않아도 됨
- **break문을 생략하면 case문 사이의 구분이 없어지므로 다른 break문을 만나거나 switch문 블럭{}의 끝을 만날 때까지 나오는 모든 문장들을 수행함**

```level
switch (level) {
    case 3:
        grantDelete(); // 삭제 권한을 준다.
    case 2:
        grantWrite(); // 쓰기 권한을 준다.
    case 1:
        grantRead(); // 읽기 권한을 준다.
}
```

- 고의적으로 break문을 생략하는 경우도 있음
- 제일 높은 등급인 3을 가진 사용자는 grantDelete(), grantWrite(), grantRead()가 모두 수행되어 읽기, 쓰기, 삭제 권한을 모두 갖게 됨
- 제일 낮은 등급인 1일 가진 사용자는 grantRead()만 수행되어 읽기 권한만 갖게 됨

#### switch문의 제약조건

```case
public static void main(String[] args) {
    int num, result;
    final int ONE = 1;
    ...
    switch(result) {
        case '1': // OK. 문자 리터럴(정수 상수 49와 동일)
        case ONE: // OK. 문자 상수
        case "YES": // OK. 문자열 리터럴. JDK 1.7부터 허용
        case num: // 에러. 변수는 불가
        case 1.0: // 에러. 실수는 불가
    }
}
```

- **switch문의 조건식 결과값은 반드시 정수 또는 문자열이어야 한다.**
- **case문의 값은 정수 상수만 가능하며, 중복되지 않아야한다.**

```
import java.util.Scanner;

class FlowEx6 {
    public static void main(String[] args) {
        System.out.print("현재 월을 입력하세요.>");
        
        Scanner scanner = new Scanner(System.in);
        int month = scanner.nextInt(); // 화면을 통해 입력받은 숫자를 month에 저장

        switch(month) {
            case 3:
            case 4:
            case 5:
                System.out.println("현재의 계절은 봄입니다.");
                break;
            case 6: case 7: case 8:
                System.out.println("현재의 계절은 여름입니다.");
                break;
            case 9: case 10: case 11:
                System.out.println("현재의 계절은 가을입니다.");
                break;
            default: case 12: case 1: case 2:
                System.out.println("현재의 계절은 겨울입니다.");
        }
    }
}
```

```실행결과
현재 월을 입력하세요.>3
현재의 계절은 봄입니다.
```
- case문은 한 줄에 하나씩 쓰던, 한 줄에 붙여서 쓰던 상관 없다.

```case-1
case 3:
case 4:
case 5:
    System.out.println("현재의 계절은 봄입니다.");
    break;
```

```case-2
case 3: case 4: case 5:
    System.out.println("현재의 계절은 봄입니다.");
    break;
```

#### if문을 switch문을 이용해 변경
- 예제 4-4의 if문을 switch문을 이용해 변경

```FlowEx9.java
import java.util.Scanner;

class FlowEx9 {
    public static void main(String[] args) {
        char grade = ' ';
        
        System.out.print("당신의 점수를 입력하세요.(1~100)>");
        
        Scanner scanner = new Scanner(System.in);
        int score = scanner.nextInt(); // 화면을 통해 입력받은 숫자를 score에 저장

        switch(score) {
            case 100: case 99: case 98: case 97: case 96:
            case 95: case 94: case 93: case 92: case 91: case 90:
                grade = 'A';
                break;
            case 89: case 88: case 87: case 86: case 85:
            case 84: case 83: case 82: case 81: case 80:
                grade = 'B';
                break;
            case 79: case 78: case 77: case 76: case 75:
            case 74: case 73: case 72: case 71: case 70:
                grade = 'C';
                break;
            default:
                grade = 'F';
        }
        System.out.println("당신의 학점은 " + grade + "입니다.");
    }
}
```

```실행결과1
당신의 점수를 입력하세요.(1~100)>82
당신의 학점은 B입니다.
```

```실행결과2
당신의 점수를 입력하세요.(1~100)>69
당신의 학점은 F입니다.
```

- switch문은 조건식을 1번만 계산하면 되므로 더 빠름
- 그러나 case문이 많아져서 좋지않은 코드가 됨

```FlowEx10.java
import java.util.Scanner;

class FlowEx10 {
    public static void main(String[] args) {
        int score = 0;
        char grade = ' ';
        
        System.out.print("당신의 점수를 입력하세요.(1~100)>");

        Scanner scanner = new Scanner(System.in);
        String tmp = scanner.nextLine(); // 화면을 통해 입력받은 내용을 tmp에 저장
        score = Integer.parseInt(tmp); // 입력받은 문자열(tmp)을 숫자로 변환

        switch(score/10) {
            case 10:
            case 9:
                grade = 'A';
                break;
            case 8:
                grade = 'B';
                break;
            case 7:
                grade = 'C';
                break;
            default:
                grade = 'F';
        }
        System.out.println("당신의 학점은 " + grade + "입니다.");
    }
}
```

```실행결과
당신의 점수를 입력하세요.(1~100)>82
당신의 학점은 B입니다.
```

- int/int의 결과가 int인 것을 이용해 간결하게 작성한 예제
- 예를 들어 88/10은 8.8이 아닌 8을 얻게 됨
- 조건식을 잘 만들어서 case문의 갯수를 줄이는 것이 중요

#### Math.random()을 이용한 가위바위보 게임

```FlowEx7.java
import java.util.Scanner;

class FlowEx7 {
    public static void main(String[] args) {
        System.out.printf("가위(1), 바위(2), 보(3) 중 하나를 입력하세요.>");

        Scanner scanner = new Scanner(System.in);
        int user = scanner.nextInt(); // 화면을 통해 입력받은 숫자를 user에 저장
        int com = (int)(Math.random() * 3) + 1; // 1,2,3 중 하나가 com에 저장됨

        System.out.println("당신은 " + user + "입니다.");
        System.out.println("컴은 " + com + "입니다.");
        
        switch (user-com) {
            case 2: case -1:
                System.out.println("당신이 졌습니다.");
                break;
            case 1: case -2:
                System.out.println("당신이 이겼습니다.");
                break;
            case 0:
                System.out.println("비겼습니다.");
        }
    }
}
```

```실행결과1
가위(1), 바위(2), 보(3) 중 하나를 입력하세요.>1
당신은 1입니다.
컴은 2입니다.
당신이 졌습니다.
```

```실행결과2
가위(1), 바위(2), 보(3) 중 하나를 입력하세요.>2
당신은 2입니다.
컴은 2입니다.
비겼습니다.
```

- `Math.random()`
    - `난수(임의의 수)`를 얻기 위해 사용
    - `0.0과 1.0사이`의 범위에 속하는 `하나의 double 값`을 반환
    - **0.0은 포함하고 1.0은 포함되지 않음**
    - Math.random() `* 3` : 0.0 ~ 2.999…
    - `(int)`(Math.random() * 3) : 소수점 부분이 필요하지 않을 경우 `정수 부분만 사용`
    - (int)(Math.random() * 3) `+ 1` : `1부터 범위 시작`

- 사용자가 입력한 값(user)과 컴퓨터가 생성한 난수(com)를 비교하여 승부결과 판단

|user\com|가위(1)|바위(2)|보(3)|
|-|-|-|-|
|가위(1)|무승부|컴승|유저승|
|바위(2)|유저승|무승부|컴승|
|보(3)|컴승|유저승|무승부|

- user에서 com의 값을 빼면 다음과 같은 결과를 얻을 수 있음
- 경우의 수가 9개에서 5개로 줄음
- 이 값들은 모두 정수이므로 switch문으로 처리 가능

|user\com|가위(1)|바위(2)|보(3)|
|-|-|-|-|
|가위(1)|0|-1|-2|
|바위(2)|1|0|-1|
|보(3)|2|1|0|

#### 주민등록번호를 입력받아 charAt()으로 성별을 확인해 출력하는 예제

```FlowEx8.java
import java.util.Scanner;

class FlowEx8 {
    public static void main(String[] args) {
        System.out.print("당신의 주민번호를 입력하세요.(011231-1111222)>");

        Scanner scanner = new Scanner(System.in);
        String regNo = scanner.nextLine();

        char gender = regNo.charAt(7); // 입력받은 번호의 8번째 문자를 gender에 저장
        
        switch (gender) {
            case '1': case '3':
                System.out.println("당신은 남자입니다.");
                break;
            case '2': case '4':
                System.out.println("당신은 여자입니다.");
                break;
            default:
                System.out.println("유효하지 않은 주민등록번호입니다.");
        }
    }
}
```

```실행결과
당신의 주민번호를 입력하세요.(011231-1111222)>110101-2111222
당신은 여자입니다.
```

- `문자열.charAt(index)`
    - 주민등록번호 뒷 번호의 첫 번째 자리의 값을 가져오는 방법
    - 입력받은 문자열(110101-2111222)에서 `8번째`에 저장된 값이 성별을 의미하는 값
    - **index는 0부터 시작하므로** `regNo.charAt(7)`을 통해 가져올 수 있음
- char타입은 정수(유니코드)로 저장되기 때문에 switch문의 조건식과 case문에 사용 가능

#### switch문의 중첩
- **중첩 switch문에서 break문을 빼먹지 않도록 주의**

```FlowEx11.java
import java.util.Scanner;

class FlowEx11 {
    public static void main(String[] args) {
        System.out.printf("당신의 주민번호를 입력하세요. (011231-1111222)>");

        Scanner scanner = new Scanner(System.in);
        String regNo = scanner.nextLine();
        char gender = regNo.charAt(7); // 입력받은 번호의 8번째 문자를 gender에 저장

        switch (gender) {
            case '1': case '3':
                switch (gender) {
                    case '1':
                        System.out.println("당신은 2000년대 이전에 출생한 남자입니다.");
                        break;
                    case '3':
                        System.out.println("당신은 2000년대 이후에 출생한 남자입니다.");
                        break;
                }
                break; // 이 break문을 빼먹지 않도록 주의
            case '2': case '4':
                switch (gender) {
                    case '2':
                        System.out.println("당신은 2000년대 이전에 출생한 여자입니다.");
                        break;
                    case '4':
                        System.out.println("당신은 2000년대 이후에 출생한 여자입니다.");
                        break;
                }
                break;
            default:
                System.out.println("유효하지 않은 주민등록번호입니다.");
        }
    }
}
```

```실행결과
당신의 주민번호를 입력하세요. (011231-1111222)>011231-1111222
당신은 2000년대 이전에 출생한 남자입니다.
```