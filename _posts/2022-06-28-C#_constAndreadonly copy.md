---
title:  "const와 readonly의 차이" 

categories:
  -  C#
tags:
  - [C#]

published: true
toc: true
toc_sticky: true

date: 2022-06-28
last_modified_at: 2022-06-28
---

# const와 readonly의 차이

---

## const

```c#
const float range = 5f; // 공격 사거리
```
컴파일러는 상수에 지정된 값을 실행파일 안에 기록해둔다.

이 말은 즉슨, 상수는 프로그램이 실행되기 전부터 이미 값이 정해져 있다는 의미가 된다. (컴파일 타임에 이루어짐)

 

그렇기 때문에 변수와 달리, 상수는 프로그램 실행 중(런타임)에는 그 값을 절대로 바꿀 수 없다.

const 상수는 다음과 같은 특징들을 가진다.

### 1.선언 시, 반드시 값을 할당해야 한다.
```c#
// const float range; 컴파일 오류
const float range = 5f;
```
### 2. 자동으로 정적 변수(static) 속성을 가진다.
```c#
class Monster
{
    public const float range = 5f;
}

class Main
{
    static void Main(string[] args)
    {
       float range = Monster.range;
    }
}
```

## readonly
```c#
class Monster
{
    private readonly float range;     // 읽기 전용 필드
    
    public Monster(float range)
    {
       this.range = range;
    }
}
```
읽기만 가능한 필드다.

클래스나 구조체의 멤버로만 존재할 수 있으며, 생성자 안에서만 초기화가 가능하다.

생성자 안에서 한 번 값을 지정하면, 그 후로는 값을 변경할 수 없는 것이 특징이다.

즉, const와 달리 런타임 중에 생성되어 값을 변경할 수 있는 기회가 한 번은 있는 셈이다.

readonly 상수는 다음과 같은 특징들이 있다.

### 1.선언 시, 값을 할당하지 않아도 된다.
```c#
readonly float range;
```

### 2.static 상수로 만들 경우에는 static 생성자를 통해 초기화할 수 있다. <br> const와 달리, 자동으로 static 속성을 가지지 않는다.

```c#
class Monster
{
   public readonly static float range;
   
   static Monster()
   {
      range = 10f;
   }
}
```
[맨 위로 이동하기](#){: .btn .btn--primary }{: .align-right}