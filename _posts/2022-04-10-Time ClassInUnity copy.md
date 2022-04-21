---
title:  "Unity C# > UnityEngine : 유니티 에서 시간이란?" 

categories:
  -  AllofUnity
tags:
  - [Game Engine, Unity]

published: true
toc: true
toc_sticky: true

date: 2021-04-10
last_modified_at: 2021-04-10
---

#유니티 사용 시 Time Class의 이해

> Deltatime,time,fixedDeltatime,timeScale
## 변수/프로퍼티

###  Deltatime

> Time.Deltatime

다음 **업데이트**까지의 시간 간격을 의미한다.
<br>
가장 많이 쓰이며 왜 써야할까?
<br>
프레임 당 연산 속도가 개인 컴퓨터의 성능에 따라 바뀔 수 있기 때문에 프레임 당 이동을 제한해 줌으로써 격차 완화가 가능하다. (60프레임 기준 1프레임까지의 시간은 0.016초)
<br>

###  time

> Time.time

스타트 함수에서 타임 시작값은 0이다
<br>
현재 플레이한 시간을 계산하고 싶을 때 사용한다. ex) 총 플레이 시간

<br>

###  fixedDeltatime

> Time.fixedDeltaTime

**Deltatime**과의 차이점은?
<br>
유니티는 물리 업데이트와 렌더링 업데이트를 구분지어 놓았기 때문에 사용한다.
**fixeddeltatime**은 물리 엔진 사용 시 필요하고
일반 **deltatime**은 렌더링 업데이트 시 필요하다.
<br>

###  timeSclae

>Time.timeScale

**언제 쓸까?** 구현에서 슬로우 모션이나 빠른 동작을 표현할 때 사용한다.
<br>
타임스케일이 0이라면 : 정지
<br>
타임스케일이 1이라면 : 일반적인 시간
<br>
타임스케일이 n 라면 : n만큼 곱해서 빨라짐

<br>

***
[맨 위로 이동하기](#){: .btn .btn--primary }{: .align-right}