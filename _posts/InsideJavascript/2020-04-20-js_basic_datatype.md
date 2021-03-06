---
layout: single
title: "[자바스크립트] 기본 데이터 타입"
categories:
  - javascript
toc: true
toc_sticky: true
toc_label: "On this Page"
date: 2020-04-20
---



자바스크립트 데이터 타입은 기본 타입과 참조 타입으로 나뉜다.  기본타입과 참조타입에 해당하는 데이터타입은 아래와 같다.



#### 기본 타입

- 숫자 (number)
- 문자열 (string)
- 불린값 (boolean)
- undefined
- null



#### 참조 타입

- 객체 (Object)
  - 배열 (Array)
  - 함수 (Function)
  - 정규표현식



이번 시간에는 자바스크립트 기본 데이터 타입에 대해 자세히 살펴보자.

***

### 기본 데이터 타입

- 기본 데이터 타입의 특징은 그 자체가 하나의 값을 나타낸다는 것
- 자바스크립트는 var 한 가지 키워드로만 변수를 선언
  - 느슨한 타입체크 언어로 어떤 형태의 데이터를 저장하느냐에 따라 해당 변수의 타입이 결정됨



아래 예제는 기본 타입 변수들을 생성하고 typeof 연산자를 통해 해당 변수의 타입을 출력한다.

```javascript
// 숫자 타입
var intVar = 10;
var floatVar = 0.1;

// 문자열 타입
var singleQuoteVar = 'single quote';
var doubleQuoteVar = "double quote";

// 불린 타입
var boolVar = true;

// undefined 타입
var undefinedVar;

// null 타입
var nullVar = null;

// typeof 를 통해 타입 출력
console.log(
	typeof intVar,
    typeof floatVar,
    typeof singleQuoteVar,
    typeof doubleQuoteVar,
    typeof boolVar,
    typeof undefinedVar,
    typeof nullVar
)
```



위 예제 코드를 실행하면 다음과 같은 결과가 나온다.

```javascript
number number string string boolean undefined object
```





#### 1. 숫자

- 자바스크립트의 숫자형은 정수, 실수형이 따로 없음.



ex) 정수와 실수가 존재하지 않아 나누기 연산시 정수만 출력하기 위해서는 Math.floor() 메서드 활용

```javascript
var test = 5/2;

console.log(test); // 2.5 출력
console.log(Math.floor(test));  // 2 출력
```





#### 2. 문자열

- 작은 따옴표(' ') 혹은 큰 따옴표(" ") 로 생성
- **한 번 정의된 문자열은 수정 불가능**. 하지만 변경된 문자열을 만드는건 가능
- 문자열은 배열처럼 인덱스를 통해 접근 가능



ex) 문자열 생성 후 이를 출력하는 예제

```javascript
// 문자열 생성
var strTest = 'Test';
console.log(strTest[0], strTest[2]);  // Ts 출력

strTest[1] = 'p';  // 문자열 수정 불가능
console.log(strTest);  // Test 출력

strTest = 'change';  // 변경은 가능
console.log(strTest);  // change 출력
```





#### 3. 불린값

- true 와 false 값을 나타냄





#### 4. null 과 undefined

- 두 타입 모두 **값이 비어있음** 을 나타냄
- undefined 는 값이 할당되지 않은 변수

  - undefined 타입 변수의 값도 undefined
  - 즉, undefined 는 타입 이면서 값을 나타냄
- null 은 개발자가 명시적으로 값이 없음을 표현할때 사용
- null 은 typeof 의 결과가 null 이 아니라 object
  - 따라서 null 타입 체크시에는 typeof 대신 일치 연산자 (===) 를 사용



ex) null 타입 체크는 typeof 대신 일치연산자를 사용

```javascript
var nullVar = null;

console.log(typeof nullVar === null);  // false 출력
console.log(nullVar === null);  // true 출력
```








