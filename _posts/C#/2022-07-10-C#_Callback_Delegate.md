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

# 콜백 (Callback)이란?

---

정의 개념으로 본다면, 피호출자가 호출자를 다시 호출하는 것이라고 한다.

이 말만 들으면 무슨 말인지 이해가 잘 안 됐다. 그래서 책과 구글에 있는 정리 잘 된 여러 블로그들을 참고했다.


## 일상 생활에서의 콜백 예
어떤 한 사업가가 모 기업의 프로젝트 투자에 관심이 있어서 직접 찾아갔다.

그런데, 사장이 부재 중이라 그의 비서에게 "사장님 오시면 전화 부탁드린다."라는 메모를 남겨주고 왔다.

사업가는 그동안 자기 자신의 다른 업무를 진행했다.

얼마 뒤, 사장이 돌아왔다. 비서는 자신이 부탁받은 메모를 사장에게 주었다.

사장은 비서에게 부탁받은 메모에 적힌 내용대로 행동을 했다. (전화걸기)

사업가는 사장에게 전화가 오자, 하던 일을 잠깐 멈추고 전화를 받아 사장과 통화를 하기 시작했다. 

## 콜백이 없었다면?
아마 저 사업가는 비서에게 두들겨 맞았을 것이다.

비동기 처리에서 콜백이 쓰이는 이유를 이해할 수 있었던 예시였다.

 

그렇다면, 이제 이런 콜백을 프로그래밍에서 적용하겠다는건데 어떻게 적용할 수 있을까?

C#에서는 이런 콜백을 적용할 수 있게끔 도와주는 대리자(Delegate)라는 멋진 녀석이 존재한다.

## 대리자 (Delegate)
위에서 예시를 들었던 인물 중에 비서 역할을 하는 것이 바로 이 대리자란 녀석이다.

대리자는 다음과 같이 선언한다.

```C#
한정자 delegate 반환형식 대리자이름(매개변수 목록);
```

형식을 보면, delegate 키워드가 있는 것을 빼면 메소드를 선언하는 것과 똑같이 생겼다.

이렇게 선언한 대리자는 부탁받은 할 일(메모)를 전달 해주는 역할을 하며, 그렇기에 먼저 할 일(메소드)를 넣어주는 것이 필요하다.

 

해야할 행동을 넣어주는 것이므로 당연히 인자는 메소드가 단위가 된다. 주의할 점은 대리자와 메소드의 반환 형식, 매개변수가 일치해야 한다.

```C#
delegate void Memo(string phoneNumber);  // 대리자 선언


void Callback(string phoneNumber);       // 사용 가능
// int Callback(string phoneNumber);       반환형 불일치로 사용 불가
// void Callback();                        매개변수 불일치로 사용 불가
```

## 대리자는 인스턴스가 아닌 형식(Type)이다.
대리자는 int, string과 같은 형식이라서, 인스턴스를 따로 만들어줘야 한다.
```C#
class Program
{
    delegate void Memo(string phoneNumber);  // 대리자 선언
    
    static void Callback(string phoneNumber)
    {
        Console.WriteLine($"{phoneNumber} 번호로 전화를 걸었습니다.");
    }
    
    static void Main(string[] args)
    {
        Memo memo = new Memo(Callback);      // 메소드를 인자로 주어 인스턴스 생성
        memo("010-1234-5678");               // 대리자 실행
    }
}
```
이렇듯, 대리자는 현재 자신이 참조하고 있는 메소드 코드를 실행하고 그 결과를 호출자에게 반환한다.

다른 메소드로 갈아 끼우고 싶다면 새로운 대리자 인스턴스를 생성해서 넣어주면 된다.
```c#
static void Absensce(string phoneNumber)
{
     Console.WriteLine($"아직 부재 중이시라, {phoneNumber} 번호로 전화를 못 겁니다.");
}


static void Main(string[] args)
{
     memo = new Memo(Absensce);
     memo("010-1234-5678");
}
```

또한 대리자의 인자로 정적 메소드(static)든 인스턴스 메소드든 상관없이 넣을 수 있다.

필자는 정적 메소드로 구현을 해본 것이다.

## "대리자는 왜, 그리고 언제 사용을 하나요?"
프로그래밍을 하다 보면, 값이 아닌 "코드" 그 자체를 매개변수에 넘기고 싶을 때가 많다.

오름차순과 내림차순 정렬을 할 때, 비교 코드를 인자로 넣을 수 있다면 참 편리할 것이다.

 

먼저 비교하는 메소드를 참조할 대리자를 선언한다.

```C#
delegate int Compare(int a, int b);
```
오름차순 비교 메소드부터 한 번 해보자. 오름차순 비교 메소드를 만들어 준다.
```C#
static int AscendCompare(int a, int b)   // 오름차순 비교 메소드
{
    if (a > b)
        return 1;
    else if (a == b)
        return 0;
    else
        return -1;
}
```

 

수많은 정렬 알고리즘들이 있지만, 가장 간단한 버블소트(BubbleSort)를 한 번 만들어보자.

이 때, 매개변수로 대리자를 받는다.
```C#
static void BubbleSort(int[] array, Compare Comparer)
{
    int i = 0;
    int j = 0;
    int temp = 0;
    
    for (i = 0; i < array.Length - 1; i++)
    {
        for (j = 0; j < array.Length - (i + 1); j++)
        {
            if (Comparer(array[j], array[j+1]) > 0)
            {
                temp = array[j+1];
                array[j+1] = array[j];
                array[j] = temp;
            }
        }
    }
}
```
테스트를 한 번 해보자.
```C#
static void Main(string[] args)
{
     int[] array = { 6, 1, 15, 3, 5 };
     BubbleSort(array, new Compare(AscendCompare));

     foreach(int i in array)
     {
         Console.Write($"{i} ");
     }
}
```
오름차순으로 잘 정렬된 것을 볼 수 있다.

그런데, 이 버블소트 코드를 그대로 활용하면서 내림차순 정렬로 변경할 순 없을까?

 

내림차순 메소드를 하나 만들어서 새로운 대리자 인스턴스를 만들어 주면 된다. 
```C#
static int DescendCompare(int a, int b)      // 내림차순 메소드
{
    if ( a < b)
        return 1;
    else if (a == b)
        return 0;
    else
        return -1;
}
```
그리고 다시 테스트를 해보자.
```C#
static void Main(string[] args)
{
     int[] array = { 6, 1, 15, 3, 5 };
     BubbleSort(array, new Compare(DescendCompare));   // 내림차순 메소드 대리자 객체 생성

     foreach(int i in array)
     {
         Console.Write($"{i} ");
     }
}
```
이 때까지 대리자의 간단한 사용 방법 및 응용에 대해 알아보았다.

글이 길어질 것 같으니 일반화 대리자, 대리자 체인 내용은 다음 글에서 다뤄보자.
[맨 위로 이동하기](#){: .btn .btn--primary }{: .align-right}