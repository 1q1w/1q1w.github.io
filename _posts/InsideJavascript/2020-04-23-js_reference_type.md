---
layout: single
title: "[자바스크립트] 참조 데이터 타입 (객체, 배열)"
categories:
  - javascript
toc: true
toc_sticky: true
toc_label: "On this Page"
date: 2020-04-23
---



이번 시간에는 자바스크립트 데이터타입 중 참조 데이터타입, 

그 중에서도 자바스크립트의 객체와 배열에 대해 알아보자.



**자바스크립트 참조 데이터 타입은 아래와 같다.**

객체 (Object)

- 배열 (Array)
- 함수 (Function)
- 정규표현식



**자바스크립트에서 기본 타입을 제외한 모든 값은 객체이다.**

- 객체는 Key : Value 형태로 데이터를 저장하는 컨테이너 역할
- Hash 자료구조와 유사한 형태
- 기본 타입은 하나의 값만 가질 수 있지만 참조 타입인 객체는 여러 프로퍼티 저장 가능
- 객체의 프로퍼티는 함수를 포함할 수 있으며,  이러한 함수는 메서드라 부른다.



#### 1. 객체 생성

자바스크립트 객체를 생성하는 방법은 크게 세가지가 있다.



1) Object() 생성자 함수 사용

- 자바스크립트 내장 Object() 생성자 함수를 활용

```javascript
var coffee = new Object();// Object() 생성자 함수를 통해 객체를 생성

// 객체 프로퍼티 생성
coffee.type = 'latte';
coffee.size = 'small';
coffee.price = 3500;

console.log(typeof coffee);  // object 출력
console.log(coffee);  // type:'latte', size:'small', price:3500 출력
```



2) 객체 리터럴 방식 이용

- 리터럴이란 용어는 "표기법" 을 의미
- 객체 리터럴 방식은 { } 중괄호를 이용해서 객체를 생성
- 중괄호 안에 Key : Value 형태로 프로퍼티 추가
- Value 가 함수일 경우 이를 메서드라 부름

```javascript
// 객체 리터럴 방식으로 coffee 객체 생성
var coffee = {
    type: 'latte',
    size: 'small',
    price: 3500
};

console.log(typeof coffee);  // object 출력
console.log(coffee);  // type:'latte', size:'small', price:3500 출력
```



3) 생성자 함수

이건 나중에 더 자세히 알아보자





#### 2. 객체 프로퍼티에 접근

[] 대괄호 또는 . 마침표를 통해 객체의 프로퍼티에 접근이 가능하다.

- 값을 할당하는 경우, 
  - 프로퍼티가 이미 있는 경우 해당 값이 갱신되지만
  - **프로퍼티가 없는 경우 새로운 프로퍼티가 동적으로 생성된 후 값이 할당됨**



예제를 살펴보자.

```javascript
// 객체 리터럴 방식으로 coffee 생성
var coffee = {
    type: 'latte'
};

// 대괄호 또는 마침표로 프로퍼티 읽기
console.log(coffee.type);  // latte 출력
console.log(coffee['type']);  // latte 출력

// 프로퍼티 갱신
coffee.type = 'americano';
console.log(coffee.type);  // americano 출력
coffee['type'] = 'latte again';
console.log(coffee.type);  // latte again 출력

// 프로퍼티 생성
coffee.price = 4000;
console.log(coffee.price)  // 4000 출력

// 대괄호 표기법을 사용해야 하는 경우
coffee['full-name'] = 'Americano and Latte';
console.log(coffee['full-name']);  // Americano and Latte 출력
console.log(coffee.full-name);  // NaN 출력
```

  위 예제에서 coffee.full-name 으로 접근하는 경우 Nan 이 출력된다.  이유는 coffee.full 에 마이너스 연산으로 name 을 빼려고 하여 NaN (Not a Number) 이 출력된 것이다.  (표현식으로 인식)

  따라서 **접근하려고 하는 프로퍼티가 표현식이나 예약어**일 경우는 **대괄호 표기법**을 사용해야 한다





#### 3. for in 문을 통한 객체 프로퍼티 출력

for in 문을 사용하면 객체 내의 모든 프로퍼티에 대해 루프를 수행할 수 있다.



```javascript
var coffee = {
    type: 'latte',
    size: 'small',
    price: 3500
};

// for in 문을 통한 객체 프로퍼티 출력
for(var key in coffee){
    console.log(key, coffee[key]);
}

/* 위 for in 문의 결과는 아래와 같다 */
// type latte
// size small
// price 3500
```

 

 

#### 4. 객체 프로퍼티 삭제

**delete 연산자를 사용해 객체의 프로퍼티를 삭제**할 수 있다.  하지만 **객체 자체는 삭제 할 수 없다**.

 

```javascript
var coffee = {
    type: 'latte',
    size: 'small'
};

delete coffee.type;  // type 프로퍼티 삭제 시도
console.log(coffee.type);  // undefined 출력

delete coffee;  // coffee 객체 삭제 시도
console.log(coffee.size);  // small 출력
```

 

 

***

#### 자바스크립트 배열

- 배열은 대괄호를 사용하여 표기
- 객체에서는 프로퍼티명으로 접근했지만, 배열은 인덱스를 통해 배열의 요소에 접근 가능.
- 인덱스는 0부터 시작

```javascript
var coffeeType = ["latte", "americano"];
console.log(coffeeType[0]);  // latte 출력
console.log(coffeeType[1]);  // americano 출력
```



배열은 값을 순차적으로 넣을 필요 없이 아무 인덱스에나 값을 넣을 수 있다.

```javascript
var coffeeType = [];

coffeeType[2] = 'latte';
coffeeType[5] = 'americano';

console.log(coffeeType);  // emptyx2, latte, emptyx2, americano 출력
console.log(coffeeType.length);  // 6 출력. 가장 큰 인덱스가 5이므로 배열의 크기가 6이 되었음.
```

1) 자바스크립트 배열의 크기는 현재 가장 큰 인덱스를 기준으로 정해진다.

2) 모든 배열은 length 프로퍼티가 있으며 이는 배열의 원소 개수를 나타낸다. 



#### 배열의 length

1) length 프로퍼티는 배열 내 가장 큰 인덱스에 1을 더한 값이다.

2) 실제 메모리는 length 크기처럼 할당되지는 않는다.

3) 명시적으로 length의 값을 변경할 수 있다.

```javascript
var num = [0, 1];

num.length = 5;  // 배열의 길이 5로 변경
```





#### 배열도 객체

배열도 객체지만 일반 객체와는 차이가 있다.

```javascript
var testArray = [1, 2, 3];
var testObj = {
    '0' : 1,
    '1' : 2,
    '2' : 3
};

console.log(typeof testArray);  // object 출력
console.log(typeof testObj);  // object 출력

console.log(testArray.length);  // 3 출력
console.log(testObj.length);  // undefined 출력
```

  위 예제에서 testArray와 testObj 객체는 유사해보인다.  하지만 length 프로퍼티는 배열인 testArray 객체에만 존재한다.  따라서 testObj.length 는 undefined 가 출력된다.



정리하자면, **객체 리터럴 방식으로 생성한 객체는 Object.prototype 객체가 프로토타입이며, 배열은 Array.prototype 객체가 프로토타입**이 된다.  Array.prototype 객체는 push(), pop() 같이 배열에서 사용할 수 있는 표준메서드를 내장하고 있으며 **Array.prototype 객체의 프로토타입은 Object.prototype 객체**이다.

```javascript
var emptyArray = [];
var emptyObj = {};

console.dir(emptyArray);  // Array.prototype 출력
console.dir(emptyObj);  // Object.prototype 출력
```





#### 배열의 프로퍼티 생성

배열도 객체이므로 인덱스가 숫자인 배열의 원소 외에도 일반 객체처럼 동적으로 프로퍼티 생성이 가능하다.  단 프로퍼티를 추가해도 length는 변하지 않는데 length는 배열 원소의 가장 큰 인덱스가 변했을 경우에만 변경된다.

```javascript
var arr = [0, 1];
arr.name = 'Test';

console.log(arr.length);  // 2 출력
```





#### 배열의 프로퍼티 열거

배열도 객체이므로 for in 문을 이용해 열거가 가능하다.  하지만 이 경우 배열의 요소 외 다른 프로퍼티들까지 열거될 수 있어 배열의 요소들만 출력하려면 for 문을 사용하는 것이 좋다.

```javascript
var tempArr = [0, 1, 2];
tempArr.name = 'temp';

for (var temp in tempArr){
    console.log(temp, tempArr[temp]);
}  // for in 문을 사용하는 경우 name 프로퍼티까지 출력

for(var i = 0; i < tempArr.length; i++){
    console.log(i, tempArr[i]);
}  // 인덱스를 이용해 배열의 요소만 출력
```





#### 배열의 요소 삭제

배열도 객체이므로 delete를 사용해 요소를 제거할 수 있다.  하지만 이 경우 배열의 length 값은 변하지 않는 문제가 있다.  따라서 배열의 요소를 삭제할때는 splice() 배열 메서드를 사용하도록 하자.

* splice(start, deleteCount, item...)

```javascript
var arr = [0, 1, 2, 3, 4, 5];

arr.splice(2, 1);  // 2번째 요소에서 1개를 삭제
console.log(arr);  // 0, 1, 3, 4, 5 출력
console.log(arr.length);  // 5 출력
```





#### Array() 생성자 함수

배열 리터럴로 배열을 생성하는 과정은 Array() 생성자 함수로 배열을 생성하는 방식을 단순화 한 것이다.

- 인자가 1개이고, 숫자인 경우 해당 인자를 length로 갖는 빈 배열 생성
- 인자가 n개인 경우 해당 인자를 요소로 갖는 배열 생성

```javascript
var arr = new Array(5);
console.log(arr);  // emptyx5 출력
console.log(arr.length); // 5 출력
```





#### 유사 배열 객체

배열에 있는 length 프로퍼티를 일반 객체에서도 가지고 있다면 어떻게 될까?  **자바스크립트에서는 length 프로퍼티를 가진 객체를 유사 배열 객체** 라고 부른다.

추후 살펴볼 apply() 메서드를 사용하면 유사 배열 객체지만 배열의 메서드를 사용하는것이 가능하다.  유사 배열 객체도 배열의 메서드를 사용하는것이 가능하다 정도로 이해하고 아래의 예제를 살펴보고 넘어가도록 하자.

```javascript
var arr = [0, 1, 2];
var obj = {name:'number'};

arr.push(3);
//obj.push(3);  // error 발생

Array.prototype.push.apply(obj, ['3']);
console.log(obj);  // 0: '3', name: 'number', length: 1  출력
```




