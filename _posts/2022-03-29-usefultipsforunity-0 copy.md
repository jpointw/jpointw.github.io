---
title:  "Unity C# > UnityEngine : useful-mathf" 

categories:
  -  AllofUnity
tags:
  - [Game Engine, Unity]

published: true
toc: true
toc_sticky: true

date: 2022-03-29
last_modified_at: 2022-03-29
---

#유니티 사용 시 유용한 mathf 함수

> 절댓값,최대/최솟값,근사값,선형보간
## 변수/프로퍼티

###  절댓값

> Math.Abs(float num)

Abs는 받은 num 인자의 절댓값을 반환

<br>

###  최대/최솟값

> Mathf.Clamp(float num,float min, float max)

Clamp는 num 값을 min,max에 맞추어 재조정
num값이 min보다 작으면 min값을, num값이 max값보다 크면 max값을 반환

<br>

###  근사

> Mathf.Approximately(flaot a, float b)

두 변수 a, b가 같은지(근사한지) 비교
물리를 사용하다 보면 주로 float를 사용하기 때문에 == 연산자 사용 시 일치하지 않을 수 있음

<br>

###  선형보간

>Math.Lerp(float a, float b, float t)

a와 b사이의 값을 반환하는 함수 t

<br>

***
[맨 위로 이동하기](#){: .btn .btn--primary }{: .align-right}