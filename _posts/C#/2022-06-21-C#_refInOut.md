---
title:  "Unity C# > 참조에 의한 매개변수 전달 (ref, out)" 

categories:
  -  C#
tags:
  - [C#]

published: true
toc: true
toc_sticky: true

date: 2022-06-21
last_modified_at: 2022-06-21
---

# 참조에 의한 매개변수 전달 (ref, out)

---
## 값에 의한 호출 (Call by value)

함수와 메소드를 호출할 때, 필요한 값들을 전달해주기 위해 매개변수에 값을 넣어 전달해 준다.

허나, 기본적으로 값 형식(Value type)은 매개 변수로 전달 시에 해당 인자의 값을 복사해서 전달을 한다.

이건 함수와 메소드에서 값을 리턴할 때도 마찬가지다. 이것을 값에 의한 호출(Call by value)이라고 한다.

## 참초에 의한 호출 (Call by reference)
매개 변수가 변수 또는 상수로부터 값을 복사하는 값에 의한 호출과 달리,

<u>참조에 의한 호출은 매개변수가 메소드에 넘겨진 원본 변수를 직접 참조</u> 한다.

## ref 키워드

```C#
/* swap함수를 사용한 예시*/
public static void Swap(ref int x, ref int y)
{
    int temp = x;
    x = y;
    y = temp;
}
```

## 참조 리턴
값 형식이라면 메소드의 리턴 결과 또한 복사되어 반환되는 형식이라고 했다.

하지만, 메소드의 결과를 참조로 다루고 싶을 때가 있을 수 있다.

**ref 한정자를 이용해서 메소드를 선언하고, return 문이 반환하는 변수 앞에도 ref 키워드를 명시해야 한다**

```C#
class Monster
{
    int hp = 100;
    
    public ref int Function()  // 메소드 앞에 ref 키워드
    {
       ...
       return ref hp;          // return 문 앞에 ref 키워드 명시
    }
}
```

만약 값을 복사하여 리턴하는 형식으로 사용할 것이라면, 평소처럼 사용하면 된다.

``` C#
Monster monster = new Monster();
int hp = monster.Function();
```

하지만, <u>메소드가 반환하는 결과를 호출자가 참조로 넘겨받고 싶다면</u> 다음과 같이 작성해야 한다.
```c#
Monster monster = new Monster();
ref int hp = ref monster.Function();   // ref 키워드 붙이기
// hp는 참조 지역 변수
```
메소드의 특성상, 값을 하나만 리턴할 수 있는 특성이 있다.

참조를 이용하게 되면, **원본 값을 여러 개 받아와서 수정이 가능하기 때문에 마치 여러 개의 값을 리턴하는 것 같은 효과**를 볼 수 있다. 이런 점을 이용하면 코드 작성 시 더 유용하게 작용할 것이다.

## out 키워드

ref 키워드는 메소드 내부에서 ref 매개변수에 값을 할당하지 않아도 컴파일 오류를 발생시키지 않는다.

예시를 들기 위해, 나눗셈 메소드를 만들어서 몫(quotient)과 나머지(remainder)를 참조로 반환하는 걸 보자.

``` c#
static void Divide(int a, int b, ref int quotient, ref int remainder)
{
    // ref로 선언된 매개변수에 값을 할당하지 않아도 컴파일 에러 발생하지 않음
    //quotient = a / b;
    //remainder = a % b;
}


static void Main(string[] args)
{
    int a = 20;
    int b = 3;
    int c = 1;
    int d = 2;

    Divide(a, b, ref c, ref d);
    Console.WriteLine("Quotient : {0}, Remainder : {1}", c, d);
}
```

이런 부분은 만약 프로그래머가 실수하여 값을 할당하지 않는다면, 오류도 발생하지 않는 런타임 에러이기 때문에 디버깅하기가 어려워진다.

out 키워드는 위와 같은 문제를 방지할 수 있게 메소드 내부에서 매개변수에 값을 할당하지 않는다면 컴파일 오류를 발생시킨다.

``` c#
static void Divide(int a, int b, out int quotient, out int remainder)
{
    // 컴파일 오류 발생
    //quotient = a / b;
    //remainder = a % b;
}


static void Main(string[] args)
{
    int a = 20;
    int b = 3;
    int c;
    int d;

    Divide(a, b, out c, out d);
    Console.WriteLine("Quotient : {0}, Remainder : {1}", c, d);
}
```
즉, 마치 인터페이스나 추상클래스의 추상 함수가 오버라이딩을 강제로 하는 것처럼 프로그래머에게 알려주는 것이다.

 

또 하나의 사실은 ref 키워드로는 초기화하지 않은 지역 변수를 매개 변수로 넘겨주면 오류가 나지만,

out 키워드는 초기화하지 않은 지역 변수를 매개 변수로 넘겨줘도 오류가 나지 않는다.

어차피 해당 메소드 내부에서 강제로 초기화를 하기 때문이다.

out 키워드는 사실 다음과 같이 <u>메소드를 호출할 때, 매개변수 목록 안에서 즉석으로 선언해도 된다.</u>

``` c#
static void Divide(int a, int b, out int quotient, out int remainder)
{
    quotient = a / b;
    remainder = a % b;
}


static void Main(string[] args)
{
    int a = 20;
    int b = 3;

    Divide(a, b, out int c, out int d);
    Console.WriteLine("Quotient : {0}, Remainder : {1}", c, d);
}
```


[맨 위로 이동하기](#){: .btn .btn--primary }{: .align-right}