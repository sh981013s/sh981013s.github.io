---
title: "VSC 리액트 auto import"
description: "VSC 리액트 auto import"
date: 2022-01-03
update: 2022-01-03
tags:
  - React
---

## 서론

나는 `webstorm` 을 사용하다가 유료이기도 하고, 나름 `VSC`에 비교하면 무겁다고 체감이 되어 다시 `VSC` 로 돌아왔는데, 
webstorm 에서는 훅을 사용할때 따로 import 를 해주지 않더라도 자동적으로 import 를 해주어 이 기능이 대단하단것을 몰랐다.
따라서, VSC 에서도 `auto import` 기능을 사용 할 수 없는지에 대해 구글링을 해보았다.

---

## 본론

결론적으로는 쉽게 가능하다. 루트 디렉토리에 `jsconfig.json` 파일을 생성한 후 아래의 내용을 넣어주면 된다.

```json
{
	"compilerOptions": {
		"module": "commonjs",
		"target": "es6",
		"checkJs": true,
		"jsx": "react"
	},
	"exclude": ["node_modules"]
}

```


