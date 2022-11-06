---
title:  "매개변수 다루기 (가변 인수, 명명된 매개변수, 디폴트 매개변수) + 로컬 함수(Local Function)" 

categories:
  -  C#
tags:
  - [C#]

published: true
toc: true
toc_sticky: true

date: 2022-06-22
last_modified_at: 2022-06-22
---

# 매개변수 다루기 (가변 인수, 명명된 매개변수, 디폴트 매개변수) + 로컬 함수(Local Function)

---


C#의 메소드 오버로딩은 오로지 매개변수의 수와 형식만을 분석(반환 형식은 보지 않음)해서 어떤 버전이 호출될 지를 컴파일 타임에 정한다는 내용을 본 후에, 매개변수와 관련된 내용들을 봤다.

## 가변 개수의 인수

매개변수의 개수가 유연하게 변할 수 있는 인수를 말한다.

가령, 다음과 같이 모든 인수의 합을 구하는 Sum() 메소드 같은 것을 구현할 때 유용하다.

``` c#
Sum(1, 2);
Sum(1, 2, 3, 4, 5);
Sum(1, 2, 3, 4, 5, 6, 7, 8, 9, 10);
```

위와 같은 메소드를 구현하기 위해 메소드 오버로딩을 일일히 다 하는 것은 힘들다.

그래서 C#에서는 params 키워드와 배열을 이용하여 가변 개수의 인수를 설정할 수 있게 해놨다.

``` c#
int Sum(params int[] args)
{
    int sum = 0;
    
    for(int i = 0; i < args.Length; i++)
    {
        sum += args[i];
    }
    
    return sum;
}
```

# 명명된 인수

메소드를 호출할 때, 매개변수 목록 중 어느 매개변수에 데이터를 할당할지 지정하는 것은 순서이다.

```c#
static void Function(int a, int b, int c)
{
    ...
}
Function(1, 2, 3);     // 순서에 따라 a = 1, b = 2, c = 3 할당
```
만약 매개 변수가 너무 많다면 이런 방법은 힘들 수 있다.

그래서 C#에서는 매개변수의 이름과 콜론(:)을 통해 직접 할당을 할 수 있게 해놨다.


## 선택적 인수(default parameter)
메소드의 매개변수는 기본값(defalut value)를 가질 수 있다.
```c#
void Function(int a, int b = 5)
{
    Console.WriteLine("{0}, {1}", a, b);
}


Function(10);      // 10, 5 출력
Function(10, 20);  // 10, 20 출력
```
선택적 인수는 항상 필수 인수 뒤에 와야 한다. 즉, 뒤쪽 매개변수부터 채워져야 한다는 소리다.

```c#
void Function(int a, int b = 5)
{
    Console.WriteLine("{0}, {1}", a, b);
}

// void Function(int a = 10, int b)   오류
// {
//     ...
// }
```

## 로컬 함수(Local Function)
메소드 안에서 선언되고, 선언된 메소드 안에서만 사용되는 특별한 함수다.

클래스의 멤버가 아니기 때문에 메소드가 아니라 함수(Function)이라고 부른다.

 

로컬 함수는 자신이 존재하는 지역에 선언되어 있는 변수를 사용할 수 있다.

```c#
class Test
{

    public void Method()
    {
        int count = 0;
        LocalFunction(1, 2);               // 로컬함수 호출
        LocalFunction(10, 20);       
        
        void LocalFunction(int x, int y)   // 로컬함수 선언
        {
            ...
            Console.WriteLine($"count : {++count}");  // 자신이 속한 메소드의 지역변수 사용
        }
    }
}
```
이런 로컬 함수는 람다식과 더불어 메소드 밖에서는 다시 쓸 일이 없는 반복적인 작업을 하나의 이름 아래 묶어놓는데 사용한다.

[맨 위로 이동하기](#){: .btn .btn--primary }{: .align-right}