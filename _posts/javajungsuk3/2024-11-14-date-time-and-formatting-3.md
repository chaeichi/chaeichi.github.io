---
layout: post
title: "Java의 정석 Chapter10. 날짜와 시간 & 형식화 (3) - java.time패키지 (1) : LocalDate, LocalTime, Instant"
subtitle: Java의 정석 3rd Edition | 남궁성
excerpt_image: /assets/images/excerpt-images/javajungsuk3/2024-11-14-date-time-and-formatting-3.png
categories: 
    - Java
tags: 
    - Java
    - Java의 정석
    - java.time패키지
    - LocalDate
    - LocalTime
---
![banner](/assets/images/excerpt-images/javajungsuk3/2024-11-14-date-time-and-formatting-3.png)

## java.time패키지
- `Date`와 `Calendar`가 가지고 있던 단점들을 해소하기 위해 JDK1.8부터 `java.time패키지` 추가

|패키지|설명|
|-|-|
|java.time|날짜와 시간을 다루는데 필요한 핵심 클래스들을 제공|
|java.time.chrono|표준(ISO)이 아닌 달력 시스템을 위한 클래스들을 제공|
|java.time.format|날짜와 시간을 파싱하고, 형식화하기 위한 클래스들을 제공|
|java.time.temporal|날짜와 시간의 필드(field)와 단위(unit)를 위한 클래스들을 제공|
|java.time.zone|시간대(time-zone)와 관련된 클래스들을 제공|

- 위의 패키지들에 속한 클래스들의 가장 큰 특징은 `불변(immutable)`
- 날짜나 시간을 변경하는 메서드들은 기존의 객체를 변경하는 대신 항상 변경된 새로운 객체를 반환한다.
- 기존 Calendar클래스는 변경이 가능하므로, 멀티 스레드 환경에서 안전하지 못하다.
- **멀티 스레드 환경에서는 동시에 여러 스레드가 같은 객체에 접근할 수 있기 때문에, 변경 가능한 객체는 데이터가 잘못될 가능성이 있으며, 이를 스레드에 안전(thread-safe)하지 않다고 한다.**
- 새로운 패키지가 도입되었음에도 불구하고, 앞으로도 기존에 작성된 프로그램과의 호환성 때문에 `Date`와 `Calendar`는 여전히 사용될 것이다.

### java.time패키지의 핵심 클래스
- 날짜를 표현할 때 사용하는 `LocalDate`클래스
- 시간을 표현할 때 사용하는 `LocalTime`클래스
- 날짜와 시간이 모두 필요할 때 사용하는 `LocalDateTime`클래스
- 여기에 시간대(time-zone)까지 다뤄야 한다면, `ZonedDateTime`클래스
- `Calendar`는 ZonedDateTime처럼 날짜와 시간 그리고 시간대까지 모두 가지고 있다.
- `Date`와 유사한 클래스로는 Instant가 있다. 이 클래스는 날짜와 시간을 초 단위(정확히는 나노초)로 표현한다.
- 날짜와 시간을 초단위로 표현한 값을 `타임스탬프(time-stamp)`라고 부르는데, 이 값은 날짜와 시간을 **하나의 정수**로 표현할 수 있으므로 날짜와 시간의 차이를 계산하거나 순서를 비교하는데 유리해서 데이터베이스에서 많이 사용된다.
- 이 외에도 날짜를 더 세부적으로 다룰 수 있는 Year, YearMonth, MonthDay와 같은 클래스도 있다.

#### Period와 Duration
- 두 날짜간의 차이를 표현하기 위한 `Period`
- 시간의 차이를 표현하기 위한 `Duration`

#### 객체 생성하기 - now(), of()
- java.time패키지에 속한 클래스의 객체를 생성하는 가장 기본적인 방법은 `now()`와 `of()`를 사용하는 것
- `now()`는 현재 날짜와 시간을 저장하는 객체를 생성한다.

```now
LocalDate date = LocalDate.now(); // 2015-11-23
LocalTime time = LocalTime.now(); // 21:54:01.875
LocalDateTime dateTime = LocalDateTime.now(); // 2015-11-23T21:54:01.875
ZonedDateTime dateTimeInkr = ZonedDateTime.now(); // 2015-11-23T21:54:01.875+09:00[Asia/Seoul]
```

- `of()`는 단순히 해당 필드의 값을 순서대로 지정해 주기만 하면 된다. 각 클래스마다 다양한 종류의 of()가 정의되어 있다.

```of
LocalDate date = LocalDate.of(2015, 11, 23); // 2015년 11월 23일
LocalTime time = LocalTime.of(23, 59, 59); // 23시 59분 59초

LocalDateTime dateTime = LocalDateTime.of(date, time);
ZonedDateTime dateTimeInkr = ZonedDateTime.of(dateTime, ZoneId.of("Asia/Seoul"));
```

#### Temporal과 TemporalAmount
- `Temporal`, `TemporalAccessor`, `TemporalAdjuster`인터페이스를 구현한 클래스
    - LocalDate, LocalTime, LocalDateTime, ZonedDateTime, Instant 등 날짜와 시간을 표현하기 위한 클래스들
- `TemporalAmount`인터페이스를 구현한 클래스
    - Period, Duration

#### TemporalUnit과 TemporalField
- 날짜와 시간의 단위를 정의해 놓은 `TemporalUnit`인터페이스
- TemporalUnit 인터페이스를 구현한 것이 열거형 `ChronoUnit`
- `TemporalField`는 년, 월, 일 등 날짜와 시간의 필드를 정의해 놓은 것
- TemporalField 인터페이스를 구현한 것이 `ChronoField`
- *열거형(enumeration)은 서로 관련된 상수를 묶어서 정의해 놓은 것*

```get
LocalTime now = LocalTime.now(); // 현재 시간
int minute = now.getMinute(); // 현재 시간에서 분(minute)만 뽑아낸다.
// int minute = now.get(ChronoField.MINUTE_OF_HOUR); // 위의 문장과 동일
```

- 날짜와 시간에서 특정 필드의 값만을 얻을 때는 `get()`이나, get으로 시작하는 이름의 메서드를 이용한다.
- 특정 날짜와 시간에서 지정된 단위의 값을 더하거나 뺄 때는 `plus()` 또는 `minus()`에 값과 함께 열거형 ChronoUnit을 사용한다.

```plus_minus
LocalDate today = LocalDate.now(); // 오늘
LocalDate tomoroow = today.plus(1, ChronoUnit.DAYS); // 오늘에 1일을 더한다.
// LocalDate tomorrow = today.plusDays(1); // 위의 문장과 동일
```

- get()과 plus()에 정의는 아래와 같다.
    - int get(TemporalField field)
    - LocalDate plus(long amountToAdd, TemporalUnit unit)

- 특정 TemporalUnit이나 TemporalField를 사용할 수 있는지 확인하는 메서드
    - boolean isSupported(TemporalUnit unit) // Temporal에 정의
    - boolean isSupported(TemporalField field) // TemporalAccessor에 정의

### LocalDate와 LocalTime
- 객체를 생성하는 방법은 현재의 날짜와 시간을 LocalDate와 LocalTime으로 각각 반환하는 `now()`와 지정된 날짜와 시간으로 LocalDate와 LocalTime객체를 생성하는 `of()`가 있다. 둘다 `static메서드`이다.

```객체_생성
LocalDate date = LocalDate.now(); // 오늘의 날짜
LocalTime time = LocalTime.now(); // 현재 시간

LocalDate date = LocalDate.of(1999, 12, 31); // 1999년 12월 31일
LocalTime time = LocalTime.of(23, 59, 59); // 23시 59분 59초
```

- of()는 다음과 같이 여러 가지 버전이 제공된다.

```of
static LocalDate of(int year, Month month, int dayOfMonth)
static LocalDate of(int year, int month, int dayOfMonth)

static LocalTime of(int hour, int min)
static LocalTime of(int hour, int min, int sec)
static LocalTime of(int hour, int min, int sec, int nanoOfSecond)
```

- 다음과 같이 일 단위나 초 단위로만 지정할 수도 있다.
    - 첫 번째 문장은 1999년의 365번째 날, 즉 마지막 날을 의미
    - 두 번째 문장은 그 날의 0시 0분 0초부터 86399초(하루는 86400초)가 지난 시간, 즉 23시 59분 59초를 의미한다.

```of
LocalDate birthDate = LocalDate.ofYearDay(1999, 365); // 1999년 12월 31일
LocalTime birthTime = LocalTime.ofSecondDay(86399); // 23시 59분 59초
```

- 또는 `parse()`를 이용하면 문자열을 날짜와 시간으로 변환할 수 있다.

```parse
LocalDate birthDate = LocalDate.parse("1999-12-31"); // 1999년 12월 31일
LocalTime birthTime = LocalTime.parse("23:59:59"); // 23시 59분 59초
```

#### 특정 필드의 값 가져오기 - get(), getXXX()
- Calendar와 달리 월(month)의 범위가 1 ~ 12
- 요일은 월요일이 1, 화요일이 2, ..., 일요일은 7

|클래스|메서드|설명(1999-12-31 23:59:59)|
|-|-|-|
|LocalDate|int getYear()|년도(1999)|
|LocalDate|int getMonthValue()|월(12)|
|LocalDate|Month getMonth()|월(DECEMBER) getMonth().getValue() = 12|
|LocalDate|int getDayOfMonth()|일(31)|
|LocalDate|int getDayOfYear()|같은 해의 1월 1일부터 몇 번째 일(365)|
|LocalDate|DayOfWeek getDayOfWeek()|요일(FRIDAY) getDayOfWeek().getValue() = 5|
|LocalDate|int lengthOfMonth()|같은 달의 총 일수(31)|
|LocalDate|int lengthOfYear()|같은 해의 총 일수(365), 윤년이면 366|
|LocalDate|boolean isLeapYear()|윤년여부 확인(false)|
|LocalTime|int getHour()|시(23)|
|LocalTime|int getMinute()|분(59)|
|LocalTime|int getSecond()|초(59)|
|LocalTime|int getNano()|나노초(0)|

- 위의 표에 소개된 메서드 외에도 `get()`과 `getLong()`이 있다.
    - int get(TemporalField field)
    - long getLong(TemporalField field)
- 원하는 필드를 직접 지정할 수 있다.
- 대부분의 필드는 int타입의 범위에 속하지만, 몇몇 필드는 int타입의 범위를 넘을 수 있다. 그럴 때 get() 대신 `getLong()`을 사용해야 한다.
- getLong()을 사용해야 하는 필드는 아래 표에서 이름 옆에 '*' 표시가 되어있다.

|TemporalField(ChronoField)|설명|
|-|-|
|ERA|시대|
|YEAR_OF_ERA_YEAR|년|
|MONTH_OF_YEAR|월|
|DAY_OF_WEEK|요일(1:월요일, 2:화요일, ..., 7:일요일)|
|DAY_OF_MONTH|일|
|AMPM_OF_DAY|오전/오후|
|HOUR_OF_DAY|시간(0~23)|
|CLOCK_HOUR_OF_DAY|시간(1~24)|
|HOUR_OF_AMPM|시간(0~11)|
|CLOCK_HOUR_OF_AMPM|시간(1~12)|
|MINUTE_OF_HOUR|분|
|SECOND_OF_MINUTE|초|
|MILLI_OF_SECOND|천분의 일초(=10<sup>-3</sup>초)|
|MICRO_OF_SECOND *|백만분의 일초(=10<sup>-6</sup>초)|
|NANO_OF_SECOND *|10억분의 일초(=10<sup>-9</sup>초)|
|DAY_OF_YEAR|그 해의 몇 번째 날|
|EPOCH_DAY *|EPOCH(1970.1.1)부터 몇 번째 날|
|MINUTE_OF_DAY|그 날의 몇 번째 분(시간을 분으로 환산)|
|SECOND_OF_DAY|그 날의 몇 번째 초(시간을 초로 환산)|
|MILLI_OF_DAY|그 날의 몇 번째 밀리초(=10<sup>-3</sup>초)|
|MICRO_OF_DAY *|그 날의 몇 번째 마이크로초(=10<sup>-6</sup>초)|
|NANO_OF_DAY *|그 날의 몇 번째 나노초(=10<sup>-9</sup>초)|
|ALIGNED_WEEK_OF_MONTH|그 달의 n번째 주(1~7일 1주, 8~14일 2주, ...)
|ALIGNED_WEEK_OF_YEAR|그 해의 n번째 주(1월 1~7일 1주, 8~14일 2주, ...)
|ALIGNED_DAY_OF_WEEK_IN_MONTH|요일(그 달의 1일을 월요일로 간주하여 계산)|
|ALIGNED_DAY_OF_WEEK_IN_YEAR|요일(그 해의 1월 1일을 월요일로 간주하여 계산)|
|INSTANT_SECONDS|년월일을 초단위로 환산(1970-01-01 00:00:00 UTC를 0초로 계산) instant에만 사용가능|
|OFFSET_SECONDS|UTC와의 시차, ZoneOffset에만 사용가능|
|PROLEPTIC_MONTH|년월일을 월단위로 환산(2015년 11월 = 2015 * 12 + 11)|

- 이 목록은 ChronoField에 정의된 모든 상수를 나열한 것
- 사용할 수 있는 필드는 클래스마다 다르다.
- 예를 들어 LocalDate는 날짜를 표현하기 위한 것이므로, MINUTE_OF_HOUR와 같이 시간에 관련된 필드는 사용할 수 없다.
- *만일 해당 클래스가 지원하지 않는 필드를 사용하면 `UnsupportedTemporalTypeException`이 발생한다.*
- 특정 필드가 가질 수 있는 값의 범위를 알고 싶으면 다음과 같이 하면 된다. `range()`
    - System.out.println(ChronoField.CLOCK_HOUR_OF_DAY.range()); // 1 - 24
    - System.out.println(ChronoField.HOUR_OF_DAY.range()); // 0 - 23

#### 필드의 값 변경하기 - with(), plus(), minus()
- 날짜와 시간에서 특정 필드 값을 변경하려면, 다음과 같이 `with`로 시작하는 메서드를 사용하면 된다.

```with
LocalDate withYear(int year)
LocalDate withMonth(int month)
LocalDate withDayOfMonth(int dayOfMonth)
LocalDate withDayOfYear(int dayOfYear)

LocalTime withHour(int hour)
LocalTime withMinute(int minute)
LocalTime withSecond(int second)
LocalTime withNano(int nanoOfSecond)
```

- `with()`를 사용하면, 원하는 필드를 직접 지정할 수 있다.
    - LocalDate with(TemporalField field, long newValue)

- **필드를 변경하는 메서드들은 항상 새로운 객체를 생성해서 반환하므로 대입 연산자를 사용해야 한다는 것을 잊으면 안 된다.**
    - date = date.withYear(2000); // 년도를 2000년으로 변경
    - time = time.withHour(12); // 시간을 12시로 변경

- 이 외에도 특정 필드에 값을 더하거나 빼는 `plus()`와 `minus()`가 있는데, 아래에는 plus()만 표시하였다.

```plus
LocalTime plus(TemporalAmount amountToAdd)
LocalTime plus(long amountToAdd, TemporalUnit unit)

LocalDate plus(TemporalAmount amountToAdd)
LocalDate plus(long amountToAdd, TemporalUnit unit)
```

- plus()로 만든 다음과 같은 메서드들도 있다.

```plus
LocalDate plusYears(long yearsToAdd)
LocalDate plusMonths(long monthsToAdd)
LocalDate plusDays(long daysToAdd)
LocalDate plusWeeks(long weeksToAdd)

LocalTime plusHours(long hoursToAdd)
LocalTime plusMinutes(long minutesToAdd)
LocalTime plusSeconds(long secondsToAdd)
LocalTime plusNanos(long nanosToAdd)
```

- 그리고 LocalTime의 `truncatedTo()`는 지정된 것보다 작은 단위의 필드를 0으로 만든다.

```truncatedTo
LocalTime time = LocalTime.of(12, 34, 56); // 12시 34분 56초
time = time.truncatedTo(ChronoUnit.HOURS); // 시(hour)보다 작은 단위를 0으로
System.out.println(time); // 12:00
```

- LocalTime과 달리 LocalDate에는 truncatedTo()가 없다.
- LocalDate의 필드인 년, 월, 일은 0이 될 수 없기 때문이다.
- 그리고 이 메서드의 매개변수로는 아래의 표 중에서 시간과 관련된 필드만 사용 가능하다.

|TemporalUnit(ChronoUnit)|설명|
|-|-|
|FOREVER|Long.MAX_VALUE초(약 3천억년)|
|EARS|1,000,000,000년|
|MILLENNIA|1,000년|
|CENTURIES|100년|
|DECADES|10년|
|YEARS|년|
|MONTHS|월|
|WEEKS|주|
|DAYS|일|
|HALF_DAYS|반나절|
|HOURS|시|
|MINUTES|분|
|SECONDS|초|
|MILLIS|천분의 일초(=10<sup>-3</sup>초)|
|MICROS|백만분의 일초(=10<sup>-6</sup>초)|
|NANOS|10억분의 일초(=10<sup>-9</sup>초)|

#### 날짜와 시간의 비교 - isAfter(), isBefore(), isEqual()
- LocalDate와 LocalTime도 `compareTo()`가 적절히 오버라이딩 되어있어서, compareTo로 비교할 수 있다.
- 그럼에도 보다 편리하게 비교할 수 있도록 메서드들이 추가로 제공된다.
    - boolean isAfter(ChronoLocalDate other)
    - boolean isBefore(ChronoLocalDate other)
    - boolean isEqual(ChronoLocalDate other) // LocalDate에만 있음

```isEqual
LocalDate kDate = LocalDate.of(1999, 12, 31);
JapaneseDate jDate = JapaneseDate.of(1999, 12, 31);

System.out.println(kDate.equals(jDate)); // false, YEAR_OF_ERA가 다름
System.out.println(KDate.isEqual(jDate)); // true
```

- `equals()`가 있는데도 `isEqual()`을 제공하는 이유는 **연표(chronology)가 다른 두 날짜를 비교하기 위해서**이다.
- 모든 필드가 일치해야하는 equals()와 달리 isEqual()은 오직 날짜만 비교한다.

```NewTimeEx1.java
import java.time.LocalDate;
import java.time.LocalTime;
import java.time.temporal.ChronoField;
import java.time.temporal.ChronoUnit;

class NewTimeEx1 {
    public static void main(String[] args) {
        LocalDate today = LocalDate.now(); // 오늘의 날짜
        LocalTime now = LocalTime.now(); // 현재 시간

        LocalDate birthDate = LocalDate.of(1999, 12, 31); // 1999년 12월 31일
        LocalTime birthTime = LocalTime.of(23, 59, 59); // 23시 59분 59초

        System.out.println("today = " + today);
        System.out.println("now = " + now);
        System.out.println("birthDate = " + birthDate); // 1999-12-31
        System.out.println("birthTime = " + birthTime); // 23:59:59

        System.out.println(birthDate.withYear(2000)); // 2000-12-31
        System.out.println(birthDate.plusDays(1)); // 2000-01-01
        System.out.println(birthDate.plus(1, ChronoUnit.DAYS)); // 2000-01-01

        // 23:59:59 -> 23:00
        System.out.println(birthTime.truncatedTo(ChronoUnit.HOURS));

        // 특정 ChronoField의 범위를 알아내는 방법
        System.out.println(ChronoField.CLOCK_HOUR_OF_DAY.range()); // 1 ~ 24
        System.out.println(ChronoField.HOUR_OF_DAY.range()); // 0 ~ 23
    }
}
```

```실행결과
today = 2024-11-12
now = 23:19:57.300637400
birthDate = 1999-12-31
birthTime = 23:59:59
2000-12-31
2000-01-01
2000-01-01
23:00
1 - 24
0 - 23
```

### Instant
- `에포크 타임`(EPOCH TIME, 1970-01-01 00:00:00 UTC)부터 경과된 시간을 나노초로 표현한다.
- 사람에겐 불편하지만, 단일 진법으로만 다루기 때문에 계산하기 쉽다.
- Instant를 생성할 때는 `now()`와 `ofEpochSecond()`를 사용한다.

```Instant
Instant now = Instant.now();
Instant now2 = Instant.ofEpochSecond(now.getEpochSecond());
Instant now3 = Instant.ofEpochSecond(now.getEpochSecond(), now.getNano());
```

- 필드에 저장된 값을 가져올 때는 다음과 같이 한다.

```get
long epochSec = now.getEpochSecond();
int nano = now.getNano();
```

- Instant는 시간을 초 단위와 나노초 단위로 나누어 저장한다.
- 오라클 데이터베이스의 `타임스탬프(timestamp)`처럼 밀리초 단위의 EPOCH TIME을 필요로 하는 경우를 위해 `toEpochMilli()`가 정의되어 있다.
    - long toEpochMilli()
- Instant는 항상 UTC(+00:00)를 기준으로 하기 때문에, LocalTime과 차이가 있을 수 있다.
- 예를 들어 한국은 시간대가 '+09:00'이므로 Instant와 LocalTime간에는 9시간의 차이가 있다.
- 시간대를 고려해야 하는 경우 `OffsetDateTime`을 사용하는 것이 더 나은 선택일 수 있다.
- *UTC(Coordinated Universal Time) : 세계 협정시, 1972년 1월 1일부터 시행된 국제 표준시*

#### Instant와 Date간의 변환
- Instant는 기존의 java.util.Date를 대체하기 위한 것이며, JDK1.8부터 Date에 Instant로 변환할 수 있는 새로운 메서드가 추가되었다.
    - static Date from(Instant instant) // Instant → Date
    - Instant toInstant() // Date → Instant