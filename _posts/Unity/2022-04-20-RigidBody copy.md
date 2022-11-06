---
title:  "Unity C# > UnityEngine : RigidBody 컴포넌트 이해" 

categories:
  -  AllofUnity
tags:
  - [Game Engine, Unity]

published: true
toc: true
toc_sticky: true

date: 2021-04-20
last_modified_at: 2021-04-20
---

#유니티 사용 시 RigidBody 컴포넌트 이해

> 
## 리지드바디

###  Mass

> 오브젝트의 질량(kg),물체 간 충돌 시 반응을 결정한다.
<br>

###  Drag

> 공기 저항(무한대라면 오브젝트는 즉시 정지)
<br>

###  Angular Drag

> 토크 회전 시 공기 저항(무한대라도 회전이 멈추지는 않음)

<br>

###  Use Gravity

>중력 on/off

<br>

###  Is Kinematic

>활성화되면 오브젝트는 물리 엔진으로 제어되지 않고 오로지 Transform으로 제어

<br>

###  Interplate

> RigidBody의 움직임이 어색할 경우 적용

<br>

###  Collision Detection

>오브젝트가 충돌의 감지 없이 다른 오브젝트를 지나쳐 가는 것을 방지

<br>


###  Constraints

>리지드바디 X,Y,Z축의 움직임을 제한

<br>

***
[맨 위로 이동하기](#){: .btn .btn--primary }{: .align-right}