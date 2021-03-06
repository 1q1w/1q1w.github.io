---
layout: single
title: "[자바스크립트] 실행 컨텍스트와 스코프체인"
categories:
  - javascript
toc: true
toc_sticky: true
toc_label: "On this Page"
date: 2020-05-11
---



이번 시간에는 자바스크립트 실행컨텍스트와 스코프체인에 대해 알아보자.



***

#### 1. 실행컨텍스트란

**실행컨텍스트란 자바스크립트 코드 블럭이 실행되는 환경** 이라 할 수 있다.  주요 고급 프로그래밍 언어에는 콜 스택 이라는 개념이 있는데 이와 유사하다.  (콜 스택은 함수 호출정보가 차곡차곡 쌓여있는 스택을 의미한다.)

아래 예제를 살펴보자.

```javascript
console.log('보노보노');

function test1(){
    console.log('test1');
}

function test2(){
    test1();
    console.log('test2');
}

test2();
```

어렵지 않은 예제이다.  첫 줄의 보노보노 가 출력되고, test2()가 실행되면서 그 안의 test1()이 먼저 실행되어 test1 이 출력된다.  그리고 마지막으로 test2()의 test2 가 출력될 것이다.

위 예제에서 실행 컨텍스트가 어떻게 되는지 정리해보면 아래와 같다.

1) 전역 실행 컨텍스트가 가장 먼저 실행.  실행 컨텍스트 스택에는 전역 컨텍스트만 존재.

2) test2()가 호출되면서 실행 컨텍스트 스택에 test2 실행 컨텍스트가 쌓임.

3) test1()이 호출되면서 실행 컨텍스트 스택에 test1 실행 컨텍스트가 쌓임.

4) test1()이 종료되면 test1 실행 컨텍스트 반환.

5) test2()가 종료되면 test2 실행 컨텍스트 반환.



**새로운 함수 호출이 발생하면 새로운 컨텍스트가 만들어지고 실행되며, 끝나면 해당 컨텍스트는 반환**된다.  이와 같은 과정이 반복된 후 전역 실행 컨텍스트의 실행이 완료되면 종료된다.





#### 2. 실행 컨텍스트 생성과정

아래 예제를 실행하면 실행 컨텍스트가 어떻게 만들어질지 생각해보자.

```javascript
function test(arg1, arg2){
    var a = 1;
    var b = 2;
    
    function func(){
        return a + b;
    }
    
    return arg1 + arg2 + func();
}

test(1, 2);
```



위 예제에서 test() 함수를 실행했을 경우를 예로 실행 컨텍스트 생성과정을 살펴보자.

##### 1) 활성 객체 생성

- 실행 컨텍스트가 생성되면 자바스크립트 엔진은 해당 컨텍스트에서 실행할 여러 정보를 담을 객체를 생성하는데 이를 활성 객체라 한다.  앞으로 사용할 매개변수나 사용자가 정의한 변수, 객체를 저장하고 새로 만들어진 컨텍스트로 접근이 가능하게 되어있다.

##### 2) arguments 객체 생성

- 활성 객체의 arguments 프로퍼티는 arguments 객체를 참조한다.  예제에서는 활성 객체의 arguments 프로퍼티는 test() 함수의 arguments 인 [arg1, arg2] 를 참조한다.

##### 3) 스코프 정보 생성

- 현재 컨텍스트의 유효범위를 나타내는 스코프를 생성한다.  이는 연결 리스트와 유사한 형식으로 만들어진다.
- 이 리스트를 통해 현재 컨텍스트의 변수뿐 아니라 상위 실행 컨텍스트의 변수에도 접근이 가능하다.  리스트에서 찾지 못한 변수는 정의되지 않은 변수로 판단하여 에러를 발생시킨다.
- 이 리스트를 스코프 체인이라 하고 [[scope]] 프로퍼티로 참조된다.

##### 4) 변수 생성

- 현재 실행 컨텍스트 내부에서 사용되는 지역 변수가 생성된다.
- test() 의 예제에서는 변수 a, b 와 함수 func()가 생성된다.
- 이 단계에서는 변수나 함수를 메모리에 생성만 할 뿐 초기화는 하지 않는다.  각 표현식이 실행되기 전까지는 초기화가 일어나지 않는다.
- 따라서 변수 a, b는 undefined 상태이다.

##### 5) this 바인딩

- 함수 호출시 this는 전역객체에 바인딩된다.

##### 6) 코드 실행

- 해당 코드에 있는 여러 표현식들이 실행된다.
- 변수 초기화 및 연산, 또 다른 함수 실행 등이 이뤄진다.



##### 참고사항

- 전역 실행 컨텍스트는 arguments 객체가 없다.
- 전역 실행 컨텍스트는 전역 객체 하나만 포함하는 스코프 체인이 있다.
- 전역 실행 컨텍스트의 변수 객체가 전역 객체로 사용된다. 



***

#### 3. 스코프 체인이란

스코프 체인의 이해는 자바스크립트 코드를 이해하는데 굉장히 중요한 부분이다.  스코프 체인이 어떻게 만들어지는지 살펴보자.  

**자바스크립트에서 변수의 유효범위는 함수 단위로 결정**된다.  이 **유효범위를 나타내는 스코프가 [[scope]] 프로퍼티로 각 함수 객체 내에서 관리되는데, 이를 스코프 체인**이라 한다.

**각 함수의 [[scope]] 프로퍼티는 자신이 생성된 실행 컨텍스트의 스코프 체인을 참조**한다.  함수가 실행되면 새로운 실행 컨텍스트가 생성되고, **자신이 실행된 함수의 [[scope]] 프로퍼티를 기반으로 새로운 스코프 체인을 생성**한다.

아래 예제를 살펴보자.

```javascript
var a = 1;
var b = 2;

function test(){
    var a = 100;
    var b = 101;
    console.log(a);  // 100 출력
    console.log(b);  // 101 출력
}
test();
console.log(a);  // 1 출력
console.log(b);  // 2 출력
```

1) 전역 실행 컨텍스트가 생성되고 test() 함수 객체가 생성된다. 

2) test() 함수가 실행되면서 새로운 실행 컨텍스트가 생성된다.

3) test() 함수 객체의 [[scope]] 는 자신을 실행한 전역 실행 컨텍스트의 [[scope]]를 그대로 복사한 뒤, 현재 생성된 변수 객체를 스코프 체인의 맨 앞에 추가한다.  복사한 [[scope]] 는 전역객체 하나만 가지고 있었으므로, test 실행 컨텍스트의 스코프 체인은 다음 순서와 같다.  [스코프 체인 : test 변수 객체 - 전역 객체]

4) test()에서 a, b 를 출력할 때 자신의 스코프 내에 있는 a, b 를 출력한다.  만약 자신의 스코프에 해당 변수가 없었다면 상위 컨텍스트의 스코프 체인을 조회한 뒤 있다면 그것을 출력했을 것이다.



**스코프 체인을 정리해보자.**

- 각 함수 객체는 [[scope]] 프로퍼티로 현재 컨텍스트의 스코프 체인을 참조
- 함수가 실행되면 새로운 실행 컨텍스트가 생성되면서 스코프 체인을 생성한다.
- 스코프 체인 = 현재 실행 컨텍스트의 변수 객체 + 상위 컨텍스트의 스코프 체인



그렇다면 아래 예제의 결과를 예상해보자.

```javascript
var str = 'A';

function test(){
    var str = 'B';
    
    function returnStr(){
        return str;
    }
    
    console.log(returnStr());
}

test();
```

1) test 함수가 실행되면서 스코프체인은 [test 변수 객체 - 전역 객체] 가 된다.

2) returnStr 함수가 실행되면서 스코프 체인은 [returnStr 변수 객체 - test 변수 객체 - 전역 객체] 가 된다.

3) returnStr 변수 객체에는 str 변수가 없어 상위 스코프를 참조한다.

4) test 변수 객체에 str 변수가 있으므로 해당 변수 값이 리턴된다.  따라서 B 가 출력된다.



아래 예제 결과도 예상해보자.

```javascript
var str = 'A';

function test(){
    return str;
}

function logFunc(func){
    var str = 'B';
    console.log(func());
}

logFunc(test);
```

**함수 객체가 처음 생성될 때의 실행 컨텍스트가 무엇인지를 잘 생각해야 한다.**

1) test와 logFunc 함수가 생성될 때 [[scope]] 는 전역 객체의 [[scope]]를 참조한다.  따라서 해당 함수들의 [[scope]] 는 [해당 함수 변수 객체 - 전역 객체] 가 된다.

2) logFunc 함수를 실행하면서 test 함수를 인자로 넘겼다.  logFunc 함수 내에서 인자로 받아진 test 함수가 실행되면서 test 함수의 리턴값이 출력되도록 되어있다.  test 함수가 실행될 때는 test 함수의 실행 컨텍스트 내에서 str 변수를 찾는다.  test 함수의 실행 컨텍스트는 [test 변수 객체 - 전역 객체] 인데, test 변수 객체에 str 변수가 없으므로, 전역 객체의 str을 리턴한다.  따라서 A 가 출력된다.



**스코프 체인으로 식별자 인식**이 이루어지는데, 식별자 인식은 스코프 체인의 첫번째 변수 객체부터 시작한다.  첫번째 **변수 객체에 일치하는 이름을 가진 프로퍼티가 있는지 확인하고 만약 없다면 다음 객체로 이동하여 찾는다.**  예외적으로 this 는 식별자가 아닌 키워드로 분류되어 스코프 체인 참조 없이 접근이 가능함을 기억하자.





##### 참고_함수 호이스팅의 원리

스코프 체인을 이해했다면 함수 호이스팅의 원리도 알 수 있다.

```javascript
bonobono();
porori();

var bonobono = function(){
    console.log('bonobono' + str);
};

function porori (){
    console.log('porori' + str);
}

var str = ' is happy';
```

위와같이 작성된 코드는 아래 코드와 동일하다고 볼 수 있다.

```javascript
// 스코프 정보 및 변수 생성 단계
// 변수의 초기화는 되지 않음
var bonobono;
function porori (){
    console.log('porori' + str);
}
var str;

// 코드 실행 단계
// 변수 초기화 및 함수 실행
bonobono();  // type error
porori();  // pororiundefined 출력

bonobono = function(){
    console.log('bonobono' + str);
};
str = ' is happy';
```

1) 스코프 정보 및 변수 생성이 일어난다.  변수의 초기화는 되지 않는다.

2) 코드 실행 단계에서 변수 초기화 및 함수가 실행된다.  

- 순차적으로 실행되면서 bonobono() 는 type 오류가 발생한다.   bonobono는 변수 선언만 된 상태고 초기화가 되지 않았기 때문이다.
- porori() 에서는 str 변수가 사용되는데, str 변수의 초기화가 되기 전이므로 undefined가 출력된다.









