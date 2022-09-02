---
title:  "구조체(Structure)" 

categories:
  -  C#
tags:
  - [C#]

published: true
toc: true
toc_sticky: true

date: 2022-07-01
last_modified_at: 2022-07-01
---

# 구조체(Structure)란?

---
C#의 복합 데이터 형식에는 클래스 말고도 구조체가 있다. C 언어에서 봤던 그 구조체가 맞긴 하지만 여러모로 다른 점이 있다. 클래스처럼 필드와 메소드를 가질 수 있는 점에서 클래스와 구조체 둘은 서로 많이 비슷해 보인다.

```c#
struct MyStructure
{
    public int data;
    
    public void Method()
    {
       ...
    }
}
```
클래스와 달리 은닉성을 비롯한 객체지향의 원칙을 강하게 적용하지 않는 편이라, public으로 필드를 사용해서 주로 쓴다.

### 1.구조체는 값 형식이다.

알아야할 제일 큰 대표적인 차이점이 구조체는 참조 형식이 아니라 값 형식이라는 점이다.

그렇기 때문에 스택에 할당되게 되고, 선언한 블록의 끝에 도달하면 자동으로 반납처리 된다. 

 

Unity에서 스크립트를 작성하다 보면, 다음과 같이 쓸 일이 많다.

```c#
public class Test : MonoBehaviour
{
    void Update()
    {
        transform.position = new Vector3(1f, 2f, 3f);
    }
}
```

여기서 Vector3는 클래스가 아니라 구조체다. new 연산자를 사용해도, 구조체이기 때문에 스택 할당이 된다.

그렇기 때문에 매 프레임마다 실행되는 Update() 함수에서 사용해도 가비지는 전혀 생성되지 않는다.

### 2. 값 형식이기 때문에 값이 복사되어 전달된다.

값 형식이기 때문에 복사를 하거나, 매개변수 등에 넣으면 원본이 아닌 복사본을 넘기게 된다.

이 때, 복사를 할 경우에는 깊은 복사가 일어나게 된다.

```c#
struct MyStructure
{
    public int data;
    
    // public MyStructure() { }   구조체는 매개변수 없는 생성자 생성 불가능
}

MyStructure myStruct1;
myStruct1.data = 10;

MyStructure myStruct2;
myStruct2 = myStruct1;     // myStruct2.data는 10

myStruct1.data = 20;       // myStruct1.data는 20,
                           // myStruct2.data는 10
``` 

참고로 구조체는 매개변수 없는 생성자를 선언할 수 없다. 대신, CLR이 구조체의 각 필드를 기본값으로 초기화를 해준다.

### 3. 변경불가능(Immutable) 객체로 만들 수 있다.

객체는 속성(상태)와 기능(행위)로 이루어진다.

상태의 변화를 허용하는 객체를 변경가능(Mutable) 객체라고 하고, 허용하지 않는 객체를 변경불가능(Immutable) 객체라고 한다.
구조체는 클래스와 달리, 변경불가능 객체를 만드는 것이 가능하다.

```c#
readonly struct 구조체이름
{
   ...
}
```
readonly를 이용하여 구조체를 선언하면, 컴파일러는 해당 구조체의 모든 필드가 readonly로 선언되도록 강제한다.
```c#
readonly struct ImmutableStruct
{
    public readonly int ImmutableField;
    // public int MutableField;   컴파일 오류
}
```

그렇다면, 변경불가능 객체는 왜 사용하는 걸까? 여러가지가 있지만, 대표적인 내용들은 다음과 같다.

 

멀티 스레드 간에 동기화(Synchronization)를 할 필요가 없기 때문에 프로그램 성능 향상이 가능하다.
버그로 인한 상태(데이터)의 오염을 막을 수 있다.
 

2번 말의 의미는, 수많은 스레드에서 객체의 상태를 변경시켜 버그를 발생시킨다면 스레드들을 전부 조사해야 하는 상황이 발생하는 걸 막을 수 있다는 말이다.

### 4. 읽기 전용 메소드를 선언할 수 있다.
읽기 전용 메소드 역시 구조체에서만 선언이 가능하다. 프로그래머도 사람이기 때문에 읽기만 하고 수정해선 안 되는 데이터를 수정해버리는 실수를 하기도 한다. 그걸 이제 컴파일러의 도움을 받아, 쉽게 알아차리면 좋지 않을까?

책에 좋은 예제가 있길래 한 번 같이 타이핑하며 공부 해본다.

에어컨 설정을 담는 구조체 코드를 작성한다고 해보자.

```c#
struct ACSetting
{
    public double target;             // 희망 온도
    public double currentInCelsius;   // 현재 온도(섭씨)
}
```
이 구조체에 섭씨로 되어 있는 현재 온도를 화씨로 변환해서 반환하는 메소드를 추가했다.
```C#
public double GetFahrenheit()
{
    target = currentInCelsius * 1.8 + 32;   // 화씨 계산 결과를 target에 저장
    return target;
}
```
위에서 말했던 프로그래머의 실수가 일어날 수 있다는 부분이 이런 경우를 말한다.

화씨 온도를 계산해서 반환하기만 하면 되는데, 목표 온도를 건드려버렸다.

 

이런 실수를 사전에 예방하고자, 읽기 전용 메소드를 선언한다.

```C#
public readonly double GetFahrenheit()
{
    // target = currentInCelsius * 1.8 + 32;     컴파일 오류
    return target;
}
```

위와 같이, readonly 한정자를 이용해서 메소드에게 상태를 바꾸지 않도록 강제할 수 있다.

 
[맨 위로 이동하기](#){: .btn .btn--primary }{: .align-right}