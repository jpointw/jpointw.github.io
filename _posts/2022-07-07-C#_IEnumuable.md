---
title:  "foreach 문 적용이 가능한 객체 만들기 : IEnumerable" 

categories:
  -  C#
tags:
  - [C#]

published: true
toc: true
toc_sticky: true

date: 2022-07-08
last_modified_at: 2022-07-08
---

# foreach

---
foreach 문은 for 문처럼 인덱스 변수가 필요 없다. 배열이나 리스트같은 컬렉션에서만 사용이 가능하다.

foreach 문이 객체 내의 요소를 순회하기 위해서는 foreach 문과의 약속을 지켜야만 한다.

그 약속은 IEnumerable 인터페이스를 상속하는 클래스 구현이다.

 

즉, 어떤 클래스라도 IEnumerable 인터페이스를 상속하기만 하면 foreach 문을 사용할 수 있다는 얘기가 된다.

IEnumerable 인터페이스
인터페이스를 상속받는 클래스는 반드시 인터페이스의 모든 내용을 구현해야 한다는 것은 기억할 것이다.

IEnumerable 인터페이스가 갖고 있는 메소드는 단 하나 뿐이며, 상속받는 클래스는 다음 메소드를 구현해야 한다.

```c#
IEumerator GetEnumerator();     // IEnumerator 형식의 객체를 반환
```

GetEnumerator() 메소드는IEnumerator 인터페이스를 상속받는 클래스의 객체를 반환해야 한다.

직접 IEnumerator 인터페이스를 상속받는 클래스를 구현해도 되고, 컴파일러가 지원하는 yield 문을 이용해도 된다.

먼저, yield 문을 사용한 방법을 알아보자.

## yield 문

yield 문을 사용하면 IEnumerator 인터페이스를 상속받는 클래스를 따로 구현하지 않아도, 컴파일러가 자동으로 해당 인터페이스를 구현한 클래스를 생성해준다.

## yield return

현재 메소드(GetEnumerator())의 실행을 일시 정지하고, 호출자에게 결과를 반환한다.

메소드가 다시 호출되면, 일시 정지된 실행을 복구하여 yield return 또는 yield break 문을 만날 때까지 작업을 계속한다.

```c#
using System;
using System.Collections;

class MyEnumerator
{
     private int[] numbers = { 1, 2, 3, 4 };
        
     public IEnumerator GetEnumerator()
     {
         yield return numbers[0];
         yield return numbers[1];
         yield return numbers[2];
         yield break;             // yield break는 GetEnumerator() 메소드를 종료시킴
         yield return numbers[3]; // 따라서 이 부분은 실행되지 않음
     }
}
```

테스트를 위해 실행해보면 다음과 같은 결과가 나온다.

```c#
var _object = new MyEnumerator();
           
foreach(int i in _object)
{
    Console.WriteLine(i);
}
```

1
2
3

yield 문을 사용하면 IEnumerator 인터페이스를 상속받는 클래스를 별도로 구현할 필요없이, 이렇게 컴파일러가 자동적처리해준다. 그렇다면, 직접 IEnumerator 인터페이스를 상속받는 클래스를 만들어본다면 어떨까?

## IEnumerator 인터페이스

```c#
interface IEnumerator
{
    boolean MoveNext();
    void Reset();
    Object Current { get;}
}
```

    MoveNext()  :  다음 요소로 이동. 컬렉션의 끝을 지난 경우에는 False, 이동이 성공한 경우에는 True 리턴
    Current  :  컬렉션의 현재 요소 리턴
    Reset()   :  컬렉션의 첫 번째 위치 "앞"으로 이동
        첫 번째 위치가 0번인 경우, Reset() 호출 시 -1 번으로 이동.
        첫 번째 위치로의 이동은 MoveNext()를 호출한 다음에 이루어짐  

## 예제 코드

```c#
using System;
using System.Collections;

namespace Enumerable
{

   class MyList : IEnumerable, IEnumerator
   {
        private int[] array;
        int position = -1;               // 컬렉션의 현재 위치를 다루는 변수
        
        public MyList()
        {
            array = new int[3];
        }
        
        public int this[int index]      // 인덱서
        {
            get { return array[index]; }
            set
            {
                if (index >= array.Length)
                {
                    Array.Resize<int>(ref array, index + 1);
                    Console.WriteLine($"Array Resized : {array.Length}");
                }
                
                array[index] = value;
            }
        }
        
        
        // IEnumerator 멤버
        public object Current
        {
            get { return array[position]; }
        }
        
        
        // IEnumerator 멤버
        public bool MoveNext()
        {
            if (position == array.Length - 1)
            {
                Reset();
                return false;
            }
            
            position++;
            return (position < array.Length);
        }
        
        
        // IEnumerator 멤버
        public void Reset()
        {
            position = -1;
        }
        
        // IEnumerable 멤버
        public IEnumerator GetEnumerator()
        {
            return this;
        }
   }
}
```

그리고 테스트를 위해 돌려보면 다음과 같이 나온다.

```C#
MyList list = new MyList();
for(int i = 0; i < 5; i++)
{
    list[i] = i;
}

foreach(int i in list)
{
    Console.WriteLine(i);
}
```

Array Resized : 4
Array Resized : 5
0
1
2
3
4

[맨 위로 이동하기](#){: .btn .btn--primary }{: .align-right}