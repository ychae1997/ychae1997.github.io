---
title: "[자료구조] 자료구조란?"
excerpt: ""

categories:
  - Data Structure
tags:
  - [자료구조]

permalink: /Data Structure/자료구조란

toc: false
toc_sticky: false

date: 2023-03-14
last_modified_at: 2023-03-14
---
<hr>

## 🤔 자료구조란 무엇일까요?
> 자료구조란 여러 데이터의 묶음을 저장하고, 사용하는 방법을 정의한 것을 말한다.

<hr class="sub">

### ✅ 자료구조를 배우는 이유
- 데이터를 체계적으로 저장하고, 효율적으로 활용하기 위해서
- 대부분의 자료구조는 특정한 상황에 놓인 문제를 해결하는 데 특화되어 있다.
<blockquote class="ex">
<ul>
<li>많은 자료구조를 알아두면, 어떠한 상황이 닥쳤을 때 적합한 자료구조를 빠르고 정확하게 적용해 문제를 해결할 수 있다.</li>
<li>문제 해결 능력을 필요로 하는 알고리즘 테스트와 밀접한 연관성이 있다.</li>
<li>특정 문제를 해결하는 데에 적합한 자료구조를 찾아 데이터를 정리하고 활용할 줄 알면, 상황에 가장 적합하고 정확한 코드를 작성할 수 있다.</li>
</ul>
</blockquote>

<hr>

## 🤔 데이터(data)는 무엇일까요?
- 데이터는 **문자, 숫자, 소리, 그림, 영상 등 실생활을 구성하고 있는 모든 값**을 말한다.
<blockquote class="ex">우리의 이름, 나이, 키, 집 주소, 목소리 혹은 유전자 DNA까지 데이터로 분류할 수 있다.</blockquote>
- 데이터는 그 자체만으로 어떤 정보를 가지기 힘들다.
<blockquote class="ex">예를들어 나이라는 데이터만 알고 있다면, 사람의 나이인지, 강아지의 나이인지, 나무의 나이인지 알 수 없다.</blockquote>
- **데이터는 분석하고 정리하여 활용해야만 의미를 가질 수 있다.**
- 데이터를 사용하려는 목적에 따라 형태를 구분하고, 분류해 사용한다.
<blockquote class="ex">
  ex. 서로 다른 형태의 데이터를 하나의 방법으로만 정리하고 활용할 때
  <ul>
    <li>모든 숫자를 전화번호부를 저장할 때처럼, 숫자를 3개 혹은 4개씩 묶음 짓고 하이픈(<code>-</code>)으로 합치는 것은 잘못된 활용이다.</li>
    <li>거리를 구하거나 다섯 자리가 넘는 숫자를 보관할 때는 하이픈(<code>-</code>)이 필요하지 않다.</li>
  </ul>
</blockquote>
- 데이터는 필요에 따라 데이터의 특징을 잘 파악(분석)하여 정리하고, 활용해야 한다.
<blockquote class="ex">
  데이터를 정해진 규칙 없이 저장하거나, 하나의 구조로만 정리하고 활용하는 것보다, <strong>데이터를 체계적으로 정리해 저장해 두는 게, 데이터를 활용하는 데 있어 훨씬 유리하다.</strong>
</blockquote>

<hr>

## 📚 자료구조의 분류
수많은 선배 개발자는 무수한 상황에 데이터를 효율적으로 다룰 수 있는 여러 방법을 연구해 두었다.
<blockquote class="ex">
무수한 상황의 예시
<ul>
<li>번호를 다 알지 않아도, 이름을 아는 것만으로 전화를 할 수 있는 방법은 무엇이 있을까?</li>
<li>웹 브라우저에서 뒤로 / 앞으로 가는 방법은 무엇이 있을까?</li>
<li>게임 매칭을 잡을 때, 수많은 사람을 통제하는 방법엔 무엇이 있을까? ...등등</li>
</ul>
</blockquote>
<figure>
  <img src="/assets/images/posts_img/Data_Structure/data_structure.png">
  <figcaption>출처: 코드스테이츠</figcaption>
</figure>

무수한 상황에서 데이터를 **효율적으로 다룰 수 있는 방법**을 모두 모아, **자료구조**라는 이름을 붙였다.
- 자주 등장하는 네 가지의 자료구조
  - Stack, Queue, Tree, Graph


<hr>

자료구조를 눈으로 보고 익힐 수 있는 시각 자료
- [Data Structure Visualizations](https://www.cs.usfca.edu/~galles/visualization/Algorithms.html){:target="_blank"}
- [Visualgo](https://visualgo.net/en){:target="_blank"}
