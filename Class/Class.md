# Class
- 상태 & 동작
- 다른 패키지에 있는 개체의 멤버 변수에 접근할 수 없음
- Java에서는 기본 자료형만 스택에 넣을 수 있어요.
- Java와 C# 모두 동적 할당한다.
- Java는 기본 자료형을 제외하면 모두 포인터다.

```java
accessModifier dataType varName;                          // 멤버 변수
 
accessModifier returnType funcName(매개변수 목록) {...}    // 멤버 함수

Human adam = new Human();                                 // 개체 생성
```

## 매개변수는 무조건 원본이 바뀜.

## 개체 생성 시 멤버 데이터의 초기화
- 0으로 준하는 값으로 초기화:
 - int : 0
 - float : 0.0
 - 참조형 : null


## '.' 연산자
- 개체 멤버에 접근할 때 사용 


## 멤버함수 호출
- someclass.somefunc();


## Java는 자동으로 메모리를 해제해줌 : garbage collection


## 생성자
- 개체를 생성할 때 호출된다. 클래스명과 같다.
- 멤버 변수값 초기화 가능
- 오버로딩 가능
- 아무 생성자도 작성 안하면 기본 생성자를 자동으로 만들어 줌
- 생성자로 초기화 해주는 게 좋음


## 접근 제어자 access modifier
- public : 누구나 접근 가능
- protected : 자식들만 접근 가능
- praivate : 외부 접근 금지, 클래스 내부에서만 접근 가능
- 생략할 경우 : 같은 패키지에 속한 클래스들만 접근 가능

### 일반적으로
- 맴버 변수 : private or protected
- 메서드 : public
- 멤버 변수 접근은 메서드를 통해서만
- private멤버 함수 대부분은 코드 중복을 피하기 위해서 사용함

### 같은 클래스에 속한 개체끼리는 private멤버에 접근 가능


## getter/setter
- private 멤버 변수의 값을 읽어 올때 : public getter
- private 멤버 변수의 값을 바꿀 때 : public setter


