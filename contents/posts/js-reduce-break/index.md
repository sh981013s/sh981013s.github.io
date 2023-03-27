---
title: "자바스크립트 reduce break"
description: "자바스크립트 reduce break"
date: 2022-03-28
update: 2022-03-28
tags:
- JavaScript
---

## 서론

Javascript `Reduce` 메서드란 `배열의` 각 요소에 대한 주어진 `reducer 함수` 를 실행하고,
하나의 결과값을 반환한다.

나에게는 애증의 관계에 있는 메서드 중 하나이다. 생각보다 복잡한 것 같아서 기피하다가도,
실제 사용해보면 이는 JS 에서 얼마나 강력한 기능을 가졌는지 매번 느끼게 된다.

보통 `reduce()` 는 보편적으로 

```js
const arr = [1,2,3,4];
return arr.reduce((acc, cur) => acc + cur);
```

위와 같이 배열의 전체 합을 구할때 쓰이지만, 익숙해진다면 적재적소에 필요한 custom reducer 를 직접
만들어 사용할 수 있다.

기존의 `for/while` 반복문을 사용할때는, 메모리 낭비 방지 및 소요시간 감소를 위해 원하는 해답 혹은 결과물이 나오면
반복을 중단해야 한다고 생각한다.

```js
const arr = [1, 2, 3, 4, 5, 6];

for (let num of arr) {
  if (num === 5) break;
  console.log(num); // 1, 2, 3, 4
}
```

위와같이 `for/while` 반복문은 `break` 를 사용하여 반복을 중단할 수 있고,

```js
const arr = [1, 2, 3, 4, 5, 6];

for (let num of arr) {
  if (num === 5) continue;
  console.log(num); // 1, 2, 3, 4, 6;
}
```

`continue` 를 사용하여, 현재의 순환부를 종료하고 다시 순회표현식으로 이동하는 기능을 쉽게 구현할 수 있다.


하지만, 위의 반복문같이 실행 중, 멈춤(break), 넘기기(continue) 는 `reduce()` 메서드에서 똑같이 사용 할 수 없고, 조금 더
복잡한 로직을 사용해야 한다.

---

## 본론

### 리듀서(reducer) 함수의 인자

1. 누산기 (acc)
2. 현재 값 (cur)
3. 현재 인덱스 (idx)
4. 원본 배열 (src)

리듀서 함수는 위의 4개의 인자를 가진다.

`누산기(acc):` 누산기는 콜백의 반환값을 누적
콜백의 첫 번째 호출이면서 initialValue를 제공한 경우에는 initialValue의 값

`현재값(cur):` 처리할 현재 요소

`현재 인덱스(idx):` 처리할 현재 요소의 인덱스, initialValue를 제공한 경우 0, 아니면 1

`원본 배열(src):` 원본 배열

### reduce break

본론으로 돌아가서, break 를 사용하려면 어떻게 해야할까?

```js
const arr = [1, 2, 3, 4, 5];

const ans = arr.reduce((acc, cur, idx, src) => {
  if (cur === 3) {
    break; // wrong
  } else {
    return acc + cur;
  }
})
```

앞서 언급했듯이 이렇게 `break` 를 사용하면 당연히 작동하지 않는다. 해결법은

```js
const arr = [1, 2, 3, 4, 5];

const ans = arr.reduce((acc, cur, idx, src) => {
  if (cur === 3) {
    src.splice(1);
    return acc;
  } else {
    return acc + cur;
  }
})

console.log(ans); // 3
conssole.log(arr); // [1]
```

위의 코드를 보면 reduce function 은 4번째 인자인 `arr` 을 `mutating` 하기에,
`slice()` 를 이용하여 강제 종료할 수 있다.

하지만 여기에서 문제점은 `slice()` 를 사용하게 되면 원본 배열인 `arr` 에게도 영향을 미치기 때문에, 
위와 같은 방식을 사용하려면 해당 배열을 `deepcopy` 를 통해 새로운 배열을 만든 후 사용하는 방식이 바람직하지 않을까? 라고 생각한다.

### reduce continue

마지막으로 reduce function 내부에서 continue 와 같은 기능을 사용하려면 어떻게 해야할까?

```js
const arr = [1, 2, 3, 4, 5];

const ans = arr.reduce((acc, cur, idx, src) => {
  if (cur === 5) {
    continue; // wrong
  } else {
    return acc + cur;
  }
})
```

당연히 위와 같이 하면 제대로 작동되지 않는다.

```js
const arr = [1, 2, 3, 4, 5];

const ans = arr.reduce((acc, cur, idx, src) => {
  if (cur === 5) {
    return acc;
  } else {
    return acc + cur;
  }
})

console.log(ans); // 10  
```

넘기고 싶은 loop 일때의 조건문을 걸어, 단순하게 여태까지 누산된 값인 `acc` 을 return 해주면 된다.

---

## 결론

`reduce function` 내부에서 간단한 스킬을 사용하여, `for/while` 문의 `break / continue` 기능을 사용하는 것을 알아보았다.

