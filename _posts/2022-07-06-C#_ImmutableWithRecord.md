---
title:  "레코드(Record) 형식으로 만드는 불변(Immutable) 객체" 

categories:
  -  C#
tags:
  - [C#]

published: true
toc: true
toc_sticky: true

date: 2022-07-05
last_modified_at: 2022-07-05
---

# 불변 객체(Immutable Object)

---
불변 객체는 내부 상태(데이터)를 변경할 수 없는 객체를 말한다. 상태를 변경할 수 없다는 특성 때문에 불변 객체에서는 데이터 복사와 비교가 많이 이뤄진다.

레코드(record)는 이런 불변 객체에서 빈번하게 일어나는 복사와 비교 연산을 편하게 수행할 수 있도록 C# 9.0에서 도입된 형식이다.

```c#
record Transaction
{
    ...
}
```
클래스는 참조 형식이므로 모든 필드를 readonly로 선언하면 불변 객체로 만들 수 있다.

구조체는 값 형식이므로 readonly struct로 구조체를 선언하면 된다. 컴파일러가 모든 필드를 readonly로 선언하도록 강제하기 때문.

값 형식 객체는 다른 객체에 할당할 때 깊은 복사를 수행한다.

깊은 복사란, 모든 필드가 새 객체가 가진 필드에 1:1로 복사하는 것을 말한다.

배열의 요소에 입력하거나 함수 인수로 사용할 때도 늘 깊은 복사가 일어나는 것이다.

이런 깊은 복사 방법은 모든 필드를 1:1로 비교해야 하는 불변 객체에 필요한 비교 방법이다.

다만, 필드가 많을수록 복사 비용이 커지고 특히 여러 곳에서 객체를 사용해야 하는 경우에는 더 커지기 때문에 오버헤드가 존재한다.

참조 형식은 위와 같은 오버헤드가 없는 것이 장점이다. 객체가 참조하는 메모리 주소만 복사하면 되기 때문.

하지만, 깊은 복사를 제공하지 않으므로 프로그래머가 직접 깊은 복사를 구현해야 한다.

보통은 object로부터 상속받은 Equals( ) 메소드를 오버라이딩해서 사용하는 편이다.

"그러면, 참조 형식과 값 형식의 장점만 가져온 불변 객체는 못 만들어?"
그것이 바로 이번 글에서 소개하는, C# 9.0에서 지원하는 레코드(Record) 형식이다.

참조 형식의 비용 효율을 가지면서도, 값 형식처럼 깊은 복사 비교를 제공한다.

레코드를 어떻게 사용하는지 한 번 알아보자.

## 레코드 선언하기

레코드는 record 키워드와 초기화 전용 자동 구현 프로퍼티를 함께 이용해서 선언한다.

물론 쓰기 가능한 프로퍼티와 필드 또한 자유롭게 선언해 넣을 수 있긴 하다.

```c#
record Transaction
{
    public string From { get; init; }
    public string To   { get; init; }
    public int Amount  { get; init; }
}
```

이렇게 선언한 레코드로 인스턴스를 만들면 불변 객체가 만들어 진다.

```c#
Transaction T1 = new Transaction {From="Alice", To="Bob", Amount=100};
Transaction T2 = new Transaction {From="Bob", To="Charlie", Amount=300};
```
## with를 이용한 레코드 복사

C# 컴파일러는 레코드 형식을 위한 복사 생성자를 자동으로 작성한다.

하지만, 이 복사 생성자는 protected로 선언되기 때문에 명시적으로 호출할 수 없고, 다음과 같이 with 식을 이용해야 한다.
```c#
Transaction T1 = new Transaction {From="Alice", To="Bob", Amount=100};
Transaction T2 = with T1 {To="Charlie"};
```
T1의 모든 값을 T2에 복사한 다음, T2의 'To' 프로퍼티만 "Charlie"로 수정한 것이다.

이런 식으로 유용하게 레코드를 복사하고 수정할 수 있다.

## 레코드 객체 비교하기

컴파일러는 레코드의 상태를 비교하는 Equals( ) 메소드를 자동으로 구현한다.

레코드는 참조 형식이지만 값 형식처럼 Equals( ) 메소드를 구현하지 않아도 비교가 가능하다.

```c#
Transaction T1 = new Transaction {From="Alice", To="Bob", Amount=100};
Transaction T2 = new Transaction {From="Alice", To="Bob", Amount=100};

Console.WriteLine(T1.Equals(T2));       // True
```
[맨 위로 이동하기](#){: .btn .btn--primary }{: .align-right}