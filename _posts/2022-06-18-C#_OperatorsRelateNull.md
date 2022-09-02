---
title:  "Unity C# > Null과 관련된 연산자들" 

categories:
  -  C#
tags:
  - [C#]

published: true
toc: true
toc_sticky: true

date: 2022-06-18
last_modified_at: 2022-06-18
---

# Null과 관련된 연산자들

---

>C# 6.0부터는 검사 방식 조건 문을 축약할 수 있는 기능을 지원한다.
# 검사방식예시
```C#
void Func(Object obj)
{
  if (obj != null)
  {
    //어쩌구저쩌구
  }
}
```

# Null 조건부 연산자

## ?.연산자

> 객체의 멤버에 접근하기 전 해당 객채가 Null인지 검사
null이면, 리턴 null, null이 아니라면 .뒤에 지정된 멤버를 리턴

### 예시코드
```C#
class Mydata
{
  public int data;
}

Mydata mydata = null;
int? data = myData?.data; // myData가 null이 아닐 때 멤버에 접근하여 값을 가져옴
```
>위 코드는 myData 객체가 null이므로 null을 리턴한다.
<br>
> 일반적인 값 형식 자료형 int로는 null을 저장할 수 없으므로 Nullavle로 선언하여 받았다.


## ?[ ] 연산자

> ?. 연산자와 동일한 기능을 수행한다.
> 차이점이라면, 객체의 멤버 접근이 아니라 배열과 같은 컬렉션 객체의 첨자를 이용한 참조에 사용된다.

### 예시코드
```C#
ArrayList arrList = null;
arrList?.Add("C#");        // arrList?.이 null을 반환하므로 Add()는 실행되지 않음
arrList?.Add("Unity");

Console.WriteLine($"Count : {arrList?.Count}");
Console.WriteLine($"{arrList?[0]}");
Console.WriteLine($"{arrList?[1]}");
```
null 밖에 없으므로 출력되는 것이 없다.

## ??연산자

>null 병합 연산자로, 두 개의 피연산자를 받아들이고 왼쪽 피연산자가 null인지를 검사한다.

>왼쪽 피연산자가 null이면, 오른쪽 피연산자를 리턴

>왼쪽 피연산자가 null이 아니라면, 왼쪽 피연산자를 리턴

### 예시코드
``` C#
int? data = null;
Console.WriteLine($"{data ?? 0}");   // data는 null이므로 0이 출력됨

data = 100;
Console.WriteLine($"{data ?? 0}");   // data는 null이 아니므로 100이 출력됨
```

[맨 위로 이동하기](#){: .btn .btn--primary }{: .align-right}