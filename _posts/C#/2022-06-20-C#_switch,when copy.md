---
title:  "Unity C# > switch 문과 when 절, 그리고 switch 식" 

categories:
  -  C#
tags:
  - [C#]

published: true
toc: true
toc_sticky: true

date: 2022-06-20
last_modified_at: 2022-06-20
---

# switch 문과 when 절, 그리고 switch 식

---

>조건문은 논리적인 프로그램을 작성하는 데 필요한 부분이다.<br>
>if, else if, else 문과 함께 switch 문이라는 것도 배우게 된다.<br>
>switch 문에 사용되는 조건식은 정수 형식과 문자열 형식만 지원을 한다.<br>
>C# 7.0부터는 데이터 형식을 조건으로 사용할 수도 있게 된 것 같다.

> switch 문에 사용되는 조건식은 **정수 형식**과 **문자열 형식**만 지원을 한다.<br>
C# 7.0부터는 **데이터 형식**을 조건으로 사용할 수도 있게 된 것 같다.


## 데이터 형식 사용 예시
```C#
Object obj = 123; // Boxing

switch (obj)
{
  case int i: //obj에 담겨 있는 데이터 형식에 따라 case절 분기
  ...
  break;
  case float f:
  ...
  break;
  default:
    ...
    break;
}
```
>데이터 형식에 따라 분기할 때는 **<u>case 절에서 데이터 형식 옆에 반드시 식별자를 붙여줘야 한다.</u>**
<br>
하지만 switch 문을 사용하면서 불편한 점이 있었는데, if 문처럼 **검사 과정에서 조건을 체크할 수 없다는 점**이었다.
<br>
하지만, C#에서는 **when**이라는 키워드를 지원하여 이러한 부분을 해결할 수 있다.

## when을 사용하여 추가적인 조건 검사 사용 예시
```C#
object obj = 123;   // Boxing

switch (obj)
{
   case float f when f >= 0:    // obj가 float 형식이며, 0보다 크거나 같은 경우
       ...
       break;
}
```
>변환 성공 여부(True, False)를 반환해주는 메소드다.
>그렇기 때문에 예외가 발생하지 않아 현재 코드 흐름을 유지할 수 있는 장점이 있다.

<br>

>변환한 데이터는 out 키워드로 선언한 변수에 저장된다.


## switch 식

>switch 문을 좀 더 축약해서 간략하게 쓸 수도 있다.<br>
식이기 때문에 계산 결과를 내놓는다. 그렇기 때문에 분기를 거쳐 값을 내야 하는 경우에는 switch 문보다는 switch 식이 낫다.

>원래라면 다음과 같이 분기문을 작성했을 것이다.

``` C#
int input = Convert.ToInt32(Console.ReadLine());

int score = (int)(Math.Truncate(input/10.0) * 10);
// 1의 자리 버림 eX) 92 -> 90

string grade = "";

switch (score)
{
    case 90:
       grade = "A";
       break;
       
    case 80:
       grade = "B";
       break;
       
    case 70:
       grade = "C";
       break;
       
    case 60:
       grade = "D";
       break;
       
    default:
       grade = "F";
       break;
}
```

>그런데 이걸 좀 더 축약해서 switch 식으로 간략하게 표현할 수 있다.

>case 키워드와 ":"는 =>로 변경<br>
switch 문의 조건식은 switch 키워드 앞으로<br>
break; 절은 작성하지 않으며, 각 케이스는 콤마(,)로 구분<br>
default 키워드는 '_'로

``` c#
int input = Convert.ToInt32(Console.ReadLine());

int score = (int)(Math.Truncate(input/10.0) * 10);

string grade = score switch
{
    90 => "A",
    80 => "B",
    70 => "C",
    60 => "D",
    _ => "F"
};
```

[맨 위로 이동하기](#){: .btn .btn--primary }{: .align-right}