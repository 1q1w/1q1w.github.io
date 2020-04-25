---
layout: single
title: "[자바스크립트] 함수 기초"
categories:
  - javascript
toc: true
toc_sticky: true
toc_label: "On this Page"
date: 2020-04-21
---



이번 포스트에서는 자바스크립트에서 가장 중요한 개념인 함수에 대해 알아보자.



### 함수 정의

자바스크립트에서 함수를 생성하는 방법은 3가지로 나뉜다.  모두 같은 함수를 생성하는 방법이지만 각 방법별로 함수 동작에 미묘한 차이가 있다.  매개변수 a 와 b 를 입력받아 이를 더한 결과를 리턴하는 함수를 세 가지 방법으로 작성해보면서 차이를 알아보도록 하자.



#### 1. 함수 리터럴

자바스크립트의 함수는 일반 객체처럼 취급된다.  객체를 리터럴 방식으로 생성할 수 있는 것처럼 함수도 리터럴 방식으로 생성이 가능하다.

```javascript
function plus(a, b){
    return a + b;
}
```

1) 함수 리터럴은 function 키워드로 시작

2) 함수명은 있어도 되고 없어도 됨.  함수명이 없는 경우 익명함수라 부른다.

3) 매개변수 타입은 따로 기입하지 않아도 됨



#### 2. 함수 선언문

함수 선언문 방식은 리터럴 형태와 같다.  하지만 리터럴 형태와의 차이는 선언문 방식은 반드시 함수명이 있어야 한다는 점이다.

```javascript
console.log(plus(4, 5));

function plus(a, b){
    return a + b;
}
```

1) 선언문으로 생성된 함수는 함수가 생성되기 이전에도 호출이 가능하다.



#### 3. 함수 표현식

함수 리터럴로 함수를 만들고, 이를 변수에 할당하여 함수를 생성하는것을 함수 표현식이라 한다.

```javascript
// 표현식으로 생성된 익명함수를 plus 변수에 할당
var plus = function (a, b){
    return a + b;
};

var temp = plus;

console.log(plus(3, 4));  // 7 출력
console.log(temp(3, 4));  // 7 출력
```

1) plus 변수는 함수의 이름이 아니라 함수 리터럴로 생성된 함수를 참조하는 변수

2) plus 변수는 참조값을 가지고 있으므로 다른 변수에 그 값을 그대로 할당할 수 있음

3) plus 와 temp 는 동일한 익명함수를 참조하고 있음



위 예제에서는 익명함수를 변수에 할당하여 사용하는 방법을 알아보았다.  그렇다면 기명함수를 사용할 때는 어떻게 동작하는지 살펴보자.

```javascript
var plus = function sum(a, b){
    return a + b;
};

console.log(plus(3, 4));  // 7 출력
console.log(sum(3, 4));  // Uncaught ReferenceError 발생
```

1) sum 함수는 plus 변수에 할당되었음

2) 함수 표현식으로 선언된 sum 함수는 외부에서 접근이 불가능하며, plus 변수를 통해 접근해야 함



그렇다면 앞에서 살펴보았던 함수 선언문으로 작성된 함수는 어떻게 동작할 수 있었을까?  자바스크립트 엔진은 함수 선언문 형태를 함수 표현식 형태로 변경해서 실행한다.

```javascript
// 함수 선언문으로 작성된 함수는
function plus(a, b){
    return a + b;
}
```

```javascript
// 자바스크립트 엔진에 의해 표현식 형태로 변경되어 실행
var plus = function plus(a, b){
    return a + b;
};
```



함수 표현식을 활용하면 재귀함수를 만들 수도 있다.

```javascript
var tempFactorial = function factorial(a){
    if (a <= 1){
        return 1;
    }
    return a * factorial(a-1);  // 본인 함수를 재귀호출
};

console.log(tempFactorial(5));  // 120 출력 (5*4*3*2*1)
console.log(factorial(5));  // Uncaught ReferenceError 발생.  외부에서는 접근 불가능
```



**주의사항**

일반적으로 함수 선언문 방식으로 생성된 함수는 끝에 세미콜론을 붙이지 않지만, **함수 표현식 방식으로 생성된 함수는 끝에 세미콜론을 붙일 것을 권장**한다.





#### Function() 생성자 함수

자바스크립트의 함수는 Function() 이라는 기본 내장 생성자 함수로 생성된 객체라고 볼 수 있다.

일반적으로 잘 사용되지 않는 방법이다.  상식으로 알고만 있자.

```javascript
var plus = new Function('a', 'b', 'return a + b');
console.log(plus(2, 4));  // 6 출력
```





#### 함수 호이스팅

함수를 선언하는 세 가지 방법에 조금씩 차이가 있다고 했는데 함수 호이스팅도 그 중의 하나이다.

위에서 함수 표현식에 대한 설명이 가장 많았는데, 많은 자바스크립트 가이드들에서는 함수 표현식을 사용할 것을 권한다.  그 이유에 대해 살펴보자.

```javascript
console.log(plus(2, 3));  // 5 출력

function plus(a, b){
    return a + b;
}

console.log(plus(3, 4));  // 7 출력
```

**선언문으로 생성된 함수의 유효범위는 코드의 처음부터 시작**한다.  이것을 **함수 호이스팅**이라 한다.



함수 호이스팅은 함수를 사용하기 전에 선언해야된다는 규칙을 무시하므로 코드의 구조를 부실하게 만들 수 있어 함수 표현식 사용을 권장한다.  아래는 함수 표현식의 예이다.

```javascript
// console.log(plus(2, 3));  // Uncaught type error

var plus = function (a, b){
    return a + b;
}

console.log(plus(3, 4));  // 7 출력
```

함수 표현식으로 생성된 함수는 호이스팅이 일어나지 않아 함수 생성 이전에 실행되는 경우 Uncaught type error가 발생한다.





#### 함수도 객체

자바스크립트에서는 함수도 객체다.  따라서 기능 뿐 아니라 프로퍼티를 가질 수 있다.

```javascript
var plus = function (a, b){
    return a + b;
};

plus.test = 'name is plus';
plus.status = '404error';

console.log(plus.test);  // name is plus 출력
console.log(plus.status);  // 404error 출력
```



이 외에도 함수는 일반 객체처럼 취급될 수 있다는 특성으로 아래와 같은 동작이 가능하다. 

- 리터럴에 의해 생성
- 변수나 배열의 요소, 객체의 프로퍼티 등에 할당 가능
- 함수의 인자로 전달 가능
- 함수의 리턴값으로 리턴 가능
- 동적으로 프로퍼티를 생성 및 할당 가능



이러한 특징들 때문에 함수는 **일급 객체** 라고 불린다.  함수를 제대로 이해하려면 함수가 기능을 수행할 뿐 아니라 객체처럼 값으로 취급될 수 있다는것을 이해할 수 있어야 한다.



아래 예제를 통해 위 특징들을 하나씩 살펴보자.

##### 1) 변수나 프로퍼티 값으로 할당

```javascript
// 변수에 함수 할당
var coffee = 'latte';
var price = function (){ return 3500; };
console.log(price());  // 3500 출력

// 객체 프로퍼티에 함수 할당
var water = {};
water.price = function(){ return 2000; };
console.log(water.price());  // 2000 출력
```

price()는 함수의 참조값을 저장하고 있어 price()를 통해 실제 함수 호출이 가능하다.  또한 객체의 요소나 배열의 요소 등에도 함수의 참조값을 저장할 수 있다.



##### 2) 함수의 인자로 전달

```javascript
var test = function(func){
    func();  // 매개변수로 전달받은 함수 호출
};

test(function(){
    console.log('익명함수를 전달합니다.');
});  // 익명함수를 전달합니다.  출력
```

test() 함수를 호출할 때 익명함수를 매개변수로 넘겼다.  test() 함수 내부에서는 전달받은 함수가 실행된다.



##### 3) 리턴값으로 활용

```javascript
// 함수를 리턴하는 test() 함수 정의
var test = function(){
    return function(){
        console.log('리턴된 함수입니다.');
    };  // 익명함수를 리턴
};

var temp = test();
temp();  // 리턴된 함수입니다.  출력
```

함수 자체가 값으로 취급되어 함수 자체를 리턴할 수 있다.





#### 함수 객체의 기본 프로퍼티

일반 객체와 달리 함수 객체에는 별도의 표준 프로퍼티가 정의되어 있다. 

아래 예제를 크롬 브라우저에서 실행시켜보자.

```javascript
function plus(a, b){
    return a + b;
}

console.dir(plus);
```

결과는 아래와 같다.

![Alt text](/assets/images/function_property.jpg "function_property")



1) 함수 객체는 length와 prototype 프로퍼티를 가진다.  length 프로퍼티는 해당 함수에 필요한 인자 수를 나타낸다.

2) name 프로퍼티는 함수의 이름이다.  익명함수라면 빈 문자열이 된다.

3) caller 프로퍼티는 자신을 호출한 함수를 표기한다.

4) arguments 프로퍼티는 함수를 호출할 때 전달된 인자값을 표기한다.

5) [[Prototype]] 프로퍼티는 Function.prototype 객체라고 정의되어 있다.  크롬 브라우저에서는 이를 Empty() 함수라고 명하고 있다.  

**Function.prototype 객체는 모든 함수들의 부모 역할**을 한다.  따라서 모든 함수는 Function.prototype 객체에 있는 프로퍼티나 메서드를 사용할 수 있다.  대표적인 함수가 **apply(), call()** 메서드이다.

**Function.prototype 의 부모 객체는 Object.prototype** 객체이다.



#### ** prototype 프로퍼티

헷갈리기 쉬운 개념인 prototype에 대해 정리를 조금 더 해보자.

- 모든 함수는 객체로서 prototype 프로퍼티를 가진다.  
- 이는 앞서 말한 객체의 부모를 나타내는 내부 프로퍼티인 [[Prototype]]과는 다르다.  
- **prototype 프로퍼티는 해당 함수가 생성자로 사용될 때 이 함수를 통해 생성된 객체의 부모 역할을 하는 프로토타입 객체**를 가리킨다.  즉 해당 함수로 생성될 객체의 부모 [[Prototype]]이라고 보면 된다.



아래 예제로 prototype을 좀 더 살펴보자.

```javascript
// testFunction() 함수 선언
function testFunction(){
    return true;
}

console.log(testFunction.prototype);
console.log(testFunction.prototype.constructor);
```

1) testFunction() 함수가 생성됨과 동시에 prototype 프로퍼티에는 해당 함수와 연결된 프로토타입 객체가 생성된다.

2) testFunction의 prototype에는 constructor와 [[Prototype]] 프로퍼티가 생성된다.

3) constructor 프로퍼티는 자신과 연결된 함수를 참조한다.  결국 함수 객체는 prototype 프로퍼티를 통해 프로토타입 객체를 참조하고, 프로토타입 객체의 constructor 프로퍼티는 해당 함수를 참조하는, 양방향 참조의 모습을 보인다.

4) prototype 객체 역시 자신의 부모인 [[Prototype]] 프로퍼티를 지니고 testFunction()의 prototype의 [[Prototype]]은 Object.prototype이다.  (testFunction 함수로 생성되는 객체의 부모는 testFunction.prototype 이 되며, testFunction.prototype의 부모 객체는 Object.prototype 이라 보면 된다.)



추후 프로토타입 체이닝에서 프로토타입에 대해 다시 한 번 살펴보도록 하자.





### 함수의 형태

#### 1. 콜백 함수

함수의 이름을 붙이지 않는 경우 익명함수라 했었다.  이러한 익명함수의 대표적인 용도가 콜백 함수이다. 

콜백 함수는 자바스크립트 이벤트 핸들러에서 주로 사용된다.  DOM 이벤트에 해당되는 클릭, 페이지 로드 등이 완료되는 시점에 브라우저는 정의된 DOM 이벤트에 해당하는 이벤트 핸들러를 실행시킨다.  이 때 이벤트 핸들러에 콜백 함수를 등록하여 이벤트가 실행될 때마다 해당 콜백 함숙 실행되도록 한다.



아래의 예제는 window.onload 라는 이벤트 핸들러에 익명함수를 등록한 것이다.  웹 페이지의 로딩이 끝나는 시점에 load 이벤트가 발생하면 실행되며, window.onload 이벤트 핸들러를 익명 함수로 연결했다.

```html
<!DOCTYPE html>
<html>
    <body>
        <script>
        	window.onload = function(){
                alert('콜백함수 입니다');
            }
        </script>
    </body>
</html>
```



#### 2. 즉시실행함수

함수를 정의함과 동시에 실행하는 함수를 즉시실행함수라 한다.  아래의 예제를 보자.

```javascript
(function(input){
    console.log('입력된 값은 ' + input);
})('7002');  // 입력된 값은 7002 출력
```

즉시실행함수는 함수 리터럴을 괄호 () 로 감싸고 그 뒤에 바로 매개변수를 입력하여 만들 수 있다.  즉시실행함수는 같은 함수를 다시 실행할 수 없다.  따라서 1회성으로 실행하는 경우 사용한다.



즉시실행함수는 jQuery같은 자바스크리브 라이브러리에서 사용된다.  jQuery의 소스코드의 시작과 끝을 보면 즉시실행함수라는 것을 알 수 있다.  



#### 3. 내부 함수

함수 코드 내부에서 다시 함수 정의가 가능한데 이를 내부함수라 한다.  예제를 살펴보자.

```javascript
function parent(){
    var a = 1;
    var b = 2;
    
    // 내부 함수 정의
    function child(){
        var b = 10;
        
        console.log(a);
        console.log(b);
    }
    
    child();
}

parent();  // 1 10 출력
child();  // Uncaught Reference error.  내부함수는 부모함수에서만 호출이 가능
```



내부함수의 특징을 정리해보자

- 내부함수에서는 자신을 둘러싼 부모 함수 변수에 접근이 가능하다.  (스코프 체이닝 때문)
- 내부함수는 자신이 정의된 부모함수에서만 호출이 가능하다.  외부에서의 호출은 불가능하다.
- 함수 외부에서도 내부함수를 호출할 수 있는데, 부모함수에서 내부함수를 리턴하면 된다.  (클로저)



ex) 부모함수에서 내부함수를 리턴

```javascript
function parent(){
    var a = 1;
    
    var child = function (){
        console.log(a);
    }
    
    return child;  // 함수 리턴
}

var test = parent();
test();  // 1 출력
```



