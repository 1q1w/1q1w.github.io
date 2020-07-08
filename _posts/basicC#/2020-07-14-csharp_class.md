---
layout: single
title: "[C# Basic] 클래스 사용법"
categories:
  - C# Basic
toc: true
toc_sticky: true
toc_label: "On this Page"
date: 2020-07-14
---



C#의 기초부터 차근차근 다시 한 번 짚어보자.  

이번에는 클래스를 작성하고 활용하는 방법에 대해 알아보자

-----

##### * 클래스의 선언

```c#
class Bonobono{
    public string Name;  // 데이터
    public int Age;
    
    public void hello(){  // 메소드
        Console.WriteLine("Hello~");
    }
}
```

- class 키워드를 이용해 선언
- 클래스 내에 선언된 변수들을 **필드** 라고 한다. (위 예제에서는 Name, Age)
- 필드와 프로퍼티, 이벤트 등 클래스 내에 선언된 요소들을 **멤버** 라고 한다.

```c#
Bonobono Bono = new Bonobono();  // BonoBono()는 생성자에 해당
BonoBono Bono2;  // null
```

- 클래스는 **생성자**라고 불리는 특별한 메소드를 통해 초기화 할 수 있다.
- 클래스는 복합 데이터 형식이다.  복합 데이터 형식은 참조 형식이다.  값을 할당하지 않으면 null을 가지게 된다.
- **new 연산자와 생성자**를 이용해 힙에 객체를 생성하고 Bono는 생성자가 힙에 생성한 객체를 가리키가 된다.

```c#
int a = new int();
```

- C#의 모든 데이터형식은 클래스이기 때문에 new 연산자와 생성자를 사용할 수 있다.  근데 귀찮으니 이렇게 쓸 필요 없다.



##### * 생성자와 종료자

```c#
class Bono{
    private string Name;
    private int Age;
    
    public Bono(){  // 생성자를 통한 초기화
        Name = "Bono",
        Age = 15;
    }
    
    public Bono(int age){  // 생성자 오버로딩
        Name = "Bono";
        Age = age;
    }
}
...

Bono bono1 = new Bono();  // 기본생성자 호출
Bono bono2 = new Bono(20);  // 오버로딩한 생성자 호출
```

- 객체는 생성자에의해 생성되고 종료자에 의해 파괴된다.
- 생성자는 클래스와 이름이 같고 반환형식이 없다.
- 생성자는 해당 형식(클래스)의 객체를 생성하는 역할만 한다.
- 클래스 선언시 생성자를 구현하지 않아도 **컴파일러가 기본 생성자를 자동으로 만들어준다**.  
- 생성자를 직접 구현하는 경우는 **객체 생성 시점에 객체를 원하는 값으로 초기화**하고자 할 때이다.
- **생성자도 메소드와 마찬가지로 오버로딩이 가능**하다.  따라서 다양한 버전의 생성자 작성이 가능하다.

```c#
class Bono{
    ~Bono(){  // 종료자
        
    }
}
```

- 종료자는 클래스 이름에 ~ 을 붙여 작성할 수 있다.
- 종료자는 사용하지 말자.  (가비지 컬렉터가 우리보다 더 똑똑하게 이를 지워줄 것이다.  그 외에도 성능문제 등이 발생할 수 있다.)



##### * 정적 필드와 메소드

```c#
class Bono{
    public static string Name;  // 정적 필드
    public static void StaticBono(){  // 정적 메소드
        // 내용
    }
}

class Porori{
    public string Name;  // 인스턴스 필드
}

...

public static void Main(){
    Bono.Name = "Mr.Bono";  // 정적 필드에 바로 접근. 단 하나만 존재
    Bono.StaticBono();  // 정적 메소드에 바로 접근
    
	Porori porori = new Porori();
    porori.Name = "Mr.Porori";  // 인스턴스를 만들어 인스턴스 필드에 접근
	Porori porori2 = new Porori();
    porori2.Name = "Ms.Porori";  // 인스턴스는 여러개일 수 있다
}
```

- 한 프로그램에 인스턴스는 여러개일 수 있으나 클래스는 단 하나만 존재한다.
- 특정 필드가 클래스에 소속된다는 것은 그 필드가 프로그램 전체에 유일하다는것을 의미한다.
- **static으로 한정하지 않은 필드는 자동으로 인스턴스에 소속되며, static으로 한정한 필드는 클래스에 소속**된다.
- **프로그램 전체에 공유해야 하는 변수나 메소드가 있다면 static을 사용**하면 된다.
- 클래스를 static으로 선언하는 경우 모든 클래스 멤버는 static으로 한정해야 한다.  (클래스를 메모리에 올려두고 쓸것이므로 그 멤버들도 메모리에 올라갈 수 있어야 함)



##### * 얕은 복사와 깊은 복사

```c#
// 얕은복사 예제
class BonoBono{
    public int Age;
    public string Name;
}
...
BonoBono bono = new BonoBono();
bono.Age = 10;
bono.Name = "BONO";

BonoBono test = bono;
test.Age = 20;

Console.WriteLine("{0} {1}", bono.Age, test.Age);  // 값은 어떻게 출력될까?
```

- 위 예제는 bono 객체를 만든 뒤 test 객체에 bono 객체를 복사하는 예제이다.  복사한 뒤 bono와 test 객체의 Age 값은 어떻게 나올까?
- 클래스는 참조 형식이기 때문에 힙에 있는 실제 객체가 아닌 스택에 있는 참조 주소를 복사하게 된다.  따라서 bono와 test 객체의 Age는 같은 객체를 바라보고 있으며 값은 모두 20에 해당된다.
- 객체를 복사할 때 참조만 복사하는 것을 **얕은복사** 라고 한다.
- 객체를 복사할 때 힙에 있는 내용을 복사해서 별도의 힙에 저장하는 것을 **깊은복사** 라 한다.  이는 C#에서 자동으로 구현해주지 않으며 필요한 경우 직접 구현해야 한다.

```c#
// 깊은복사 예제
class BonoBono{
    public int Age;
    public string Name;
    
    public Bono DeepCopy(){
        BonoBono temp = new BonoBono();  // 객체에 힙을 할당해서 값을 하나씩 복사해 넣음
        temp.Age = this.Age;
        temp.Name = this.Name;
        return temp;
    }
}
```



##### * this 키워드

``` c#
class BonoBono{
    private string Name;
    
    public BonoBono(string Name){
        this.Name = Name;
    }
}
```

- this는 객체가 자신을 지칭할 때 사용하는 키워드이다.
- 객체 외부에서 객체의 필드나 메소드에 접근할 때 객체의 이름을 사용한다면, 객체 내부에서는 자신의 필드나 메소드에 접근할 때 this 키워드를 사용한다.
- 위 예제에서 BonoBono의 생성자에는 매개변수 Name과 필드 Name 이름이 중복된다.  이런 경우 this 키워드를 이용해 어떤것이 자신의 필드이며 어떤것이 매개변수인지 구분할 수 있다.



##### * this() 생성자

```c#
class BonoBono{
    int a, b, c;
    
    public BonoBono(){
        this.a = 10;
    }
    public BonoBono(int b) : this(){
        this.b = b;
    }
    public BonoBono(int b, int c) : this(b){
        this.c = c;
    }
}
```

- 위 예제는 생성자를 오버로딩하는 클래스의 코드이다.  매개변수가 없는 경우와 1개,  2개인 경우의 버전이 준비되어 있다.
- 매개변수 2개인 생성자에서는 this(b) 키워드를 통해 매개변수가 1개인 생성자를 호출한다.
- 매개변수가 1개인 생성자에서는 this() 키워드를 통해 매개변수가 없는 생성자를 호출한다.
- 이 때 생성자의 실행 순서는 최상위부터 순차적으로 실행된다.



##### * 접근한정자

- 클래스에 선언된 필드와 메소드 중 어떤것은 외부에서 쓸 수 있게 하고, 어떤것은 제한할 필요가 있다.  이 때 접근한정자를 사용한다.
- 특히 필드의 경우 상수를 제외하고는 감추는 것이 좋다.
- **public** : 클래스 내/외부 모든곳에서 접근이 가능하다
- **protected** : 클래스의 외부에서는 접근이 불가하지만 파생 클래스에서는 접근이 가능하다
- **private** : 클래스 내부에서만 접근이 가능하다.  파생 클래스에서의 접근도 불가하다
- **internal** : 같은 어셈블리에 있는 코드에서만 public으로 접근할 수 있다
- **protected internal** : 같은 어셈블리에 있는 코드에서만 protected로 접근할 수 있다
- **private protected** : 같은 어셈블리에 있는 클래스에서 상속받은 클래스 내부에서만 접근이 가능하다
- **접근한정자를 수식하지 않은 클래스 멤버의 접근한정자는 private 으로 지정**된다.



##### * 클래스의 상속

```c#
class Character{
    // 멤버 선언
}

class BonoBono : Character {
    // 기반 클래스인 Character의 멤버를 물려받게 된다
    // private으로 선언된 멤버는 제외된다
}
```

- 클래스는 다른 클래스의 멤버를 상속받아 사용하는것이 가능하다.
- 파생클래스 이름 뒤에 콜론(:) 을 붙이고 그 뒤에 기반 클래스의 이름을 명시하면 된다.
- 객체 생성시 내부적으로 기반 클래스의 생성자 > 파생 클래스의 생성자 순서로 호출한다.
- 객체 소멸시 반대로 파생 클래스의 종료자 > 기반 클래스의 종료자 순서로 호출한다.



##### * base() 키워드

```c#
class Character {
    public Character(string talking){  // 기반클래스 생성자
        Console.WriteLine(talking);
    }
    public void CharacterMethod(){
        
    }
}

class BonoBono : Character {
    public BonoBono(string hello, string bye) : base(hello){  // 파생클래스 생성자에서 base 키워드를 통해 기반클래스 생성자에 접근
        // 생성자
    }
    
    public void BonoBonoMethod(){
        base.CharacterMethod();  // base 키워드를 통해 기반 클래스에 접근 가능
    }
}
```

- this() 가 자신의 생성자를 가리키는 것처럼, **base()는 자신의 기반 클래스를 가리킨다**.
- 생성자에서도 기반클래스의 어떤 생성자를 사용할지 base() 키워드를 통해 지정이 가능하다.



##### * sealed 한정자

```c#
sealed class Character{
    
}
class Bono : Character{  // 컴파일시 에러발생
    
}
```

- 기반클래스에서는 의도하지 않은 상속이나 파생클래스의 구현을 막기 위해 상속이 불가능하도록 sealed 한정자를 사용할 수 있다.



##### * 기반클래스와 파생클래스 사이의 형식변환

```c#
class Animal{
    public void Talk(){
        Console.WriteLine("Animal!");
    }
}
class BonoBono : Animal{
    public void TalkBono(){
        Console.WriteLine("Bono!");
    }
}
class Porori : Animal{
    public void TalkPorori(){
        Console.WriteLine("Porori!");
    }
}
```

- 위와 같이 Animal 기반 클래스를 상속받는 BonoBono와 Porori 파생클래스가 있다고 가정해보자.

```c#
Animal animal = new Animal();
animal.Talk();

BonoBono bono = new BonoBono();
bono.Talk();
bono.TalkBono();

Porori porori = new Porori();
porori.Talk();
porori.TalkPorori();
```

- 보노보노 클래스와 포로리 클래스는 Animal을 상속받아 자신의 메소드 외에도 기반클래스의 메소드에도 접근이 가능하다.

```c#
Animal animal = new Animal();
animal.Talk();

animal = new BonoBono();
animal.Talk();

BonoBono bono = (BonoBono)animal;
bono.Talk();
bono.TalkBono();

animal = new Porori();
animal.Talk();

Porori porori = (Porori)animal;
porori.Talk();
porori.TalkPorori();
```

- **기반 클래스와 파생 클래스 사이에서 형식 변환이 가능하며, 파생 클래스의 인스턴스는 기반 클래스의 인스턴스로서도 사용할 수 있다.**



##### * 형변환을 위한 is, as 연산자

```c#
Animal animal = new BonoBono();
BonoBono bono;

if(animal is BonoBono){  // 형변환이 가능한지 bool값 반환
    bono = (BonoBono)animal;  // 형변환
    bono.TalkBono();
}

Animal animal2 = new Porori();
Porori porori = animal2 as Porori;  // animal2 객체를 Porori로 변환이 가능하다면 변환한 값을 반환하고, 실패하면 null을 반환

if(porori != null){
    porori.TalkPorori();
}
```

- **is 연산자는 객체가 해당 형식에 해당하는지 검사하여 결과를 bool 값으로 반환**
- **as 연산자는 직접 형식 변환을 실행.  형식 변환에 실패하는 경우 객체 참조를 null로** 만들어 오류가 발생하지 않음
- 일반적으로 as 연산자 사용을 권장.  형변환에 실패하더라도 예외가 일어나거나 코드가 갑자기 점프하는 일이 없어 코드 관리가 더 수월



##### * 오버라이딩

```c#
class Animal{
    public virtual void Talk(){
        Console.WriteLine("Animal Talk");
    }
}

class BonoBono : Animal{
    public overrid void Talk(){
        base.Talk();
        Console.WriteLine("BonoBono Talk");
    }
}
```

- 오버라이딩은 **virtual 키워드**를 사용한 기반 클래스의 메소드만 가능하다.
- 파생 클래스에서 **override 키워드**를 사용해서 메소드를 재정의 할 수 있다.
- virtual 키워드를 사용한 경우 해당 메소드 안에 내용이 존재할 수 있다.  파생 클래스에서는 base 키워드를 사용해 기반 클래스의 메소드를 호출할 수 있다.
- private로 선언한 메소드는 오버라이딩 할 수 없다.  파생클래스에서도 접근을 제한한다.



##### * 메소드 숨기기

```c#
class Animal{
    public void Talk(){
        Console.WriteLine("Animal Talk");
    }
}

class BonoBono : Animal{
    public new void Talk(){
        Console.WriteLine("BonoBono Talk");
    }
}
```

- 메소드 숨기기란 기반 클래스에서 구현된 버전의 메소드를 감추고 파생 클래스에서 구현된 버전만을 보여주는것을 말한다.
- 파생 클래스의 메소드에서 **new 한정자**로 수식하면 기반 클래스의 메소드는 숨겨지고 파생 클래스의 메소드만 노출된다.

```c#
BonoBono bono = new BonoBono();
bono.Talk();  // BonoBono Talk 출력

Animal animal = new BonoBono();
animal.Talk();  // Aniaml Talk 출력
```

- new 키워드는 단지 파생클래스로 접근할 때 기반 클래스의 메소드를 숨길 뿐이다.  따라서 위와 같이 객체를 생성하는 경우 기반 클래스의 메소드도 사용이 가능하다.



##### * 오버라이딩 봉인하기

```c#
class Animal{
    public virtual void Talk(){
        Console.WriteLine("Animal Talk");
    }
}

class BonoBono : Animal{
    public sealed overrid void Talk(){
        base.Talk();
        Console.WriteLine("BonoBono Talk");
    }
}
```

- 클래스를 상속 불가능하도록 sealed 한정자를 사용해 봉인하는 것처럼, 오버라이딩도 불가능하도록 봉인할 수 있다.
- 단, **virtual로 선언된 가상 메소드를 오버라이딩한 버전의 메소드만 sealed 키워드를 이용해 더이상 오버라이딩이 불가하도록** 만들 수 있다.
- virtual 한정자가 붙은 버전은 하위 클래스에서 재정의해서 사용이 가능하다는 의미이므로 봉인이 불가능하다.  오버라이딩을 원치 않는 경우 virtual 한정자를 붙이지 않으면 된다.
- 오버라이딩한 메소드는 파생클래스의 파생 클래스에서도 오버라이딩이 가능하다.  따라서 파생클래스에서 오버라이딩 후 더이상의 오버라이딩을 원치 않는 경우 sealed 한정자를 붙이면 된다.



##### * 중첩 클래스

```c#
class Animal{
    class BonoBono{
        
    }
}
```

- 클래스 안에 클래스를 선언하는 것을 중첩 클래스라 한다.
- 중첩 클래스와 일반 클래스가 다른 점은, 중첩 클래스는 자신이 소속된 클래스의 멤버에 자유롭게 접근이 가능하다는 점이다.  private 멤버에도 접근이 가능하다.
- 중첩클래스를 사용하는 이유는 클래스 외부에 공개하고 싶지 않은 형식을 만들거나, 현재 클래스의 일부분처럼 표현할 수 있는 클래스를 만들고자 할 때 사용한다.
- 다른 클래스의 private 멤버에도 접근이 가능해 유연하면서도 은닉성을 무너뜨리는 방법이다.  사용할지 말지 잘 고민해보고 쓰도록 하자.



##### * 분할 클래스

```c#
partial class BonoBono{
    public string Name {get; set;}
}
partial class BonoBono{
    public int Age {get; set;}
}

...
BonoBono bono = new BonoBono();
bono.Name = "BonoBono";
bono.Age = 15;
```

- 분할 클래스는 partial 키워드를 이용해 작성한다.
- 여러번에 걸쳐 정의되더라도 C# 컴파일러는 이를 하나의 클래스로 묶어 컴파일한다.  따라서 그냥 하나의 클래스로 여기고 사용하면 된다.



##### * 확장메소드

```c#
namespace 네임스페이스이름{
    public static class 클래스이름{
        public static 반환형식 메소드이름(this 확장하고자하는클래스또는형식 식별자, 매개변수){
            
        }
    }
}
```

- 확장메소드란 기존 클래스의 기능을 확장하는 기법을 의미한다.  **기존 클래스의 기능을 확장**한다는 표현을 기억하자.
- 확장메소드 사용 방법은 아래와 같다.
- 1. **static 한정자로 수식된 메소드를 선언**한다.
- 2. **첫 번째 매개 변수는 this 키워드와 함께 확장하고자 하는 클래스의 인스턴스**여야 한다.

- 3. 이 때 메소드를 포함하는 **클래스도 static 한정자로 수식**되어야 한다.

```c#
// 확장메소드 선언
namespace BonoExtension{
    public static class StringExtension{
        public static void GetHello(this string name){
            Console.WriteLine(Name + " Hello~!");
        }
    }
}
...
// 확장메소드 사용시    
using BonoExtension;  // 확장메소드를 담는 클래스의 네임스페이스 사용
...
string bono = "Bono";
bono.GetHello();  // Bono Hello~! 출력.  string 클래스가 가지고 있던 기능처럼 사용이 가능하다
```

- 위와 같이 확장메소드를 선언하고 이를 사용하면 애초에 해당 클래스가 가지고 있던 기능처럼 사용이 가능하다.
- 프로그램 전반적으로 자주 사용되는 클래스의 기능이 있다면 확장메소드를 통해 좀 더 편하게 사용이 가능할 것이다.

```c#
// int 클래스에서 사용할 수 있는 vToString 구현 예제
namespace IntExtension{
    public static class ExtensionClass{
        public static string vToString(this int num){
            return "나이는 " + num.ToString();
        }
    }
}

...
string myAge = 15.vToString();  // myAge 는 "나이는 15"
```



##### * 구조체

```c#
struct BonoBono{
    public int Age;
    public string Name;
    
    public SetAge(int age){
        this.Age = age;
    }
}
```

- 클래스와 유사한 구조체는 데이터를 담기 위한 자료구조로 사용.  따라서 편의를 위해 public으로 선언해 사용하는 경우가 많음
- 클래스는 참조 형식이지만 구조체는 값 형식
- 클래스는 new 연산자와 생성자를 통해 인스턴스를 생성하지만 구조체는 선언만으로 생성이 가능
- 구조체는 매개변수가 없는 생성자는 선언 불가능.  대신 CLR이 구조체의 필드를 기본값으로 초기화해주는 역할을 함

```c#
BonoBono bono;
bono.Name = "Bono";

BonoBono bono2;  
bono2 = bono;  // 값 형식이기 때문에 그대로 복사됨
bono2.Name = "Bono2";  // bono.Name은 "Bono"이며 bono2.Name은 "Bono2"
```



**클래스와 구조체의 차이**

| 특징    | 클래스  | 구조체  |
| ------- | ------- | ------- |
| 키워드 | class | struct |
| 형식 | 참조 형식 | 값 형식 |
| 복사 | 얕은 복사 | 깊은 복사 |
| 인스턴스 생성 | new 연산자와 생성자 필요 | 선언만으로 생성 |
| 생성자 | 매개변수 없는 생성자 선언 가능 | 매개변수 없는 생성자 선언 불가능 |
| 상속 | 가능 | 모든 구조체는 System.Object 형식을 상속하는 System.ValueType으로부터 직접 상속받음 |



##### * 튜플

```c#
var tuple = (123, 456);  // 명명되지 않은 튜플. 컴파일러는 첫 요소를 Item1이라는 필드에, 두번째 요소를 Item2라는 필드에 담는다
var tuple2 = (Name:"Bono", Age:15); // 명명된 튜플

var (name, age) = tuple2;  // 튜플 분해.  name에 Name 필드를, age에 Age 필드를 담음
```

- 튜플은 여러 필드를 담을 수 있는 구조체.  구조체이므로 값 형식에 해당.
- 값 형식이므로 생성된 지역을 벗어나면 스택에서 소멸되어 프로그램의 부담을 줄여준다는 장점
- 임시적으로 사용할 복합 데이터 형식을 선언할 때 적합

- 이런 개념이 있다 정도로만 알아두자.




