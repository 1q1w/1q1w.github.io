---
layout: single
title: "자바스크립트 데이터 타입 (참조 타입)"
categories:
  - javascript
toc: true
toc_sticky: true
toc_label: "On this Page"
date: 2020-04-19
---

### 참조 데이터 타입 (객체)



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

   

### 참조 타입의 특성

#### 1.  참조값 복사 (얕은복사)

아래 예제를 우선 살펴보자.



```javascript
var latte ={
    price: 4000
};
var americano = latte;

console.log(latte.price);  // 4000 출력
console.log(americano.price);  // 4000 출력

americano.price = 5000;
console.log(latte.price);  // 5000 출력
console.log(americano.price);  // 5000 출력
```

1) latte 변수는 객체 자체를 저장하고 있는것이 아닌 생성된 객체를 가리키는 참조값을 저장한다.

2) americano에 latte 변수를 할당했을 때 americano에는 latte에 저장된 객체의 참조값이 저장된다.

3) 즉, 두 개의 변수가 같은 객체를 바라보고 있어 위와 같은 출력값이 나타나게 된다.

 

위와 같이 참조값 복사가 아닌 실제 데이터를 복사하고 싶을 경우 어떻게 하면 될까?

방법은 여러가지가 있겠지만 아래 예시를 보자.

```javascript
var latte ={
    price: 4000
};
var americano = JSON.parse(JSON.stringify(latte));
americano.price = 5000;

console.log(latte.price);  //4000 출력
console.log(americano.price);  //5000 출력
```

1) JSON.stringify() 메서드로 객체의 데이터를 JSON 형태의 문자열로 복사한 뒤,

2) JSON.parse() 메서드로 다시 객체로 만드는 방법이다.



깊은복사를 하는 방법은 여러가지가 있겠지만 객체는 참조값이 복사된다는 특성을 기억하면서 일단 넘어가도록 하자.





#### 2. 객체 비교

동등 연산자를 사용하여 **객체를 비교하는 경우에도 참조값을 비교**한다.

아래 예제를 살펴보자.



```javascript
// 기본 타입 변수
var lattePrice = 4000;
var americanoPrice = 4000;

// 참조 타입 변수
var latte = { price:4000 };
var americano = { price:4000 };
var testCopy = americano;

console.log(lattePrice == americanoPrice);  // true 출력
console.log(latte == americano);  // false 출력
console.log(americano == testCopy);  // true 출력
```

1) lattePrice와 americanoPrice는 기본 타입이므로 값을 직접 비교한다.

2) latte와 americano는 참조 타입이므로 참조값을 비교한다. 참조값이 다르므로 비교결과는 false이다.

3) americano와 testCopy는 참조값이 일치하므로 비교결과 true이다.





#### 3. 값에 의한 호출 (Call By Value) / 참조에 의한 호출 (Call By Reference)

```javascript
var a = 1;
var b = {value : 2};

function ChangeValue (a, b){
    a = 100;  // 값이 복사된 상태
    b.value = 200;  // 참조값이 복사된 상태
    
    console.log(a);  
    console.log(b.value);  
}

ChangeValue(a, b);
console.log(a);
console.log(b.value);
```



위 예제의 결과는 다음과 같다

```javascript
100
200
1
200
```

1) 함수 호출시 기본 타입 변수를 넘기는 경우, 호출된 함수의 매개변수로 **복사된 값**이 전달된다.  변수 a를 넘기는 경우 a가 그대로 넘어가는것이 아니라 a에 저장된 값이 복사되어 함수에 전달된다.  이를 **값에 의한 호출 (Call By Value)**라 한다.

2) 함수 호출시 참조 타입 변수를 넘기는 경우, 호출된 함수의 매개변수로 **복사된 참조값**이 전달된다.  변수 b를 넘기는 경우 해당 변수의 참조값이 전달되면서 b.value를 변경하는 경우 참조값이 직접 변경된다.  이를 **참조에 의한 호출(Call By Reference)**라 한다.





***

#### 프로토타입

모든 객체는 부모 역할을 하는 객체와 연결되어있는데 이 때 부모 객체를 **프로토타입**이라 부른다.



아래 예제를 직접 실행시켜보자.

```javascript
var coffee={
    price:4000
};

console.dir(coffee);
```



크롬 브라우저에서 이를 실행하면 아래와 같이 proto 프로퍼티를 볼 수 있다.

![Alt text](/assets/images/prototype.jpg "prototype")



proto 프로퍼티는 coffee 객체의 부모인 프로토타입 객체를 가리킨다.  coffee 객체의 프로토타입은 Object.Prototype()이며 여기에 포함된 다양한 내장 메서드를 자신의 것처럼 상속받아 사용할 수 있게 된다.

**모든 객체는 자신의 프로토타입을 가리키는 Prototype라는 숨겨진 프로퍼티를 가지고, 객체 리터럴 방식으로 생성된 객체는 Object.prototype 객체가 프로토타입 객체**가 된다는것을 기억하자.

프로토타입 객체는 임의의 다른 객체로 변경하는것도 가능하다.  프로토타입에 대해서는 추후 프로토타입 체이닝에서 더 자세히 살펴보도록 하자.





***

### 배열

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





#### 연산자

- 더하기 (+) 연산자

  두 요소가 숫자일 경우 더하기 연산이 이뤄지고, 그 외의 경우 문자열 연결 연산이 이뤄진다.

- typeof() 연산자

  피연산자의 타입을 문자열 형태로 리턴한다.

  null과 배열은 object, 함수는 function 이라는 점만 유의하자.

- == 연산자와 === 연산자

  == 연산자는 타입이 다를 경우 타입변환 후 데이터를 비교한다

  === 연산자는 타입이 다를 경우 그냥 비교한다.

```javascript
// == 와 === 의 차이
var a = 1;
var b = '1';

console.log(a == b);  // 타입 변환 후 true 출력
console.log(a === b);  // false 출력
```

- !! 연산자

  피연산자를 bollean 값으로 변환하는 역할을 한다

  숫자 0, 공백, false, null, undefined 등은 false로 반환되고 그 외 값이 있는 것들은 true로 반환된다.  하지만 객체는 빈 객체라도 true로 반환됨을 유의하자.  (빈 배열도 객체이므로 빈 배열도 true로 반환된다.)




