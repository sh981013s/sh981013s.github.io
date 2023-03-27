---
title: "사이드바, 모달 with Vanilla JS"
description: "사이드바, 모달 with Vanilla JS"
date: 2021-11-28
update: 2021-11-28
tags:
- VanillaJS
---

## Intro

6번째 Mini Vanilla Js Project 이다.

이번에는 순수 `css` 와 `vanilla JS` 를 사용하여 Sidebar 와 Modal 을 직접 구현해보았다.

---

## Main

- [소스코드](https://github.com/sh981013s/Projects-with-VanillaJs/tree/main/Menu-Sidebar-Modal)
- [liveDemo](https://hwani-vanillajs.netlify.app/menu-sidebar-modal/)


![](modal.gif)

---

## Conclusion

사실 내가 프로젝트를 진행할때 `UI Library` 를 사용했던 이유는 style 적인 이유보다는 JS? 적인 이유가 더 컸다.
이 뜻은, UI Library 를 사용할때도 보통 style 적인 부분은 모두 갈아엎으며 나의 입맛대로 꾸몄지만, 예를 들어,

- 'x' 표시를 누르면 모달이 닫힌다.
- 모달 바깥의 영역을 누르면 부드럽게 animation 과 함께 자연스럽게 닫힌다.
- 사이드바 햄버거 아이콘을 누르면 자연스럽게 화면이 이동하며 sidebar 가 펼쳐진다

와 같은 기능적인 부분에 대해 무지했기때문에 이런부분은 주로 library 에 의존하곤 했다.

하지만, 지속적인 구글링과 공부 끝에, 이번 기회에 노력하면 해볼 수 있지 않을까? 라는 결론에 도달했고, 
나의 기대치에 맞는 화면을 직접 구성 할 수 있게 되었다. 

결론적으로는, 사실 JS 부분은 복잡하지 않고 간단하지만, 주로 CSS 적인 변화와 움직임을 toggle 로 컨트롤하면서 여러가지 상황에 대처할 수 있게 구현하는 방식을 택하였다.