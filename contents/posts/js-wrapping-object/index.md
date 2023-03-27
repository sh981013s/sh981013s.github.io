---  
title: "[JavaScript] 자바스크립트의 객체 wrapper에 대해 알아보자"  
description: "[JavaScript] 자바스크립트의 객체 wrapper에 대해 알아보자"  
date: 2023-03-27  
update: 2023-03-27  
tags:
- JavaScript
---  

## Intro

오늘 진행한 `자바스크립트 스터디` 에서 나눈 대화다.

🙋🏻 아래 코드는 생성자 함수로 생성한 `String Object` 이다.

```js  
const a = new Number(1);  
```   
🙋🏻 `a` 변수의 타입은 `object` 일테고.

```js  
const a = new Number(1);  
typeof a // object  
```  

🙋🏻 `a + 2` 가 어떻게 `3`이 나오는걸까?

```js  
const a = new Number(1);  
typeof a cosnsole.log(a + 2) // 3  
```  

🙋🏻‍♂️ `JavaScript 엔진`이 그렇게 변환하는거 아니야?

🙋🏻 그래서 어떻게 변환하길래 `object` 와 `number` 의 덧셈이 가능한거지?


<br/>  
본고에서는 어떻게 위처럼 결과가 나올 수 있었는지에 대해 다룰 예정이다. 단순 "`JavaScript 엔진`이 형변환을 한다." 라는 개념을 넘어보고 싶었다.   
  
---  

## Main

이는 `ECMAScript 11.6.1 The Addition operator ( + )` 에서 찾을 수 있었다.


> AdditiveExpression 규칙은 덧셈 연산자(+)가 사용된 경우를 나타냅니다. 이 규칙은 다음과 같이 평가됩니다:

> 1. AdditiveExpression을 평가한 결과를 lref에 할당합니다.
> 2. lref에서 값(value)을 가져와 lval에 할당합니다. 이때, lval은 앞서 평가된 결과의 원시 값(primitive value)입니다.
> 3. MultiplicativeExpression을 평가한 결과를 rref에 할당합니다.
> 4. rref에서 값(value)을 가져와 rval에 할당합니다. 이때, rval은 앞서 평가된 결과의 원시 값(primitive value)입니다.
> 5. lval과 rval의 원시 값을 구합니다. 이때, ToPrimitive() 메소드를 사용하여 객체 래퍼 타입을 원시 값으로 변환합니다.
> 6. lprim과 rprim의 타입을 비교하여 둘 중 하나라도 문자열(string) 타입이라면, lprim과 rprim을 문자열로 변환하여 이어 붙인 결과를 반환합니다.
> 7. 그렇지 않다면, lprim과 rprim을 숫자(number) 타입으로 변환하여 덧셈 연산을 수행합니다.

언뜻 보면 이해가 가지 않을 수 있다. 우선 아래의 과정에서 `값을 가져온다` 는 말은 일반적인 원시 타입이면 간단할 수 있으나, `객체 wrapper` 타입인 경우 말이 달라진다.  
JavaScript 엔진은 연산자나 피연산자가 참조 타입일 때, 해당 값을 가져오기 위해 `GetValue()` 메서드를 사용한다. 이때, `GetValue()` 메서드 내부에서는 해당 값이 객체 `wrapper 타입`인 경우 `ToPrimitive()` 메소드를 사용하여 객체 래퍼를 원시 값으로 변환한 후 반환한다.

따라서, 참조 타입 값에 대한 연산이 수행될 때, 자바스크립트 엔진은 내부적으로 `GetValue()` 함수와 `ToPrimitive()` 메소드를 사용하여 값을 처리하게 된다.

> 2. lref에서 값(value)을 가져와 lval에 할당합니다. 이때, lval은 앞서 평가된 결과의 원시 값(primitive value)입니다.

다시한번 처음에 나왔던 예시를 살펴보자.

`new Number(1)`은 내부적으로 자동으로 `객체 wrapper`를 생성한다.

`new` 키워드를 사용하여 `Number 생성자 함수`를 호출할 때, 반환되는 값은 `Number 객체 wrapper`이다. 따라서, `new Number(1)`은 1이라는 원시 값에 대해 `Number 객체 wrapper`를 생성하는 것이다.

이렇게 생성된 Number 객체 wrapper는, `Number.prototype` 객체에 정의된 메소드를 포함하여 객체처럼 다양한 기능을 제공한다. 예를 들어, `toFixed()` 메소드를 호출하여 소수점 이하 자리수를 조절할 수 있다.

```js  
const a = new Number(1);  
cosnsole.log(a + 2) // 3  
```  

`new Number(1)` 은 내부적으로 자동 객체 wrapper 를 생성하고, 평가시에 `GetValueOf()` 메서드를 사용하여 값을 추출해낸다. 이후 `ToPrimitive()` 메서드를 사용하여 이를 `원시 타입` 으로 형변환 한다.

`a + 2`와 같은 덧셈 연산은 위와 같은 규칙을 따라 `1 + 2` 로 평가되어 연산된다.



  
---  

## Wrapping Up

위의 과정에서 나왔듯이 객체 래퍼를 사용하는 것이 일반적인 원시 값 처리보다 더 많은 자원을 사용하므로 성능상의 문제가 있을 수 있다.

또한, 객체 래퍼는 원시 값과는 다르게 불변하지 않다. 따라서, 객체 래퍼를 사용하여 값에 대한 연산을 수행할 경우, 예상치 못한 결과가 발생할 수 있다.

예를 들어, 다음과 같이 `== 연산자`를 사용하여 객체 래퍼와 원시 값의 비교를 수행하면, 예상치 못한 결과가 발생할 수 있다.

```js  
const num = 10;  
const numObj = new Number(10);  
  
console.log(num == numObj); // true  
console.log(num === numObj); // false  
```  

이처럼, 객체 래퍼를 사용하여 값을 처리할 때는 위와 같은 예기치 않은 결과가 발생할 가능성이 있으므로 주의해야 한다. 따라서, 가능하다면 원시 값 처리를 우선적으로 고려하는 것이 좋다.
  
---
