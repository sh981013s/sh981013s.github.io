---
title: "자바스크립트 indexOf() 와 시간 복잡도의 상관관계에 대한 고찰"
description: "자바스크립트 indexOf() 와 시간 복잡도의 상관관계에 대한 고찰"
date: 2022-01-29
update: 2022-01-29
tags:
- JavaScript
---

## 서론

알고리즘을 연습하고 탐구하다 여지껏 쓴 코드들을 보며 `시간복잡도` 에 대해 생각을 해보았다.

적어도 빅오표기법 기준 `O(n)` 어쩔 수 없지 라고 생각하며 마구잡이 식으로 `for문` 내부에 `indexOf()` 메소드를 남발하고 있었다.  

```javascript
const arr = [1, 2, 3];


for (let elIdx in arr) {
  return elIdx; // 0, 1, 2
} 

// O(n)

for (let el of arr) {
  return arr.indexOf(el); // 0, 1, 2
}

// O(n) ????????????????????
```

위의 코드를 비교하며 나는 헷갈리기 시작했다. `indexOf()` 메소드가 단순히 마법처럼 해당 value 의 index 를 뿅 하고 찾아 리턴해줄까..? 
만일 그렇다면 두개의 예시의 공통된 시간복잡도는 `O(n)` 일것이다.

만일 아니라면? indexOf() 의 동작방식이 element 들을 순차적으로 모두 돌며 체크한다면? 그렇다면 두번째 예시의 시간복잡도는 `O(n^2)` 이지 않을까
라며 생각했다.

---

## 본론

결론부터 말하자면, `indexOf()` 는 모든 element search 하지 않는다. 하지만 위의 두번째 예시의 시간복잡도는 `O(n^2)` 으로 표기된다.

https://tc39.es/ecma262/#sec-array.prototype.indexof

해답은 `ECMAScript` 의 공식문서에서 찾을 수 있었다.</br> 이에 따르면, `indexOf()` 는 인자로 `fromIndex` 를 별도로 받지 않았을 경우, 
물론 받아도 마찬가지이지만, 받지 않았을 경우 0 index 부터 순차적으로 탐색을 시작하며, 찾을경우 즉시 해당값을 `return` 해준다.<br/> 받았을 경우에도
해당 index 부터 탐색을 시작하며, 찾을 경우 곧바로 값을 `return` 해준다. 

키포인트는 값을 찾을경우 `즉시` return, 즉 탐색을 끝낸다는 뜻이다.<br/> 이는 곧 '`indexOf()` 는 모든 element search 하지 않는다.' 로 해석된다.

그렇다면 `indexOf()` 의 단순 시간복잡도는 `O(1)` 일까? 그렇지 않다. 시간복잡도를 계산할때는 항상 `최악`의 상황을 고려해야한다. `indexOf()` 의
최악의 상황이라 꼽자면, array 에는 99999999999999999999999999개의 3이라는 element 가 존재하지만 ( [3,3,3,3,3,3,3........) ], 코더가
array.indexOf(1) 이라고 작성 후 실행하는 상황이라 생각된다. 이경우에는 0 index 부터 array.length-1, 값을 찾을 수 없기때문에 처음부터 끝까지 탐색을 
하게된다. 과연 이 경우에도 시간복잡도를 `O(1)` 이라고 표현할 수 있을까?

이에따른 해답을 찾기위해 부끄럼움을 무릅쓰고 `stackOverflow` 에 직접 질문을 하게 된다.

https://stackoverflow.com/questions/70902661/does-indexof-in-js-searches-all-the-elements-of-an-array-to-execute

이에따른 사람들의 답은, 위에 같은 상황때문에 `indexOf()` 의 시간복잡도는 `O(n)` 으로 표기된다고 한다. 그러므로 최악의 경우를 피하기 위해, 무분별한
`indexOf()` 의 사용 보다는 `binary search` 를 통한 `O(log n)` 을 노려볼 수도 있다 라고 답한분도 있었다.

---

## 결론

Q1. `indexOf()` 는 모든 element 를 탐색하여 값을 도출해낼까?

A1. 아니다. `indexOf()` 는 `startIndex` 를 따로 인자로 받지 않는상황에서는 index 0 부터 해당 value 값을 찾을때까지 search 를 진행하고, 찾을경우
바로 값을 return 한다.

Q2. 그렇다면 `indexOf()` 를 사용해 1번의 search 를 통해 정상적인 값을 리턴받았다면 단순 시간복잡도는 `O(1)` 으로 표기될수 있지 않은가?

A2. 아니다. 시간복잡도를 표현할때는 항상 `최악의` case 에 입각하여 계산을 해야하기에, 무한개의 element 를 가진 array 에 탐색하고자 하는 값이 없다면? 
과 같은 상황이 있기에, `O(n)` 으로 표기된다.



