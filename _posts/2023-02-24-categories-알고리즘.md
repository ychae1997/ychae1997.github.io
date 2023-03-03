---
title: "[알고리즘] 프로그래머스 0단계"
excerpt: "프로그래머스 0단계"

categories:
  - Algorithm
tags:
  - [JavaScript, 알고리즘]

permalink: /Algorithm/프로그래머스 0단계

toc: true
toc_sticky: true

date: 2023-02-24
last_modified_at: 2023-02-24
---
<hr>

## 몫 구하기
* 나의 풀이

```javascript
function solution(num1, num2) {
    var answer = 0;
    answer = Math.floor(num1 / num2);
    return answer;
}
```

* 인상깊었던 풀이

```javascript
function solution(num1, num2) {
    return ~~(num1/num2);
}
```
틸트연산자 사용

<hr>

## 나이구하기
* 나의 풀이

```javascript
const solution = (age) => 2022 - age + 1;
```

* 인상깊었던 풀이

```javascript
function solution(age) {
    return new Date().getFullYear() - age + 1;
}
```
<hr>

## 두 수의 나눗셈
* 나의 풀이

```javascript
const solution = (num1, num2) => ~~(num1 / num2 * 1000);
```
새롭게 배운 틸트연산자 활용

<hr>

## 배열의 두 배 만들기
* 나의 풀이

```javascript
const solution = (numbers) => numbers.map(el => el * 2);
```

* 인상깊었던 풀이

```javascript
function solution(numbers) {
    return numbers.reduce((a, b) => [...a, b * 2], []);
}
```
대부분이 map을 사용했는데 reduce를 사용한 분이 계셨다.

<hr>

## 짝수 홀수 개수
* 나의 풀이
```javascript
function solution(num_list) {
    let even = 0, odd = 0;
    for(let num of num_list) {
        num % 2 ?  odd++ : even++
    }
    return [even, odd]
}
```

* 인상깊었던 풀이

```javascript
function solution(num_list) {
    var answer = [0,0];

    for(let a of num_list){
        answer[a%2] += 1
    }

    return answer;
}
```