---
title:  "인터페이스(Interface) - 2편" 

categories:
  -  C#
tags:
  - [C#]

published: true
toc: true
toc_sticky: true

date: 2022-07-04
last_modified_at: 2022-07-04
---

# 인터페이스(Interface) 2편

---
인터페이스는 상속이 가능하다.
인터페이스를 상속할 수 있는 것은 클래스만 있는 것이 아니다. 구조체는 물론이고, 인터페이스도 인터페이스를 상속할 수 있다. 기존의 인터페이스에 새로운 기능을 추가한 인터페이스를 만들고 싶을 때 인터페이스를 상속하는 인터페이스를 만들면 된다.

가능하다면 그렇게 해도 된다. 다만, 인터페이스를 수정할 수 없는 상황이 있기 마련이다.

 

  >상속하려는 인터페이스가 소스 코드가 아닌 어셈블리로만 제공되는 경우
  >상속하려는 인터페이스의 소스 코드를 갖고 있어도, 이미 인터페이스를 상속하는 클래스들이 존재하는 경우

첫 번째 문장의 의미는, .NET SDK에서 제공하는 인터페이스들 같은 경우에는 어셈블리 안에 있기 때문에 인터페이스를 수정할 수가 없다. 따라서, 이 인터페이스에 새로운 기능을 추가하고 싶다면 상속하는 수 밖에 없다.

 

두 번째 문장의 의미는, 인터페이스를 상속받은 클래스는 반드시 인터페이스의 모든 메소드와 프로퍼티를 구현해야 한다. 예를 들어, 다음과 같이 코드가 있다고 한다.
```c#
interface ILogger      
{
    void WriteLog(string message);
}

class ConsoleLogger : ILogger
{
    public void WriteLog(string message)
    {
        ...
    }
}

class FileLogger : ILogger
{
    public void WriteLog(string message)
    {
        ...
    }
}
```

ConsoleLogger 클래스와 FileLogger 클래스는 ILogger 인터페이스를 상속받아, ILogger 인터페이스의 WriteLog() 메소드를 강제 구현한 상태다. 그런데, 여기에서 ILogger 인터페이스를 수정하거나 새로운 기능을 넣는다면?

```c#
interface ILogger
{
    void WriteLog(string message);
    void NewMethod();    // 새로운 기능 추가
}


/*  새로 기능이 추가됐기 때문에 상속받은 클래스들은 이 기능을 구현하지 않아 컴파일 오류가 뜬다.
class ConsoleLogger : ILogger
{
    ...
}

class FileLogger : ILogger
{
    ...
}
*/
```
즉, 기존에 상속받았던 모든 클래스들을 수정해줘야 하는 번거로움이 발생한다.

그렇기 때문에 이런 수고를 덜기 위해서는 새로운 인터페이스를 만들어 상속받아 사용하는 것이 좋다.

## 인터페이스의 기본 구현 메소드
인터페이스에서 구현부가 없었던 기존 메소드와 달리, 구현부를 가지는 메소드를 말한다.

인터페이스 참조로 업캐스팅을 했을 때만 사용할 수 있다. 어떤 경우에 사용하는 걸까?

어떤 한 인터페이스를 정의했고, 그것을 상속받는 수많은 파생 클래스들이 있다고 가정해보자.
```c#
interface IExample
{
    void Function1();
}

class Example1 : IExample
{
    public void Function1()
    {
        ...
    }
    ...
}

class Example2 : IExample
{
    public void Function1()
    {
        ...
    }
    ...
}

class Example3 : IExample
{
    public void Function1()
    {
        ...
    }
    ...
}
...
```

이런 코드들을 레거시(Legacy : 유산)라고 한다. 레거시 코드는 업그레이드에 각별한 주의가 필요하다.

아무튼, 위와 같은 상황에서 초기 설계에서 놓친 메소드를 인터페이스에 안전하게 추가할 수 있을까?
```c#
interface IExample
{
    void Function1();
    void Function2();    // 새로 추가한 메소드
}


/*      컴파일 오류
class Example1 : IExample
{
    ...
}

class Example2 : IExample
{
    ...
}

class Example3 : IExample
{
    ...
}

...
*/
```
이럴 때, 기본 구현 메소드가 사용된다. 인터페이스를 수정했지만, 다른 기존 코드에는 아무런 영향을 끼치지 않는다.
```c#
interface IExample
{
    void Function1();
    void Function2()        // 기본 구현 메소드
    {
       Console.WriteLine("새로 추가한 기능");
    }
}


class Example1 : IExample
{
    ...
}

class Example2 : IExample
{
    ...
}

class Example3 : IExample
{
    ...
}

...
```
또한, 위에서 설명했지만 인터페이스 참조로 업캐스팅 했을 때만 기본 구현 메소드를 호출할 수 있다.
```c#
// 인터페이스 참조
IExample example = new Example1();
example.Function1();
example.Function2();


// 파생 클래스 참조
Example1 classExample = new Example1();
classExample.Function1();
// classExample.Function2();    불가능
```
 

파생 클래스 참조로 기본 구현 메소드를 호출하려고 하면 컴파일 오류를 일으킨다.

해당 파생 클래스는 기본 구현 메소드를 오버라이딩하지 않았기 때문이다.
[맨 위로 이동하기](#){: .btn .btn--primary }{: .align-right}