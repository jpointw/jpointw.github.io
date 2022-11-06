---
title:  "부모 클래스와 자식 클래스 사이의 형변환을 해 보자! (is, as)" 

categories:
  -  C#
tags:
  - [C#]

published: true
toc: true
toc_sticky: true

date: 2022-06-27
last_modified_at: 2022-06-27
---

# 형변환 is,as?

---

개인적으로 생각하는 객체 지향의 꽃 중 하나는 업캐스팅(Upcasting)이라고 생각한다. 

수많은 자식 클래스들이 있지만, 그 많은 클래스들을 부모 클래스 하나로 가리킬 수 있다는 것은 생산성에 정말 큰 영향을 미치는 것 같다. 처음 객체지향언어를 다룰 때는 당연히 나도 이게 무슨 의미인지 잘 알지 못했다.

 

내가 이해한 내용을 예시를 들면서 한 번 설명해보려고 한다. 다음과 같이 자식 클래스들이 있다고 하자.

```c#
class Monster
{
    public void Attack() { ... }
}

class Slime : Monster
{
    public void LiquidAttack() { ... }
}

class Bubbling : Monster
{
    public void TailAttack() { ... }
}

class RibbonPig : Monster
{
    public void StrikeAttack() { ... }
}
```
수많은 몬스터 종류가 있고, 이제 이 몬스터들을 맵에서 스폰(Spawn)되도록 하는 클래스를 만들려고 한다.

근데 업 캐스팅을 할 수 없다면, 다음과 같이 몬스터 종류 수만큼 오버로딩을 해야 하는 번거로움이 생긴다.
```c#
class MonsterSpawn
{
    public void Spawn(Slime slime) { ... }
    public void Spawn(Bubbling bubbling) { ... }
    public void Spawn(RibbonPig ribbonPig) { ... }
    ...
}
```
"객체를 받은 건 좋아, 근데 그러면 각 자식 클래스의 고유 기능은 어떻게 써?"
업캐스팅을 통해 자식 클래스의 객체를 받았다. 그런데, 받은 자식 클래스의 고유 기능은 어떻게 사용해야 할까?

자식 클래스는 부모 클래스의 확장 버전이기 때문에 부모 클래스 변수로는 자식 클래스의 멤버에 접근할 수 없다.

 

이럴 때 사용하는 게 바로 형변환(Type casting)이다. C#에서는 is와 as라는 멋진 형변환 연산자를 제공한다.

## is 연산자

객체가 해당 형식에 해당하는지 검사하여 그 결과를 bool 값으로 반환한다.

```c#
Monster monster = new Slime();
Slime slime;

if(monster is Slime)
{
    slime = (Slime)monster;
    slime.LiquidAttack();
}
```

## as 연산자
형식 변환 연산자와 같은 역할을 하나, 변환 실패 시 예외를 던지는 형식 변환 연산자와 달리 as 연산자는 객체 참조를 null로 만든다.

어렵게 말했지만, 형변환에 성공하면 그 타입으로 사용하면 되고, 실패하면 null을 반환한다는 말이다.

```c#
Monster monster = new Bubbling();
Bubbling bubbling = monster as Bubbling;

bubbling?.TailAttack();
```
일반적으로 (Slime) 과 같은 꼴로 형변환을 수행하는 것 대신 as 연산자를 사용하는 것을 권장한다고 한다.

형변환에 실패하더라도 예외가 일어나 갑자기 코드의 실행이 점프되는 일이 없기 때문에 코드를 관리하기가 더욱 수월하다고 한다.

 

단, as 연산자는 참조 형식에 대해서만 사용 가능하므로 값 형식의 객체는 기존 형변환 방식대로 해야 한다.

[맨 위로 이동하기](#){: .btn .btn--primary }{: .align-right}