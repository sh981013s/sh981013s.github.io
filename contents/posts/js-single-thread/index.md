---
title: "싱글스레드인 자바스크립트는 어떻게 비동기 처리를 하는가"
description: "싱글스레드인 자바스크립트는 어떻게 비동기 처리를 하는가"
date: 2022-01-24
update: 2022-01-24
tags:
- JavaScript
---

## 서론

자바스크립트는 하나의 `call stack` 과 하나의 `memory heap` 을 가진 
`single threaded language` 이다. 그러므로 당연히 위에서부터 순차적으로 코드를
실행하고 다음 코드로 넘어가기 전 실행하고 있는 코드를 끝내야만 한다. 맞다. `자바스크립트(js)` 는 `동기적이다(synchronous)`.

허나, 내 머리를 지끈거리게 만든 점은 코르를 작성하다보면 의도적으로 코드를 `비동기 처리` 하는 행위를 
심심치 않게 해왔지만, 

'도대체 어떻게 싱글스레드인  자바스크립트는 코드를 비동기적으로 실행시킬까?' 에서 머리가 복잡해졌다.


---

## 본론

고맙게도 자바스크립트 엔진은(V8, Spidermonkey, JavascriptCore 등등..)은 
이와같은 task 들을 병렬적으로 background 에서 동작시키기 위해 필요한 `Web API` 를 가지고 있다.

call stack 은 Web API 의 함수들을 인지하고 필요에 의해 브라우저에게 task를 맡긴다.
맡겨진 task가 끝난 후에는, 브라우저는 해당 task를 다시 반환 후 callback 으로써 다시 스택에 밀어넣는다.

브라우저 콘솔에서 `window` 를 쳐본다면 ajax calls, 이벤트 리스너, fetch API, setTimeout 등 
Web API 가 제공하는 모든것들을 확인할 수 있다. 이제 어떻게 자바스크립트가 브라우저에서 `비동기적으로` 동작하는지 예시와 함께 알아보자.

```javascript
console.log("first")
setTimeout(() => {
    console.log("second")
}, 1000)
console.log("third")
```

위의 코드를 적어내면

```javascript
first
third
undefined
second
```

위의 결과가 나온다. 벌써 머리가 어질어질하다. 'first', 'third', 'second' 의 결과가 나와야 하지 않나? 하지만 이는 에러로 인해 해당 결과가 나온것이 아니다. 한줄한줄 추적해보자.

1. `console.log(first)` 가 처음 스택에 쌓이게 되고 콘솔에 찍힌다. 

2. 엔진은 자바스크립트에서 동작하지 않는 `setTimeout()` 을 발견하고는 비동기적인 처리를 위해 `Web API` 에게 보내버린다.

3. 2번의 동작을 신경쓰지 않은체 `console.log(three)` 를 스택에 쌓고 실행시킨다. 

4. 자바스크립트의 엔진의 `event loop`이 개입하여 '우리 이제 끝난거야..?' 하고 물어본다.

5. `web API` 가 '아니? 아직인데? 줄 value 가 없어!' 라고 대답을 하자 디폴트 값인 `undefined` 를 콘솔에 찍는다.

6. 비동기처리를 하던 task 가 완료 후, callback 으로써 스택에 들어오게 되고, 이는 실행되어 콘솔에 second 를 찍는다.

---

## 결론

결국 내가 생각하던게 어느정도는 맞다. 자바스크립트는 single threaded language 이고, 하나의 
콜스택과 하나의 힙스택을 가지고 있기에 동기적으로 task 를 처리한다. 하지만 이는 브라우저에서 실행될때
Web API 라는 녀석을 사용하여 비동기적으로 task 를 처리할 수 있다.



