---
title:  "확장 메소드 (Extension Method)" 

categories:
  -  C#
tags:
  - [C#]

published: true
toc: true
toc_sticky: true

date: 2022-06-29
last_modified_at: 2022-06-29
---

# 확장 메소드 (Extension Method)란?

---

기존 클래스의 기능을 확장하는 C#에서 지원하는 기법이다.

일반적인 사용자 정의 클래스의 경우에는 추가적인 메소드가 필요하다면 클래스 내부에서 추가로 정의하면 된다.

하지만 내장 라이브러리들 같은 경우에는 그럴 수가 없다.

이런 경우에 유용한 것이 확장 메소드이다.
 
확장 메소드를 사용하려면 다음과 같이 필요하다.

    네임스페이스를 선언
    메소드를 선언하되, static 한정자로 수식해야 한다.
    선언하는 클래스 또한 static 한정자로 수식해야 한다.
    메소드의 첫 번째 매개변수는 반드시 this 키워드와 함께 확장하고자 하는 클래스의 인스턴스여야 한다.
    메소드에 필요한 매개변수는 두 번째 매개변수부터 채워준다.


```c#
namespace 네임스페이스이름
{
    public static class 클래스이름
    {
        public static 반환형 메소드이름(this 대상형식 식별자, 매개변수목록)
        {
        
        }
    }
}
```
책에 나와있는 코드를 이용하여 연습해봤다.

정수(integer) 자료형에 제곱을 해주는 Power() 메소드를 추가한 것이다.
```c#
namespace IntegerExtension
{
    public static class IntegerExtension
    {
        public static int Power(this int myInt, int exponent)
        {
            int result = myInt;

            for(int i = 1; i < exponent; i++)
            {
                result *= myInt;
            }
            return result;
        }
    }
}

using IntegerExtension;

int variable = 3;
Console.WriteLine(variable.Power(2));
Console.WriteLine(variable.Power(3));
```
9
27

int형으로 선언한 변수이지만, 기존에 없었던 Power() 메소드를 사용할 수 있는 걸 볼 수 있다.

확장 메소드는 이런 식으로 사용된다.

 

물론 문자열(string)에도 적용이 가능하고, 그 외 다른 클래스에도 적용이 가능하다.

## string 클래스에 확장 메소드 적용 해보기
```c#
string hello = "Hello";
Console.WriteLine(hello.Append(", World!"));  // "Hello, World!" 출력
```
위와 같이 string 클래스에 Append()라는 메소드를 확장해볼 것이다.

```c#
namespace StringExtension
{
    public static class StringExtension
    {
        static StringBuilder strBuilder = new StringBuilder();

        public static string Append(this string originString, string appendString)
        {
            strBuilder.Clear();
            strBuilder.Append(originString);
            strBuilder.Append(appendString);
            return strBuilder.ToString();
        }
    }
}
```
무분별한 스트링 가비지 생성을 막기 위해, StringBuilder 클래스를 사용하여 만들어 줬다.

그리고 한 번 실행 해봤다.
```C#
using StringExtension;

string hello = "Hello";
Console.WriteLine(hello.Append(", World!"));
```
Hello, World!
[맨 위로 이동하기](#){: .btn .btn--primary }{: .align-right}