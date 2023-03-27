---
title: "환전기 with Vanilla JS"
description: "환전기 with Vanilla JS"
date: 2021-11-27
update: 2021-11-27
tags:
- VanillaJS
---

## Intro

4번째 Mini Vanilla Js Project 이다.

이번에는 `ExchangeRate-API` 라는 `Third-party api` 를 사용하여 환전 기능을 만들어보았다!

---

## Main

- [소스코드](https://github.com/sh981013s/Projects-with-VanillaJs/tree/main/Exchange-Rate-Calculator)
- [liveDemo](https://hwani-vanillajs.netlify.app/exchange-rate-calculator/)


![](exchange-cal.gif)

---

## Conclusion

처음으로 vanillaJS 를 사용하며, third-party api 를 사용해보았다.

기존에 리액트를 배우고 사용했을때는 데이터를 요청하는 방식 자체에 대해서 당연하게 고민도 하지 않은채 마구잡이로 남들이 사용하는 방식대로 `request` 나 `axios` 를 썼었는데,

이번 프로젝트를 진행하며, 브라우저가 자체적으로 `fetch()` 함수를 지원한다는 것을 배웠고, 이를 사용해보았다. 결과는 내 기준에서는 성공적이였고, 고민을 해보았던 점은, 결국 `request`, `axios` 도 같은 `Http 비동기 통신 library` 일텐데

이들을 대체할 수 있는 내장 함수인 `fetch()` 를 main 으로 사용한다면 `js bundling size` 면에서 유리하지 않을까 였다.

이 문제에 대해 찾아본 결과, 내 생각대로라면 `fetch()` 를 main 으로 쓰는것이 바람직 할 수 있겠지만, 

- 지원하지 않는 브라우저가 있다  (...IE11)
- 네트워크 에러 발생 시 response timeout 없이 기다려야한다.
- JSON 으로 변환해주는 과정이 필요하다.

위 세가지의 단점때문에 아직 외부 라이브러리들을 사용을 한다는 의견이 많고,  나의 실력으로써는 더 많이 경험하고 써보면서 각자의 장단점을 몸소 직접 체험하는 수밖에 없을 것 같다.