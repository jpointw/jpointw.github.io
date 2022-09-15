---
title:  "콜백 함수(Callback)는 무엇이고 대리자(Delegate)는 뭘까?" 

categories:
  -  C#
tags:
  - [C#]

published: true
toc: true
toc_sticky: true

date: 2022-07-10
last_modified_at: 2022-07-10
---

# 일반화 대리자 (Generic Delegate)

---

대리자는 일반 메소드뿐만 아니라 일반화 메소드도 참조할 수 있다.

물론 이 경우에 대리자 또한 일반화 메소드를 참조할 수 있도록 형식 매개변수를 이용하여 선언되어야 한다.

```C#
delegate void Memo<T>(T memo);
```
참조할 메소드 또한 당연히 일반화 메소드로 작성해줘야 한다.

```C#
static void WriteMemo<T>(T memo)
{
     Console.WriteLine($"{memo.GetType()} 형식의 메모");
     Console.WriteLine($"내용은 {memo}\n");
}
```

테스트를 해보면 이제 다음과 같이 뜨는 것을 볼 수 있다.
```C#
static void Main(string[] args)
{
     Memo<string> memo = new Memo<string>(WriteMemo);
     memo("사장님께 전화드리기!");
}
```

## 대리자 체인 (Delegate Chain)
대리자는 메소드의 참조라고 여러 번 말했다. 그런데, 이 메소드 참조를 하나가 아닌 여러 개를 할 수 있는 특성이 대리자에게 있다.

예제를 한 번 보자. 다음과 같이 여러 개의 메소드들을 선언했다.

```C#
delegate void ThereIsAFire(string location);

static void Call119(string location)
{
    Console.WriteLine($"소방서죠? 불났어요! 주소는 {location}");
}
     
static void ShoutOut(string location)
{
    Console.WriteLine($"피하세요! {location}에 불났어요!");
}

static void Escape(string location)
{
    Console.WriteLine($"{location}에서 어서 나갑시다!");
}
```
그리고 다음과 같이 테스트 코드를 작성했다.
```C#
static void Main(string[] args)
{
    ThereIsAFire Fire = new ThereIsAFire(Call119);
    Fire += new ThereIsAFire(ShoutOut);
    Fire += new ThereIsAFire(Escape);

    Fire("우리집");
}
```
C#에서는 += 연산자를 통해 대리자에 메소드 참조를 추가할 수 있다.

그리고 대리자를 한 번만 호출하게 되면, 자신이 참조하고 있는 메소드를 모두 호출한다.

 

체인(Chain)이라고 불리는 이유가 여기에 있는 것이다.

그 외에도 다음과 같이 대리자 체인을 만들 수 있는 방법들이 있다.
```c#
// +와 = 연산자 사용
ThereIsAFire Fire = new ThereIsAFire(Call119)
                  + new ThereIsAFire(ShoutOut)
                  + new ThereIsAFire(Escape);
                  
                  
// Delegate.Combine() 메소드 사용
ThereIsAFire Fire = (ThereIsAFire) Delegate.Combine(
                  new ThereIsAFire(Call119),
                  new ThereIsAFire(ShoutOut),
                  new ThereIsAFire(Escape));
 
```
또한, 대리자에 추가한 메소드 참조들은 -= 연산자로 삭제할 수 있으며 Delegate.Remove() 메소드를 사용해도 된다.

## 익명 메소드

대리자에게 참조할 메소드를 넘겨야 할 일이 생겼는데, 이 메소드가 두 번 다시 사용할 일이 없다고 판단된다면?

이 때 익명 메소드를 유용하게 활용할 수 있다.

 

대리자에 익명 메소드 참조는 다음과 같이 한다.

```C#
대리자_인스턴스 = delegate(매개변수_목록)
                  {
                      // 실행하고자 하는 코드
                  };
```
당연한 얘기겠지만, 익명 메소드는 자신을 참조할 대리자의 형식과 동일한 형식으로 선언되어야 한다.

익명 메소드를 사용한 예시를 한 번 보자.

 

다음과 같이 대리자를 선언하고,

```C#
delegate int Calculate(int a, int b);
```

delegate int Calculate(int a, int b);

```C#
Calculate Calc;

Calc = delegate(int a, int b)
       {
           return a + b;
       };


Console.WriteLine("{0} + {1} = {2}", 3, 4, Calc(3, 4));
```
[맨 위로 이동하기](#){: .btn .btn--primary }{: .align-right}