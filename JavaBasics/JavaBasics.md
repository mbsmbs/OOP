# Java 언어의 기본 문법
- 언제나 클래스가 필요
- c#과는 달리 한 .java 파일에는 최고 레벨 public 클래스가 하나만 있어야 함

# 1. Hello World
```java
public class HelloWorld
{
  public static void main(String[] args)
  {
    System.out.println("Hello World!");     // 출력
  }
}
```

### 여러 출력 함수들 이 있다 : println(), print(), printf()

### 새 줄 문자 : '\n' 대신 lineSeparator()가 좋음.
```java
System.out.printf("%s's score: %d%s", name, score, System.lineSeparator());
```

### 가변 인자
```java
함수(dataType... 매개변수 이름){...}
```
- C와는 달리 자료형을 꼭 넣어야 함
- Object라는 자료형을 쓸 경우 모든 자료형을 넣을 수 있음.

## 2. package
```java
package folder.packageName;

import packagename.Classname;          // C #include & C# using과 같음
import packagename.*;
```
- C#에서 namespace와 비슷
- package = folder
- built-in package & user-defined package
- 이름 충돌을 피함
- 패키지 이름만 적으면 안됨. 폴더 구조와 일치 해야 함.
- 패키지명과 똑같은 폴더 트리에 .java 파일을 넣어야 함


## 3. 빌드 및 실행
### 빌드
```
java -d <컴파일 결과물을 저장할 경로> <컴파일할 .java 파일>
```
- java파일이 무사히 컴파일 되면 .class파일이 나옴
  - .java파일과 같은 이름
  - .class 파일에는 바이트 코드가 있음
- 옵션: -d
  - .class 파일을 저장할 경로
- <컴파일 결과물을 저장할 경로>에 .java파일의 패키지 구조와 동일한 폴더가 생김

### 실행
```
java -classpath <class 파일 위치> <클래스 이름>
```
- C나 C#처럼 컴파일하면 실행 파일이 나오지 않고 대신 바이트 코드가 나옴 (.class) 가 나옴
- 이 바이트 코드를 실행
- <클래스 이름> : 실행할 .class 파일 이름, 반드시 main 함수가 들어 있어야 함
- 클래스 이름 앞에는 반드시 패키지 이름을 붙여야 함.

### 내가 만든 라이브러리나 프로그램을 배포하는 방법
- C#의 경우 :
  - 라이브러리 : .dll
  - 프로그램 : .exe

- Java : 둘 다 .jar

### jar 명령어도 있다.


## 실행 모델
- 바이트 코드란 JVM이 이해하는 명령어
- JVM이 실행 중에 최종 플랫폼에 맞는 명령어로 바꿔서 실행해줌
- JVM: Java Virtual Machine


## Data types
- C#처럼 자료형 크기가 고정
- char만 빼고 unsigned가 없다
- 기본 자료형은 모두 값형
- 클래스는 참조형 

### byte, short, int, long, float, double, boolean, char

### String : class
- new 키워드 사용 가능
- Immutable 변경 불가
- 바꾸고 싶다면 새로운 String 만들어야

### 리터럴
```java
int num1 = 1234;
long num2 = 1234567890L;
float num3 = 3.14f;
double num4 = 8457.91;   // d 나 D붙여도 되지만 생략 가능
String str1 = "...";
char ch = 'a';
null                     // 참조형에 사용 가능
```

## final
```java
public final int MAX_STUDENT= 10;
```
- const 대신 final
- 시용 가능한 곳:
  - 지역 변수
  - 클래스 멤버 변수
  - 메서드와 매개변수
  - 클래스와 메서드
- C#과 달리 java는 final에 static을 붙일 수 있다.
- final은 선언 후에 초기화 가능 단 사용하기 전에 해야 함


## 주석
- //
- /*...*/
- /**...*/
- @...

## 연산자
### 연산자 우선순위가 있다

### 산술 연산자
- 기본적으로 숫자를 표현하는 자료형만 피연산자로 사용 가능
- char는 가능
- 문자열 가능 (더하기)

### 대입 연산자
- 값형 참조형 주의하자

### Casting 가능

### 논리 연산자 비슷


## 조건문
### if
```java
if(expression)
{
  ...
}
else if(expression)
{

}
else
{

}

### switch/case
- case에 사용 가능한 자료형 : char, byte, int, enum, String
- break빼면 C와 같이 fall-through


## 반복문
- for
- while
- do while
- continue, break
- goto와 비슷한 것 : break 라벨이름;
  - break를 감싸고 있는 라벨로만 점프 가능
  ```java
  outer:
  for(...)
  {
    if(..)
    {
      break outer;
    }
  }
  ```
- foreach 스타일 for
```java
for(int score : scores)
{
  ...
}
```

## 함수
- C#과 거의 비슷

### 함수 호출과 참조형 인자
- 메서드 안에서 참조형 인자의 값을 바꾸면 원본이 바뀜

## 배열
### 1차원
```java
int[] new int[5];
int scores[] = new int[20];

### 다차원
- 포인터 배열과 비슷하다
- 배열의 배열이다.
```java
int[][] checkerboard = new int[8][8];
float[][][] world = new float[100][100][100];
```

## 열거형
- 클래스 내부에 선언할 수 있음
- 각 원소에 원하는 값을 대입 불가능
- 클래스다
- 생성자 추가 가능
  - 언제나 private
  - new 키워드 사용 불가 
  - 사용은 C/C#과 동일
  
  
## var
- 컴파일러가 알아서 자료형을 추론해줌
- 선언과 동시에 대입
- C# 과 비슷
- 배열 선언시 [] 사용 불가능, {} 대입 불가능


## 람다
- 이름 없는 함수
```
() -> 표현식
() -> {표현식}
```


## 모듈
- 패키지 상위, 내포 개념
