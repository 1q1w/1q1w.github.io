---
layout: single
title: "[C# Basic] 예외처리"
categories:
  - C# Basic
toc: true
toc_sticky: true
toc_label: "On this Page"
date: 2020-07-24
---



이번에는 C#의 예외처리에 대해 알아보자.

-----

##### * 예외처리?

- 프로그래머가 생각하는 시나리오에서 벗어나는 사건을 예외라 한다.
- 예외가 프로그램의 오류나 다운으로 이어지지 않도록 하는 방어적 코딩의 개념으로 예외처리가 있다.



##### * try ~ catch 로 예외처리

```c#
try{
    // 실행하고자 하는 코드
}
catch(IndexOutOfRangeException e){  // 인덱스 범위 초과 오류 발생시
    // 예외 발생시 처리
}
catch(예외객체2){
    // 예외 발생시 처리
}
```

- try 절의 코드블록에는 예외가 없는 경우 실행될 코드가 들어가고 catch 절에는 예외 발생시의 처리 코드가 들어간다.
- catch절의 예외 객체와 예외의 형식이 일치해야 해당 catch 절로 들어갈 수 있다.
- catch절은 여러개가 될 수 있다.



##### * System.Exception 클래스

- System.Exception 클래스는 모든 예외의 기반 클래스이다.  C#의 모든 예외 클래스는 반드시 해당 클래스를 상속받아야 한다.
- 따라서 catch 절에서 Exception 형식의 예외를 받는다면 모든 예외를 다 처리할 수 있게 된다.
- 예외 상황에 따라 섬세한 처리가 필요한 경우 Exception 형식으로 처리하면 안되고 더 세분화된 클래스로 처리해야 한다.



##### * 예외 던지기

```c#
try{
    if(true){
    	throw new Exception("예외가 발생했습니다");  // throw 문을 통해 예외 객체를 catch로 던진다.
    }
}
catch(Exception e){
    Console.WriteLine(e.Message);
}
```

- throw문을 통해 예외를 catch절로 던질 수 있다.
- C# 7.0부터는 throw를 식(expression)으로도 사용할 수 있도록 개선되었다.  따라서 아래와 같은 코딩이 가능하다.

```c#
int a = b ?? throw new ArgumentNullException();  // b가 null이면 예외 발생
```



##### * try ~ catch, finally

```c#
try{
    // 실행할 코드
}
catch(Exception e){
    // 예외 발생시 처리
}
finally{
    // 예외가 나든 안나든 마지막에 실행할 코드
}
```

- try 블록에서 코드 실행중 예외가 발생하면 catch절로 바로 건너뛴다.  이러한 경우 try 절의 자원해제와 같이 중요한 코드를 실행하지 못할 수 있다.
- 이 경우 finally 절을 사용할 수 있다.  finally 절은 try ~ catch 문의 마지막에 사용한다.
- **자원해제와 같이 꼭 실행되어야 하는 코드는 finally 절에 넣어두면 된다**.
- **try절 안에 return이나 throw문이 있더라도 finally절은 항상 실행**된다.
- finally절 안에서 또 예외가 발생하면 이때는 이 예외를 받아주는 코드가 없으므로 처리되지 않은 예외가 된다.  finally 절 안의 내용을 다시 한번 try ~ catch 로 감싸는 방법도 있다.



##### * 사용자 정의 예외 클래스 만들기

```c#
class BonoException : Exception {
    // 생성자 및 프로퍼티 등 정의
}
```

- 모든 예외 객체는 System.Exception 클래스로부터 파생되어야 한다.
- Exception 클래스를 상속받기만 하면 새로운 예외클래스를 만들 수 있다.



##### * 예외 필터하기

```c#
class BonoException : Exception{  // 예외 클래스 생성
    public int ErrorNo{get; set;}  // 예외 클래스의 프로퍼티
}

//...

try{
    int num = 1;
    
    if(num < 5){
        throw new BonoException(){ErrorNo = num};  // num이 5보다 작으면 예외 발생
    }
}
catch(BonoException e) when (e.ErrorNo < 2){  // 5보다 작아서 예외가 발생했지만 ErrorNo가 2보다 작을때만 해당 catch절을 타도록 함
    Console.WriteLine("ErrNo는 5보다 작을때 오류가 나지만 여기서는 2보다 작을때만 처리");
}
```

- C# 6.0부터 catch절이 받아들일 예외 객체에 제약조건을 걸 수 있는 예외 필터 기능이 도입되었다.
- catch() 문 뒤에 when 키워드를 이용해 제약조건을 명시하면 된다.  (when을 if라고 생각하면서 이해하면 쉽다.)



##### * 예외처리에 대해..

- 예외처리는 실제 로직과 문제 발생시 이를 처리하는 코드를 분리시켜 코드를 간결하게 만들어준다.
- 또한 예외 객체의 StackTrace 프로퍼티를 통해 문제가 발생한 부분의 소스코드 위치를 알려줌으로써 디버깅이 용이해진다.
- 또한 여러 문제점을 하나로 묶거나 종류별로 정리해주는 효과가 있다.
- 즉 **예외처리를 사용한 코드는 작성하기도 쉽고, 읽기도 쉽다는 장점**이 있다.





