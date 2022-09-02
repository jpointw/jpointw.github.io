---
title:  "new 와 sealed 키워드" 

categories:
  -  C#
tags:
  - [C#]

published: true
toc: true
toc_sticky: true

date: 2022-06-26
last_modified_at: 2022-06-26
---

# new 키워드란?

---

객체를 생성할 때 사용하는 new의 의미와는 다르다.

부모 클래스로부터 상속 받은 멤버와 이름은 동일하지만 완전히 다른 새로운 멤버로 재정의하고 싶을 때 사용한다.

클래스를 작성하다 보면, 만들고 있는 메소드를 나중에 오버라이딩을 할 지 안 할지를 미리 생각한다는 건 쉬운 게 아니다.

왜냐하면, C#에서 오버라이딩을 하기 위해서는 부모 클래스의 메소드를 virtual으로 선언해야 하기 때문이다.

이런 부분까지 전부 설계를 신경써서 작성한다는 것은 경험이 많은 프로그래머가 아니면 어려울 것이다.

그래서 C#에서는 우리 같은 사람들을 위해서, 메소드 숨기기를 지원한다.
``` c#
class Monster
{
    public void Attack()
    {
         Console.WriteLine("기본 공격");
    }
}

class Slime : Monster
{
    public new void Attack()            // Monster의 Attack()은 숨겨짐
    {
         Console.WriteLine("점성 공격");
    }
}
```

이렇게 선언을 하고 프로그램을 돌려보면 다음과 같은 결과가 나온다.

```C#
Slime slime = new Slime();
slime.Attack();           // "점성 공격" 출력
```

이렇게 하면, 부모 클래스인 Monster의 Attack() 메소드는 숨겨진 듯한 효과를 보게 된다.

"왜 이런 기능을 넣어놨을까? 오버라이딩과 다를 게 뭐야?"
이런 의문이 들 수 있다. 사실 나도 그랬다.

차이점을 보여주기 위해 다음과 같이 오버라이딩 코드를 추가로 넣었다.

```c#
class Monster
{
    public void Attack()
    {
        Console.WriteLine("기본 공격!");
    }

    public virtual void Skill()              // 추가
    {
        Console.WriteLine("스킬 공격");
    }
}

class Slime : Monster
{
    public new void Attack()
    {
        Console.WriteLine("슬라임 점성 공격!");
    }

    public override void Skill()             // 오버라이딩
    {
        Console.WriteLine("슬라임 스킬 공격!");
    }
}
```
우리는 객체를 참조할 때, 해당 클래스의 참조 변수를 통해 참조를 한다.

하지만, 자식 클래스들이 많을 경우에는 부모 클래스를 통해 자식 클래스를 가리키는 업 캐스팅(Upcasting)을 이용한다.

```c#
Monster monster = new Slime();
monster.Attack();      // "기본 공격!" 출력
monster.Skill();       // "슬라임 스킬 공격!" 출력
```
그렇다. 메소드 숨기기를 해 놓은 경우에는 업 캐스팅으로 출력할 경우, 부모의 메소드가 호출된다.

오버라이딩은 업 캐스팅을 해도 자식 클래스의 메소드가 호출된다.

 

이게 차이점이라고 할 수 있다. 메소드 숨기기는 말 그대로 진짜 숨겨 놓은 것일 뿐이다.

다형성을 표현하기에는 한계점이 분명 존재한다. 그래서 오버라이딩이 있는 것이다.

 

그런데 사용해보니, 딱히 new 키워드를 사용하지 않아도 있는 것처럼 작동하는 것 같다.

## 상속과 overiding을 봉인한 sealed 키워드

클래스를 작성하고 나서, 해당 클래스가 의도치 않은 상속이 이루어지는 것을 막기 위해 사용할 수 있다.

해당 클래스를 봉인 클래스라고 한다.

```c#
sealed class Slime
{
    ...
}


/*
class Bubbling : Slime     상속을 봉인했기 때문에 컴파일 오류가 발생
{
    ...
}
*/
```
물론 메소드 오버라이딩 봉인에 대해서도 사용이 가능하다.

모든 메소드를 봉인할 수 있는 것은 아니고, virtual로 선언된 가상 메소드를 오버라이딩한 메소드만 봉인이 가능하다.
``` c#
class Monster
{
    public virtual void Attack()
    {
       ...
    }
}

class Slime : Monster
{
    public sealed override void Attack()
    {
       ...
    }
}

/*
class Bubbling : Slime
{
    public override void Attack()    봉인되었기 때문에 오버라이드 불가능
    {
       ...
    }
}
*/
```
그리고 이건 추가로 알아두면 좋을 상식으로, private으로 선언한 메소드는 오버라이딩할 수 없다.

컴파일러는 해당 메소드를 오버라이딩한다고 생각하지 않고, 전혀 없었던 메소드를 선언한다고 간주할 것이다.
[맨 위로 이동하기](#){: .btn .btn--primary }{: .align-right}