---
title:  "Unity C# > TryParse()와 Parse 비교" 

categories:
  -  C#
tags:
  - [C#]

published: true
toc: true
toc_sticky: true

date: 2022-06-19
last_modified_at: 2022-06-19
---

# TryParse() VS Parse()

---

>C#에서 제공하는 기본 숫자 형식은 Parse()와 TryParse() 메소드를 제공한다.
문자열(string)을 숫자로 변환시켜야할 상황이 있을 때 사용하는 메소드들이다.


# Parse( )
```C#
string _string = "123";

int iData = int.Parse(_string);
float fData = float.Parse(_string);
double ...
char ...
```
>문자열을 지정된 형식의 숫자 타입으로 변환해주는 메소드다.
>다만, 변환을 실패했을 때 예외를 발생시킨다.

# TryParse( )
```C#
string _string = "123";

if (int.TryParse(_string, out int result))
{
  int data = result;
  ...
}
```
>변환 성공 여부(True, False)를 반환해주는 메소드다.
>그렇기 때문에 예외가 발생하지 않아 현재 코드 흐름을 유지할 수 있는 장점이 있다.

<br>

>변환한 데이터는 out 키워드로 선언한 변수에 저장된다.


[맨 위로 이동하기](#){: .btn .btn--primary }{: .align-right}