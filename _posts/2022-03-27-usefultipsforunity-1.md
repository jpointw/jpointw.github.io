---
title:  "Unity C# > UnityEngine : Vector2, Vector3" 

categories:
  -  AllofUnity
tags:
  - [Game Engine, Unity]

published: true
toc: true
toc_sticky: true

date: 2022-03-27
last_modified_at: 2022-03-27
---

#Vector2, Vector3

> 위치 좌표, 벡터
## 변수/프로퍼티

###  magnitude

벡터의 길이 및 크기. float

<br>

###  sqrMagnitude

벡터의 길이 제곱. float

<br>

###  normalized

해당 벡터의 방향 벡터. (길이 1)

<br>

###  forward, backward, left, right, up, down

- `Vector3.forward`  그저 언제나 Vector3(0, 0, 1) 값이다. 
  - 이것을 Local로 쓸지, World로 쓸지는 개발자 선택
- `Vector3.right`  그저 언제나 Vector3(1, 0, 0) 값이다. 
  - 이것을 Local로 쓸지, World로 쓸지는 개발자 선택

<br>

###  zero

Vector3.zero 는 Vector3(0, 0, 0,)과  같다.


<br>



##  함수

###  SmoothDamp

> public static Vector3 SmoothDamp(Vector3 current, Vector3 target, ref Vector3 currentVelocity, float smoothTime, float maxSpeed = Mathf.Infinity, float deltaTime = Time.deltaTime);
`current` 벡터로부터 `target` 벡터 값까지 `smoothTime`동안 스무스하게 변화하는 과정에서의 벡터를 `currentVelocity`에 업데이트. 마지막 프레임에서의 속도를 나타낸다. 함수 안에서 계산되어 바깥으로 꺼내짐.

- 매개변수 첫번째 벡터가
- 매개변수 두번째 벡터로 되기까지
- 네번째 매개변수인 시간(float)동안 스무스하게 변화하는 값을 리턴한다.
- 세번째 매개변수는 Call by reference인 ref 참조 변수로서 직전, 마지막 프레임에서의 속도를 나타낸다. 함수 안에서 계산되어 바깥으로 꺼내짐.

<br>

###  Distance

> public static float Distance(Vector3 a, Vector3 b);
a 와 b 사이의 거리를 리턴한다. float 리턴.

<br>

###  Angle

> public static float Angle(Vector3 from, Vector3 to);
a 와 b 사이의 각도를 리턴한다. float 리턴.

<br>

###  Set

> public void Set(float newX, float newY, float newZ);
이 함수를 호출한 벡터의 x, y, z 요소를 설정한다.

<br>

###  SqrMagnitude

> public static float SqrMagnitude(Vector3 vector3);
- 벡터의 길이의 제곱 (루트는 취하지 않은 상태)
- 그래서 완전히 루트 취한 길이를 구하는 것 보다 쪼끔 더 성능이 더 좋다.

<br>

###  MoveForwards

> public static Vector3 MoveTowards(Vector3 current, Vector3 target, float maxDistanceDelta);
현재 위치 `current`에서 목표 위치 `target`까지 `maxDistanceDelta`의 속도로 이동한 결과인 **Vector3 를 리턴**한다.

```c#
transform.position = Vector3.MoveTowards(transform.position, destPos, moveSpeed * Time.deltaTime); 
```

<br>

###  Lerp

> public static Vector3 Lerp(Vector3 a, Vector3 b, float t);
  - 두 Vector3 `a`, `b` 사이를 `t` 비율만큼 보간한 Vector3 리턴
    - `0.5f`면 딱 중간
    - `1.0f`면 `b`을 그대로 따름
    - `0.0f`면 `a`을 그대로 따름
    - `0.2f`면 `a`에 좀 더 가깝게 회전

***
[맨 위로 이동하기](#){: .btn .btn--primary }{: .align-right}