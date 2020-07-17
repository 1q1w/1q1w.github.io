---
layout: single
title: "[C# Basic] 연산자"
categories:
  - C# Basic
toc: true
toc_sticky: true
toc_label: "On this Page"
date: 2020-07-08
---



C#의 기초부터 차근차근 다시 한 번 짚어보자.  

이번에는 C#의 연산자에 대해 알아보자.

-----

##### * 산술연산자

- +, -, *, /, % 가 해당.  정수, 부동소수점, decimal 형식에 대해서만 사용이 가능하다.



##### * 증가연산자와 감소연산자

```c#
int a = 10;
Console.WriteLine(a++);  // 10 출력 후 1을 더함
Console.WriteLine(++a);  // 1을 더한 뒤 출력.  즉 12가 출력
```

- ++ 는 피연산자의 값을 1 증가
- -- 는 피연산자의 값을 1 감소
- 변수의 앞에 사용하면 변수의 값을 변경한 뒤 해당 문장이 실행
- 변수의 뒤에 사용하면 해당 문장의 실행이 끝난 후 변수의 값이 변경



##### * 문자열 결합 연산자

```c#
string result = "가나" + "다라";
Console.WriteLine(result);  // "가나다라" 출력
```

- 산술연산자와 달리 문자를 더해주는 문자열 결합 연산자



##### * 관계연산자

- 크고 작음, 같음 등을 비교하는 관계연산자.  true 또는 false를 반환
- < , >, <=, >=, ==, != 가 해당
- != 는 왼쪽 피연산자와 오른쪽 피연산자가 다르면 true, 같으면 false를 반환



##### * 논리연산자

- 논리연산자는 논리곱연산자(&&)와 논리합연산자(||), 부정연산자(!) 가 있음
- && 논리곱연산자는 모두 참이어야 그 결과가 참이 되고 그 외에는 모두 거짓
- || 논리합 연산자는 하나라도 참이면 참이고 모두 거짓인 경우만 거짓
- ! 부정 연산자는 참, 거짓 값을 반대로 뒤집어서 출력



##### * 조건연산자

```c#
int a = 30;
string result = a == 10 ? "숫자는십" : "숫자는 십아님";
```

- 조건식이 참이면 참일때 값을, 거짓이면 거짓일때 값을 선택
- [조건식 ? 참일때 값 : 거짓일때 값]  형태로 사용



##### * 널 조건부 연산자

```c#
class Bono{
    public int a;
}

Bono bono = null;

int? temp = bono?.a;  // bono가 null이 아니므로 a에 접근하여 0을 반환
```

- 널 조건부 연산자 ?. 는 C# 6.0에서 적용.  해당 객체가 null인지 검사하여 객체가 null이면 null을 반환하고, 그렇지 않은 경우 . 뒤에 지정된 멤버를 반환

```c#
ArrayList a = null;
a?.Add("일번");  // a가 null이므로 add는 호출되지 않음
Console.WriteLine($"{a?[0]}");  // a가 null 이므로 반환되지 않음

a = new ArrayList();
a?.Add("일번");  // a가 null이 아니므로 a에 요소 추가
Console.WriteLine($"{a?[0]}");  // a가 null이 아니므로 a[0] 출력
```

- ?[] 도 동일한 역할을 하는 연산자.  null이 아니면 배열과 같은 컬렉션의 인덱스에 접근 가능



##### * 비트연산자

- <<, >> : 첫 피연산자의 비트를 두번째 피연산자의 수만큼 왼쪽, 오른쪽으로 이동
- & 두 피연산자의 비트 논리곱 시행
- | 두 피연산자의 비트 논리합 수행
- ^ 두 피연산자의 비트 베터작 논리합 수행
- ~ 보수 연산자 : 피연산자이 비트를 0은 1로, 1은 0으로 반전



##### * 할당연산자

- = 는 오른쪽 피연산자를 왼쪽에 할당한다.  이에 += 와 같이 연산자를 하나 더 붙이면 더한 뒤 왼쪽에 할당하는 것을 간단히 표현하는 것이 가능하다.
- 간단히 표현하면 a += b; 는 a = a + b;  와 동일하다.



##### * null 병합 연산자

```c#
int? a = null;
int b = 1;
var c = a ?? 0;  // a가 null이므로 0이 반환
var d = b ?? 0;  // b가 null이 아니므로 1 반환
```

- ?? 는 변수/객체의 null 검사를 간결하게 만들어준다.
- 왼쪽 피연산자가 null이면 오른쪽 피연산자를, null이 아니면 왼쪽 피연산자를 반환












