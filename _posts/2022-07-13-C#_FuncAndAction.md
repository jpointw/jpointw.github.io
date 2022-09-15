---
title:  "더 간편하게 무명 함수 만들기! : Func, Action" 

categories:
  -  C#
tags:
  - [C#]

published: true
toc: true
toc_sticky: true

date: 2022-07-13
last_modified_at: 2022-07-13
---

# Func
---

결과를 반환하는 메소드를 참조하기 위해 만들어졌다.

.NET에는 모두 17가지 버전의 Func 대리자가 있다.

```C#
public delegate TResult Func<out TResult>()
public delegate TResult Func<in T, out TResult>(T arg)
public delegate TResult Func<in T1, in T2, out TResult>(T1 arg1, T2 arg2)
public delegate TResult Func<in T1, in T2, in T3, out TResult>(T1 arg1, T2 arg2, T3 arg3)

...

public delegate TResult Func<in T1, in T2, ..., in T16, out TResult>(T1 arg1, ..., T16 arg16)
```

모든 Func 대리자의 형식 매개변수 중 가장 마지막에 있는 것이 반환 형식이다.

```c#
public delegate TResult Func<out TResult>()  // 그 자체가 반환 형식
public delegate TResult Func<in T, out TResult>(T arg)  // 두 번째 형식이 반환
public delegate TResult Func<in T1, in T2, out TResult>(T1 arg1, T2 arg2) // 세 번째 형식이 반환
```
이렇듯, 입력 매개변수가 없는 것부터 16개에 이르는 것까지 버전이 다양하게 때문에 대부분 커버를 할 수 있다.

물론 입력변수가 16개가 넘는다던가, ref나 out 한정자로 수식된 매개변수를 사용해야 하는 경우에는 별도로 만들어줘야한다.

## Func 대리자 사용 예 
입력 매개변수가 없는 경우이다.
```c#
Func<int> func1 = () => 10;   // 입력 매개변수는 없고, 무조건 10 출력
Console.WriteLine(func1());
```
입력 매개변수가 하나 있는 경우이다.
```C#
Func<int, int> func2 = (x) => x*2;   // 입력 매개변수는 int 형식 하나, 반환 형식도 int
Console.WriteLine(func2(3));
```
입력 매개변수가 두 개 있는 경우이다.
```c#
Func<int, int, int> func3 = (x, y) => x + y;   // int 형식 입력 매개변수 2개, 반환 형식도 int
Console.WriteLine(func3(3, 4));
```
이런 식으로 사용하면 된다.

다음은 Action 대리자를 살펴보자.

## Action
Action 대리자도 Func 대리자와 거의 똑같다. 차이점이라면 Action 대리자는 반환 형식이 없다.

Action 대리자도 Func 대리자처럼 17개 버전이 선언되어 있다.

```C#
public delegate void Action<>()
public delegate void Action<in T>(T arg)
public delegate void Action<in T1, in T2>(T1 arg1, T2 arg2)

...

public delegate void Action<in T1, ..., in T16>(T1 arg1, ..., T16 arg16)

```
Action 대리자의 형식 매개변수는 모두 입력 매개변수를 위해 선언되어 있다.

Func와 달리 어떤 결과를 반환하는 것을 목적으로 하지 않고, 일련의 작업 수행이 목적이기 때문이다.

## Action 대리자 사용 예
입력 매개변수가 아무것도 없는 경우다. 위에서도 말했듯, Action은 반환 형식이 없다.
```C#
Action act1 = () => Console.WriteLine("Action()");
act1();
```
다음은 매개변수가 하나뿐인 경우다.
```c#
int result = 0;
Action<int> act2 = (x) => result = x * x;   // 람다식 밖에서 선언한 result에 결과를 저장
act2(5);

Console.WriteLine("result : {0}", result);
```
다음은 매개변수가 두 개인 경우다.
```c#
Action<double, double> act3 = (x, y) =>
            {
                double pi = x / y;
                Console.WriteLine("Action<T1, T2>({0}, {1}) : {2}", x, y, pi);
            };

act3(22.0, 7.0);
```
[맨 위로 이동하기](#){: .btn .btn--primary }{: .align-right}