---
title:  "Unity C# > 기초 문법 Property" 

categories:
  -  C#
tags:
  - [C#]

published: true
toc: true
toc_sticky: true

date: 2022-06-15
last_modified_at: 2022-06-15
---

#프로퍼티(Property)
---

> 외부에서 클래스 변수에 값을 할당할 때, 일반적으로 다음과 같이 원할 것이다.
변수의 값이 항상 올바르도록 강제하여, 할당된 값을 확실히 하길 바랄 경우
변수의 값이 변경되었을 때를 감지하여, 이 값에 영향을 받는 다른 함수나 동작을 실행하길 원할 경우

# 왜 사용하는가?

> 예시를 통해 학습하기

#  예시 코드
``` C#
using UnityEngine;

public class Test : MonoBehaviour
{
    public float HP
    {
      get
      {
        return _hp;
      }

      set
      {
        if(_hp >= 0f && _hp <= 100f) _hp = value;
      }
    }

    private float _hp;
}
```
>변수처럼 선언되지만, 함수처럼 중괄호로 묶이는 것이 특징
get: 외부에서 해당 프로퍼티에 접근하여 읽어야 하는 상황에서 호출됨
set:외부에서 해당 프로퍼티에 접근하여 값을 할당하는 상황에서 호출됨
***
[맨 위로 이동하기](#){: .btn .btn--primary }{: .align-right}