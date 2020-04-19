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
}

delete coffee.type;  // type 프로퍼티 삭제 시도
console.log(coffee.type);  // undefined 출력

delete coffee;  // coffee 객체 삭제 시도
console.log(coffee.size);  // small 출력
```





***

### 참조 타입의 특성
