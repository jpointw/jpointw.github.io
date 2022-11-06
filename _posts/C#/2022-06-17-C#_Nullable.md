---
title:  "Unity C# > Nullable" 

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

# Nullable

---

>C#에서는 값 형식에 해당하는 자료형들이 존재(byte,int float, ...)하며 이런 형식의 변수들은 초기화해서 사용하지 않으면 컴파일이 불가능하다.

>하지만, 개발을 하다 보면 이런 값 형식의 데이터에도 어떠한 값도 가지지 않게 하고 싶은 경우가 종종 있는데 이런 값 형식 자료형 변수에도 null 값을 할당할 수 있도록 C#에서는 Nullable 형식을 지원한다.

# 사용예시

```C#
int? data = null;
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