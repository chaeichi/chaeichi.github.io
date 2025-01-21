---
layout: post
title: "Java의 정석 Chapter10. 날짜와 시간 & 형식화 (4) - java.time패키지 (2) : LocalDateTime, ZonedDateTime, DateTimeFormatter"
subtitle: Java의 정석 3rd Edition | 남궁성
excerpt_image: /assets/images/excerpt-images/javajungsuk3/2024-11-18-date-time-and-formatting-4.png
categories: 
    - Java
tags: 
    - Java
    - Java의 정석
    - java.time패키지
    - LocalDateTime
    - ZonedDateTime
    - OffsetDateTime
    - TemporalAdjusters
    - Period
    - Duration
    - DateTimeFormatter
---
![banner](/assets/images/excerpt-images/javajungsuk3/2024-11-18-date-time-and-formatting-4.png)

## java.time패키지
### LocalDateTime과 ZonedDateTime
- LocalDate + LocalTime = `LocalDateTime`
- LocalDateTime + 시간대(time zone) = `ZonedDateTime`

#### LocalDate와 LocalTime으로 LocalDateTime 만들기
- LocalDate와 LocalTime으로 합쳐서 하나의 LocalDateTime을 만들 수 있다.

```LocalDateTime
LocalDate date = LocalDate.of(2015, 12, 31);
LocalTime time = LocalTime.of(12, 34, 56);

LocalDateTime dt = LocalDateTime.of(date, time);
LocalDateTime dt2 = date.atTime(time);
LocalDateTime dt3 = time.atDate(date);
LocalDateTime dt4 = date.atTime(12, 34, 56);
LocalDateTime dt5 = time.atDate(LocalDate.of(2015, 12, 31));
LocalDateTime dt6 = date.atStartOfDay(); // dt6 = date.atTime(0, 0, 0);
```

- LocalDateTime에도 날짜와 시간을 직접 지정할 수 있는 다양한 버전의 `of()`와 `now()`가 정의되어 있다.

```LocalDateTime
// 2015년 12월 31일 12시 34분 56초
LocalDateTime dateTime = LocalDateTime.of(2015, 12, 31, 12, 34, 56);
LocalDateTime today = LocalDateTime.now();
```

#### LocalDateTime의 변환
- 반대로 LocalDateTime을 LocalDate 또는 LocalTime으로 변환할 수 있다.

```LocalDateTime
LocalDateTime dt = LocalDateTime.of(2015, 12, 31, 12, 34, 56);
LocalDate date = dt.toLocalDate(); // LocalDateTime → LocalDate
LocalTime time = dt.toLocalTime(); // LocalDateTime → LocalTime
```

#### LocalDateTime으로 ZonedDateTime 만들기
- LocalDateTime에 시간대(time-zone)를 추가하면, ZonedDateTime이 된다.
- 기존에는 TimeZone클래스로 시간대를 다뤘지만 새로운 시간 패키지에서는 `ZoneId`라는 클래스를 사용한다.
- `ZoneId`는 일광 절약 시간(DST, Daylight Saving Time)을 자동적으로 처리해주므로 더 편리하다.
- LocalDateTime에 `atZone()`으로 시간대 정보를 추가하면, `ZonedDateTime`을 얻을 수 있다.
- *사용가능한 ZoneId의 목록은 `ZoneId.getAvailableZoneIds()`로 얻을 수 있다.*

```ZonedDateTime
ZoneId zid = ZoneId.of("Asia/Seoul");
ZonedDateTime zdt = dateTime.atZone(zid);
System.out.println(zdt); // 2015-11-27T17:47:50.451+09:00[Asia/Seoul]
```

- LocalDate에 `atStartOfDay()`라는 메서드가 있는데, 이 메서드에 매개변수로 `ZoneId`를 지정해도 ZonedDateTime을 얻을 수 있다.
- 메서드의 이름에서 알 수 있듯이 시간이 0시 0분 0초로 되어 있는 것을 확인할 수 있다.

```ZonedDateTime
ZonedDateTime zdt = LocalDate.now().atStartOfDay(zid);
System.out.println(zdt); // 2015-11-27T00:00+09:00[Asia/Seoul]
```

- 만일 현재 특정 시간대의 시간을 알고 싶다면 다음과 같이 하면 된다. `withZoneSameInstant`

```ZonedDateTime
ZoneId nyId = ZoneId.of("America/New_York");
ZonedDateTime nyTime = ZonedDateTime.now.withZoneSameInstant(nyId);
```

- 위의 코드에서 `now()` 대신 `of()`를 사용하면 날짜와 시간을 지정할 수 있다.

#### ZoneOffset
- UTC로부터 얼마만큼 떨어져 있는지를 `ZoneOffset`으로 표현한다. *서울은 '+9'이다.*

```ZoneOffset
ZoneOffset krOffset = ZonedDateTime.now().getOffset();
// ZoneOffset krOffset = zoneOffset.of("+9");
int krOffsetInSec = krOffset.get(ChronoField.OFFSET_SECONDS); // 32400초
```

#### OffsetDateTime
- ZonedDateTime은 `ZoneId`로 구역을 표현
- ZoneId가 아닌 `ZoneOffset`을 사용하는 것이 `OffsetDateTime`
- ZoneId는 `일광 절약 시간`처럼 시간대와 관련된 규칙들을 포함
- ZoneOffset은 시간대를 `시간의 차이`로만 구분
- 서로 다른 시간대에 존재하는 컴퓨터간의 통신에는 `OffsetDateTime`이 필요하다.
- OffsetDateTime은 ZonedDateTime처럼, LocalDate와 LocalTime에 `ZoneOffset`을 더하거나, ZonedDateTime에 `toOffsetDateTime()`을 호출해서 얻을 수도 있다.

```OffsetDateTime
ZonedDateTime zdt = ZonedDateTime.of(date, time, zid);
OffsetDateTime odt = OffsetDateTime.of(date, time, krOffset);

// ZonedDateTime → OffsetDateTime
OffsetDateTime odt = zdt.toOffsetDateTime();
```

#### ZonedDateTime의 변환
- ZonedDateTime도 LocalDateTime처럼 날짜와 시간에 관련된 다른 클래스로 변환하는 메서드들을 가지고 있다.
    - LocalDate toLocalDate()
    - LocalTime toLocalTime()
    - LocalDateTime toLocalDateTime()
    - OffsetDateTime toOffsetDateTime()
    - long toEpochSecond()
    - Instant toInstant()

```ZonedDateTIme
// ZonedDateTIme → GregorianCalendar
GregorianCalendar from(ZonedDateTIme zdt)

// GregorianCalendar → ZonedDateTIme
ZonedDateTIme toZonedDateTIme()
```
- GregorianCalendar와 가장 유사한 것이 ZonedDateTIme이다. GregorianCalendar와 ZonedDateTIme간의 변환방법만 알면, 그 다음엔 위에 나열한 메서드를 이용해서 다른 날짜와 시간 클래스들로 변환할 수 있다.

```NewTimeEx2.java
import java.time.*;

class NewTimeEx2 {
    public static void main(String[] args) {
        LocalDate date = LocalDate.of(2015, 12, 31); // 2015년 12월 31일
        LocalTime time = LocalTime.of(12, 34, 56); // 12시 34분 56초

        // 2015년 12월 31일 12시 34분 56초
        LocalDateTime dt = LocalDateTime.of(date, time);

        ZoneId zid = ZoneId.of("Asia/Seoul");
        ZonedDateTime zdt = dt.atZone(zid);
//        String strZid = zdt.getZone().getId();

        ZonedDateTime seoulTime = ZonedDateTime.now();
        ZoneId nyId = ZoneId.of("America/New_York");
        ZonedDateTime nyTime = ZonedDateTime.now().withZoneSameInstant(nyId);

        // ZonedDateTime → OffsetDateTime
        OffsetDateTime odt = zdt.toOffsetDateTime();

        System.out.println(dt);
        System.out.println(zid);
        System.out.println(zdt);
        System.out.println(seoulTime);
        System.out.println(nyTime);
        System.out.println(odt);
    }
}
```

```실행결과
2015-12-31T12:34:56
Asia/Seoul
2015-12-31T12:34:56+09:00[Asia/Seoul]
2024-11-12T23:40:39.719246300+09:00[Asia/Seoul]
2024-11-12T09:40:39.721249500-05:00[America/New_York]
2015-12-31T12:34:56+09:00
```

### TemporalAdjusters
- 자주 쓰일만한 날짜 계산들을 대신 해주는 메서드를 정의해놓은 것

```TemporalAdjusters
LocalDate today = LocalDate.now();
LocalDate nextMonday = today.with(TemporalAdjusters.next(DayOfWeek.MONDAY()));
```

- 다음 주 월요일의 날짜를 계산할 때 TemporalAdjusters에 정의된 `next()`를 사용
- 이 외에도 다음과 같은 유용한 메서드들이 정의되어 있다.

|메서드|설명|
|-|-|
|firstDayOfNextYear()|다음 해의 첫 날|
|firstDayOfNextMonth()|다음 달의 첫 날|
|firstDayOfYear()|올 해의 첫 날|
|firstDayOfMonth()|이번 달의 첫 날|
|lastDayOfYear()|올 해의 마지막 날|
|lastDayOfMonth()|이번 달의 마지막 날|
|firstInMonth(DayOfWeek dayOfWeek)|이번 달의 첫 번째 ?요일|
|lastInMonth(DayOfWeek dayOfWeek)|이번 달의 마지막 ?요일|
|previous(DayOfWeek dayOfWeek)|지난 ?요일(당일 미포함)|
|previousOrSame(DayOfWeek dayOfWeek)|지난 ?요일(당일 포함)|
|next(DayOfWeek dayOfWeek)|다음 ?요일(당일 미포함)|
|nextOrSame(DayOfWeek dayOfWeek)|다음 ?요일(당일 포함)|
|dayOfWeekInMonth(int ordinal, DayOfWeek dayOfWeek)|이번 달의 n번째 ?요일|

#### TemporalAdjusters 직접 구현하기
- 필요하면 자주 사용되는 날짜계산을 해주는 메서드를 직접 만들 수도 있다.
- LocalDate의 `with()`는 다음과 같이 정의되어 있으며, `TemporalAdjusters`인터페이스를 구현한 클래스의 객체를 매개변수로 제공해야한다.
    - LocalDate with(TemporalAdjusters adjuster)
- with()는 LocalTime, LocalDateTime, ZonedDateTime, Instant 등 대부분의 날짜와 시간에 관련된 클래스에 포함되어 있다.
- TemporalAdjusters인터페이스는 다음과 같이 추상 메서드 하나만 정의되어 있으며, 이 메서드만 구현하면 된다.

```TemporalAdjusters
@FunctionalInterface
public interface TemporalAdjuster {
    Temporal adjustInto(Temporal temporal);
}
```

- 실제로 구현해야 하는 것은 `adjustInto()`이지만 내부적으로 사용할 의도로 작성된 것이기 때문에, `with()`를 사용해야 한다.
- 날짜와 시간에 관련된 대부분의 클래스는 Temporal인터페이스를 구현하였으므로 adjustInto()의 매개변수가 될 수 있다.

```NewTimeEx3.java
import java.time.*;
import java.time.temporal.*;
import static java.time.DayOfWeek.*;
import static java.time.temporal.TemporalAdjusters.*;

// 특정 날짜로부터 2일 후의 날짜를 계산
class DayAfterTomorrow implements TemporalAdjuster {
    @Override
    public Temporal adjustInto(Temporal temporal) {
        return temporal.plus(2, ChronoUnit.DAYS);
    }
}

class NewTimeEx3 {
    public static void main(String[] args) {
        LocalDate today = LocalDate.now();
        LocalDate date = today.with(new DayAfterTomorrow());

        System.out.println(today);
        System.out.println(date);
        System.out.println(today.with(firstDayOfNextMonth())); // 다음 달의 첫 날
        System.out.println(today.with(firstDayOfMonth())); // 이 달의 첫 날
        System.out.println(today.with(lastDayOfMonth())); // 이 달의 마지막 날
        System.out.println(today.with(firstInMonth(WEDNESDAY))); // 이 달의 첫번째 수요일
        System.out.println(today.with(lastInMonth(WEDNESDAY))); // 이 달의 마지막 수요일
        System.out.println(today.with(previous(WEDNESDAY))); // 지난 주 수요일
        System.out.println(today.with(previousOrSame(WEDNESDAY))); // 지난 주 수요일(오늘 포함)
        System.out.println(today.with(next(WEDNESDAY))); // 다음 주 수요일
        System.out.println(today.with(nextOrSame(WEDNESDAY))); // 다음 주 수요일(오늘 포함)
        System.out.println(today.with(dayOfWeekInMonth(4, WEDNESDAY))); // 이 달의 4번째 수요일
    }
}
```

```실행결과
2024-11-13
2024-11-15
2024-12-01
2024-11-01
2024-11-30
2024-11-06
2024-11-27
2024-11-06
2024-11-13
2024-11-20
2024-11-13
2024-11-27
```

### Period와 Duration
- 날짜의 차이를 계산하기 위한 `Period`
- 시간의 차이를 계산하기 위한 `Duration`

#### between()
- 두 날짜 date1과 date2의 차이를 나타내는 Period는 `between()`으로 얻을 수 있다.

```between
LocalDate date1 = LocalDate.of(2014, 1, 1);
LocalDate date2 = LocalDate.of(2015, 12, 31);
Period pe = Period.between(date1, date2);
```

- date1이 date2보다 날짜 상으로 이전이면 양수로, 이후면 음수로 Period에 저장된다.
- 시간차이를 구할 때는 `Duration`을 사용한다는 것을 제외하고는 Period와 똑같다.

```between
LocalTime time1 = LocalTime.of(00, 00, 00);
LocalTime time2 = LocalTime.of(12, 34, 56); // 12시 34분 56초
Duration du = Duration.between(time1, time2);
```

- Period, Duration에서 특정 필드의 값을 얻을 때는 `get()`을 사용한다.

```get
long year = pe.get(ChronoUnit.YEARS); // int getYears()
long month = pe.get(ChronoUnit.MONTHS); // int getMonths()
long day = pe.get(ChronoUnit.DAYS); // int getDays()

long sec = du.get(ChronoUnit.SECONDS); // long getSeconds()
long nano = du.get(ChronoUnit.NANOS); // int getNano()
```

- Duration에는 getHours(), getMinutes() 같은 메서드가 없다.
- `getUnits()`라는 메서드로 get()에 사용할 수 있는 ChronoUnit의 종류를 확인할 수 있다.

```getUnits
System.out.println(pe.getUnits()); [Years, Months, Days]
System.out.println(du.getUnits()); [Seconds, Nanos]
```

- Duration을 LocalTime으로 변환한 다음에, LocalTime이 가지고 있는 get메서드들을 사용하면 간단하게 계산할 수 있다.

```LocalTime
LocalTime tmpTime = LocalTime.of(0,0).plusSeconds(du.getSeconds());

int hour = tmpTime.getHour();
int min = tmpTime.getMinute();
int sec = tmpTime.getSecond();
int nano = du.getNano();
```

#### between()과 until()
- until()은 between()과 거의 같은 일을 한다.
- between()은 `static메서드`
- until()은 `인스턴스메서드`

```until
Period pe = Period.between(today, myBirthDay);
Period pe = today.until(myBirthDay);
long dday = today.until(myBirthDay, ChronoUnit.DAYS);
```

- Period는 년월일을 분리해서 저장하기 때문에, D-day를 구하려는 경우에는 두 개의 매개변수를 받는 until()을 사용하는 것이 낫다.
- 날짜가 아닌 시간에도 until()을 사용할 수 있지만, Duration으로 변환하는 until()은 없다.
    - long sec = LocalTime.now().until(endTime, ChronoUnit.SECONDS);

#### of(), with()
- `Period`에는 of(), ofYears(), ofMonths(), ofWeeks(), ofDays()가 있다.
- `Duration`에는 of(), ofDays(), ofHours(), ofMinutes(), ofSeconds() 등이 있다.

```of
Period pe = Period.of(1, 12, 31); // 1년 12개월 31일
Duration du = Duration.of(60, ChronoUnit.SECONDS); // 60초
// Duration du = Duration.ofSeconds(60); // 위의 문장과 동일
```
- 특정 필드의 값을 변경하는 `with()`도 있다.

```with
pe = pe.withYears(2); // 1년에서 2년으로 변경. withMonths(), withDays()
du = du.withSeconds(120); // 60초에서 120초로 변경. withNanos()
```

#### 사칙연산, 비교연산, 기타 메서드
- `plus()`, `minus()` 외에 곱셈과 나눗셈을 위한 메서드도 있다.
- `Period`는 날짜의 기간을 표현하기 위한 것이므로 나눗셈을 위한 메서드가 없다.

```arithmetic
pe = pe.minusYears(1).multipliedBy(2); // 1년을 빼고, 2배를 곱한다
du = du.plusHours(1).divideBy(60); // 1시간을 더하고 60으로 나눈다.
```

- 음수인지 확인하는 `isNegative()`와 0인지 확인하는 `isZero()`가 있다.
- 두 날짜 또는 시간을 비교할 때 사용하면 어느 쪽이 앞인지 또는 같은지 알아낼 수 있다.

```isNegative_isZero
boolean sameDate = Period.between(date1, date2).isZero();
boolean isBefore = Duration.between(time1, time2).isNegative();
```

- 부호를 반대로 변경하는 `negated()`와 부호를 없애는 `abs()`가 있다.
- `Period`에는 abs()가 없어서 `isNegative`로 음수인지 확인한 후 `negated()`를 이용해야 한다.
    - if(pe.isNegative()) pe = pe.negated();

- Period에 `normalized()`라는 메서드가 있는데, 이 메서드는 월(month)의 값이 12를 넘기지 않게, 즉 1년 13개월을 2년 1개월로 바꿔준다. 일(day)의 길이는 일정하지 않으므로 그대로 놔눈다.
    - pe = Period.of(1, 13, 32).normalized(); // 1년 13개월 32일 → 2년 1개월 32일

#### 다른 단위로 변환 - toTotalMonths(), toDays(), toHours(), toMinutes()
- 이름이 `to`로 시작하는 메서드들은 Period와 Duration을 다른 단위의 값으로 변환하는데 사용된다.
- `get()`은 특정 필드의 값을 그대로 가져오는 것이지만, 아래의 메서드들은 특정 단위로 변환한 결과를 반환한다는 차이가 있다.
- *이 메서드들의 반환타입은 모두 정수(long 타입)인데, 이것은 지정된 단위 이하의 값들은 버려진다는 뜻이다.*

|클래스|메서드|설명|
|-|-|-|
|Period|long toTotalMonths()|년월일을 월단위로 변환해서 반환(일 단위는 무시)|
|Duration|long toDyas()|일 단위로 변환해서 반환|
|Duration|long toHours()|시간 단위로 변환해서 반환|
|Duration|long toMinutes()|분 단위로 변환해서 반환|
|Duration|long toMillis()|천분의 일초 단위로 변환해서 반환|
|Duration|long toNanos()|나노초 단위로 변환해서 반환|

- 참고로 LocalDate의 `toEpochDay()`는 Epoch Day인 `1970-01-01`부터 날짜를 세어서 반환한다. 이 메서드를 이용하면 Period를 사용하지 않고도 두 날짜간의 일수를 편리하게 계산할 수 있다. 단, 두 날짜 모두 Epoch Day 이후의 것이어야 한다.

```toEpochDay
LocalDate date1 = LocalDate.of(2015, 11, 28);
LocalDate date2 = LocalDate.of(2015, 11, 29);
long period = date2.toEpochDay() - date1.toEpochDay(); // 1
```

- LocalTime에도 `toSecondOfDay()`와 `toNanoOfDay()` 메서드가 있어서, Duration을 사용하지 않고도 위와 같이 뺄셈으로 시간차이를 계산할 수 있다.

```NewTimeEx4.java
import java.time.Duration;
import java.time.LocalDate;
import java.time.LocalTime;
import java.time.Period;
import java.time.temporal.ChronoUnit;

class NewTimeEx4 {
    public static void main(String[] args) {
        LocalDate date1 = LocalDate.of(2014, 1, 1);
        LocalDate date2 = LocalDate.of(2015, 12, 31);

        Period pe = Period.between(date1, date2);

        System.out.println("date1 = " + date1);
        System.out.println("date2 = " + date2);
        System.out.println("pe = " + pe);

        System.out.println("YEAR = " + pe.get(ChronoUnit.YEARS));
        System.out.println("MONTH = " + pe.get(ChronoUnit.MONTHS));
        System.out.println("DAY = " + pe.get(ChronoUnit.DAYS));

        LocalTime time1 = LocalTime.of(0, 0, 0);
        LocalTime time2 = LocalTime.of(12, 34, 56); // 12시간 34분 56초

        Duration du = Duration.between(time1, time2);

        System.out.println("time1 = " + time1);
        System.out.println("time2 = " + time2);
        System.out.println("du = " + du);

        LocalTime tmpTime = LocalTime.of(0, 0).plusSeconds(du.getSeconds());

        System.out.println("HOUR = " + tmpTime.getHour());
        System.out.println("MINUTE = " + tmpTime.getMinute());
        System.out.println("SECOND = " + tmpTime.getSecond());
        System.out.println("NANO = " + tmpTime.getNano());
    }
}
```

```실행결과
date1 = 2014-01-01
date2 = 2015-12-31
pe = P1Y11M30D
YEAR = 1
MONTH = 11
DAY = 30
time1 = 00:00
time2 = 12:34:56
du = PT12H34M56S
HOUR = 12
MINUTE = 34
SECOND = 56
NANO = 0
```

### 파싱과 포맷
- 날짜와 시간을 원하는 형식으로 출력하고 해석(파싱, parsing)하는 방법
- `형식화(formatting)`와 관련된 클래스들은 `java.time.format`패키지에 들어있다.
- 이 중에서 `DateTimeFormatter`가 핵심이다. 자주 쓰이는 다양한 형식들을 기본적으로 정의하고 있으며, 그 외의 형식이 필요하다면 직접 정의해서 사용할 수도 있다.

```DateTimeFormatter
LocalDate date = LocalDate.of(2016, 1, 2);
String yyyymmdd = DateTimeFormatter.ISO_LOCAL_DATE.format(date); // "2016-01-02"
String yyyymmdd = date.format(DateTimeFormatter.ISO_LOCAL_DATE); // "2016-01-02"
```

- 날짜와 시간의 형식화에는 위와 같이 `format()`이 사용되는데, 이 메서드는 DateTimeFormatter뿐만 아니라 LocalDate나 LocalTime같은 클래스에도 있다.
- 아래의 표는 DateTimeFormatter에 상수로 정의된 형식들의 목록이다.

|DateTimeFormatter|설명|보기|
|-|-|-|
|ISO_DATE_TIME|Date and time with ZoneId|2011-12-03T10:15:30+01:00[Europe/Paris]|
|ISO_LOCAL_DATE|ISO Local Date|2011-12-03|
|ISO_LOCAL_TIME|Time without offset|10:15:30|
|ISO_LOCAL_DATE_TIME|ISO Local Date and Time|2011-12-03T10:15:30|
|ISO_OFFSET_DATE|ISO Date with offset|2011-12-03+01:00|
|ISO_OFFSET_TIME|Time with offset|10:15:30+01:00|
|ISO_OFFSET_DATE_TIME|Date Time with Offset|2011-12-03T10:15:30+01:00|
|ISO_ZONED_DATE_TIME|Zoned Date Time|2011-12-03T10:15:30+01:00[Europe/Paris]|
|ISO_INSTANT|Date and Time of an Instant|2011-12-03T10:15:30Z|
|BASIC_ISO_DATE|Basic ISO date|20111203|
|ISO_DATE|ISO Date with or without offset|2011-12-03+01:00<br/>2011-12-03|
|ISO_TIME|Time with or without offset|10:15:30+01:00<br/>10:15:30|
|ISO_ORDINAL_DATE|Year and day of year|2012-337|
|ISO_WEEK_DATE|Year and Week|2012-W48-6|
|RFC_1123_DATE_TIME|RFC 1123 / RFC 822|Tue, 3 Jun 2008 11:05:30 GMT|

#### 로케일에 종속된 형식화
- DateTimeFormatter의 static메서드 `ofLocalizedDate()`, `ofLocalizedTime()`, `ofLocalizedDateTime()`은 `로케일(locale)`에 종속적인 포맷터를 생성한다.

```DateTimeFormatter
DateTimeFormatter formatter = DateTimeFormatter.ofLocalizedDate(FormatStyle.SHORT);
String shortFormat = formatter.format(LocalDate.now());
```

- FormatStyle의 종류에 따른 출력 형태는 다음과 같다.

|FormatStyle|날짜|시간|
|-|-|-|
|FULL|2015년 11월 28일 토요일|N/A|
|LONG|2015년 11월 28일(토)|오후 9시 15분 13초|
|MEDIUM|2015. 11. 28|오후 9:15:13|
|SHORT|15. 11. 28|오후 9:15|

#### 출력형식 직접 정의하기
- DateTimeFormatter의 `ofPattern()`으로 원하는 출력형식을 직접 작성할 수도 있다.
    - DateTimeFormatter formatter = DateTimeFormatter.ofPattern("yyyy/MM/dd");
- 출력형식에 사용되는 기호의 목록은 다음과 같다.

|기호|의미|보기|
|-|-|-|
|G|연대(BC, AD)|서기 또는 AD|
|y 또는 u|년도|2015|
|M 또는 L|월(1~12 또는 1월~12월)|11|
|Q 또는 q|분기(quarter)|4|
|w|년의 몇 번째 주(1~53)|48|
|W|월의 몇 번째 주(1~5)|4|
|D|년의 몇 번째 일(1~366)|332|
|d|월의 몇 번째 일(1~31)|28|
|F|월의 몇 번째 요일(1~5)|4|
|E 또는 e|요일|토 또는 7|
|a|오전/오후(AM,PM)|오후|
|H|시간(0~23)|22|
|k|시간(1~24)|22|
|K|시간(0~11)|10|
|h|시간(1~12)|10|
|m|분(0~59)|12|
|s|초(0~59)|35|
|S|천분의 일초(0~999)|7|
|A|천분의 일초(그 날의 0시 0분 0초부터의 시간)|80263808|
|n|나노초(0~999999999)|475000000|
|N|나노초(그 날의 0시 0분 0초부터의 시간)|81069992000000|
|V|시간대 ID(VV)|Asia/Seoul|
|z|시간대(time-zone) 이름|KST|
|O|지역화된 zone-offset|GMT+9|
|Z|zone-offset|+0900|
|X 또는 x|zone-offset(Z는 +00:00를 의미)|+09|
|'|escape문자(특수문자를 표현하는데 사용)|없음|

```DateFormatterEx1.java
import java.time.ZonedDateTime;
import java.time.format.DateTimeFormatter;

class DateFormatterEx1 {
    public static void main(String[] args) {
        ZonedDateTime zdateTime = ZonedDateTime.now();

        String[] patternArr = {
                "yyyy-MM-dd HH:mm:ss",
                "''yy년 MMM dd일 E요일",
                "yyyy-MM-dd HH:mm:ss.SSS Z VV",
                "yyyy-MM-dd hh:mm:ss a",
                "오늘은 올 해의 D번째 날입니다.",
                "오늘은 이 달의 d번째 날입니다.",
                "오늘은 올 해의 w번째 주입니다.",
                "오늘은 이 달의 W번째 주입니다.",
                "오늘은 이 달의 W번째 E요일입니다."
        };

        for (String p : patternArr) {
            DateTimeFormatter formatter = DateTimeFormatter.ofPattern(p);
            System.out.println(zdateTime.format(formatter));
        }
    }
}
```

```실행결과
2024-11-13 23:02:52
'24년 11월 13일 수요일
2024-11-13 23:02:52.097 +0900 Asia/Seoul
2024-11-13 11:02:52 오후
오늘은 올 해의 318번째 날입니다.
오늘은 이 달의 13번째 날입니다.
오늘은 올 해의 46번째 주입니다.
오늘은 이 달의 3번째 주입니다.
오늘은 이 달의 3번째 수요일입니다.
```

#### 문자열을 날짜와 시간으로 파싱하기
- 문자열을 날짜 또는 시간으로 변환하려면 static메서드 `parse()`를 사용하면 된다.
- parse()는 오버로딩된 메서드가 여러 개 있는데, 그 중에서 다음의 2개가 자주 쓰인다.

```parse
static LocalDateTime parse(CharSequence text)
static LocalDateTime parse(CharSequence text, DateTimeFormatter formatter)
```

- DateTimeFormatter에 상수로 정의된 형식을 사용할 때는 다음과 같이 한다.

```parse
LocalDate date = LocalDate.parse("2016-01-02", DateTimeFormatter.ISO_LOCAL_DATE);
```

- 자주 사용되는 기본적인 형식의 문자열은 ISO_LOCAL_DATE와 같은 형식화 상수를 사용하지 않고도 파싱이 가능하다.

```parse
LocalDate newDate = LocalDate.parse("2001-01-01");
LocalTime newTime = LocalTime.parse("23:59:59");
LocalDateTime newDateTime = LocalDateTime.parse("2001-01-01T23:59:59");
```

- 다음과 같이 `ofPattern()`을 이용해서 파싱을 할 수도 있다.

```ofPattern
DateTimeFormatter pattern = DateTimeFormatter.ofPattern("yyyy-MM-dd HH:mm:ss");
LocalDateTime endOfYear = LocalDateTime.parse("2015-12-31 23:59:59", pattern);
```

```DateFormatterEx2.java
import java.time.*;
import java.time.format.*;

class DateFormatterEx2 {
    public static void main(String[] args) {
        LocalDate newYear = LocalDate.parse("2016-01-01", DateTimeFormatter.ISO_DATE);

        LocalDate date = LocalDate.parse("2001-01-01");
        LocalTime time = LocalTime.parse("23:59:59");
        LocalDateTime dateTime = LocalDateTime.parse("2001-01-01T23:59:59");

        DateTimeFormatter pattern = DateTimeFormatter.ofPattern("yyyy-MM-dd HH:mm:ss");
        LocalDateTime endOfYear = LocalDateTime.parse("2015-12-31 23:59:59", pattern);

        System.out.println(newYear);
        System.out.println(date);
        System.out.println(time);
        System.out.println(dateTime);
        System.out.println(endOfYear);
    }
}
```

```실행결과
2016-01-01
2001-01-01
23:59:59
2001-01-01T23:59:59
2015-12-31T23:59:59
```

## 🚩 잊지않기
- `LocalDateTime`과 `ZonedDateTime`
    - LocalDate + LocalTime = LocalDateTime
    - LocalDateTime + 시간대(time zone) = ZonedDateTime
- LocalDateTime으로 ZonedDateTime 만들기
    - 기존에는 TimeZone클래스로 시간대를 다뤘지만 새로운 시간 패키지에서는 `ZoneId`라는 클래스를 사용한다.
    - LocalDateTime에 `atZone()`으로 시간대 정보를 추가하면, ZonedDateTime을 얻을 수 있다.
    - 사용가능한 ZoneId의 목록은 `ZoneId.getAvailableZoneIds()`로 얻을 수 있다.
- `OffsetDateTime`
    - ZoneId가 아닌 `ZoneOffset`을 사용하는 것
    - ZoneId는 `일광 절약 시간`처럼 시간대와 관련된 규칙들을 포함
    - ZoneOffset은 시간대를 `시간의 차이`로만 구분
    - 서로 다른 시간대에 존재하는 컴퓨터간의 통신에는 `OffsetDateTime`이 필요하다.
    - LocalDate와 LocalTime에 `ZoneOffset`을 더하거나, ZonedDateTime에 `toOffsetDateTime()`을 호출해서 얻을 수 있다.
- `TemporalAdjusters`
    - 자주 쓰일만한 날짜 계산들을 대신 해주는 메서드를 정의해놓은 것
- `Period`와 `Duration`
    - 날짜의 차이를 계산하기 위한 Period
    - 시간의 차이를 계산하기 위한 Duration
    - 차이를 구할 때는 `between()`을 사용한다.
    - 특정 필드의 값을 얻을 때는 `get()`을 사용한다.
    - Duration에는 getHours(), getMinutes() 같은 메서드가 없다.
    - `getUnits()`라는 메서드로 get()에 사용할 수 있는 ChronoUnit의 종류를 확인할 수 있다.

    ```getUnits
    System.out.println(pe.getUnits()); [Years, Months, Days]
    System.out.println(du.getUnits()); [Seconds, Nanos]
    ```

    - Duration을 LocalTime으로 변환한 다음에, LocalTime이 가지고 있는 get메서드들을 사용하면 간단하게 계산할 수 있다.

    - `between()`과 `until()`
        - between()은 `static메서드`
        - until()은 `인스턴스메서드`
    - `of()`
        - Period에는 of(), ofYears(), ofMonths(), ofWeeks(), ofDays()가 있다.
        - Duration에는 of(), ofDays(), ofHours(), ofMinutes(), ofSeconds() 등이 있다.
    - 특정 필드의 값을 변경하는 `with()`
        - Period에는 withMonths(), withDays()
        - Duration에는 withNanos()
    - 사칙연산, 비교연산, 기타 메서드
        - `plus()`, `minus()` 외에 곱셈과 나눗셈을 위한 메서드도 있다.
        - Period는 날짜의 기간을 표현하기 위한 것이므로 나눗셈을 위한 메서드가 없다.
        - 음수인지 확인하는 `isNegative()`, 0인지 확인하는 `isZero()`
        - 부호를 반대로 변경하는 `negated()`, 부호를 없애는 `abs()`
        - Period에는 abs()가 없어서 isNegative로 음수인지 확인한 후 negated()를 이용해야 한다.
        - Period에 `normalized()`라는 메서드가 있는데, 이 메서드는 월(month)의 값이 12를 넘기지 않게, 즉 1년 13개월을 2년 1개월로 바꿔준다. 일(day)의 길이는 일정하지 않으므로 그대로 놔눈다.
    - 다른 단위로 변환
        - 이름이 `to`로 시작하는 메서드들은 Period와 Duration을 다른 단위의 값으로 변환하는데 사용된다.
        - toTotalMonths(), toDays(), toHours(), toMinutes()
        - LocalTime에도 `toSecondOfDay()`와 `toNanoOfDay()` 메서드가 있어서, Duration을 사용하지 않고도 뺄셈으로 시간차이를 계산할 수 있다.
- 파싱과 포맷
    - `형식화(formatting)`와 관련된 클래스들은 `java.time.format`패키지에 들어있다.
    - 이 중에서 `DateTimeFormatter`가 핵심이다.
    - DateTimeFormatter의 `ofPattern()`으로 원하는 출력형식을 직접 작성할 수도 있다.
    - 문자열을 날짜 또는 시간으로 변환하려면 static메서드 `parse()`를 사용하면 된다.

    ```parse
    static LocalDateTime parse(CharSequence text)
    static LocalDateTime parse(CharSequence text, DateTimeFormatter formatter)
    ```