---
title:  "부모 클래스를 가리키는 Base 키워드와 생성자" 

categories:
  -  C#
tags:
  - [C#]

published: true
toc: true
toc_sticky: true

date: 2022-06-25
last_modified_at: 2022-06-25
---

# base 키워드란?

---

자식 클래스에서 부모 클래스의 멤버에 접근할 때 사용한다.

이런 상황이 있을 수 있다.

**"부모 클래스와 자식 클래스 모두 동일한 이름의 변수가 있다면?"**

이 경우에는 부모 클래스의 변수와 자식 클래스의 변수를 구분해줄 식별자가 필요하다.

이 때, base 키워드가 사용된다.

``` c#
class Monster
{
    protected string name;
    protected int hp;
    protected int attackDamage = 1;
}


class Slime : Monster
{
    protected int attackDamage;

    public Slime()
    {
        this.attackDamage = 5;
        Console.WriteLine("자식 클래스의 attackPower = {0}", this.attackDamage);
        Console.WriteLine("부모 클래스의 attackPower = {0}", base.attackDamage);
    }
}
```

즉, 위와 같이 base 키워드를 통해 부모 클래스의 멤버에 접근할 수 있다.

참고로, base와 this 키워드를 사용하지 않고 이름이 겹치는 멤버를 호출했다면 this가 우선권을 갖는다.

## base( ) 생성자란?

this( ) 생성자를 통해 클래스 내에 있는 다른 생성자를 호출할 수 있었다.

비슷한 개념으로, base( ) 생성자를 통해 자식 클래스에서 부모 클래스 내부의 생성자를 명시적으로 호출할 수 있다. 

``` c#
class Monster
{
    protected string name;  
    protected int hp;   
    protected int attackPower = 10; 

    public Monster(string name)           // 이름을 매개 변수로 받는 생성자
    {
        this.name = name;
    }

    public Monster(string name, int hp)  // 이름과 hp를 매개 변수로 받는 생성자
    {
        this.name = name;
        this.hp = hp;
    }
}


class Slime : Monster
{

     public Slime() : base("슬라임")  // Monster(string name) 생성자 호출
     {
         Console.WriteLine("몬스터 이름 : {0}", name);
     }    // 부모 생성자가 잘 호출되었는지 알아보기 위해 몬스터 이름 출력
}
```

원하는 부모 클래스의 생성자를 골라서 호출하는 모습을 볼 수 있다.
[맨 위로 이동하기](#){: .btn .btn--primary }{: .align-right}