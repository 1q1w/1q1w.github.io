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

latte 변수는 객체 자체를 저장하고 있는것이 아닌 생성된 객체를 가리키는 참조값을 저장한다.

americano에 latte 변수를 할당했을 때 americano에는 latte에 저장된 객체의 참조값이 저장된다.

즉, 두 개의 변수가 같은 객체를 바라보고 있어 위와 같은 출력값이 나타나게 된다.

 

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

1) 함수 호출시 기본 타입 변수를 넘기는 경우, 호출된 함수의 매개변수로 **복사된 값**이 전달된다.

변수 a를 넘기는 경우 a가 그대로 넘어가는것이 아니라 a에 저장된 값이 복사되어 함수에 전달된다.

이를 **값에 의한 호출 (Call By Value)**라 한다.

2) 함수 호출시 참조 탕비 변수를 넘기는 경우, 호출된 함수의 매개변수로 **복사된 참조값**이 전달된다.

변수 b를 넘기는 경우 해당 변수의 참조값이 전달되면서 b.value를 변경하는 경우 참조값이 직접 변경된다.

이를 **참조에 의한 호출(Call By Reference)**라 한다.





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



<img src="/assets/images/prototype.jpg" width="40%" height="40%" title="prototype" alt="prototype"></img>


크롬 브라우저에서 이를 실행하면 proto 프로퍼티를 볼 수 있다.

이 프로퍼티가 coffee 객체의 부모인 프로토타입 객체를 가리킨다.

**객체 리터럴 방식으로 생성된 객체는 Object.prototype 객체가 프로토타입 객체**가 된다는것을 기억하자.



