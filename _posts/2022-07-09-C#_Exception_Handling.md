---
title:  "프로그램이 어떠한 상황에서도 잘 견딜 수 있도록! : 예외 처리(Exception Handling)" 

categories:
  -  C#
tags:
  - [C#]

published: true
toc: true
toc_sticky: true

date: 2022-07-09
last_modified_at: 2022-07-09
---

# 예외 처리 (Exception Handling)

---

배열 객체에 잘못된 인덱스를 통해 접근하면, 배열 객체가 이 문제에 대한 상세 정보를 IndexOutOfRangeException의 객체에 담는다.

```c#
class Program
{
    static void Main(string[] args)
    {
        int[] array = { 1, 2, 3 };

        for(int i = 0; i < 5; i++)
        {
            Console.WriteLine(array[i]);     // 배열의 인덱스를 넘어서는 예외가 발생함
        }                                    // 예외가 발생한 부분 이후의 코드들은 실행되지 않음
        Console.WriteLine("프로그램 정상 종료");       // 실행되지 않음

    }
}
```

그리고 호출자(Main() 메소드)에게 던져지게 되는데, Main() 메소드는 이런 예외를 처리하는 부분이 없다.

예외를 처리할 방도가 없기 때문에 이 예외는 다시 CLR에게 던져지게 되고, CLR까지 던져지게 된 예외는 처리되지 않은 예외가 된다. CLR은 처리되지 않은 예외를 받게 되면, 예외 객체에 담긴 내용을 출력한 후 프로그램을 강제 종료한다.

 

프로그램 기능이 아무리 많더라도, 예외 처리를 잘 해놓지 않으면 신뢰할 수 없는 프로그램이 된다.

어떻게 예외 처리를 해야 하는지 다음 절에서 살펴보자.

## try ~ catch로 예외 받기
```C#
try
{
    // 실행하고자 하는 코드
}
catch ( 예외 객체1)
{
    // 예외가 발생했을 때 처리할 코드
}
catch ( 예외 객체2)
{
    // 예외가 발생했을 때 처리할 코드
}
```
try 절의 코드 블록에는 예외가 일어나지 않을 경우 실행되어야할 코드들이 들어간다.

catch 절의 코드 블록에는 예외가 발생했을 때의 처리 코드들이 들어간다.

 

try 절에서 원래 실행하고자 했던 코드를 쭉 처리해나가다가 예외가 던져지면, catch 블록이 이 예외를 받아낸다.

이때, catch 절은 try 블록에서 던질 예외 객체와 형식이 일치해야 한다.

 

catch 절에서 예외를 받지 못하면 처리되지 않은 예외로 남게 되며, try 절에서 여러 종류의 예외를 던질 가능성이 있다면 catch 절 블록을 여러 개 둘 수도 있다.

```C#
class Program
{
    static void Main(string[] args)
    {
        int[] array = { 1, 2, 3 };

        try
        {
            for(int i = 0; i < 5; i++)
            {
                Console.WriteLine(array[i]);
            }                                   
         }
         catch(IndexOutOfRangeException e)
         {
             Console.WriteLine($"예외가 발생했습니다 : {e.Message}");
         }
        
         Console.WriteLine("프로그램 정상 종료");    // 예외처리하여 이제 실행됨
    }
}
```

## System.Exception 클래스

모든 예외의 조상 클래스. C#에서 모든 예외 클래스들은 반드시 이 클래스로부터 상속받아야 한다.

앞에서 사용했던 IndexOutOfRangeException 또한 Exception 클래스를 상속받은 C#에서 제공하는 예외 클래스이다.

Exception 클래스는 모든 예외 클래스들의 조상이기 때문에, 모든 예외를 다 받아낼 수 있다.

```C#
try
{
   ...
}
catch(Exception e)     // 모든 예외를 받을 수 있다
{
   ...
}
```

모든 예외를 받아낼 수 있지만, 상황에 따라 섬세한 예외 처리가 필요할 때가 있다.

프로그래머가 예상한 예외 말고도 다른 예외들까지 받아내기 때문에 때로는 버그를 발생하는 코드로 간주될 수 있다.

따라서, 코드를 면밀히 검토해 처리하지 않아야 할 예외까지 처리하지 않도록 조심해서 사용해야 한다.

## 예외 던지기  :  throw

try~catch 문으로 예외를 받는다는 것은 어디선가 예외를 던진다는 이야기다.

예외는 throw 문을 통해 던질 수 있다.

```C#
try
{
    ...
    throw new Exception("예외 토스!");
}
catch(Exception e)       // throw 문을 통해 던져진 예외 객체는 catch 문을 통해 받는다.
{
    Console.WriteLine(e.Message);
}
```

위와 같이 try~catch 구문 안에서 예외를 던져도 되고,

메소드 안에서 예외를 던지고 호출자 측의 try~catch 문에서 받아낼 수도 있다.

```C#
class Program
{
    static void Main(string[] args)
    {
        try
        {
            DoSomething(13);
        }
        catch(Exception e)
        {
            Console.WriteLine(e.Message);
        }
    }

    static void DoSomething(int arg)
    {
        if (arg < 10)
            Console.WriteLine("arg : {0}", arg);
        else
            throw new Exception("arg가 10보다 큽니다.");
    }
}
```

throw는 보통 문(statement)으로 사용하지만, C# 7.0부터는 식(expression)으로도 사용할 수 있게 개선되었다.

```C#
int? a = null;
int b = a ?? throw new ArgumentNullException();

// a는 null이므로 b에 a를 할당하지 않고 throw 식이 실행됨
```

조건 연산자에서도 식으로 사용할 수 있다.

```c#
int array = new[] {1, 2, 3};
int index = 4;
int value = array[ index >= 0 && index < 3 ? index : throw new IndexOutOfRangeException()];
```

## 너는 꼭 실행되도록 보장할게!  :  finally
try 블록에서 코드를 실행하다가 예외가 던져지면 프로그램의 실행이 catch 절로 바로 뛰어 넘어온다.

만약 예외 때문에 try 블록의 자원 해제 같은 중요한 코드를 미처 실행하지 못한다면 이는 곧 버그를 만드는 원인이 된다.

그렇기 때문에 try 블록 끝 부분에 중요한 코드들을 적지 말고, 예외가 발생하더라도 반드시 실행하도록 해주는 finally 블록에다 작성해야 한다. finally 절은 try~catch 문의 제일 마지막에 연결해서 작성한다.

```C#
try
{
    ...
}
catch(Exception e)
{
    ...
}
finally
{
    // 예외가 발생하더라도 반드시 실행된다.
}
```

자신이 소속된 try 절이 실행된다면 finally 절은 어떤 경우라도 실행된다.

심지어, 프로그램의 실행 흐름이 변경되게 하는 return, throw 문이 try 블록 내부에 있어도 finally 블록은 반드시 실행된다. 또한, 예외가 발생하던 발생하지 않던 무조건 finally 블록은 실행된다. 이 점을 기억하자.

## finally 안에서 예외가 또 발생한다면?
그러면, finally 블록 안에서 받아주거나 처리해주는 코드가 없으므로 해당 예외는 처리되지 않은 예외가 된다.

최대한 예외가 일어나지 않도록 하되, 장담할 수 없다면 finally 안에서 다시 한번 try~catch로 묶어주는 것도 방법이 된다.

## 사용자 정의 예외 클래스
System.Exception 클래스를 상속받아서 사용자가 원하는 새로운 예외 클래스를 작성해주면 된다.
```C#
class MyException : Exception
{
   public int ErroNo {get; set;}
}
```
그리고 throw 문을 통해 예외를 던져주면 된다.
```C#
try
{
   ...
   throw new MyException();
}
catch(MyException e)
{
    ...
}
```
만약 예외를 필터링해서 받고 싶다면, when을 통한 추가 조건을 주면 된다.
```C#
try
{
   ...
}
catch(MyException e) when (e.ErroNo < 0)
{
   ...
}
```
[맨 위로 이동하기](#){: .btn .btn--primary }{: .align-right}