---
layout: single
title: "[C# Basic] 프로퍼티 사용법"
categories:
  - C# Basic
toc: true
toc_sticky: true
toc_label: "On this Page"
date: 2020-07-18
---



C#은 객체지향의 요소 중 하나인 은닉성을 표현하기 위해 프로퍼티라는 기능을 제공한다.

이번에는 프로퍼티를 작성하고 활용하는 방법에 대해 알아보자

-----

##### * 일반 메소드를 사용한 접근법

```c#
//일반 메소드 사용 예제
class Bono{
    private string Name;
    
    public string GetName(){ return Name; }
    public void SetName(string name){ this.Name = name; }
}
```

위 예제는 Name이라는 필드를 선언했지만 외부에서는 이를 감추고 GetName()과 SetName() 메소드를 통해 Name 필드에 접근하도록 하는 예시이다.  위 클래스는 아래와 같이 사용될 수 있다.

```c#
Bono bono = new Bono();
bono.SetName("Bono");
Console.WriteLine(bono.GetName());
```

코드에 문제는 없지만 C#은 위와 같은 기능을 더 편리하게 사용하기 위해 프로퍼티 기능을 제공한다.



##### * 프로퍼티를 사용한 접근법

프로퍼티의 예제를 살펴보자.

```c#
// 프로퍼티 사용 예제
class Bono{
    string name;
    public string Name{
        get{ return name; }  // 데이터를 읽어온다.  get이 없으면 해당 프로퍼티는 쓰기 전용이 된다
        set{ name = value; }  // 데이터를 쓴다.  set이 없으면 해당 프로퍼티는 읽기 전용이 된다
    }
}
```

별 차이는 없어 보인다.  하지만 사용법의 예시를 보자.

```c#
Bono bono = new Bono();
bono.Name = "Bono";  // = 연산자를 통해 값을 할당할 수 있다.
Console.WriteLine(bono.Name);  // 데이터를 바로 읽어올 수 있다.
```

- 메소드 사용 없이 객체의 데이터를 바로 읽고 쓸 수 있다.
- get 구현부가 없는 경우 이는 쓰기 전용이 된다
- set 구현부가 없는 경우 이는 읽기 전용이 된다



##### * 자동구현프로퍼티

```c#
public string Name {get; set;}  // 자동구현프로퍼티
public int Age {get; set;} = 15;  // 자동구현프로퍼티 초기화
```

- 프로퍼티는 데이터의 오염에 대해서는 메소드처럼 안전하고, 데이터를 다룰 때는 필드처럼 간결하다는 장점이 있다.
- 단순하게 데이터를 읽고 쓰는 경우 C#은 자동구현프로퍼티 기능을 제공해 편리함을 제공하고 있다.
- 필드를 선언할 필요도 없고 위 예제처럼 get, set 만 넣어주면 된다.
- 자동구현프로퍼티의 초기화 또한 간단하게 할 수 있다.
- 자동구현프로퍼티 사용시 C# 컴파일러가 내부적으로 새로운 변수를 만들어 일반 프로퍼티처럼 알아서 관리를 해준다.



##### * 프로퍼티와 생성자

```c#
class Bono{
    public string Name {get; set;}
}

...
Bono bono = new Bono(){
    Name = "Bono";
}
```

- 객체 생성시 프로퍼티를 이용한 초기화를 위와 같이 할 수 있다.
- 초기화 하고 싶은 프로퍼티와 값을 생성자에 넣으면 객체 생성시 값이 초기화된 상태로 객체가 만들어진다.



##### * 무명형식

```c#
var bono = new {Name="Bono", Age=15};

Console.WriteLine(bono.Name);
Console.WriteLine(bono.Age);
```

- 인스턴스를 만들고 다시는 사용할 필요가 없는 경우 무명형식을 사용할 수 있다.
- 무명 형식 선언시 new 키워드를 통해 임의의 프로퍼티 이름과 값을 할당한다.
- 무명형식은 객체처럼 프로퍼티에 접근해서 사용할 수 있다.
- 주의할점은 **무명 형식의 프로퍼티에 할당된 값은 변경이 불가능**하다는 점이다.



##### * 인터페이스의 프로퍼티

```c#
interface Bono{
    string Name {get; set;}
}
```

- 인터페이스는 메소드뿐 아니라 프로퍼티와 인덱서도 가질 수 있다.
- 인터페이스는 구현을 가지지 않아야 하는데 따라서 프로퍼티 선언시 생김새가 자동구현 프로퍼티와 비슷하다.
- 이를 상속받는 클래스는 자동구현프로퍼티를 써도 되고 직접 구현을 해도 된다.  (해당 프로퍼티를 사용만 하면 된다)



##### * 추상클래스의 프로퍼티

```c#
abstract class Bono{
    abstract string Name {get; set;}  // 추상프로퍼티
}
```

- 추상클래스는 구현이 된것과 구현되지 않은 것 모두 가질 수 있다.
- abstract로 한정된 프로퍼티는 파생클래스에서 구현을 강제한다.
- 인터페이스 상속과 동일하게 추상프로퍼티를 오버라이드 하는 경우에도 자동구현프로퍼티를 써도 되고 직접 구현을 해도 된다. (오버라이드 하기만 하면 된다)

