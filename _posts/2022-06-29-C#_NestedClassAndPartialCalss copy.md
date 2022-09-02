---
title:  "중첩 클래스(Nested Class)와 분할 클래스(Partial Class)" 

categories:
  -  C#
tags:
  - [C#]

published: true
toc: true
toc_sticky: true

date: 2022-06-29
last_modified_at: 2022-06-29
---

# 중첩 클래스(Nested Class)와 분할 클래스(Partial Class)

---

## 중첩 클래스(Nested Class)

가끔식 코드들을 보면 중첩 클래스로 구현한 코드들이 보이곤 했다.
```c#
class OuterClass
{
    class NestedClass    // 중첩 클래스
    {
    
    }
}
```

사실 아직까지 중첩 클래스를 사용의 필요성을 잘 느끼지 못하고 있긴 하다.

책을 읽어보니, 기본적으로 다음과 같은 두 가지 이유 때문에 사용하곤 한다고 한다.

클래스 외부에 공개하고 싶지 않은 형식을 만들고 싶을 때
현재 클래스의 일부분처럼 표현할 수 있는 클래스를 만들고자 할 때
 
중첩 클래스는 다른 클래스의 private 멤버에도 접근할 수 있다는 특징을 가지고 있다.

테스트를 해보니, 외부 클래스에서 내부 중첩 클래스의 private 멤버 접근은 안 되는 것 같다.

```c#
class OuterClass
{
    private int outerVar;
    
    class NestedClass
    {
        public void Function()
        {
            OuterClass outerClass = new OuterClass();
            outerClass.outerVar = 20;      // private 멤버 접근
    }
}
```
몇몇 예제들을 보고 느낀 결과, 현재 작성 중인 클래스 내부에서만 사용 가능하도록 할 때 유용한 것 같기도 하다.

예를 들어, 현재 클래스 내부에서만 다루고 외부에서는 건들면 안 되는 중요한 데이터들을 관리하는 클래스는 아무래도 중첩 클래스로 선언하는 것이 안전할 것이다.

```c#
class Document
{
     List<ImportantData> dataList = new List<ImportantData>();
     
     private class ImportantData   // private으로 선언 했기에 Document 클래스 외부에서 안 보인다.
     {
         ...                 // 즉, ImportantData 클래스는 Document 클래스 내부에서만 처리 가능
     }
}
```

## 분할 클래스(Partial Calss)

여러 번에 나눠서 구현하는 클래스를 말한다.

그 자체로 특별한 기능이 있는 것은 아니고, 클래스 내용이 길어질 경우 여러 파일에 나눠서 구현할 수 있게 해주는 것이다.

한 마디로, 소스 코드 관리의 편의를 제공하는데에 그 의미가 있다.

 

분할 클래스는 partial 키워드를 이용해서 작성한다.

물론 클래스 이름은 동일해야 한다.

```C#
partial class Monster
{
    public void Skill1() { }
    public void Skill2() { }
}

partial class Monster
{
    public void Skill3() { }
    public void Skill4() { }
}
```
Monster 클래스는 두 번에 걸쳐 정의되고 있지만, C# 컴파일러는 이렇게 분할 구현된 코드를 하나의 Monster 클래스로 묶어 컴파일 한다. 몇 개로 나눠 분할 구현했는지 신경 쓸 필요 없이, 그냥 하나의 클래스인 것처럼 사용하면 된다.
```C#
Monster monster = new Monster();
monster.Skill1();
monster.Skill2();
monster.Skill3();
monster.Skill4();
```
[맨 위로 이동하기](#){: .btn .btn--primary }{: .align-right}