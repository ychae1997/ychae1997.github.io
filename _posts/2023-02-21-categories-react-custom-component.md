---
title: "[React] Custom Component"
excerpt: "styled-components 이용해 UI 디자인 패턴 만들기"

categories:
  - React
tags:
  - [React, 코드스테이츠]

permalink: /React/custom-component/

toc: true
toc_sticky: true

date: 2023-02-21
last_modified_at: 2023-02-21
---

>들어가기 앞서 custom component 관련해 필요한 개념들에 대해 알아보자!
<img src="/assets/images/posts_img/react-custom-component/letsgocat.jpeg">

## Component-Driven Development(CDD)
부품 단위로 UI컴포넌트를 만들어 나가는 개발 방법이다. 기본적인 컴포넌트 부터 시작해 페이지를 조립하는 상향식(bottom-up) 성향을 띈다.

## CSS in JS
프로젝트의 규모나 복잡와 비례해 협업해야할 팀원수가 많아지면서 CSS 작성 패턴 또한 일관성이 없어지고 복잡해지게 되었다.
따라서 CSS작업을 효율적으로 하기 위해 구조화된 CSS의 필요성이 대두되었고, CSS를 구조화 하는 방법에 대한 연구가 필요해졌다. 
아래는 대표적인 CSS 방법론에 대해 요약한 표이다.

|    | 특징 | 장점 | 단점 |
|:--:|:---:|:---:|:---:|
|CSS|기본적인 스타일링 방법|-|일관된 패턴을 갖기 어렵다. !important의 남용| 
|SASS(preprocessor)|프로그래밍 방법론을 도입해, 컴파일된 CSS를 만들어내는 전처리기|변수/함수/상속/개념을 활용해 재사용 가능 CSS의 구조화|전처리 과정 필요, 디버깅의 어려움 있다. 컴파일한 CSS파일이 거대해진다.|
|BEM|CSS 클래스명 작성에 일관된 패턴을 강제하는 방법론|네이밍으로 문제 해결. 전처리 과정 불필요|선택자의 이름이 장황하고, 클래스 목록이 너무 많아짐|
Styled-Component(CSS-in-JS)|컴포넌트 기반으로 CSS를 작성할 수 있게 도와주는 라이브러리|CSS를 컴포넌트 안으로 캡슐화, 네이밍이나 최적화를 신경 쓸 필요없다|빠른 페이지 로드에 불리|

CSS 방법론은 코드의 재사용, 코드의 간결화(유지 보수 용이), 코드의 확장성, 코드의 예측성을 지향한다는 점에서 같다.