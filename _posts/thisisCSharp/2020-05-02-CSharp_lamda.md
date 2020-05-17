---
layout: single
title: "[C#] 람다식과 Func, Action 그리고 식 트리"
categories:
  - C#
toc: true
toc_sticky: true
toc_label: "On this Page"
date: 2020-05-02
---



이번 시간에는 C#의 람다식과 Func, Action 대리자, 식 트리에 대해 알아보자.



-----------

#### 1. 람다식이란

람다식이란 익명 메소드를 만들기 위해 사용합니다.  람다식으로 만드는 익명 메소드는 무명 함수라는 이름으로 부릅니다.  메소드는 매개변수와 반환값을 가지고 있는데 람다식도 마찬가지입니다.  선언하는 방법은 다음과 같습니다.

```c#
매개변수목록 => 식
```

문법이 꽤 특이합니다.  =>를 중심으로 왼편에는 매개변수가, 오른쪽에는 식이 위치합니다.  람다식의 사용 예를 살펴봅시다.

```c#
delegate int Calc(int a, int b);

//...
    
static void Main(string[] args){
    // 두 개의 int를 입력받아 하나의 int를 반환하는 익명메소드
    Calc calc = (int a, int b) => a + b;
}
```

C#에서는 위 코드를 더 간결하게 만들 수 있도록 형식 유추 기능을 제공합니다.  대리자의 형식을 가지고 익명메소드의 형식을 제거할 수 있습니다.

```c#
delegate int Calc(int a, int b);

//...
    
static void Main(string[] args){
    // 대리자를 통해 a와 b의 형식이 int임을 유추
    Calc calc = (a, b) => a + b;
}
```

대리자를 이용해 익명메소드를 만들었떤것과 비교하면 굉장히 간결합니다.

```c#
delegate int Calc(int a, int b);

//...
    
static void Main(string[] args){
    Calc calc = delegate(int a, int b){
        return a + b;
    }
}
```

그렇다면 왜 익명메소드를 만드는 방법으로 람다식을 제공하면 되는데 대리자도 제공을 하고 있을까요?  대리자를 이용한 익명메소드는 C# 2.0 버전에서 제공이 되었고, 람다식을 이용한 방법은 C# 3.0 에서 도입되었기 때문입니다.  대리자를 사용한 익명메소드를 제공하지 않게 되면 기존에 사용하던 개발자들은 이를 모두 수정해야겠죠?



#### 2. 문 형식의 람다식

문 형식의 람다식은 => 연산자 오른편에 식 대신 { }로 감싸진 코드블록이 위치합니다.

```c#
delegate void DoSomething();

// ...

static void Main(string[] args){
    DoSomething Test = ( ) => {
        Console.WriteLine("테스트1");
        Console.WriteLine("테스트2");
        Console.WriteLine("테스트3");
    }
    
    Test();
}
```

문 형식의 람다식 예제를 한 번 만들어봅시다.

```c#
using System;

namespace BonoLamda{
    class MainApp{
        delegate string Concat(string[] args);
        
        static void Main(string[] args){
            Concat concat = (arr) => {
                string result = string.Empty;
                foreach(string temp in arr){
                    result += temp;
                }
                return result;
            }
            
            Console.WriteLine(concat(args));
        }
    }
}
```



#### 3. Func와 Action

익명메소드나 무명함수를 사용하려면 대리자가 항상 있어야 합니다.  한 번 쓰고 말건데 이를 위해 대리자를 매번 선언해야한다니 정말 귀찮네요.  이러한 문제를 해결하기 위해 마이크로소프트에서는 .NET 프레임워크에 Func와 Action 대리자를 미리 선언해뒀습니다.   이 두 대리자가 어떤 편리함을 주는지 살펴보죠.



##### 1) Func 대리자

Func 대리자는 결과를 반환하는 메소드를 참조하기 위해 만들어졌습니다.  총 17개가 선언되어 있으며 아래와 같이 선언되어 있습니다.

```c#
public delegate TResult Func<out TResult>()
public delegate TResult Func<in T, out TResult>(T arg)
public delegate TResult Func<in T1, in T2, out TResult>(T1 arg1, T2 arg2)
    ...
public delegate TResult Func<in T1, in T2, ... in T16, out TResult>(T1 arg1, T2 arg2, ... T16 arg16)
```

Func 대리자의 마지막 형식 매개 변수는 반환 형식에 해당합니다.  형식 매개 변수가 1개인 Func 대리자는 1개가 반환형식이고, 2개인 대리자는 1개는 입력매개변수, 1개는 반환형식입니다.  Func는 입력매개변수가 하나도 없는것부터 16개인 것까지 선언이 되어있어 이를 벗어나는 경우가 아니면 별도의 대리자를 만들 필요가 없습니다.  Func 대리자 사용 예제를 볼까요?

```c#
Func<int> test = () => 10;  // 입력 매개변수는 없고 10 반환
Console.WriteLine(test()); // 10 출력

Func<int, int> test1 = (a) => a * 10;  // a를 입력받아 a*10 출력
Console.WriteLine(test1(3));  // 30 출력

Func<double, double, double> test2 = (a, b) => a / b;
Console.WriteLine(test2(22/7));  // 3.142857.. 출력
```

별 것 없습니다.  그저 미리 선언된 대리자일 뿐입니다.



##### 2. Action 대리자

Action 대리자는 Func대리자와 크게 다르지 않습니다.  Func 대리자는 반환 형식이 1개 있었지만 Action 대리자는 반환 형식이 없다는 것만 다릅니다.  Action 대리자도 Func 대리자와 같이 17개가 선언되어 있습니다.

```c#
public delegate TResult Action<>()
public delegate TResult Action<in T>(T arg)
public delegate TResult Action<in T1, in T2>(T1 arg1, T2 arg2)
    ...
public delegate TResult Action<in T1, in T2, ... in T16>(T1 arg1, T2 arg2, ... T16 arg16)
```

입력 매개변수가 없는 것부터, 16개인 것까지 미리 선언이 되어있습니다.   사용 예를 봅시다.

```c#
Action act1 = () => Console.WriteLine("act1");
act1();  // act1 출력

int result = 0;
Action<int> act2 = (x) => result = x * x;  // Action 대리자는 반환을 하지 않으므로 따로 변수에 저장

act2(3);
Console.WriteLine(result);  // 9 출력

Action<double, double> act3 = (a, b) => {
    double pi = a / b;
    Console.WriteLine(pi);
}

act3(22.0, 7.0);  // 3.14... 출력
```



#### 4. 식트리

자료구조나 알고리즘을 공부해본 경험이 없다면 식트리 개념이 생소할겁니다.  식트리는 노드로 구성되며 각 노드는 부모-자식 관계로 연결됩니다.  식 트리는 컴파일러나 인터프리터를 제작하는데 응용됩니다.  따로 공부를 해볼법한 주제입니다.  이번에는 식트리를 알고 있다는 전제 하에 간략하게 소개만 하겠습니다.

식 트리를 다루는데 필요한 클래스는 .NET 프레임워크의 System.Linq.Expressions 네임스페이스 안에 준비되어 있습니다.

Expression 클래스는 식 트리를 구성하는 노드를 표현합니다.  따라서 Expression 을 상속받는 클래스는 식 트리의 각 노드를 표현할 수 있습니다.  Expression 클래스는 노드를 표현하는 것 외에도 파생 클래스들의 객체를 생성하는 역할도 담당하고 있습니다.  Expression 클래스는 abstract로 선언되어 자신의 인스턴스 생성은 불가하지만 파생 클래스의 인스턴스를 생성하는 정적 팩토리 메소드를 제공하고 있습니다.

[[MS Docs]Expression 클래스의 파생클래스](https://docs.microsoft.com/ko-kr/dotnet/api/system.linq.expressions.expression?view=netcore-3.1).

예를 들어 상수를 표현하는 ConstantExpression 객체 하나와 매개 변수를 표현하는 ParameterExpression 객체 하나를 선언하고, 이 둘에 대해 덧셈 연산을 수행하는 BinaryExpression 객체를 선언해 봅시다.  이들 객체는 Expression 클래스의 팩소리 메소드를 통해 생성할 것입니다.

```c#
Expression const1 = Expression.Constant(1);  // 상수 1
Expression param1 = Expression.Parameter(typeof(int), "x");  // 매개변수 x

Expression exp = Expression.Add(const1, param1); // 1 + x;
```

Expression.Constant() 메소드로 ConstantExpression 형식의 const1 객체를 선언했습니다.   뭔가 이상합니다.  ConstantExpression 이 아닌 Expression.Constant() 가 사용되었습니다.

ConstantExpression은 Expression 을 상속하고 있습니다.

```c#
public class ConstantExpression : System.Linq.Expressions.Expression
```

Expression 클래스에는 아래와 같이 선언되어 있습니다.  따라서  ConstantExpression 객체는 Expression 형식의 참조를 통해 가리킬 수 있습니다.   Expression 클래스의 정적 팩토리 메소드들은 Expression 클래스의 파생 클래스의 인스턴스를 생성하는 기능을 제공함으로써 개발자의 수고를 줄여줍니다.

```c#
public static System.Linq.Expressions.ConstantExpression Constant (object value);
```

덕분에 개발자는 각 노드가 어떤 타입인지 신경쓰지 않고 Expression 형식의 참조를 선언해서 사용할 수 있습니다.  필요한 경우 각 세부 형식으로 형변환을 하면 됩니다.

위 예제에서 exp는 실행 가능한 상태는 아니며 데이터로만 머물러 있습니다.  exp가 자신의 트리 자료 구조 안에 정의된 식을 실행하려면 람다식으로 컴파일 되어야 합니다.  람다식으로의 컴파일은 Expression<TDelegate> 클래스를 이용합니다.  Expression<TDelegate> 는 LambdaExpression 클래스의 파생클래스입니다.

```c#
Expression const1 = Expression.Constant(1);  // 노드 상수 1
Expression param1 = Expression.Parameter(typeof(int), "x");  // 노드 매개변수 x

Expression exp = Expression.Add(const1, param1); // 식트리 1 + x;

Expression<Func<int, int>> lambda1 = Expression<Func<int, int>>.Lambda<Func<int, int>>(
	exp,  // 식트리
    new ParameterExpression[]{(ParameterExpression)param1}  // 파라미터
);

Func<int, int> compiledExp = lambda1.Compile();  // 실행 가능한 코드로 컴파일

Console.WriteLine(compiledExp(3));  // 4 출력
```

이해를 돕기 위해 예제를 하나 더 만들어봅시다.

```c#
using System;
using System.Linq.Expressions;

namespace BonoExpressionTree{
    class MainApp{
        static void Main(string[] args){
            // 1*2+(x-y)
            Expression const1 = Expression.Constant(1);
            Expression const2 = Expression.Constant(2);
            Expression leftExp = Expression.Multiply(const1, const2);  // 1*2
            
            Expression param1 = Expression.Parameter(typeof(int)); // x 변수
            Expression param2 = Expression.Parameter(typeof(int)); // y 변수
            Expression rightExp = Expression.Subtract(param1, param2);  // x-y
            
            Expression exp = Expression.Add(leftExp, rightExp);
            
            Expression<Func<int, int, int>> expression =
                Expression<Func<int, int, int>>.Lambda<Func<int, int, int>>(
            		exp, // 식트리
                	new ParameterExpression[]{  //파라미터
                    	(ParameterExpression)param1,
                    	(ParameterExpression)param2
                    }
            	);
            
            Func<int, int, int> func = expression.Compile();
            
            Console.WriteLine(func(6, 4));  // 1*2+(6-4) =  4 출력
        }
    }
}
```

람다식을 이용하면 더 간편하게 식트리를 만들 수 있습니다.  다만 이 경우는 동적으로 식 트리를 만들기는 어려워집니다.  Expression 형식은 불변이기 때문에 인스턴스가 만들어지고 난 후에는 변경할 수 없습니다.   람다식을 이용해서 조금 전 예제를 다시 만들어 보겠습니다.

```c#
using System;
using System.Linq.Expressions;

namespace BonoExpressionTree{
    class MainApp{
        static void Main(string[] args){
            
            Expression<Func<int, int, int>> expression =
                (a, b) => 1 * 2 + (x - y);
            Func<int, int, int> func = expression.Compile();
            
            Console.WriteLine(func(6, 4));  // 1*2+(6-4) =  4 출력
        }
    }
}
```

살펴본 것처럼 식 트리는 코드를 '데이터'로 보관할 수 있습니다.  이것은 파일에 저장할 수도 있고 네트워크를 통해 다른 프로세스에 전달할 수도 있습니다.  심지어 코드를 담고 있는 식 트리 데이터를 데이터베이스 서버에 보내서 실행시킬 수도 있습니다.  데이터베이스 처리를 위한 식 트리는 LINQ에서 사용됩니다. 



#### 5. 식으로 이루어지는 멤버

메소드를 비롯해 인덱서, 생성자, 종료자는 공통된 특징이 있습니다.  모두 클래스의 멤버로서 본문이 중괄호로 이루어져 있다는 점입니다.  이러한 멤버의 본문을 식 만으로 구현하는 것이 가능합니다.  구현 방법은 다음과 같습니다.

```c#
멤버 => 식;
```

예제를 한 번 볼까요?

```c#
class BonoList{
    private List<string> list = new List<string>();
    
    public void Add(string name) => list.Add(name);
    public void Remove(string name) => list.Remove(name);
    
    public BonoList() => Console.WriteLine("생성자");
    ~BonoList() => Console.WriteLine("종료자");
    
    public int Capacity => list.Capacity; // 읽기 전용 속성
    public string this[int index] => list[index]  // 읽기 전용 인덱서
}
```

읽기와 쓰기 모두 가능한 속성 또는 인덱서를 구현하려면 코드가 늘어납니다.  읽기 전용일때는 생략했던 get, set 키워드도 명시적으로 기술해주어야 합니다.

```c#
class BonoList{
    //...
    public int Capacity{
        get => list.Capacity;
        set => list.Capacity = value;
    }
    public string this[int index]{
        get => list[index];
        set => list[index] = value;
    }
}
```





---------

###### 참고도서 : 박상현, 이것이 C#이다, 한빛미디어