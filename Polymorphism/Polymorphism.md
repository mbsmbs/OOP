# Polymorphism
- 같은지시 다른 종류의 개체가 동작을 다르게 한다.
- 상속 관계 필요
- 부모 개체 : 함수 선언
- 자식 개체 : 함수 구현 (Overriding)

## 무조건 실제 개체 자료형에 있는 메서드가 호출
- 다형성은 무늬가 아닌 실체를 따라간다
```java
Animal animal = new Dog();
animal.shout();

Animal animal = new Animal();
animal.shout();
```
