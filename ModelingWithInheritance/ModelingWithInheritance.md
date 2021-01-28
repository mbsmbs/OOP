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
|-seconds: int = 0|
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
