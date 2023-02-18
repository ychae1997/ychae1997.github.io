---
title: "[JavaScript] this 키워드"
excerpt: "this에 대해 알아보자"

categories:
  - JavaScript
tags:
  - [JavaScript, ES6]

permalink: /JavaScript/this 키워드/

toc: true
toc_sticky: true

date: 2023-02-17
last_modified_at: 2023-02-17
---
영어에서 this는 '이것'을 뜻한다. 그렇다면 자바스크립트 내에서 this란 무엇일까? <br>
자바스크립트 내에서 this는 함수가 호출되는 시점의 **실행 컨텍스트(Excution Context)**이다. 여기서 실행 컨텍스트란 코드가 실행되는 환경, 실행하기 위해 필요한 환경을 말한다. <br>

이 말은 함수를 실행하는 방법 즉, 호출하는 방법이 달라지면 this도 변한다는 뜻이다. this는 **누가 호출 해는지**에 따라 달라진다. 이제부터 상황별로 this가 어떻게 바인딩 되는지 알아보자.

<br>

## 전역문맥
전역범위에서 this를 호출하면 `this`는 브라우저에서 `window`라는 전역 객체를, Node.js에서는 `Global`를 가리킨다.
```javascript
console.log(this); // window
```

<hr>

## 함수문맥
### 1. 일반 함수에서 호출
* 일반함수에서 this를 호출하면 `window`객체를 가리킨다. (default binding)

```javascript
function func() {
  return this;
}
console.log(func()); // window 객체
console.log(this) // window 객체
```

* `use strict` 모드 에서는 `undefined`가 출력된다.

```javascript
'use strict';
function func() {
  return this;
}
console.log(func()); // undefined
```

사실 일반함수 안의 this는 해당 함수를 담고 있는 객체를 가리킨다.
위에서 일반함수에서 this는 window를 가리킨다고 했는데 그 이유는
함수나 변수를 전역공간에 만들면, window라는 전역객체에 보관되기 때문이다.
2번에서 자세히 알아보자.

<br>

### 2. 객체의 메서드
* 메서드 내부에서 this는 해당 메서드를 `호출한 객체`에 바인딩된다.

```javascript
const obj = {
  namd : 'kim',
  func : function() {
    console.log(this);
  }
}
obj.func(); // {name: 'kim', func: ƒ}
```

* 함수를 객체외부(전역) 에서 선언하고, 객체 안에서 호출하는 경우에도 호출한 객체에 this가 바인딩된다.

```javascript
function hello(){
  console.log(this);
}
const obj = {
  name : 'kim'
}

// 1. 함수를 그냥 호출했을 때 -> window
hello(); // window

// 🌟 2. 객체에 hello라는 속성에 hello() 추가 -> obj 객체
obj.hello = hello;
obj.hello(); // {name: 'kim', hello: ƒ}}

```

* 다른 객체에서 선언된 함수 또한 호출한 객체에 this가 바인딩된다.

```javascript
const obj1 = {
  name : 'kim',
  func() {
    console.log(this);
  }
}
const obj2 = {
  name : 'lee'
}

// obj2에 func라는 속성을 만들고 obj1의 func함수를 할당
obj2.func = obj1.func;
obj1.func(); // {name: 'kim', func: ƒ}
obj2.func(); // {name: 'lee', func: ƒ}
```

* 그렇다면 객체의 함수를 외부에서 호출하면 어떻게 될까?

```javascript
const obj = {
  name : 'kim',
  func() {
    console.log(this);
  }
}
obj.func(); // {name: 'kim', func: ƒ}

// funcThis라는 변수에 obj func메서드를 할당
let funcThis = obj.func;
funcThis(); // window
``` 
funcThis라는 함수를 담고있는 window 객체가 바인딩되는 것을 알 수 있다.

* 객체의 메서드 안에서 내부 함수를 선언하는 경우에는 어떻게 될까?

```javascript
const obj = {
  name : 'kim',
  func(){
    function hello() {
      return this;
    }
    console.log(hello());
  }
}
obj.func(); // window
```
객체의 메서드와는 다르게 `window`를 가리키는 것을 알 수 있다. 이는 자바스크립트에서 내부 함수를 호출하는 패턴을 정의해 놓지 않아 일반함수로 취급되기 때문이다. 즉, 설계오류인것! 그렇다면 this를 내가 의도한 대로 바인딩하는 방법은 무엇이 있을까? 

<br>

### 3. call(), apply(), bind()
위의 문제를 해결하기 위해 자바스크립트에선 this를 정확히 원하는 객체에 `명시적으로 바인딩`하는 함수를 제공한다.

#### call()
call은 함수의 `첫번째 인자`로 전달하는 값에 this를 바인딩한다.

```javascript
function userInfo(num1, num2){
  console.log('이름: ' + this.name); // 이름: kim
  console.log('나이: ' + this.age); // 나이: age
  console.log(num1 + num2); // 3
}

const user = {
  name : 'kim',
  age : 20
}

userInfo.call(user, 1, 2);
```
전역객체(window)가 아닌 user 객체에 바인딩 된 것을 알 수 있다.

<br>

#### apply()
apply는 call과 비슷하지만 배열 형태로 인자를 전달한다.

```javascript
function userInfo(im, hi){
  console.log(`${im} ${this.name}이야 ${hi}`); // 나는 kim이야 반가워!
}

const user = {
  name : 'kim',
  age : 20
}

const sayHi = ['나는', '반가워!']
userInfo.apply(user, sayHi);
```
call을 이용해 배열을 전달하고 싶다면 spread문법을 사용하면된다.
```javascript
userInfo.call(user, ...sayHi);
```

<br>

#### bind()
bind는 함수를 호출하지 않고, 첫번째 인자에 this를 바인딩한 `새로운 함수`를 반환한다.

```javascript
function userInfo(num1, num2){
  console.log('이름: ' + this.name); // 이름: kim
  console.log('나이: ' + this.age); // 나이: age
  console.log(num1 + num2); // 3
}

const user = {
  name : 'kim',
  age : 20 
}

const kim = userInfo.bind(user, 1);
kim(2);
```
apply, call과는 달리 함수를 호출하는 것이 아닌 새로운 함수를 반환하기 때문에 콜백함수가 될 수 있다.

```javascript
function userInfo(hi){
  console.log(`안녕 나는 ${this.name}야! ${hi}`)
}
const user = {
  name : 'Mark'
}
const mark = userInfo.bind(user, '친하게 지내자ㅎㅎ')
setTimeout(mark, 1000);
```

<br>

### 4. 생성자 함수 / Class
생성자 함수나 Class 통해 만들어진 객체의 this에는 해당 인스턴스가 바인딩된다.

* 생성자 함수

```javascript
function User() { // constructor
  this.name = 'kim',
  this.func = function(){
    console.log('이름: ' + this.name);
  }
}
let kim = new User();
console.log(kim); // {name: 'kim', func: ƒ}
kim.func(); // 이름: kim
```

* Class

```javascript
class User {
  constructor(name) {
    this.name = name;
  }
  hello() {
    console.log(`안녕 내 이름음 ${this.name}!`)
  }
}

let mark = new User('Mark');
mark.hello(); // 안녕 내 이름은 Mark!
```
<br>

### 5. 이벤트리스너 this -> 
이벤트리스너 안의 thiss는 `e.currentTarget`을 가리킨다.

```javascript
document.getElementById('btn').addEventListener('click', function(e) {
  console.log(this); // e.currentTarget;
  console.log(e.currentTarget === this); // true
});
```
<br>

### 6. 🚨 주의해야 할 화살표함수
화살표함수는 일반함수와 조금 다르게 동작한다. 먼저 MDN을 살펴보자.
> - ES2015는 스스로의 this 바인딩을 제공하지 않는 화살표 함수를 추가했습니다(이는 렉시컬 컨텍스트안의 this값을 유지합니다).
> - 화살표 함수에서 this는 자신을 감싼 정적 범위입니다. 전역 코드에서는 전역 객체를 가리킵니다

이게 대체 뭔 소릴까..?

화살표 함수는 this를 가지지 않는다. 즉, 화살표 함수는 내부의 this값을 변화시키지 않고, `상위의 this값을 그대로 재사용`한다. 예시를 통해 더 자세히 알아보자

```javascript
const obj = {
  namd : 'kim',
  func : function() {
    console.log(this);
  },
  arrow : () => {
    console.log(this);
  }
}
obj.func(); // {name: 'kim', func: ƒ}
obj.arrow(); // 🌟 window
```
위의 객체 메서드 안에서 일반함수의 this는 메서드를 가진 객체를 가리켰다. 여기서 일반함수를 화살표함수로 바꾸면 해당함수에는 this가 없고 그 상위 컨텍스트인 window를 참조하게 된다. 때문에 사용에 유의해야한다.

이런 화살표함수의 특징은 콜백함수에서 활용하면 좋다.

* 예제1
```javascript
const obj = {
  name : 'kim',
  func(){
    function hello() {
      return this;
    }
    console.log(hello());
  },
  arrow(){
    hello = () => {
      return this;
    }
    console.log(hello());
  }
}
obj.func(); // window
obj.arrow(); // 🌟 {name: 'kim', func: ƒ, arrow: ƒ}
```

* 예제2
```javascript
const user = {
  name : ['kim', 'lee', 'park'],
  func(){
    user.name.forEach(function(){
      console.log(this);
    });
  },
  arrow(){
    user.name.forEach(() => {
      console.log(this) 
    })
  }
}
user.func(); // window 3번 반복
user.arrow(); // 🌟 {name: Array(3), func: ƒ} 3번 반복 
```

* 예제3
```javascript
let user = {
  name : 'mark',
  func(){
    setTimeout(function(){
      console.log(`반가워 내 이름은 ${this.name}!`);
    }, 1000)
  },
  arrow(){
    setTimeout(() => {
      console.log(`안녕 나는 ${this.name}!`);
    }, 1000)
  }
}
user.func(); // 반가워 내 이름은 !
user.arrow(); // 🌟 안녕 나는 Mark!
```