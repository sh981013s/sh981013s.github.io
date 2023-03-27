---
title: "React - 코드 분할"
description: "React - 코드 분할"
date: 2021-12-18
update: 2021-12-18
tags:
- React
series: "React"
---

## 서론

대부분 React 앱들은 `Webpack`, `Rollup` 또는 `Browserify` 같은 Bundling Tool 을 사용하여 여러 파일을 하나로 병합한 `Bundled file` 을 웹 패이지에 포함하여 
한번에 전체 앱을 로드 할 수 있다.

#### 예시)

##### App

```javascript
// app.js
import { add } from './math.js';

console.log(add(16, 26)); // 42
```

```javascript
// math.js
export function add(a, b) {
  return a + b;
}
```

##### Bundle

```javascript
function add(a, b) {
  return a + b;
}

console.log(add(16, 26)); // 42
```

위의 예시는 어떠한 방식으로 Bundling 을 하는지에 대한 예시일 뿐이다. (실제와는 많이 다르다)
`CRA` 나 `Next.js` 와 같은 툴을 사용한다면 이미 App 자체에 `Webpack` 이 설치되어 있을것이다.

'흩어져 있는 파일들을 압축하여 bundling size 를 줄인다.' 라는 명목 자체는 매우 훌륭하지만, 앱이 커질수록 
해당 번들도 커질 가능성이 농후하다. 특히 큰 사이즈의 3rd party library 를 사용하였을떄는 번들 사이즈 또한 커지기 때문에
로드 시간이 길어지고 이는 곧 웹서비스의 성능에 악영향을 끼친다.

번들이 거대해지는 것을 방지하기 위해서 `code splitting` 을 고민해보아야 한다. 코드분할은 앱을 `지연로딩` 하게 도와주고, 이는 
앱의 코드 양을 줄이지 않고도 사용자가 필요하지 않은 코드를 불러오지 않게 하며 앱의 initializing 에 필요한 loading resource 자체를 줄여준다.

---

## 본론

### import()

App 에 code splitting 을 도입하는 방식중 하나는 `동적 import()` 를 사용하는 것이다.

#### Before

```javascript
import { add } from './math';

console.log(add(16, 26));
```

#### After 

```javascript
import("./math").then(math => {
  console.log(math.add(16, 26));
});
```

`Webpack` 이 해당 구문을 만나게 되면 자체적으로 앱의 Code 를 Splitting 한다.

하지만, `Babel` 을 사용한다면 Babel 은 위와같은 동적 import 를 `인식` 할수는 있지만, 자체적으로 `분할` 하지는 않는다.
그에따라, `babel-plugin-synyax-dynamic-import` 를 사용해야만 한다.


### React.lazy

`React.lazy` 함수를 사용하 동적 import 를 사용하여 컴포넌트를 렌더링 할 수 있다.

#### Before

```javascript
import ExpampleConponent from './ExpampleConponent';
```

#### After

```javascript
const Main = lazy(() => import("./components/Main"));
const AboutPage = lazy(() => import("./components/AboutPage"));
const MySkillsPage = lazy(() => import("./components/MySkillsPage"));
```

### Entire Code

```javascript
const Main = lazy(() => import("./components/Main"));
const AboutPage = lazy(() => import("./components/AboutPage"));
const MySkillsPage = lazy(() => import("./components/MySkillsPage"));
const BlogPage = lazy(() => import("./components/BlogPage"));
const WorkPage = lazy(() => import("./components/WorkPage"));
const SoundBar = lazy(() => import("./subComponents/SoundBar"));

function App() {
  const location = useLocation();

  return (
    <>
      <GlobalStyle />

      <ThemeProvider theme={lightTheme}>
        <Suspense fallback={<Loading />}>
          <SoundBar />

          <AnimatePresence exitBeforeEnter>
            <Switch location={location} key={location.pathname}>
              <Route exact path="/" component={Main} />

              <Route exact path="/about" component={AboutPage} />

              <Route exact path="/blog" component={BlogPage} />

              <Route exact path="/work" component={WorkPage} />

              <Route exact path="/skills" component={MySkillsPage} />
            </Switch>
          </AnimatePresence>
        </Suspense>
      </ThemeProvider>
    </>
  );
}
```

`App` Component 가 처음 rendering 될때, 내부에 속한 번들을 자동으로 불러온다.

`React.lazy` 는 동적 import() 를 호출하는 함수를 인자로 가진다. 이 함수는 React 컴포넌트를 포함하여 `default export ` 를 
가진 모둘로 결정되는 `Promise` 를 반환해야 한다.

또한, `lazy component` 는 `Suspense component` 하위에서 rendering 되어야하며, `Suspense` 는 `lazy component`  
가 로드되길 기다리는 동안 로딩 화면과 같은 예비 컨텐츠를 보여줄 수 있게 해준다.

마지막의 Entire Code 에서 주목해야 할 점은, 그래서 Code Splitting 을 도대체 어느 곳에 도입하여야 한다는 점이다.
나는 이를 시작하는 좋은 장소는  `Route` 라고 생각하여 해당 코드를 작성하였다. 아무리 `SPA` 라고 할지라도,  page transition 이 발생하는 부분에서
loading 시간은 필연적으로 발생하며 대부분의 페이지를 한번에 렌더링하기에 사용자가 페이지를 렌더링 하는 동안 다른 요소와 상호작용 또한 하지 않기 때문이다.

---

## 결론

Bundling 툴은 보통 코드를 압축하기에 이에따른 import 하는 과정에서 너무 많은 리소스를 낭비 할 수 있기에,
이부분에 대해서는 code splitting 이 유용하게 쓰인다. 

이 과정에서 동적으로 import() 함수를 쓰는 방법이 존재하지만, page transition 시간에 다른 예비 요소를 사용할 수 있기에
나는 주로 Route 에 Suspense 와 lazy 를 사용하는 방법을 좋아한다.




