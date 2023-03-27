---
title: "importing React 와 JSX 사용의 상관관계"
description: "importing React 와 JSX 사용의 상관관계"
date: 2022-01-02
update: 2022-01-02
tags:
  - React
---

## 서론

종종 리액트 공부를 하다보면 `JSX` 문법을 사용할 것이기 때문에 `import React from 'react'` 와 같이 리액트를
import 해주어야 한다는 말들을 들었다.

하지만 생각해보면 나는 딱히 리액트로 프로젝트를 진행할때, JSX 문법을 사용해야 한다 하더라도 컴포넌트마다 리액트를 import 하는
행위는 따르지 않고도 문제가 발견되지 않았었다.

---

## 본론

이에따라 `stack overflow` 와 `구글링` 을 통하여 찾아보니, 결국 이는 과거에 행해졌던 일종의 `관습` 이라고 나는 판단하였다.

리액트 공식문서에 따르면, JSX 를 사용할때 더는 import react 를 하지 않아도 된 이유는 결국 새로운 `JSX Transform` 때문이라고 한다.

리액트 17버전 부터 새로운 `JSX Transform` 을 사용할 수 있는데,

이는 React 를 import 하지 않고도 JSX 문법을 사용 가능하게 해주며,

세팅에 따라서, bundle size 자체를 미세하게 나마 줄여줄 수 있다.

### 이전 방식과의 차이

```javascript
import React from "react"

function App() {
  return <h1>Hello World</h1>
}
```

이전의 방식으로는 위와같은 JSX 코드를 컴파일러가 브라우저가 이해할 수 있게 컴파일을 해주었다.

```js
import React from "react"

function App() {
  return React.createElement("h1", null, "Hello world")
}
```

이를 (Old JSX Transform), 자바스크립트 방식으로 바꾼다면 위와 같은 모양을 띄울 것이다.

```js
function App() {
  return <h1>Hello World</h1>
}
```

```js
import { jsx as _jsx } from "react/jsx-runtime"

function App() {
  return _jsx("h1", { children: "Hello world" })
}
```

하지만 `React 17`에 포함되어있는 `New JSX Transform` 으로 인하여, 더는 리액트는 JSX 를 `React.createElement` 로 변환하지 않고, 리액트 패키지 자체의 함수를 불러와 위와같이 작동하게된다.

---

## 결론

리액트 17버전 이전에는, JSX 문법을 사용하기 위하여 React 를 import 했던것은 사실이다.

하지만, 리액트 17버전에 출시된 NEW JSX Transform 으로 인하여, 기존에 JSX 코드를 React.createElement 로 변환하여 사용하는 방식이 아닌, 자체적으로 리액트 패키지의 엔드포인트로부터 변환하는 함수를 호출하여 변환하기에 리액트 17 버전 이상을 사용한다면, JSX 문법을 사용하기위해 더이상은 React 를 굳이 import 해줄 필요는 없다.
