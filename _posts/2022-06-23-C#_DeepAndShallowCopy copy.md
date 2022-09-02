---
title:  "얕은 복사(Shallow Copy)와 깊은 복사(Deep Copy) + ICloneable 인터페이스" 

categories:
  -  C#
tags:
  - [C#]

published: true
toc: true
toc_sticky: true

date: 2022-06-23
last_modified_at: 2022-06-23
---

# 얕은 복사(Shallow Copy)와 깊은 복사(Deep Copy) + ICloneable 인터페이스

---


C#의 클래스 부분을 공부하고 있었다.

어떤 객체지향언어를 배우든 클래스와 생성자란 개념이 나왔고, 이것은 익숙하게 알고 있는 내용이었다.

이번에 정리할 글인 얕은 복사와 깊은 복사 또한 잘 알고 있는 내용이었지만, 복사 생성자를 사용할 일이 나에겐 잘 없었다. 그래서 나중에 막힘없이 사용하기 위해 이렇게 글을 포스팅할까 한다.

그 전에 참고로 알아두면 좋은 지식을 먼저 보자.

**C#에도 종료자(소멸자)는 존재한다.**

나는 C#에 생성자만 존재하고 소멸자는 없는 줄 알았다. 하지만 존재했다.

```c#
class Monster
{

    ~Monster()    // 소멸자
    {
        ...
    }
}
```
생성자와 다르게 오버로딩이 불가능하고, 매개변수를 받지 않는다는 점까지 C++의 소멸자와 성질이 같았다.

다만, C#에서는 소멸자의 사용을 지양하라고 한다.

 

C#은 CLR이 힙 메모리가 부족해지면, 가비지 컬렉션(Garbage Collection) 작업을 통해 메모리 해제 작업을 한다.

즉, 언제 소멸자가 실행될 지를 모르는 것이다.

 

그렇기 때문에 중요한 리소스를 소멸자에서 해제하도록 나두면, 금세 메모리가 부족해지는 현상을 겪을 수도 있다.

또한, 소멸자를 명시적으로 구현하면 가비지 컬렉터는 클래스의 족보를 타고 올라가 객체로부터 상속받은 Finalize() 메소드를 호출한다. 

이 방법은 응용 프로그램의 성능 저하를 초래할 확률이 높아 권장하지 않는다고 한다.

 

결론은 **소멸자는 사용하지 말고, 가비지 컬렉터에게 맡기고,**

이제 본격적으로 얕은 복사와 깊은 복사를 하는 방법에 대해 알아보자.

## 얕은 복사 (Shallow Copy)
클래스는 참조 형식이다. 그렇기 때문에 다음과 같이 복사를 해주면 문제가 발생한다.
```c#
class Monster
{
     public int hp;
     public int damage;
     
     public Monster(int hp, int damage)
     {
         this.hp = hp;
         this.damage = damage;
     }
}

Monster myMonster = new Monster(100, 5);

Monster otherMonster = myMonster;          // 얕은 복사
otherMonster.hp = 200;

Console.WriteLine($"myMonster HP : {myMonster.hp}");
Console.WriteLine($"otherMonster HP : {otherMonster.hp}");
```
객체는 하나만 생성하고 그걸 가리키는 참조 변수는 두 개(myMonster, otherMonster)가 된 것이다.

그러니, otherMonster의 체력을 변경해도 myMonster의 체력이 같이 변경되는 일이 벌어졌다.

즉, 제대로된 복사를 해주려면 새로운 객체를 생성해고 내부 값들을 복사하여 넘겨주는 깊은 복사 작업이 필요하다.

## 깊은 복사 (Deep Copy)

C#에서는 깊은 복사를 자동으로 해주는 구문이 없기 때문에 직접 만들어 줘야 한다.

```c#
class Monster
{
     public int hp;
     public int damage;
     
     public Monster(int hp, int damage)
     {
         this.hp = hp;
         this.damage = damage;
     }
     
     public Monster DeepCopy()       // 깊은 복사 메소드 생성
     {
         Monster newMonster = new Monster(0, 0);
         newMonster.hp = this.hp;
         newMonster.damage = this.damage;
         
         return newMonster;
     }
}
Monster myMonster = new Monster(100, 5);

Monster otherMonster = myMonster.DeepCopy();   // 깊은 복사
otherMonster.hp = 200;

Console.WriteLine($"myMonster HP : {myMonster.hp}");
Console.WriteLine($"otherMonster HP : {otherMonster.hp}");
```
새로운 객체를 만들어서 내부값들을 복사해주고 리턴하는 DeepCopy() 라는 메소드를 만들었다.

이제 myMonster와 otherMonster는 서로 다른 객체를 참조하고 있다.

## ICloneable 인터페이스를 이용하여 깊은 복사 만들기

System 네임스페이스에는 ICloneable이라는 인터페이스가 있다.

위에서 DeepCopy() 메소드를 직접 만들었던 것처럼 깊은 복사를 해도 되지만, .NET의 다른 유틸리티 클래스나 다른 프로그래머가 작성한 코드와 호환되도록 하려면 ICloneable을 상속하는 것이 좋다.

ICloneable 인터페이스는 Clone()이라는 메소드 하나만 오버라이딩하여 구현하면 된다.

``` C#
class Monster : ICloneable
{
     public int hp;
     public int damage;
     
     public Monster(int hp, int damage)
     {
         this.hp = hp;
         this.damage = damage;
     }
     
     public Monster Clone()                // ICloneable Clone() 메소드 오버라이딩
     {
         Monster newMonster = new Monster(0, 0);
         newMonster.hp = this.hp;
         newMonster.damage = this.damage;
         
         return newMonster;
     }
}
```

사실상 사용하는 방법은 DeepCopy() 메소드와 별 차이가 없는 것 같다.

[맨 위로 이동하기](#){: .btn .btn--primary }{: .align-right}