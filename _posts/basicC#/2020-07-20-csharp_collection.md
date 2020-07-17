---
layout: single
title: "[C# Basic] 배열, 컬렉션, 인덱서"
categories:
  - C# Basic
toc: true
toc_sticky: true
toc_label: "On this Page"
date: 2020-07-20
---



이번에는 데이터를 다루기 편하게 만들어주는 C#의 배열과 컬렉션, 인덱서에 대해 알아보자. 

-----

##### * 배열의 선언

```c#
데이터형식[] 배열이름 = new 데이터형식[용량];  // 선언방법

string[] array = new string[5];  // 선언예제
array[0] = "0번";
Console.WriteLine(array[0]);  // 0번 출력
```

- 같은 데이터 타입의 여러 변수를 한 곳에 담기 위해 배열을 사용할 수 있다.
- for 문이나 foreach 문을 이용해 더욱 간결한 소스코드를 만들 수 있다.



##### * 배열의 초기화

```c#
string[] array = new string[3]{"Bono", "Porori", "Hello"};  // 1. 컬렉션초기자를 이용한 초기화
string[] array2 = new string[]{"Bono", "Porori", "Hello"};  // 2. 배열의 용량을 생략
string[] array3 = {"Bono", "Porori", "Hello"};  // 3. 할당할 데이터만 입력
```

- 위와 같이 초기화 방법은 세가지로 볼 수 있다.
- 세번째 방법이 가장 간단해 보이지만 첫번째 방법이 가독성이 제일 좋다.  코드는 남이 읽기 쉽게 쓰는것이 좋다.



##### * System.Array

- 배열은 System.Array 형식에서 파생된다.  따라서 System.Array의 특성과 메소드를 파악하면 배열을 이용한 많은 것을 할 수 있다.
- 예를 들어 몇가지만 살펴보자면,
  - Sort() : 배열을 정렬한다
  - IndexOf() : 배열에서 찾고자 하는 특정 데이터의 인덱스를 반환한다.
  - Length : 배열의 길이를 반환한다.



##### * 2차원배열, 다차원배열

```c#
int[ , ] array = new int [1, 2];  // 2차원배열
array[0,0] = 1;
array[0,1] = 2;
int[ , ] array2 = new int[1, 2]{{1, 2}}  // 선언과 동시에 초기화
```

- 2차원 배열은 위와 같이 사용할 수 있다.  표 형태로 생각하면 이해하기 쉽다.
- 2차원 배열 외에도 3차원, 4차원 등의 다차원 배열이 있다.  2차원배열도 읽기 쉬운편은 아니다.  꼭 사용해야 하는 경우라면 2차원배열까지만 사용하고 그 이상의 배열은 모르는편이 낫다.



##### * 가변배열

```c#
int[][] array = new int[3][];
array[0] = new int[5]{1, 2, 3, 4, 5};
array[1] = new int[3]{1, 2, 3};
array[2] = new int[1]{1};
```

- 2차원 배열과 비슷하지만 가변배열은 길이가 서로 다를 수 있다는점이 다르다.
- 위 예제에서는 0번 요소에는 길이가 5인 배열, 1번 요소에는 길이가 3인 배열, 2번 요소에는 길이가 1인 배열이 할당되었다.



##### * Collection

- 컬렉션이란 같은 성격의 데이터를 담는 자료 구조를 말한다.
- 배열도 컬렉션의 일부에 해당된다.
- .NET 프레임워크의 대표적인 컬렉션 클래스는 ArrayList, Queue, Stack, Hashtable 이 있다.



##### * ArrayList

```c#
ArrayList list = new ArrayList();
list.Add(10);  // 리스트에 요소 추가.  서로 다른 데이터타입이 올 수 있다.
list.Add("Bono");
list.Add(true);

list.RemoveAt(1);  // "Bono" 삭제

list.Insert(1, "Porori");  // 1번 인덱스에 "Porori" 삽입.
```

- ArrayList는 배열과 가장 유사한 컬렉션이다.
- ArrayList는 배열과 달리 용량을 지정할 필요 없이 필요에 따라 자동으로 용량이 늘어나거나 줄어든다.
- Add() 메소드는 컬렉션의 가장 마지막 요소 뒤에 새 요소를 추가한다
- RemoveAt() 메소드는 특정 인덱스의 요소를 삭제한다
- Insert() 메소드는 특정 인덱스에 새로운 요소를 삽입한다.
- 각 요소는 데이터타입이 모두 달라도 된다.  
- ArrayList의 요소의 데이터타입이 달라도 되는 이유는 각 메소드들이 object 형식의 매개변수를 받고 있기 때문이다.  하지만 이로 인해 불필요한 박싱/언박싱이 일어나면서 성능에는 좋지 않은 영향을 준다.  ArrayList가 다루는 데이터가 많아지면 성능 저하는 더 많이 생길 것이다.  이는 바로 뒤에서 살펴볼 Queue, Stack, Hashtable에서도 동일하게 발생하는 문제이다.  이러한 문제를 해결하기 위해 일반화 컬렉션이 존재한다.  이는 다음 장에서 살펴보자.



##### * Queue

```c#
Queue que = new Queue();
que.Enqueue(1);
que.Enqueue("2");

object a = que.Dequeue();  // 가장 앞 요소 1을 꺼낸다
```

- 선입선출법에 해당하는 자료구조이다.  입력은 가장 뒤에서, 출력은 가장 앞에서만 이루어진다.
- Enqueue() 메소드는 가장 뒤에 데이터를 입력한다
- Dequeue() 메소드는 가장 앞 요소를 반환한다



##### * Stack

```c#
Stack stack = new Stack();
stack.Push(1);
stack.Push(2);

int a = stack.Pop();  // 가장 뒤 요소 2를 꺼낸다
```

- Queue와는 반대로 먼저 들어온 데이터가 나중에 나가는 구조이다.  후입선출 방식에 해당된다.
- Push() 메소드는 데이터를 가장 뒤에 입력한다
- Pop() 메소드는 가장 뒤에 있는 데이터를 반환한다



##### * Hashtable

```c#
Hashtable hash = new Hashtable();
hash["Name"] = "Bono";  // Name 이라는 Key 에 Bono 라는 Value 할당
hash["Age"] = 15;

Console.WriteLine(hash["Age"]);
```

- Hashtable은 키와 값 형태로 이주어진 자료구조이다.
- 데이터 출력시 키를 가지고 검색하므로 속도가 빠르다는 장점이 있다.  배열에서 요소를 찾는 경우 컬렉션을 정렬해서 이진탐색을 하거나 순차적으로 리스트를 탐색하지만 Hashtable은 키를 이용해 데이터가 저장된 컬렉션 내의 주소를 바로 계산해낸다.  이 과정을 Hashing이라 한다.



##### * 컬렉션 초기화 방법

```c#
int[] array = {1, 2, 3};

ArrayList list = new ArrayList(arr);  // 배열을 이용한 초기화
Queue queue = new Queue(arr);
Stack stack = new Stack(arr);

ArrayList list2 = new ArrayList(){1, 2, 3};  // 컬렉션 초기자를 이용한 초기화

Hashtable hash = new Hashtable(){  // 딕셔너리 초기자를 사용한 초기화
    [1] = 1,  // ; 이 아닌 , 로 항목을 구분
    [2] = 2
}

Hashtable hash2 = new Hashtable(){  // 컬렉션 초기자를 이용한 초기화
    {1, 1},
    {2, 2}
}
```

- ArrayList, Queue, Stack 은 배열을 이용한 초기화가 가능하다.
- ArrayList 는 추가적으로 컬렉션 초기자를 이용해 초기화하는 것이 가능하다.  Queue, Stack은 불가능하다.  컬렉션 초기자는 IEnumerable 인터페이스와 Add() 메소드를 구현하는 컬렉션만 지원하는데 두 컬렉션은 IEnumerable은 상속하지만 Add() 메소드를 구현하고 있지 않다.
- Hashtable은 딕셔너리 초기자를 이용할 수도 있고 컬렉션 초기자를 이용할 수도 있다.



##### * 인덱서

```c#
class Bono{
    private string[] say;  // private 필드 선언
    
    public Bono(){
        say = new String[3];  // private 필드 초기화
    }
    
    // **인덱서**
    public string this[int index]{  
        get{
            return say[index];  // 해당 인덱스 값을 반환
        }
        set{
            if(index >= say.Length){  // 인덱스가 배열의 길이보다 큰 경우
                Array.Resize<string>(ref say, index+1); // 배열 길이 추가
            }
            say[index] = value;  // 해당 인덱스에 값 할당
        }
    }
}

...
// 인덱서 사용시
Bono bono = new Bono();
bono[0] = "안녕";
bono[1] = "Hello";
```

- 인덱서는 인덱스를 이용해 객체 내의 데이터에 접근할 수 있도록 해주는 프로퍼티라고 생각할 수 있다.  객체를 배열처럼 사용할 수 있게 해준다.

- 인덱서와 프로퍼티는 비슷해 보이는데 이 둘이 다른 점은 인덱서에서는 인덱스를 이용해 데이터를 읽고 쓴다는 점이다.

- 위 예제에서 Bono 객체를 생성하면 해당 객체를 배열처럼 쓸 수 있게 된다.

  

##### * foreach가 가능한 객체 만들기

- **foreach 구문은 IEnumerable과 IEnumerator을 상속하는 형식만 지원**한다.  즉 배열이나 리스트 같은 컬렉션에서만 사용이 가능하다.
- 새로 만든 객체를 foreach가 가능하도록 하려면 해당 인터페이스를 상속받고 구현해야한다.

**1) IEnumerable 인터페이스**

- IEnumerable 인터페이스가 가지고 있는 메소드는 **IEnumerator GetEnumerator()** 하나 뿐이다.
- **GetEnumerator() 메소드를 구현할때는 yield return문**을 사용해야 한다.  
- yield return문은 메소드의 실행을 일시정지시키고 호출자에게 결과를 반환한다
- 메소드가 다시 호출되면 yield return이나 yield break(완전 중지)를 만날때까지 나머지 작업을 실행한다.

**2) IEnumerator 인터페이스**

- GetEnumerator() 메소드는 IEnumerator 인터페이스를 상속하는 클래스의 객체를 반환하면 된다.
- IEnumerator 인터페이스는 총 3개의 메소드를 가지고 있다
- boolean MoveNext() : 다음 요소로 이동.  컬렉션의 끝을 지나면 false, 이동 성공시 true 반환
- void Reset() : 컬렉션의 첫 위치의 앞으로 이동.  0번이 첫 요소라면 -1로 이동한다.
- Object Current { get; } : 컬렉션의 현재 요소를 반환



예제를 한 번 만들어보자.

```c#
using System;
using System.Collections;

namespace EnumerableTest{
    class Bono : IEnumerable, IEnumerator{  // 두 인터페이스 상속
        private int[] array;
        private int position = -1;  // 컬렉션의 현재 위치 초기값.  0이 아닌 -1로 시작.  movenext() 시 0으로 이동하게 됨
        
        public Bono(){  // 생성자에서 배열 초기화
            array = new int[3];
        }

        public int this[int index]{  // 인덱서
            get{
                return array[index];  // 해당 인덱스의 값 반환
            }
            set{
                if(index >= array.Length){  // 인덱스가 배열의 길이보다 크면
                    Array.Resize<int>(ref array, index + 1);  // 배열 리사이즈
                }
                array[index] = value;  // 해당 인덱스에 값 할당
            }
        }
        
        // IEnumerator 멤버
        public object Current{
            get{
                return array[position];  // 현재 위치의 값을 반환
            }
        }
        
        // IEnumerator 멤버
        public bool MoveNext(){
            if (position == array.Length -1){  // 더이상 이동이 불가능한 경우 초기화 후 false 반환
                Reset();
                return false;
            }
            position++;  // 위치 1 이동
            return true;
        }
        
        // IEnumerator 멤버
        public void Reset(){
            position = -1;  // 위치값 초기화
        }
        
        // IEnumerable 멤버
        public IEnumerator GetEnumerator(){
            for (int i = 0; i < array.Length; i++){
                yield return array[i];  // 각 요소를 하나씩 반환
            }
        }
    }
    
    class MainApp{
        static void Main(){
            Bono bono = new Bono();
            
            for(int i=0; i < 5; i++){
                bono[i] = i;  // 인덱서를 이용한 변수 세팅
            }
            
            foreach(var temp in bono){  // IEnumerator GetEnumerator() 이 실행되면서 각 요소를 하나씩 반환
                Console.WriteLine(temp);
            }
        }
    }
}
```





