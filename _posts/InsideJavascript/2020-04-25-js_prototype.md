---
layout: single
title: "[자바스크립트] 프로토타입과 프로토타입 체이닝"
categories:
  - javascript
toc: true
toc_sticky: true
toc_label: "On this Page"
date:2020-04-24
---



이번 시간에는 자바스크립트 프로토타입과 프로토타입 체이닝에 대해 알아보자.



***

#### 1. 프로토타입이란

**자바스크립트는 프로토타입을 기반으로 객체지향 프로그래밍 구현**을 지원한다.  

- 자바스크립트의 모든 객체는 자신의 부모인 프로토타입 객체를 가리키는 참조 링크 형태의 숨겨진 프로퍼티가 존재한다.
- ECMAScript에서는 이를 암묵적 프로토타입 링크라 부르며 이는 [[Prototype]] 프로퍼티에 저장된다.



**주의사항**

prototype 프로퍼티와 [[Prototype]] 프로퍼티를 구분할 수 있어야 한다.

- 모든 객체는 자신을 생성한 생성자 함수의 prototype 프로퍼티 객체를 자신의 부모를 가리키는 [[Prototype]] 링크로 연결한다.

아래 예제를 크롬 개발자도구에서 실행시켜보자.

```javascript
function Animal(name){
    this.name = name;
}

var bonobono = new Animal('bonobono');
console.dir(Animal);
console.dir(bonobono);
```

결과는 아래와 같다

![prototype]("/assets/image/js_7_prototype");

- 참고_ 크롬에서는 [[Prototype]]을 __ proto __ 로 표기한다.

- Animal 은 생성자 함수로 prototype 프로퍼티에 자기 자신을 담고 있다.  자신을 통해 객체가 생성되는 경우 새로 생성된 객체의 [[Prototype]]에는 Animal 의 prototype이 링크된다.
- bonobono는 Animal 생성자 함수로 생성되었다.  생성되는 과정에서 Animal 함수의 prototype이 bonobono의 [[Prototype]]에 링크된다.
- Animal의 prototype 프로퍼티와 bonobono의 [[Prototype]] 프로퍼티는 같은 객체를 가리킨다.





#### 2. 프로토타입 체이닝

##### 1) 객체 리터럴 방식으로 생성된 객체의 프로토타입 체이닝

자바스크립트 객체는 자기 자신의 프로퍼티뿐 아니라 자신의 부모의 프로퍼티에 접근이 가능하다.  이것을 가능하게 하는것이 **프로토타입 체이닝**이다.

**프토토타입 체이닝이란, 특정 객체의 프로퍼티나 메서드에 접근하려고 할 때 해당 객체에 프로퍼티나 메서드가 없다면 [[Prototype]] 링크에 연결된 자신의 부모의 프로퍼티나 메서드를 검색하는것을 의미한다.**

아래 예제를 한번 살펴보자.

```java
var Animal = {
    name: 'bonobono',
    sayName: function(){
        console.log(this.name);
    }
};

Animal.sayName();  // bonobono 출력
console.log(Animal.hasOwnProperty('name'));  // true 출력
console.log(Animal.hasOwnProperty('age'));  // false 출력
```

hasOwnProperty() 메서드는 인자로 넘긴 문자열에 해당하는 프로퍼티나 메서드가 있는지 체크하는 자바스크립트 표준 API이다.  name 프로퍼티는 있지만 age 프로퍼티는 없어 false가 출력된다.

그런데 자세히 살펴보니 이상한 점이 있다.  Animal 객체에는 hasOwnProperty()라는 메서드가 없는데 어떻게 오류가 발생하지 않은걸까?

이유는 다음과 같다.

- Animal 객체는 객체 리터럴 방식으로 생성되었다.  이는 Object() 내장 생성자 함수로 생성된 것이다.
- 객체 리터럴 방식으로 생성된 객체의 [[Prototype]]은 Object.prototype이다.
- Object.prototype 객체에는 hasOwnProperty() 메서드가 존재한다.
- Animal 객체에서 hasOwnProperty() 메서드가 없어 [[Prototype]] 객체에 있는 hasOwnProperty() 메서드를 사용하게 된다.
- 만약 Animal 객체에 hasOwnProperty() 메서드가 있었다면 해당 메서드가 실행된다.
- 참고_ 모든 객체는 결국 Object.prototype과 연결되어있다.  따라서 모든 객체는 toString(), hasOwnProperty() 등의 메서드를 사용할 수 있다.



##### 2) 생성자 함수로 생성된 객체의 프로토타입 체이닝

생성자 함수로 생성된 객체의 프로토타입 체이닝은 객체 리터럴 방식으로 생성된 객체의 프로토타입 체이닝과는 약간의 차이가 있다.  하지만 **모든 객체는 자신을 생성한 객체의 prototype을 자신의 [[Prototype]] 으로 설정한다**는 주요 개념은 동일하다.

```javascript
function Animal(name) = {
    this.name = name;
};

var bonobono = new Animal('bonobono');  // 객체 생성
console.log(bonobono.hasOwnProperty('name'));  // true 출력
```

위 예제에서 hasOwnProperty() 메서드를 호출했을때 오류가 발생하지 않았다.

- bonobono 객체의 [[Prototype]] 은 Animal.prototype 객체이다.
- Animal.protytpe 객체에는 hasOwnProperty() 메서드가 없다.
- Animal.prototype 객체의 [[Prototype]]은 Object.prototype이다.
- Object.prototype에는 hasOwnProperty() 메서드가 있고 이를 사용한다.



위와 같이 **프로토타입 체이닝은 계속해서 이뤄지고 그 종착지는 Object.prototype 객체**가 된다.  **모든 객체는 Object.prototype에서 프로토타입 체이닝이 끝나게 된다.**





#### 3. 데이터 타입의 확장

모든 객체는 프로토타입 체이닝으로 Object.prototype에 정의된 메서드가 사용 가능하다.  이와 비슷하게 String.prototype, Array.prototye, Number.prototype 등에는 문자, 배열, 숫자 등에 사용되는 표준 메서드들이 정의되어 있다.

자바스크립트는 위와 같은 표준 내장 프로토타입 객체에도 사용자가 직접 추가적인 메서드를 정의하는 것을 허용하고 있다.

```javascript
String.prototype.sayName = function(){
    console.log('string.sayName()를 추가했다.');
};

var test = "bonobono";
test.sayName();  // string.sayName()를 추가했다.  출력

console.dir(String.prototype);  // sayName() 메서드가 추가된 것을 볼 수 있다.
```

위와 같이 String.prototype에 추가 메서드를 정의하고 이를 모든 문자열의 표준 메서드처럼 사용할 수 있다.





#### 4. 프로토타입도 객체

함수가 생성될 때 prototype 프로퍼티에 연결되는 프로토타입 객체는 기본적으로 constructor 프로퍼티만 가진 객체이다.  프로토타입 객체도 자바스크립트의 객체이므로 프로퍼티를 추가/제거 하는 것이 가능하다.  이렇게 변경된 프로퍼티는 실시간으로 프로토타입 체이닝에 반영된다.

ex) sayName() 메서드를 Animal.prototype 에 정의하여 이를 호출

```javascript
function Animal(name){
    this.name = name;
}

var bonobono = new Animal('bonobono');

// 프로토타입 객체에 메서드 정의
Animal.prototype.sayName = function(){
    return this.name;
}

// Animal.prototype의 sayName() 메서드 호출
console.log(bonobono.sayName());  // bonobono 출력
```





#### 5. 프로토타입 객체의 메서드와 this 바인딩

프로토타입 객체는 메서드를 가질 수 있다.  만약 프로토타입 객체의 메서드에서 this가 사용되면 이는 어디에 바인딩될까?  객체의 메서드를 호출할 때 this는 메서드를 호출한 객체가 this에 바인딩된다.  프로토타입 객체에서도 동일하다.

```javascript
function Animal(name){
    this.name = name;
}

var bonobono = new Animal('bonobono');

// 프로토타입 객체에 메서드 정의
Animal.prototype.sayName = function(){
    return this.name;
}

// Animal.prototype의 sayName() 메서드 호출
// 이 때 this는 bonobono
console.log(bonobono.sayName());  // bonobono 출력

Animal.prototype.name = 'Animal';
console.log(Animal.prototype.sayName());  // Animal 출력
```





#### 6. prototype 객체 변경

기본 prototype 객체는 다른 객체로 변경이 가능하다.

- prototype 객체 변경으로 객체지향의 상속을 구현할 수 있다.
- prototype 객체가 변경된 이후 생성된 객체들은 변경된 prototype으로 [[Prototype]]을 링크한다.
- 변경 이전에 생성된 객체는 기존 prototype으로의 링크를 유지한다.

예제를 살펴보자.

```javascript
// Animal 생성자 함수
function Animal (name){
    this.name = name;
}

console.log(Animal.prototype.constructor);  // 1. Animal(name) 출력

var bonobono = new Animal('bonobono');
console.log(bonobono.age);  // 2. undefined 출력

// Animal 생성자 함수의 prototype 객체 변경
Animal.prototype = {
    age: 16
};

console.log(Animal.prototype.constructor);  // 3. Object() 출력

// porori 객체 생성
var porori = new Animal('porori');
console.log(bonobono.age);  // 4. undefined 출력
console.log(porori.age);  // 5. 16 출력
console.log(bonobono.constructor);  // 6. Animal(name) 출력
console.log(porori.constructor);  // 7. Object() 출력
```

1)  Animal 생성자 함수 생성시 Animal.prototype 객체는 자신과 연결된 Animal(name) 생성자 함수를 가리키는 constructor 프로퍼티만 가진다.

2) bonobono 객체가 생성된 후 age 프로퍼티를 찾으려 하면 bonobono 객체 및 [[Prototype]] 에 age 프로퍼티가 없어 undefined가 출력된다.

3) Animal 생성자 함수의 prototype 객체를 변경했다.  Animal.prototype 객체에는 constructor 프로퍼티가 없다.  그런데 어떻게 출력되었을까?  Animal.prototype 객체를 리터럴 방식으로 생성하여 해당 객체의 [[Prototype]]은 Object.prototype이 된다.  Object.prototye의 constructor 프로퍼티는 Object() 생성자 함수이다.  프로토타입 체이닝을 통해 Animal.prototype.constructor은 Object()가 출력된다.

4) porori 객체를 생성하였고 이후 bonobono 객체와 porori 객체의 age를 출력하면 bonobono.age는 undefined가, porori.age 는 16이 출력된다.  bonobono 객체는 변경 이전의 Animal.prototype 객체를 바라보고 있고, porori 객체는 변경된 Animal.prototype 객체를 바라보고 있어 다른 결과가 출력된다.

5) bonobono 객체와 porori 객체의 constructor을 출력할때에도 동일하다.  Animal.prototype 변경 전 후에 따라 다른 결과가 출력된다.





#### 7. 프로토타입 체이닝 동작 시점

객체의 프로퍼티 읽기나 메서드 실행시에만 프로토타입 체이닝이 동작한다.  프로퍼테/메서드를 찾는데 해당 객체에 없으면 부모 객체에서 이를 찾게 된다.

반대로 객체에 값을 쓰려고 할때는 프로토타입 체이닝이 일어나지 않는다.  프로토타입 체이닝 없이 동적으로 해당 객체에 프로퍼티나 메서드를 추가하게 된다.

```javascript
// Animal 생성자 함수
function Animal (name){
    this.name = name;
}

Animal.prototype.color = 'blue';

var bonobono = new Animal('bonobono');
var porori = new Animal('porori');
console.log(bonobono.color);  // blue 출력
console.log(porori.color);  // blue 출력

porori.color = 'pink';
console.log(bonobono.color);  // blue 출력
console.log(porori.color);  // pink 출력
console.log(Animal.prototype.color);  // blue 출력
```

1) bonobono 객체와 porori 객체의 color을 출력하려 할 때는 해당 객체들에 color 프로퍼티가 없으므로 프로토타입 체이닝이 이뤄져 Animal.protytpe.color 이 출력된다.

2) porori 객체에 color 프로퍼티를 동적으로 할당한 뒤, porori.color을 출력하면 porori 객체에 color 프로퍼티가 있으므로 프로토타입 체이닝이 일어나지 않고 porori.color을 출력한다.



















