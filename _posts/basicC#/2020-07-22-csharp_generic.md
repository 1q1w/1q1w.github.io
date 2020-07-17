---
layout: single
title: "[C# Basic] 일반화 프로그래밍(Generic)"
categories:
  - C# Basic
toc: true
toc_sticky: true
toc_label: "On this Page"
date: 2020-07-22
---



이번에는 Generic을 이용한 일반화 프로그래밍에 대해 알아보자.

-----

##### * 일반화 프로그래밍?

- 데이터 타입을 활용하는 프로그래밍 기법을 의미한다.
- 특정 데이터 타입을 지정해주면서 박싱/언박싱이 일어나지 않게 되고 오버로딩 없이 메소드를 구현할 수 있다는 장점이 있다.



##### * 일반화 메소드

```c#
한정자 반환형식 메소드이름<형식매개변수> (매개변수목록){  // 일반화 메소드 작성방법
    // 내용
}

public void Print<T> (T value){  // 일반화메소드 선언
    Console.WriteLine(value);
}

// 사용 예제
Print<int>(3);
Print<string>("Bono");
```

- 일반화 메소드는 데이터 형식으로 메소드를 일반화한다.
- 메소드 이름 뒤에 **<형식매개변수>** 가 들어가게 된다.
- 매개변수 목록에서도 형식매개변수가 필요한 경우 메소드 이름 뒤에 작성한 형식매개변수와 동일한 형식매개변수를 넣어준다.
- 호출시 메소드이름 뒤의 형식매개변수 위치에 데이터타입을 대입하고 그에 맞는 매개변수를 주입한다.
- 복잡해 보일 수 있지만 몇 번 쓰다보면 코드를 짤 때 유용하다는 점을 알 수 있다.



##### * 일반화 클래스

```c#
class 클래스이름 <형식 매개변수> {  // 일반화클래스 작성방법
    
}

class Bono <T>{  // 일반화클래스 예제
    private T field;
    
    // ...
    
    public T GetField(){
        return field;
    }
}

// 일반화클래스 사용시
Bono<int> bono = new Bono<int>();
Bono<string> bono2 = new Bono<string>();
int temp = bono.GetField();  // int를 반환
string temp = bono2.GetField();  // string을 반환
```

- 일반화클래스도 일반화메소드와 동일하게 데이터 형식으로 클래스를 일반화한다.
- 클래스 이름 뒤에 **<형식매개변수>**가 들어간다.
- 클래스 내 필드나 메소드에서도 형식매개변수가 필요한 경우 클래스 이름 뒤에 붙은 형식매개변수를 동일하게 작성한다.
- 객체 생성시 형식매개변수에 데이터타입을 대입하여 사용한다.



##### * 형식매개변수의 제약

```c#
class Bono<T> where T : Bono{  // 형식매개변수의 제약 예시
    
}
```

- 위 예제에서는 형식매개변수 T에 오는 데이터 타입을 특정 타입으로 한정하고 있다.
- 데이터타입을 한정짓기 위해서는 **where 절**을 추가하고 그 뒤에 **형식매개변수 : 제약조건**을 입력하면 된다.
- 제약조건의 종류는 아래와 같다
- where T : struct   //  T는 값 형식이어야 한다.
- where T : class  //  T는 참조 형식이어야 한다.
- where T : new()  //  T는 매개변수가 없는 생성자가 있어야 한다.
- where T : 기반클래스이름  //  T는 기반클래스의 파생클래스여야 한다.
- where T : 인터페이스이름  //  T는 인터페이스를 구현해야만 한다.  여러개의 인터페이스를 작성할 수도 있다.
- where T : U  // 또다른 형식매개변수 U로부터 상속받은 클래스여야 한다.



##### * 일반화컬렉션

- 컬렉션은 object 형식 기반으로 박싱/언박싱이 일어나 성능에 좋지 않다.
- 일반화 컬렉션은 형변환이 일어나지 않도록 데이터타입을 지정해준다.  따라서 정해진 데이터타입만 사용 가능하다.
- 대표적으로 LIST< T>, Queue< T >, Stack< T >, Dictionary<TKey, TValue> 가 있다.



##### * LIST< T >

```c#
List<string> Bono = new List<string>();
Bono.Add("Bono");
```

- 형식매개변수를 요구한다는 점만 다를 뿐, 비일반화 클래스인 ArrayList와 동일한 기능을 하며 사용법은 동일하다.



##### * Queue< T >

```c#
Queue<int> Bono = new Queue<int>();
Bono.Enqueue(1);
```

- 형식매개변수를 요구한다는 점만 다를 뿐, 비일반화 클래스인 Queue와 사용법은 동일하다.



##### * Stack< T >

```c#
Stack<int> Bono = new Stack<int>();
Stack.Push(1)''
```

- 형식매개변수를 요구한다는 점만 다를 뿐, 비일반화 클래스인 Stack과 사용법은 동일하다.



##### * Dictionary<TKey, TValue>

```c#
Dictionary<int, string> Bono = new Dictionary<int, string>();
Bono.Add(1, "Bono");
Bono.Add(2, "Porori");
Bono.Add(3, "Hello");

Console.WriteLine(Bono[1]);
Console.WriteLine(Bono[2]);
```

- Dictionary<TKey, TValue>는 Key와 Value 형태로 이루어진 Hashtable의 일반화 버전이다.



##### * foreach를 사용할 수 있는 일반화 클래스

- 지난 시간에 foreach를 사용하기 위해서는 IEnumerable 인터페이스와 IEnumerator 인터페이스를 상속하고 구현해야한다고 설명했었다.
- 일반화클래스에서도 이를 구현하기 위한 IEnumerable< T > 와 IEnumerator < T > 인터페이스가 준비되어 있다.

**IEnumerator< T > 인터페이스**

- IEnumerable< T > 인터페이스는 두개의 메소드를 가지고 있다.  
- IEnumerator< T > GetEnumerator() : IEnumerator< T > 형식의 객체를 반환
- IEnumerator GetEnumerator() : IEnumerator 형식의 객체를 반환  (IEnumerable로부터 상속받은 메소드)
- IEnumerable< T > 인터페이스는 IEnumerable 을 상속받고 있어 두 개의 메소드를 구현해야 한다.

**IEnumerator< T > 인터페이스**

- IEnumerator< T > 인터페이스는 총 네개의 메소드, 프로퍼티를 가지고 있다.
- bollean MoveNext() : 다음 요소로 이동
- void Reset() : 컬렉션의 첫 번째 위치의 앞 으로 이동
- Object Current { get; } : 컬렉션의 현재 요소를 반환  (IEnumerator로부터 상속받은 프로퍼티)
- T Current { get; } : 컬렉션의 현재 요소를 반환
- IEnumerator< T > 인터페이스는 IEnumerator 을 상속받고 있어 Current 버전이 두 개이다.



참고로 형식매개변수를 제외하면 지난 시간에 설명한 IEnumerable/IEnumerator과 다른 점이 거의 없다.













