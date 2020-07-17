---
layout: single
title: "[C#] 대리자와 이벤트"
categories:
  - C#
toc: true
toc_sticky: true
toc_label: "On this Page"
date: 2020-05-18
---



이번 시간에는 C#의 대리자와 이벤트에 대해 알아보자.



-----------

#### 1. 대리자란

C#에서 대리자는 콜백을 구현하기 위해 사용됩니다.  대리자는 메소드에 대한 참조입니다.  (객체의 주소를 가리키는 참조에 대해 헷갈린다면 데이터타입에 대해 먼저 공부해보는것도 좋습니다.)  대리자에 메소드의 주소를 할당한 뒤 대리자를 호출하면 대리자가 메소드를 호출해줍니다.  대리자는 다음과 같이 delegate 키워드를 사용해서 선언합니다.

```c#
한정자 delegate 반환형식 대리자이름 (매개변수목록);
```

위 문법에서 delegate만 빼면 평소에 사용하던 메소드와 동일합니다.  대리자는 메소드에 대한 참조이기 때문에 자신이 참조할 메소드의 반환형식과 매개변수를 명시해주어야 합니다.  대리자의 예시를 한번 볼까요?

```c#
delegate int Calc(int a, int b);
```

여기 한 가지 주의할 점이 있습니다.  **대리자는 인스턴스가 아닌 형식**입니다.  대리자는 int, string과 같은 형식이며 이를 참조하는 메소드를 만들기 위해 인스턴스를 따로 만들어야 합니다.

```c#
int Plus(int a, int b){
    return a + b;
}
int Minus(int a, int b){
    return a - b;
}
```

위 예시의 Plus와 Minus 메소드는 Calc 대리자의 반환형식과 매개변수를 따르고 있습니다.  이 메소드를 Calc가 참조하도록 해보겠습니다.

```c#
Calc Callback;

Callback = new Calc(Plus);  //대리자의 인스턴스를 만들 때에도 new 연산자가 필요합니다
Console.WriteLine(Callback(4, 3));  // 7 출력

Callback = new Calc(Minus);
Console.WriteLine(Callback(4, 3));  // 1 출력
```

Callback은 반환형식이 int, 매개변수가 (int, int)인 Calc 대리자의 인스턴스입니다.  이 Callback 객체를 할당하기 위해 Calc() 생성자가 호출되었고, 이 생성자의 매개변수로 Plus, Minus 메소드가 사용되었습니다.  **대리자에 메소드를 매개변수로 넘기면 Callback은 각 메소드를 참조**하게 됩니다.

대리자의 생성 과정을 정리해보면,

1. 대리자를 선언한다
2. 대리자의 인스턴스를 생성한다.  이 때 대리자가 참조할 메소드를 매개변수로 넘긴다
3. 대리자를 호출한다

위 내용을 종합해서 대리자의 예제 프로그램을 한 번 만들어봅시다.

```c#
using System;

namespace Delegate{
    delegate int MyDelegate(int a, int b);
    
    class Calculator{
        public static int Plus(int a, int b){  // 대리자는 정적 메소드도 참조 가능
            return a + b;
        }
        public int Minus(int a, int b){
            return a - b;
        }
    }
    
    class MainApp{
        Calculator Calc = new Calculator();
        MyDelegate Callback;
        
        Callback = new MyDelegate(Calculator.Plus);
        Console.WriteLine(Callback(4, 3)); // 7 출력
        
        Callback = new MyDelegate(Calc.Minus);
        Console.WriteLine(Callback(4, 3)); // 1 출력
    }
}
```



#### 2. 대리자를 왜 사용할까?

프로그래밍을 하다보면 '값'이 아닌 '코드'를 매개변수로 넘기고 싶을 때가 있습니다.  예를 들어 배열을 정렬하는 메소드를 만드는 경우, 오름차순으로 할지 내림차순으로 처리할지가 고민됩니다.  고민은 이 메소드를 사용하게 될 프로그래머가 하게 만들면 우리는 기능을 만들어놓기만 하면 될 것입니다.  이럴 때 대리자가 사용됩니다.  우리는 대리자를 만들어놓고 이에 매개변수로 넣을 수 있는 정렬 기능을 작성해두면 됩니다.

step 1. 대리자를 선언합니다.

```c#
delegate int Compare(int a, int b);
```

step 2. Compare 대리자가 참조할 비교 메소드를 작성합니다.

```c#
static int AscendComp(int a, int b){
    if(a > b){
        return 1;
    }
    else if (a == b){
        return 0;
    }
    else{
        return -1;
    }
}
```

step 3. 정렬할 배열과 비교 메소드를 참조할 대리자를 매개변수로 받는 정렬 메소드를 작성합니다.

```c#
static void BubbleSort(int[] arr, Compare Comparer){
    int i = 0;
    int j = 0;
    int temp = 0;
    
    for(i = 0; i < arr.Length - 1; i++){
        for(j = 0; j < arr.Length - (i + 1); j++){
            if(Comparer(arr[j], arr[j + 1]) > 0){  
                // Compare가 어떤 메소드를 참조하냐에 따라 정렬 결과가 달라짐
                temp = arr[j + 1];
                arr[j + 1] = arr[j];
                arr[j] = temp;
            }
        }
    }
}
```

step 4. 정렬하고싶은 방법에 맞는 메소드를 BubbleSort 호출시 매개변수로 넘깁니다.

```c#
int[] array = [3, 4, 2, 5, 1, 6];
BubbleSort(array, new Compare(AscendComp));
```



위에서 예로 들었던 대리자를 이용한 정렬 프로그램을 한 번 만들어 보도록 합시다.

```c#
using System;

namespace bonobono{
    delegate int Compare(int a, int b);
    
    class MainApp{
        static int AscendComp(int a, int b){
            if(a > b){
                return 1;
            }
            else if (a == b){
                return 0;
            }
            else{
                return -1;
            }
        }
        static int DescendComp(int a, int b){
            if(a < b){
                return 1;
            }
            else if (a == b){
                return 0;
            }
            else{
                return -1;
            }
        }
        static void BubbleSort(int[] arr, Compare Comparer){
            int i = 0;
            int j = 0;
            int temp = 0;

            for(i = 0; i < arr.Length - 1; i++){
                for(j = 0; j < arr.Length - (i + 1); j++){
                    if(Comparer(arr[j], arr[j + 1]) > 0){  
                        // Compare가 어떤 메소드를 참조하냐에 따라 정렬 결과가 달라짐
                        temp = arr[j + 1];
                        arr[j + 1] = arr[j];
                        arr[j] = temp;
                    }
                }
            }
		}
        
        static void Main(string[] args){
            int[] array = {4, 6, 8, 2, 3, 5};
            
            BubbleSort(array, new Compare(AscendComp));
            for (int i = 0; i < array.Length; i++){
                Console.Write($"{array[i]} ");  
            }
            // 2 3 4 5 6 8 출력
            
            BubbleSort(array, new Compare(DescendComp));
            for (int i = 0; i < array.Length; i++){
                Console.Write($"{array[i]} ");  
            }
            // 8 6 5 4 3 2 출력
        }
    }
}
```



#### 3. 일반화 대리자

대리자는 보통 메소드뿐 아니라 일반화 메소드도 참조할 수 있습니다.  물론 이 경우 대리자도 일반화 메소드를 참조할 수 있도록 형식 매개변수를 이용하여 선언되어야 합니다.

```c#
delegate int Compare<T>(T a, T b);
```

delegate 선언만 빼면 제네릭 메소드 선언과 동일합니다.  제네릭을 사용한 경우 대리자를 매개변수를 사용하는 메소드도 변경해야 합니다.

```c#
static void BubbleSort<T>(T[] arr, Compare<T> Comparer){
    int i = 0;
    int j = 0;
    int temp = 0;
    
    for(i = 0; i < arr.Length - 1; i++){
        for(j = 0; j < arr.Length - (i + 1); j++){
            if(Comparer(arr[j], arr[j + 1]) > 0){  
                // Compare가 어떤 메소드를 참조하냐에 따라 정렬 결과가 달라짐
                temp = arr[j + 1];
                arr[j + 1] = arr[j];
                arr[j] = temp;
            }
        }
    }
}
```

대리자에서 참조할 비교 메소드도 작성해봅시다.

```c#
statinc int AscendCompare<T>(T a, T b) where T : IComparable<T>{
    return a.CompareTo(b);
}
```

위 코드에서는 T의 제약조건으로 IComparable<T>의 상속을 추가했습니다.  이렇게 처리한 이유는 System.Int32, System.Double, System.String 등은 모두 IComparable을 상속해서 CompareTo() 메소드를 구현하고 있습니다.  CompareTo() 메소드는 매개변수가 자신보다 크면 -1, 같으면 0, 작으면 1 을 반환합니다.  따라서 a.CompareTo(b) 를 호출하면 오름차순 정렬에 필요한 비교 결과를 얻을 수 있게 됩니다.



#### 4. 대리자 체인

대리자에는 특이한 속성이 하나 있습니다.  바로 **하나의 대리자는 여러 개의 메소드를 동시에 참조할 수 있다**는 것입니다.

```c#
delegate void ThereIsAFire(string location);

void Call119(string location){
    Console.WriteLine("불났어요~! 주소는 {0}", location);
}
void ShutOut(string location){
    Console.WriteLine("피하세요~! {0}에 불났어요~!", location);
}
void Escape(string location){
    Console.WriteLine("{0}에서 탈출합시다~!", location);
}

// 대리자 결합
ThereIsAFire Fire = new ThereIsAFire(Call119);
Fire += new ThereIsAFire(ShutOut);
Fire += new ThereIsAFire(Escape);
```

위와 같이 대리자를 결합한 상태에서 대리자를 호출하면, 자신이 참조하고 있는 메소드들을 모두 호출합니다.

```c#
Fire("우리집");
// 불났어요~! 주소는 우리집 피하세요~! 우리집에 불났어요~! 우리집에서 탈출합시다~! 출력
```

대리자 체인은 여러 콜백을 한번에 호출할 떄 유용합니다.  대리자를 결합하는 방법은 아래와 같습니다.

```c#
// += 연산자 사용
ThereIsAFire Fire = new ThereIsAFire(Call119);
Fire += new ThereIsAFire(ShutOut);
Fire += new ThereIsAFire(Escape);

// Delegate.Combine() 메소드 사용
ThereIsAFire Fire = (ThereIsAFire) Delegate.Combine(
	new ThereIsAFire(Call119),
	new ThereIsAFire(ShutOut),
	new ThereIsAFire(Escape),
)
```

대리자 체인에서 특정 대리자를 끊어야 할 때도 있습니다.  이 경우는 -= 연산자를 사용하거나 Delegate.Remove() 메소드를 사용하면 됩니다.



#### 5. 익명 메소드

메소드는 보통 다음과 같이 선언됩니다.  한정자가 없어도, 반환값이 없어도, 매개변수가 없어도 이름은 꼭 있어야 합니다.

```c#
void Method(){
    
}
```

익명 메소드는 이름이 없는 메소드를 말합니다.  아래 예시를 한 번 살펴봅시다.

```c#
delegate int Calc(int a, int b);

public static void Main(){
    Calc Calc;
    
    Calc = delegate (int a, int b){
        return a + b;
    }
    
    Console.WriteLine(Calc(4, 5));  // 9 출력
}
```

위 예시와 같이 익명메소드는 delegate 키워드를 사용해 선언합니다.  대리자가 참조할 메소드를 넘겨야 하는데 이 메소드가 한 번만 쓰이고 더이상 쓰일 일이 없는 경우 익명메소드를 사용하면 됩니다.



#### 6. 이벤트

알람이 필요한 경우 시계의 알람을 맞춰야 합니다.  이러한 알람과 같이 특정 이벤트가 발생했을 때 (사용자가 버튼을 클릭하는 등) 이를 알려주고 싶을 때 사용하는것이 바로 이벤트 입니다.  이벤트 사용 방법은 다음과 같습니다.

1. 대리자를 선언합니다. 대리자는 클래스 밖에 선언해도 되고 안에 선언해도 됩니다.
2. 대리자의 인스턴스를 event 한정자로 수식해서 선언합니다.
3. 이벤트 핸들러를 작성합니다. 이벤트 핸들러는 선언된 대리자와 일치하는 메소드면 됩니다.
4. 클래스의 인스턴스를 생성하고 해당 객체의 이벤트에 3에서 작성한 이벤트핸들러를 등록합니다.
5. 이벤트 발생시 이벤트 핸들러가 호출됩니다.

예제를 통해 살펴봅시다.

step 1. 대리자를 선언합니다. 대리자는 클래스 밖에 선언해도 되고 안에 선언해도 됩니다.

```c#
delegate void BonoHandler(string message);
```

step 2. 대리자의 인스턴스를 event 한정자로 수식해서 선언합니다.

```c#
class MyNotifier{
    public event BonoHandler SomethingHappened;
    public void DoSomething(int a){
        int temp = number % 10;
        
        // 10으로 나눈 나머지가 0이 아니고, 3의 배수이면 이벤트 호출
        if (temp != 0 && temp % 3 == 0){
            SomethingHappened(a);
        }
    }
}
```

step 3. 이벤트 핸들러를 작성합니다. 이벤트 핸들러는 선언된 대리자와 일치하는 메소드면 됩니다.

```c#
class MainApp{
    static public void MyHandler(string message){
        COnsole.WriteLine(Message);
    }
}
```

step 4. 클래스의 인스턴스를 생성하고 해당 객체의 이벤트에 3에서 작성한 이벤트핸들러를 등록합니다.

```c#
class MainApp{
    MyNotifire notifire = new MyNotifire();
    notifire.SomethingHappened += new BonoHandler(MyHandler);
    
    for (int i = 1; i < 30; i++){
        notifire.DoSomething(i);
    }
}
```

위 코드를 정리해봅시다.

```c#
using System;

namespace BonoTest
{
    public delegate void BonoHandler(int num);

    public class MyNotifire
    {
        public event BonoHandler SomethingHappened;
        public void DoSomething(int a)
        {
            int temp = a % 10;

            // 10으로 나눈 나머지가 0이 아니고, 3의 배수이면 이벤트 호출
            if (temp != 0 && temp % 3 == 0)
            {
                SomethingHappened(a);
            }
        }
    }

    class MainApp
    {
        static public void MyHandler(int num)
        {
            Console.WriteLine(num);
        }

        static void Main()
        {
            MyNotifire notifire = new MyNotifire();
            notifire.SomethingHappened += new BonoHandler(MyHandler);

            for (int i = 1; i < 30; i++)
            {
                notifire.DoSomething(i);  // 3 6 9 12 ... 출력
            }
        }
    }
}
```

일반 메소드를 사용하는것과 별 차이가 없어보입니다.  왜 이벤트를 사용하는 걸까요?



#### 7. 대리자와 이벤트

이벤트는 대리자에 event를 수식해서 선언한것에 불과합니다.  그렇다면 왜 있는걸까요?  **이벤트와 대리자가 가장 크게 다른 점은 이벤트는 외부에서 직접 사용이 불가능**하다는 데 있습니다.  이벤트는 public 한정자로 수식되어 있어도 자신이 선언된 클래스 외부에서는 호출이 불가합니다.  반면 대리자는 public 이나 internal로 수식되어 있으면 클래스 외부에서도 얼마든 호출이 가능합니다.

따라서 직접 호출이 불가하고 클래스 인스턴스의 이벤트핸들러에 이벤트를 등록하고 인스턴스 내부에서만 사용이 가능합니다.  **대리자는 콜백 용도로 사용하고, 이벤트는 객체의 상태 변화나 사건의 발생을 알리는 용도로 구분해서 사용해야 합니다.**





---------

###### 참고도서 : 박상현, 이것이 C#이다, 한빛미디어