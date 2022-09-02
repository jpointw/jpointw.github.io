---
title:  "Unity C# > Null과 관련된 연산자들" 

categories:
  -  C#
tags:
  - [C#]

published: true
toc: true
toc_sticky: true

date: 2022-06-17
last_modified_at: 2022-06-17
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

# hasValue와 Value 속성

모든 Nullable 형식은 hasValue와 Value 속성을 가진다

```C#
int? data = null;
Console.WriteLine(data.HasValue); // data는 null이므로 False 출력

int a = 100;
Console.WriteLine(data.HasValue); // a는 100이므로 True 출력
Console.WriteLine(data.Value); // 100출력
```
>만약 hasValue = False인데 Value를 사용한다면, CLR은 InvalidOperationExeption 예외를 띄운다.

***
[맨 위로 이동하기](#){: .btn .btn--primary }{: .align-right}