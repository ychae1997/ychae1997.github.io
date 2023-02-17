---
title: "[JavaScript] this 키워드"
excerpt: "this에 대해 알아보자"

categories:
  - JavaScript
tags:
  - [JavaScript, ES6]

permalink: /JavaScript/재귀함수 예제/

toc: true
toc_sticky: true

date: 2023-02-17
last_modified_at: 2023-02-17
---
영어에서 this는 '이것'을 뜻한다. 그렇다면 자바스크립트 내에서 this란 무엇일까? <br>
자바스크립트 내에서 this는 함수가 호출되는 시점의 **실행 컨텍스트(Excution Context)**이다. 여기서 실행 컨텍스트란 코드가 실행되는 환경, 실행하기 위해 필요한 환경을 말한다. <br>

이 말은 함수를 실행하는 방법 즉, 호출하는 방법이 달라지면 this도 변한다는 뜻이다. this는 **누가 호출 해는지**에 따라 달라진다. 이제부터 상황별로 this가 어떻게 바인딩 되는지 알아보자.

## 1. 일반 함수에서의 this
엄격모드가 아닌 곳에서 일반함수에서 this를 호출하면 window객체가 출력된다. <br>
일반 전역에서 this를 불렀을 때도 똑같이 window가 출력된다.
```javascript
function func() {
  return this;
}
console.log(func()); // window 객체
console.log(this) // window 객체
```

`use strict` 모드 에서는 undefined가 출력된다.
```javascript
'use strict';
function func() {
  console.log(this);
}
func(); // undefined
```

<hr>

## 2. object내에서 this
오브젝트 내의 메서드 안에서 쓰면 그 메서드를 가지고 있는 오브젝트를 가리킨다. <br>
이 때 메서드가 일반함수인지 화살표함수인지에 따라 this값이 달라진다.
```javascript
const obj = {
  data : 'kim';,
  func : function() {
    console.log(this);
  },
  arrow : () => {
    console.log(this);
  }
}
obj.func(); // obj
obj.arrow(); // window
```
일반함수에서의 this는 이 함수를 담고 있는 객체를 출력하고,
화살표함수에서의 this는 외부의 this값 즉, window를 출력한다.

<br>

이 외에도 생성자 함수내에서의 this 값과, 화살표함수에서 this를 어떻게 활용할 수 있는지에 대해서는 다음에 이어서 포스팅 하겠다.😅