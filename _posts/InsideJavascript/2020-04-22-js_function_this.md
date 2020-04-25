---
layout: single
title: "[자바스크립트] 함수 호출과 arguments, this"
categories:
  - javascript
tag:
  - arguments
  - this
toc: true
toc_sticky: true
toc_label: "On this Page"
date: 2020-04-22
---



이번 시간에는 함수 호출과 arguments, this에 대해 알아보자.



#### 1. arguments 객체

자바스크립트는 함수를 호출할 때 함수 형식에 맞춰 인자를 넘기지 않더라도 에러가 발생하지 않는다.  필요한 인자 수보다 부족하게 넘어간 경우 undefined 값이 할당된다.  초과하는 인자는 무시된다.

ex) 함수 형식에 맞춰 인자를 넘기지 않아도 함수 실행은 된다.

```javascript
function test(arg1, arg2){
    console.log(arg1, arg2);
}

test();  // undefined undefined 출력
test(1);  // 1 undefined 출력
test(1, 2);  // 1 2 출력
test(1, 2, 3);  // 1 2 출력
```



자바스크립트에서는 함수를 호출할 때 암묵적으로 arguments 객체가 함수 내부로 전달된다.  arguments 객체에는 함수 호출시 전달되는 인자들이 배열 형태로 저장된다.  하지만 **arguments 객체**는 실제 배열이 아닌 **유사배열객체**라는 점에 주의해야 한다.  아래 예시를 실행해보자.

```javascript
function test(arg1, arg2){
    // arguments 객체 출력
    console.dir(arguments);
    
    console.log(arg1 + arg2);
}

test();  // NaN 출력
test(1);  // NaN 출력
test(1, 2);  // 3 출력
test(1, 2, 3);  // 3 출력
```

결과는 아래와 같다.

![Alt text](/assets/images/js_5_arguments.jpg "function_arguments")



arguments 객체는 함수 호출시 넘긴 인자를 배열 형태로 저장한다. (배열이 아닌 유사배열객체이다.)

- 함수 호출시 넘겨진 인자는 배열 형태로 저장된다.
- length 프로퍼티는 인자 개수를 의미한다.
- callee 프로퍼티는 현재 실행중인 함수의 참조값을 의미한다.



arguments 객체를 활용하면 매개변수 개수가 정확히 정해지지 않거나, 매개변수 개수에 따라 서로 다른 처리를 해야 하는 함수를 구현할 때 유용하다.

ex) 매개변수가 정해지지 않은 함수의 예

```javascript
// 입력 받은 매개변수를 모두 더하는 함수
function allSum(){
    var result = 0;
    
    for(var i = 0; i < arguments.length; i++){
        result += arguments[i];
    }
    
    console.log(result);
}

allSum(2, 5, 7);  // 14 출력
allSum(2, 5, 7, 2, 3, 54, 4);  // 77 출력
```





#### 2. this 바인딩

함수를 호출할 때 arguments 객체에 매개변수가 저장되어 함수 내부로 전달된다고 했는데, 같이 전달되는 인자가 하나 더 있다.  바로 **this 인자**이다.

this는 굉장히 중요하면서도 어려운 개념이다.  this가 이해하기 어려운 이유는 **함수가 호출되는 방식**에 따라 **this가 다른 객체를 참조 (this 바인딩)**하기 때문이다.  차근차근 같이 이해해보도록 하자.



##### 2_1) 객체의 메서드 호출시 this 바인딩

객체의 프로퍼티가 함수일 경우 이를 메서드라 부른다.  메서드를 호출하는 경우, 메서드 내부에서 사용된 this 에는 **메서드를 호출한 객체가 this 로 바인딩** 된다.

```javascript
var bonobono = {
    name: 'bonobono',
    sayName: function (){
        console.log(this.name);
    }
};

var porori = {
    name: 'porori'
};

porori.sayName = bonobono.sayName;

bonobono.sayName();  // bonobono 출력
porori.sayName();  // porori 출력
```

1) bonobono와 porori의 sayName() 메서드 참조값은 같다.

2) bonobono에서 sayName() 호출시 this는 bonobono 객체가 된다.

3) porori에서 sayName() 호출시 this는 porori 객체가 된다.



위 예제에서 보다시피 **this는 자신을 호출한 객체에 바인딩**된다.



##### 2_2) 함수 호출시 this 바인딩

**함수를 호출하는 경우, this는 전역 객체에 바인딩**된다. 

만약 브라우저에서 실행하는 경우라면 전역객체는 window 객체가 된다.  window 객체에 Node.js와 같은 자바스크립트 런타임 환경에서의 전역객체는 global 객체가 된다.   이러한 부가적인 정보는 따로 시간내어 찾아보기 바란다.  (나중에 다룰건데 언제 다룰지는 미정)

자바스크립트의 모든 전역변수는 전역 객체의 프로퍼티에 해당된다.  아래 예제를 살펴보자.

```javascript
var name = 'bonobono';

console.log(name);  // bonobono 출력
console.log(window.name);  // bonobono 출력
```

따라서 전역변수는 전역객체의 프로퍼티로도 접근이 가능하다.



함수 호출시에는 this가 어떻게 바인딩되는지 살펴보자.

```javascript
var name = 'bonobono';

var sayName = function(){
    console.log(this.name);  // this는 전역객체에 바인딩된다.
};

sayName();  // bonobono 출력
```





내부함수를 호출하는 경우를 살펴보자.

```javascript
var num = 100;

var test = {
    num: 1,
    addNum1: function (){
        this.num += 1;
        console.log(this.num);  // 2단계
		
        // 내부함수
        addNum2 = function(){
            this.num += 1;
                console.log(this.num);  // 4단계
			
            // 내부함수
            addNum3 = function(){
                this.num += 1;
                console.log(this.num);  // 6단계

            }
            addNum3();  // 5단계. 내부함수 호출
        }
        addNum2();  // 3단계. 내부함수 호출
    }
}

test.addNum1();  // 1단계.  메서드 호출
```

1단계) test 객체의 addNum1() 메서드 호출.  이 때 this는 자신을 호출한 test 객체가 됨. 

2단계) this는 test 객체이므로  this.num 은 2

3단계) addNum2() 함수 호출.  이 때 this는 함수 호출 규칙에 따라 전역객체에 바인딩.  window 객체가 this가 됨

4단계) window 객체의 num은 100이므로 4단계의 this.num은 101

5단계) 3, 4단계와 동일.  this에 window 객체가 바인딩됨.



**함수를 호출했냐, 메서드를 호출했냐**에 따라 this 바인딩이 달라지는 점 꼭 기억하도록 하자.



 위 예제에서 전역변수에 접근을 하지 않고 내부변수를 활용할 수 있는 방법은 없을까?

```javascript
var num = 100;

var test = {
    num: 1,
    addNum1: function (){
        var self = this;
        
        this.num += 1;
        console.log(this.num);  // 2단계
		
        // 내부함수
        addNum2 = function(){
            self.num += 1;
            console.log(self.num);  // 4단계
			
            // 내부함수
            addNum3 = function(){
                self.num += 1;
                console.log(self.num);  // 6단계

            }
            addNum3();  // 5단계. 내부함수 호출
        }
        addNum2();  // 3단계. 내부함수 호출
    }
}

test.addNum1();  // 1단계.  메서드 호출
```

 위의 예제를 보면 addNum1() 메서드 안에 self 변수에 this를 저장해서 내부함수에서 self 변수를 사용하고 있다.  내부함수는 자신을 둘러싼 부모함수의 변수에 접근이 가능하다는 특징을 활용한 것이다.  따라서 위와 같이 처리하면  addNum()의 self에 저장된 this를 내부함수에서 사용할 수 있다.



 자바스크립트에서는 이러한 this 바인딩을 원활하게 하기 위해 call과 apply 메서드를 제공하고 있다.  또한 jQuery 같은 자바스크립트 라이브러리에서는 bind라는 이름의 메서드를 통해 원하는 객체를 this에 바인딩 할 수 있는 기능을 제공하고 있다.  call과 apply는 이번 장에서, bind는 조금 더 뒤에서 살펴보도록 하자.



##### 2_3) 생성자 함수 호출시 this 바인딩

 자바스크립트에서는 **기존 함수에 new 연산자를 붙여서 호출하면 생성자 함수로 동작**한다.  자바스크립트 스타일 가이드에서는 생성자 함수를 구분하기 위해 생성자 함수는 함수 이름의 첫 글자를 대문자로 쓰기를 권하고 있다.  



우선 생성자 함수에 대해 알아보자.

ex) 생성자함수 사용 예제

```javascript
// Animal 생성자 함수 생성
var Animal = function(name){
    this.name = name;
}

// bonobono 객체 생성
var bonobono = new Animal('bonobono');
console.log(bonobono.name);
```

1) 생성자 함수가 실행되기 전 빈 객체가 생성되고 해당 객체가 this로 바인딩된다.  따라서 this는 빈 객체를 의미한다.  // 정확히 말하면 빈 객체는 아니다.  모든 객체는 자신의 부모인 프로토타입과 연결되어있기 때문이다.  생성자 함수가 생성한 빈 객체는 자신을 생성한 생성자 함수의 prototype 프로퍼티가 가리키는 객체를 자신의 프로토타입 객체로 설정한다.  

2) 함수 내부에서 this를 통해 생성된 빈 객체에 동적으로 프로퍼티나 메서드를 생성할 수 있다.

3) 생성자 함수의 특징은 리턴이 없는 경우 this로 바인딩된 새로 생성한 객체가 리턴된다.  명시적으로 this를 리턴한것과 같은 결과이다.   생성자 함수가 아닌 일반 함수를 호출할 때 리턴값이 명시되어 있지 않으면 undefined가 리턴된다.  생성자 함수를 호출했더라도 리턴값이 this가 아닌 다른 객체를 반환하는 경우 해당 객체가 리턴된다.  즉 리턴값이 없거나, 리턴값이 this인 경우 생성자 함수 역할에 맞게 동작할 수 있다.  (함수 호출시 new 연산자 여부에 따라 생성자인지 일반함수인지 인식한다.)



 리터럴 방식으로 생성된 객체는 같은 형태의 차이는 무엇일까?

첫째, 리터럴 방식으로 생성된 객체는 재사용이 불가능한데, 생성자 함수를 사용하면 재생성이 자유롭다.

둘째, 리터럴 방식으로 생성된 객체의 [[Prototype]]은 객체 생성자 함수 Object()지만 생성자 함수의 경우 해당 함수 그 자체가 된다.

```javascript
// Animal 생성자 함수 생성
var Animal = function(name, size, age){
    this.name = name;
    this.size = size;
    this.age = age;
}

// 여러 객체 생성
var bonobono = new Animal('bonobono', 'middle', 15);
var porori = new Animal('porori', 'small', 16);
console.dir(bonobono);
console.dir(porori);
```

 

생성자 함수를 new를 붙이지 않고 호출하는 경우 일반 함수로 인식한다.  객체 생성을 목적으로 만든 생성자함수를 new 없이 호출하거나 일반 함수를 new를 붙여서 호출하는 경우 오류가 발생할 수 있다.

 this가 바인딩되는 방식의 차이때문인데 일반 함수 호출의 경우 this는 window(전역 객체)에 바인딩되지만, 생성자 함수의 경우 새로 생성되는 빈 객체에 this가 바인딩되기 때문이다.

ex) 생성자 함수를 new 없이 호출하는 경우

```javascript
// Animal 생성자 함수 생성
var Animal = function(name, size, age){
    this.name = name;
    this.size = size;
    this.age = age;
}

// 생성자 함수를 new 없이 호출
var bonobono = Animal('bonobono', 'middle', 15);  // this는 전역객체로 바인딩
// console.log(bonobono.name);  // 오류 발생. bonobono 객체에는 name이 없음
console.log(window.name);  // bonobono 출력
console.log(this.name);  // bonobono 출력
```



위와 같은 현상을 피하기 위해 아래와 같이 강제로 객체를 생성시키기도 한다.

```javascript
function Animal(name){
    // this가 Animal의 객체가 아니면
    if(!(this instanceof Animal)){
       // 생성자 함수로 다시 호출하여 이를 반환
       return new Animal(name)  
	}
    this.name = name;
}

var bonobono = Animal('bonobono');
console.log(bonobono.name);
```

혹은 아래와 같이 쓰기도 한다.

```javascript
// arguments.callee는 호출된 함수를 가리킨다. 
// 함수 이름과 상관없이 모든 생성자 함수가 공통된 아래 내용을 가질 수 있다.
if(!(this instanceof arguments.callee)){
   
}
```





##### 2_4) 자바스크립트 call과 apply를 이용한 this 바인딩

자바스크립트는 특정 객체에 **this를 명시적으로 바인딩**하는 방법도 제공하고 있다.  바로 call() 과 apply() 메서드이다.  모든 함수는 Function.prototype 객체의 메서드이므로 모든 함수는 아래의 형식으로 메서드를 호출하는 것이 가능하다.

우선 apply() 메서드에 대해 알아보자.

ex) apply() 메서드 예시

```javascript
sayName.apply(thisArg, argArray);
```

- apply()의 본질적인 기능은 함수 호출이다.  예를들어 sayName() 라는 함수가 있는데 sayName.apply() 처럼 사용한다면 이것은 sayName() 함수를 호출하는 것이다.
- 첫번째 인자 thisArg는 apply() 메서드를 호출한 함수 내부에서 사용할 this에 바인딩할 객체를 가리킨다.
- 두번째 인자 argArray는 함수 호출시 넘길 인자들의 배열을 가리킨다.

```javascript
// 생성자함수 Animal
function Animal (name, age, color){
    this.name = name;
    this.age = age;
    this.color = color;
    console.log('실행됨');
}

var bonobono = {};
Animal.apply(bonobono, ['bonobono', 12, 'blue']);  // 실행 출력
console.dir(bonobono);  // bonobono는 생성자함수로 생성되어 Object 상속
```



call() 메서드는 apply() 객체에서 argArray 배열 부분을 배열이 아닌 각각 하나의 인자로 넘긴다는 차이가 있다.

```javascript
sayName.call(thisArg, arg1, arg2, ...)
```



위 예시를 call() 메서드로 변경하면 아래와 같다.

```javascript
// 생성자함수 Animal
function Animal (name, age, color){
    this.name = name;
    this.age = age;
    this.color = color;
    console.log('실행됨');
}

var bonobono = {};
Animal.call(bonobono, 'bonobono', 12, 'blue');  // 실행 출력
console.dir(bonobono);  // bonobono는 생성자함수로 생성되어 Object 상속
```



이렇듯 call()과 apply()를 사용하면 this를 원하는 값으로 매핑해서 특정 함수를 호출할 수 있다.  가장 대표적인 예가 **유사배열객체를 배열**로 만드는 방법이다.

유사배열객체는 배열이 아니므로 배열의 메서드를 사용할 수 없다.  하지만 아래와 같은 방법으로 배열로 변환하고 나면 배열의 메서드를 사용할 수 있다.

```javascript
function createArr(){
    console.dir(arguments);  // arguments 객체 출력
    
    var arr = Array.prototype.slice.apply(arguments);  // 배열로 변환
    console.dir(arr);  // array 객체 출력
}

createArr(1, 2, 3);
```

배열로 변환하는 과정은 다음과 같다.

- Array.prototype.slice() 메서드에 apply()를 사용하여 this를 arguments 객체로 바인딩
- slice() 메서드는 배열 객체만 사용할 수 있지만, this를 arguments 객체로 바인딩하여 배열이 아님에도 배열처럼 사용하도록 함
- arguments는 객체이므로 [[Prototype]]은 Object.prototype
- arr는 배열이므로 [[Prototype]]은 Array.prototype





#### 3. 함수 리턴 규칙

자바스크립트의 함수는 항상 리턴값(반환하는 값)이 있다.

- 일반 함수나 메서드에서 리턴값을 지정하지 않는 경우 undefined 리턴

- 생성자 함수에서 리턴값을 지정하지 않는 경우 생성된 객체 리턴.
  - 생성된 객체가 아닌 다른 객체를 리턴하는 경우 해당 객체 리턴
  - 객체가 아닌 숫자, 문자 등을 명시적으로 리턴하는 경우 이를 무시하고 this로 바인딩된 객체가 리턴





