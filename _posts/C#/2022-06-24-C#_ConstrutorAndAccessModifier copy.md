---
title:  "this( ) 생성자와 접근 한정자(Access Modifier)" 

categories:
  -  C#
tags:
  - [C#]

published: true
toc: true
toc_sticky: true

date: 2022-06-24
last_modified_at: 2022-06-24
---

# this( ) 생성자란?

---


자기 자신의 생성자를 가리킨다.

생성자들을 여러 개 만들다 보면, **생성자들 사이에서 코드가 중복되는 상황이 발생하곤 한다.**

이런 부분을 해결하고 싶을 때 this( ) 생성자가 유용하게 사용된다.


```c#
class Monster
{

class Monster
{
    string name;
    int hp;
    int damage;
    
    public Monster(string name)
    { 
        Console.WriteLine("Monster(string name) 생성자 호출");
        this.name = name;
    }
    
    public Monster(string name, int damage) : this(name)
    {
        // this(name)는 Monster(string name) 생성자를 호출함
        Console.WriteLine("Monster(string name, int damage) 생성자 호출");
        this.damage = damage;
    }
    
    public Monster(string name, int hp, int damage) : this(name, damage)
    {
        // this(name, damage)는 Monster(string name, int damage) 생성자를 호출함
        Console.WriteLine("Monster(string name, int hp, int damage) 생성자 호출");
        this.hp = hp;
    }
}
}
```
Monster(string name, int hp, damage) 생성자를 호출하게 되면, this( )로 이어진 생성자들을 타고 올라간다.

위 코드에서 제일 끝에 도달한 생성자가 Monster(string name)이므로 이 생성자가 먼저 호출된 후, 그 다음엔  Monster(string name, int damage) 생성자가 호출되고, 이런 식으로 진행하게 된다.

생성자를 호출하려면 new 키워드를 사용해야 했지만, 이렇게 this( ) 생성자를 통해 호출할 수도 있는 것을 알았다.

게다가, new 키워드와 달리 현재 생성하려는 객체 외에 또 다른 객체를 생성하지는 않는다.

한 번 테스트를 해보자.

``` c#
Monster testMonster = mew Monster("Slime", 500, 10)'
```
Monster(string name) 생성자 호출
Monster(string name, int damage) 생성자 호출
Monster(string name, int hp, int damage) 생성자 호출

## 접근 한정자 (Access Modifier)란?

클래스 내부 필드에 접근할 수 있는 보안 수준을 말한다.

현재 이 글을 작성하는 시점 기준으로, C#에는 6가지의 접근 한정자가 있다.

**private** - 가장 높은 보안 수준이며, 자기 자신 클래스의 내부에서만 접근이 가능하다.

즉, 클래스의 외부에서 접근하는 것이 불가능하다.

자식 클래스에서 접근을 할 수 없다.

**protected** - 자기 자신의 클래스 내부에서와 자식 클래스에서만 접근이 가능하다.

클래스의 외부에서 접근하는 것은 불가능하다.

다른 어셈블리에 있는 클래스가 내 클래스의 자식 클래스인 경우에는 접근이 가능하다.

일반적으로 자식 클래스에게 상속해줄 멤버에게 지정한다.

**public** - 클래스의 내부와 외부 모두에서 접근이 가능하다.

가장 낮은 수준의 보안 등급이다.

**internal** - 같은 어셈블리에 있는 코드에서만 public으로 접근이 가능하다.

다른 어셈블리에 있는 코드에서는 private과 같은 수준의 접근성을 가진다.

**protected internal** - 같은 어셈블리에 있는 코드에서만 protected로 접근할 수 있다.

다른 어셈블리에 있는 코드에서는 private과 같은 수준의 접근성을 가진다.

**private protected** - 같은 어셈블리에 있는 클래스에서 상속 받은 클래스 내부에서만 접근이 가능하다.

[맨 위로 이동하기](#){: .btn .btn--primary }{: .align-right}