# 상속을 이용한 개체 모델링

## 모델링 대상 : 벽시계
- 기본 상태와 메서드

|Clock|
|---|
|-hours: byte = 12|
|-minutes: byte = 0|
|-seconds: byte = 0|
||
|+Clock()|
|+getHours(): byte|
|+setHours(byte)|
|+getMinutes(): byte|
|+setMinutes(byte)|
|+getSeconds(): byte|
|+setSeconds(byte) |
|+setTime(byte, byte, byte)|

### 이 클래스의 문제 : 음수가 들어올 수 있다.

### 해결법 1 : 최솟값, 최댓값으로 제한하기
- 최소값보다 작은 값이 들어오는 경우, 최솟값으로 변경
- 최솟값보다 큰 값이 들어오는 경우, 최댓값으로 변경

### 해결법 2 : 최댓값/최솟값으로 래핑하기
- 최솟값/최대값 원형으로 연결
- 시 : 12가 넘으면 1로
- 분, 초 : 59가 넘으면 0으로

```java
public void setHours(byte hours)
{
  int value = hours - 1;
  
  while (value<0)
  {
    value += 12;
  }
  
  this.hours = (byte)(value % 12+1);
}

public void setMinutes(byte minutes)
{
  while(minutes < 0)
  {
    minutes += 60;
  }
  
  this.minutes = (byte) (minutes % 60);
}

public void setSeconds(byte seconds)
{
  while(seconds < 0)
  {
    seconds += 60;
  }
  
  this.seconds = (byte) (seconds % 60);
}
```

### 받아올림을 적용한 코드
```java
public void setMinutes(byte minutes)
{
  int wrapCount = 0;
  
  while(minutes < 0)
  {
    --wrapCount;
    minutes += 60;
  }
  
  wrapCount += minutes / 60;
  
  this.minutes = (byte)(minutes % 60);
  
  if(wrapCount != 0)
  {
    setHours((byte)(this.hours + wrapCount));
  }
}
```

### 메서드 호출 순서 주의

### 시간 입력을 개선한  Clock 클래스
- 모든 setter 제거
- addSeconds(short) : 시간을 초로 환산해서 입력

|Clock|
|---|
|-hours: byte = 12|
|-minutes: byte = 0|
|-seconds: byte = 0|
||
|+Clock()|
|+getHours(): byte|
|+getMinutes(): byte|
|+getSeconds(): byte|
|+addSeconds(short)|

```java
public void addSeconds(short seconds)
{
  final int HALF_DAY_IN_SECONDS = 60 * 60 * 12;
  
  int value = this.seconds + seconds;
  while(value < 0)
  {
    value += HALF_DAY_IN_SECONDS;
  }
  
  this.seconds = (byte)(value % 60);
  
  value = value / 60;
  value += this.minutes;
  
  this.minutes = (byte) (value % 60);
  
  value = value / 60;
  value += this.hours - 1;
  this.hours = (byte) (value % 12 + 1);
}
```

### 너무 많은 상태를 변경하기 때문에 복잡하다

### 해결책 : 모든 것을 초로 저장하게 수정한 클래스
|Clock|
|---|
|-seconds: int = 0|
||
|+Clock()|
|+getHours(): byte|
|+getMinutes(): byte|
|+getSeconds(): byte|
|+addSeconds(short)|

```java
public class Clock
{
  private int seconds;
  
  public byte getHours()
  {
    int hours = this.seconds / 60 / 60;
    
    return hours == 0 ? 12 : (byte) hours;
  }
  
  public byte getMinutes()
  {
    return (byte) (this.seconds / 60 % 60);
  }
  
  public byte getSeconds()
  {
    return (byte) (this.seconds % 60);
  }
  
  public void addSeconds(short seconds)
  {
    final int HALF_DAY_IN_SECONDS = 60 * 60 * 12;

    int value = this.seconds + seconds;
    while(value < 0)
    {
      value += HALF_DAY_IN_SECONDS;
    }

    this.seconds = value % HALF_DAY_IN_SECONDS;
  }
}
```

### 시침, 분침, 초침의 각도를 추가 + 알파
|Clock|
|---|
|-hours: byte = 12|
|-minutes: byte = 0|
|-seconds: byte = 0|
||
|+Clock()|
|+getHours(): byte|
|+getMinutes(): byte|
|+getSeconds(): byte|
|+addSeconds(short)|
|+getSecondHandAngle(): short|
|+getMinuteHandAngle(): short|
|+getHourHandAngle(): short|
|+tick()|
|+mount()|


## 디지털 벽시계 추가
- 두 시계의 공통점은 부모 클래스로 만들고 차이점은 자식 클래스로 분리하면 된다.

|Clock|
|---|
|-seconds: int = 0|
||
|+Clock()|
|+getHours(): byte|
|+getMinutes(): byte|
|+getSeconds(): byte|
|+addSeconds(short)|
|+getSecondHandAngle(): short|
|+getMinuteHandAngle(): short|
|+getHourHandAngle(): short|
|+tick()|
|+mount()|

- 분리

|Clock|
|---|
|-seconds: int = 0|
||
|+Clock()|
|+getHours(): byte|
|+getMinutes(): byte|
|+getSeconds(): byte|
|+tick()|
|+mount()|

|AnalogClock|
|---|
||
|+AnalogClock()|
|+addSeconds(short)|
|+getSecondHandAngle(): short|
|+getMinuteHandAngle(): short|
|+getHourHandAngle(): short|

AnalogClock extends Clock

### tick() 수정
```java
public void tick()
{
  final int HALF_DAY_IN_SECONDS = 60 * 60 * 12;
  this.seconds = (this.seconds + 1) % HALF_DAY_IN_SECONDS;
}
```

### 부모 클래스에 있는 private 멤버 변수 -> protected로 변경

### final변수 메서드 밖에 정의해서 중복을 피하자
```java
// 부모 클래스에
protected static final int HALF_DAY_IN_SECONDS = 60 * 60 * 12;
```

### 클래스 다이어그램에서 # : protected 의미

### 클래스 다이어그램에서 상수는 표시하지 않는다.
```java
public class Clock
{
  protected static final int HALF_DAY_IN_SECONDS = 60 * 60 * 12;
  protected int seconds;
  
  public byte getHours()
  {
    int hours = this.seconds / 60 / 60;
    
    return hours == 0 ? 12 : (byte) hours;
  }
  
  public byte getMinutes()
  {
    return (byte) (this.seconds / 60 % 60);
  }
  
  public byte getSeconds()
  {
    return (byte) (this.seconds % 60);
  }
  
  public void tick()
  {
    this seconds = (this.seconds + 1) % HALF_DAY_IN_SECONDS;
  }
  
  public void mount()
  {
    ...
  }
}
```
```java
public class AnalogClock extends Clock
{
  public short gerSecondHandAngle()
  {
    return (short) (getSeconds * 6);
  }
  
  public short getMinuteHandAngle()
  {
    return (short) (getMinutes() * 6);
  }
  
  public short getHourHandAngle()
  {
    final int ANGLE_PER_HOUR = 360 / 12;
    
    int hours = getHours() % 12;
    return (short) (hours * ANGLE_PER_HOUR + getMinutes() * ANGLE_PER_HOUR / 60);
  }
  
  public void addSeconds(int seconds)
  {
    int value = this.seconds;
    while(value < 0)
    {
      value += HALF_DAY_IN_SECONDS;
    }
    
    this.seconds = value % HALF_DAY_IN_SECONDS;
  }
}
```

### 디지털 벽시계 전용 기능
1. 오전/오후 구분하기
- 내부적으로 24시간을 사용하되 아날로그 출력 방법을 변경
  - 12 ~ 24시간 범위에서는 12를 뺌
  - 12 ~ 24시간 범위내에 있으면 PM, 그 외에는 AM

|Clock|
|---|
|#seconds: int = 0|
||
|+Clock()|
|+getHours(): byte|
|+getMinutes(): byte|
|+getSeconds(): byte|
|+tick()|
|+mount()|

|AnalogClock|
|---|
||
|+AnalogClock()|
|+addSeconds(short)|
|+getSecondHandAngle(): short|
|+getMinuteHandAngle(): short|
|+getHourHandAngle(): short|

|DigitalClock|
|---|
|+DigitalClock()|
|+isBeforeMidday(): boolean|

AnalogClock, DigitalClock -> Clock

```java
public class Clock
{
  protected static final int DAY_IN_SECONDS = 60 * 60 * 12;
  protected int seconds;
  
  public byte getHours()
  {
    int hours = this.seconds / 60 / 60;
    hours = hours % 12;
    
    return hours == 0 ? 12 : (byte) hours;
  }
  
  public byte getMinutes()
  {
    return (byte) (this.seconds / 60 % 60);
  }
  
  public byte getSeconds()
  {
    return (byte) (this.seconds % 60);
  }
  
  public void tick()
  {
    this seconds = (this.seconds + 1) % DAY_IN_SECONDS;
  }
  
  public void mount()
  {
    ...
  }
}
```
```java
public class AnalogClock extends Clock
{
  ...
  
  public void addSeconds(int seconds)
  {
    int value = this.seconds;
    while(value < 0)
    {
      value += HDAY_IN_SECONDS;
    }
    
    this.seconds = value % DAY_IN_SECONDS;
  }
}
```

```java
public class DigitalClock extends Clock
{
  public boolean isBeforeMidday()
  {
    return (super.seconds / (DAY_IN_SECONDS / 2) == 0);
  }
}
```

- 자식 클래스를 추가함에 따라 부모 클래스가 변경될 수도 있다.


2. 시간 맞추는 방식

|DigitalClock|
|---|
|+setIsBeforeMidday(boolean)|
|+setHour(byte)|
|+setMinutes(byte)|
|+setSeconds(byte)|
|+setTime(byte, byte, byte, boolean)|

3. 7 세그먼트 디스플레이를 이용한 시간 출력
.
.
.


# 상속은 1 ~ 2단계만 상속하는게 보통
# 깊어질수록 추상화 능력이 필요
