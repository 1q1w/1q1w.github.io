---
layout: single
title: "[자바스크립트] 객체의 특징, 특성"
categories:
  - javascript
toc: true
toc_sticky: true
toc_label: "On this Page"
date: 2020-04-27
---



이번 시간에는 자바스크립트 객체 특징, 특성에 대해 알아보자.



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

#### 번외로 연산자에 대해

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




