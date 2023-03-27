---
title: "[BOJ] 1654 랜선 자르기 (Node.js)"
description: "[BOJ] 1654 랜선 자르기 (Node.js)"
date: 2021-11-22
update: 2021-11-22
tags:
- BOJ
---

<a href="https://www.acmicpc.net/problem/1654" target="_blank">백준 1653번 링크</a>

## 풀이

처음에는 해멧지만, 이후에 
전형적인 이분탐색 문제라고 판단이 들었다.

이분탐색이란, 탐색 범위를 두 부분으로 분할해서 찾는 방식으로, `max` `min` `mid` 값을 잡아서 탐색한다.

```javascript
'use strict';

(() => {
	const fs = require('fs');
	const input = fs.readFileSync('/dev/stdin').toString().trim().split('\n');

	const [n, k] = input
		.shift()
		.split(' ')
		.map((a) => +a);
	const lines = input.map((a) => +a).sort();

	let max = Math.max(...lines);
	let min = 1;

	while (min <= max) {
		let mid = parseInt((max + min) / 2);
		let howManyPieces = lines
			.map((line) => parseInt(line / mid))
			.reduce((a, b) => a + b, 0);
		if (howManyPieces >= k) {
			min = mid + 1;
		} else {
			max = mid - 1;
		}
	}

	console.log(max);
})();

```