---
title:  "인터페이스와 클래스의 사이 : 추상 클래스(Abstract Class)" 

categories:
  -  C#
tags:
  - [C#]

published: true
toc: true
toc_sticky: true

date: 2022-07-05
last_modified_at: 2022-07-05
---

# 추상 클래스 (Abstract Class)

---
구현부를 가질 수 있지만, 인스턴스는 생성할 수 없다.
추상 클래스는 구현부를 가질 수 있다. 구현부를 가질 수는 있지만, 클래스와 달리 인스턴스 생성은 할 수 없다.

하지만 추상 클래스를 상속 받은 클래스의 인스턴스는 생성 가능하며, 업캐스팅 또한 가능하다.

 

추상 클래스 선언은 다음과 같이 absract 키워드를 사용하여 선언한다.

```c#
abstract class 클래스이름
{
    // 클래스와 동일하게 구현
}
```
접근성 측면에서 본다면, 클래스와 더 가깝다.

인터페이스는 모든 메소드가 public으로 선언되는 반면, 클래스는 한정자를 명시하지 않으면 모든 메소드가 private으로 선언된다.

## 추상 메소드(Abstract Method)를 가질 수 있다.

```c#
abstract class AbstractClass
{
    public abstract void AbstractMethod();    // 추상 메소드
}


class DerivedClass : AbstractClass
{
    public override void AbstractMethod()     // 추상 메소드 구현 강제
    {
        ...
    }
}
```

추상 메소드는 추상 클래스가 인터페이스의 역할을 할 수 있게 해주는 장치이다.

구현을 갖지는 못하지만, 자식 클래스에서 반드시 구현하도록 강제한다.

자식 클래스들은 반드시 이 추상 메소드들을 가지고 구현해놨을 거라는 일종의 약속인 셈이다.

## 추상 메소드의 기본 접근성은 무엇일까?
```c#
abstract class AbstractClass
{
    abstract void AbstractMethod();   // public? private?
}
```
추상 클래스나 클래스는 그 안에서 선언되는 모든 필드, 메소드, 프로퍼티, 이벤트 모두 접근 한정자를 명시하지 않으면 private으로 간주한다. 여기에 추상 메소드 또한 예외가 될 수는 없다.

 

하지만 약속 역할을 하는 추상 메소드가 private이라는 것이 말이 안 된다.

그래서 C# 컴파일러는 추상 메소드가 반드시 public, protected, internal, protected internal 한정자 중 하나로 수식될 것을 강요한다.

## 추상 클래스가 또 다른 추상 클래스를 상속하는 경우

"그래서, 추상 클래스는 왜 사용하는건데?"
추상 클래스를 사용하지 않으면 어떤 일이 발생할까? 만약 팀끼리 일을 하는데, 다음과 같은 말을 들었다고 생각 해보자.

 

 

"이 클래스는 직접 인스턴스화하지 말고 자식 클래스를 만들어 사용하세요.
Method1()과 Method2()는 꼭 오버라이딩 해야 합니다." 

 

 

프로그래머도 사람이기 때문에 아무리 이런 좋은 메뉴얼이 있다고 하더라도, 일이 많다보면 까먹을 수도 있다.

추상 클래스를 사용하면 이런 문제가 발생할 일이 없어진다.

컴파일러가 오버라이딩을 강제하도록 요구하고, 안 하면 컴파일 오류를 발생시키기 때문이다.

 

이것이 추상 클래스를 사용하는 이유다.

 
[맨 위로 이동하기](#){: .btn .btn--primary }{: .align-right}