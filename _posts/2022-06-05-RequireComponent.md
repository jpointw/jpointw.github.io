---
title:  "Unity C# > RequireComponent" 

categories:
  -  AllofUnity
tags:
  - [Game Engine, Unity]

published: true
toc: true
toc_sticky: true

date: 2022-06-05
last_modified_at: 2021-04-22
---

#RequireComponent의 이해

> RequireComponent 속성은 요구되는 컴포넌트를 종속성으로 **자동으로** 추가해줍니다.

# 왜 사용하는가?

> RequireComponent는 **설정 오류를 방지**하는데 유용합니다. 예를 들어 RequireComponent가 있는 스크립트는 Rigidbody가 항상 같은 GameObject에 추가되도록 요구할 수 있습니다. 그래서 이를 통해 자동으로 수행되므로 설정이 잘못 될 수 없습니다. 

#  사용 방법 코드
``` C#
using UnityEngine;

[RequireComponent(typeof(Rigidbody))]
public class Test : MonoBehaviour
{
    Rigidbody rb;
    void Start()
    {
        rb = GetComponent<Rigidbody>();
    }

    private void FixedUpdate()
    {
        rb.AddForce(Vector3.forward);
    }
}
```
<br>

# 힙 파편화

> **힙에 메모리가 저장될 때** 데이터의 크기에 따라 블록 단위로 저장이 되는데 **서로 다른 크기**의 메모리 블록이 저장되고 해제되면서 남은 공간이 생기게 된다. 그렇기 때문에 실제 개발자가 사용하려했던 프로그램 메모리 사용량이 늘어나고, **메모리를 찾기 위해 가비지 컬렉터가 더 자주 실행하게 된다.**
<br>
***
[맨 위로 이동하기](#){: .btn .btn--primary }{: .align-right}