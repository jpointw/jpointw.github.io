---
title:  "인터페이스(Interface) - 1편" 

categories:
  -  C#
tags:
  - [C#]

published: true
toc: true
toc_sticky: true

date: 2022-07-02
last_modified_at: 2022-07-02
---

# 인터페이스(Interface) 

---
다중상속이 허용되지 않는 C#에서 인터페이스(Interface)는 객체지향 프로그래밍을 한층 더 강력하게 만들어주는 요소다. 객체지향의 꽃이며, 객체지향 프로그래밍의 고수는 인터페이스를 잘 활용할 수 있어야 한다고 한다.


```c#
interface 인터페이스이름
{
    반환형 메소드이름1(매개변수 목록);
    반환형 메소드이름2(매개변수 목록);
    ...
}
```
인터페이스는 메소드, 이벤트, 인덱서, 프로퍼티만 가질 수 있다.
접근 제한 한정자를 사용할 수 없고, 모든 것이 public으로 선언된다.
인터페이스는 자신을 상속받는 클래스에게 오버라이딩을 강제한다.
자식 클래스에서 구현할 메소드들은 public 한정자로 수식해야 한다.
구현부가 없다.
인스턴스를 만들 수 없지만, 인터페이스를 상속받는 클래스의 인스턴스를 만드는 것은 가능하다.
클래스는 인터페이스를 여러 개 상속 받는 것이 가능하다.

튜플이 System.ValueTuple 구조체를 기반으로 만들어지기 때문에 위와 같이 담긴다고 한다.

## 인터페이스는 상호간의 약속

필드명:의 꼴로 필드의 이름을 지정하여 선언할 수도 있다.

```C#
interface ILogger       // C#에서는 인터페이스명 첫 글자에 'I'를 붙이는 것이 암묵적인 룰이다.
{
    void WriteLog(string message);
}
```
위 인터페이스를 상속받아 구현한 ConsoleLogger 클래스이다.

ConsoleLogger 클래스는 WriteLog(string message) 메소드를 강제로 구현해야 한다.

```c#
class ConsoleLogger : ILogger
{
    public void WriteLog(string message)
    {
        Console.WriteLine("{0} {1}", DateTime.Now.ToLocalTime(), message);
    }
}
```
인터페이스 인스턴스는 생성하지 못하지만, 참조는 만들 수 있다.

```c#
ILogger logger = new ConsoleLogger();
logger.WriteLog("Hello, World!");
```

[맨 위로 이동하기](#){: .btn .btn--primary }{: .align-right}