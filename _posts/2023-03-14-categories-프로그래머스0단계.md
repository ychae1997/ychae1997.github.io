---
title: "[알고리즘] 프로그래머스 0단계 (1)"
excerpt: "프로그래머스 0단계 - Day1 ~ Day5"

categories:
  - Algorithm
tags:
  - [JavaScript, 알고리즘]

permalink: /Algorithm/프로그래머스 0단계(1)

toc: true
toc_sticky: true

date: 2023-03-14
last_modified_at: 2023-03-14
---
<hr>

## 1. 몫 구하기
> 정수 num1, num2가 매개변수로 주어질 때, num1을 num2로 나눈 몫을 return 하도록 solution 함수를 완성해주세요.

<h4 class="sub-title">✅ 제한사항</h4>
- 0 < num1 ≤ 100
- 0 < num2 ≤ 100

<h4 class="sub-title">✅ 입출력 예</h4>

|num1|num2|result|
|:--:|:--:|:--:|
|10|5|2|
|7|2|3|

<h4 class="sub-title">✅ 입출력 예 설명</h4>

`입출력 예 #1`
- num1이 10, num2가 5이므로 10을 5로 나눈 몫 2를 return 합니다.

`입출력 예 #2`
- num1이 7, num2가 2이므로 7을 2로 나눈 몫 3을 return 합니다.

<hr class="sub">

### 💡 풀이
<h4 class="sub-title">✅ 나의 풀이</h4>

```javascript
function solution(num1, num2) {
  var answer = 0;
  answer = Math.floor(num1 / num2);
  return answer;
}
```

<h4 class="sub-title">✅ 다른 사람 풀이</h4>

```javascript
function solution(num1, num2) {
  return ~~(num1/num2);
}
```
틸트 연산자를 사용했다.

<hr class="sub">

#### 📌 Tilt(~) 연산자
`틸트(Tilt)`연산자는 내부적으로 32비트 정수로 변환 후 `NOT`연산자를 실행한다. 
- 2의 보수 `-(n+1)`와 같은 동작

```javascript
9
~9 // -10

~(-1) // 0
~(-2) // 1
```
- Tilt(~) 두 개 사용해 비트 잘라내기

Tilt(~)를 사용하면 숫자의 소수점을 잘라내게 되므로, Tilt(~)를 두 번 사용해 소수점만 자른 원래 상태로 되돌린다. <br>
`양수`에서 사용시 `Math.floor()`와 동일한 결과값을 내놓지만, `음수`에서는 결과값이 달라짐

```javascript
~~12.3 // 12
Math.floor(12.3) // 12

~~-12.3 // -12
Math.floor(-12.3) // -13
```

<hr>

## 2. 분수의 덧셈
> 첫 번째 분수의 분자와 분모를 뜻하는 `numer1`, `denom1`, 두 번째 분수의 분자와 분모를 뜻하는 `numer2`, `denom2`가 매개변수로 주어집니다. 두 분수를 더한 값을 기약 분수로 나타냈을 때 분자와 분모를 순서대로 담은 배열을 return 하도록 solution 함수를 완성해보세요.

<h4 class="sub-title">✅ 제한사항</h4>
- 0 < `numer1`, `denom1`, `numer2`, `denom2` < 1,000

<h4 class="sub-title">✅ 입출력 예</h4>

|number1|denom1|number2|denom2|result|
|:--:|:--:|:--:|:--:|:--:|
|1|2|3|4|[5,4]|
|9|2|1|3|[29,6]|

<h4 class="sub-title">✅ 입출력 예 설명</h4>
`입출력 예 #1`
- 1 / 2 + 3 / 4 = 5 / 4입니다. 따라서 [5, 4]를 return 합니다.

`입출력 예 #2`
- 9 / 2 + 1 / 3 = 29 / 6입니다. 따라서 [29, 6]을 return 합니다.

<hr class="sub">

### 💡 풀이
<h4 class="sub-title">✅ 나의 풀이</h4>

```javascript
function solution(numer1, denom1, numer2, denom2) {
  let numer = (numer1 * denom2) + (numer2 * denom1);
  let denom = denom1 * denom2;
  let gcd = (numer, denom) => (numer % denom === 0 ? denom : gcd(denom, (numer % denom)));
  let min = gcd(numer, denom)
  return [numer/min, denom/min]
}
```

<h4 class="sub-title">✅ 다른 사람 풀이</h4>

```javascript
function fnGCD(a, b){
  return (a%b)? fnGCD(b, a%b) : b;
}

function solution(denum1, num1, denum2, num2) {
  let denum = denum1*num2 + denum2*num1;
  let num = num1 * num2;
  let gcd = fnGCD(denum, num); //최대공약수
  return [denum/gcd, num/gcd];
}

```
<hr>

## 3. 배열 두배 만들기
> 정수 배열 `numbers`가 매개변수로 주어집니다. `numbers`의 각 원소에 두배한 원소를 가진 배열을 return하도록 solution 함수를 완성해주세요.

<h4 class="sub-title">✅ 제한사항</h4>
- -10,000 ≤ `numbers`의 원소 ≤ 10,000
- 1 ≤ `numbers`의 길이 ≤ 1,000

<h4 class="sub-title">✅ 입출력 예</h4>

|numbers|result|
|:--:|:--:|
|[1, 2, 3, 4, 5]|[2, 4, 6, 8, 10]|
|[1, 2, 100, -99, 1, 2, 3]|[2, 4, 200, -198, 2, 4, 6]|

<h4 class="sub-title">✅ 입출력 예 설명</h4>
`입출력 예 #1`
- [1, 2, 3, 4, 5]의 각 원소에 두배를 한 배열 [2, 4, 6, 8, 10]을 return합니다.

`입출력 예 #2`
- [1, 2, 100, -99, 1, 2, 3]의 각 원소에 두배를 한 배열 [2, 4, 200, -198, 2, 4, 6]을 return합니다.

<hr class="sub">

### 💡 풀이
<h4 class="sub-title">✅ 나의 풀이</h4>

```javascript
const solution = (numbers) => numbers.map(el => el * 2);
```

<h4 class="sub-title">✅ 다른 사람 풀이</h4>

```javascript
function solution(numbers) {
    return numbers.reduce((a, b) => [...a, b * 2], []);
}
```
map이 아닌 reduce를 사용해서 풀었다.

<hr>

## 3. 최빈값 구하기
> 최빈값은 주어진 값 중에서 가장 자주 나오는 값을 의미합니다. 정수 배열 `array`가 매개변수로 주어질 때, 최빈값을 return 하도록 solution 함수를 완성해보세요. 최빈값이 여러 개면 -1을 return 합니다.

<h4 class="sub-title">✅ 제한사항</h4>
- 0 < `array`의 길이 < 100
- 0 ≤ `array`의 원소 < 1000

<h4 class="sub-title">✅ 입출력 예</h4>

|array|result|
|:--:|:--:|
|[1, 2, 3, 3, 3, 4]|3|
|[1, 1, 2, 2]|-1|
|[1]|-1|

<h4 class="sub-title">✅ 입출력 예 설명</h4>
`입출력 예 #1`
- [1, 2, 3, 3, 3, 4]에서 1은 1개 2는 1개 3은 3개 4는 1개로 최빈값은 3입니다.

`입출력 예 #2`
- [1, 1, 2, 2]에서 1은 2개 2는 2개로 최빈값이 1, 2입니다. 최빈값이 여러 개이므로 -1을 return 합니다.

`입출력 예 #3`
- [1]에는 1만 있으므로 최빈값은 1입니다.

<hr class="sub">

### 💡 풀이

```javascript
function solution(array) {
  let m = new Map();
  for (let n of array) m.set(n, (m.get(n) || 0)+1);
  m = [...m].sort((a,b)=>b[1]-a[1]);
  return m.length === 1 || m[0][1] > m[1][1] ? m[0][0] : -1;
}
```
```javascript
//https://github.com/codeisneverodd/programmers-coding-test
//완벽한 정답이 아닙니다.
//정답 1 - codeisneverodd
function solution(array) {
  const counts = array.reduce((a, c) => (a[c] ? { ...a, [c]: a[c] + 1 } : { ...a, [c]: 1 }), {});
  const max = Math.max(...Object.values(counts));
  const modes = Object.keys(counts).filter(key => counts[key] === max);
  return modes.length === 1 ? +modes[0] : -1;
}
```
<hr>

## 4. 피자 나눠 먹기(2)
> 머쓱이네 피자가게는 피자를 여섯 조각으로 잘라 줍니다. 피자를 나눠먹을 사람의 수 `n`이 매개변수로 주어질 때, `n`명이 주문한 피자를 남기지 않고 모두 같은 수의 피자 조각을 먹어야 한다면 최소 몇 판을 시켜야 하는지를 return 하도록 solution 함수를 완성해보세요.

<h4 class="sub-title">✅ 제한사항</h4>
- 1 ≤ `n` ≤ 100

<h4 class="sub-title">✅ 입출력 예</h4>

|n|result|
|:--:|:--:|
|6|1|
|10|5|
|4|2|

<h4 class="sub-title">✅ 입출력 예 설명</h4>
`입출력 예 #1`
- 6명이 모두 같은 양을 먹기 위해 한 판을 시켜야 피자가 6조각으로 모두 한 조각씩 먹을 수 있습니다.

`입출력 예 #2`
- 10명이 모두 같은 양을 먹기 위해 최소 5판을 시켜야 피자가 30조각으로 모두 세 조각씩 먹을 수 있습니다.

`입출력 예 #3`
- 4명이 모두 같은 양을 먹기 위해 최소 2판을 시키면 피자가 12조각으로 모두 세 조각씩 먹을 수 있습니다.

<hr class="sub">

### 💡 풀이
<h4 class="sub-title">✅ 나의 풀이</h4>

```javascript
const getLcm = (num1, num2) => {
 let lcm = 1;
 while(1){
   if(!(lcm % num1) && !(lcm % num2)) break;
   
   lcm++
 }   
  return lcm
}

const solution = (n) => getLcm(n, 6) / 6;
```

<h4 class="sub-title">✅ 다른 사람 풀이</h4>

```javascript
const solution = (n) => {
  let piece = 6
  while(true) {
    if (piece % n === 0) {
      break
    }
    piece += 6
  }
  return piece / 6
}
```

<hr>

## 5. 옷가게 할인 받기
> 머쓱이네 옷가게는 10만 원 이상 사면 5%, 30만 원 이상 사면 10%, 50만 원 이상 사면 20%를 할인해줍니다. <br>
구매한 옷의 가격 `price`가 주어질 때, 지불해야 할 금액을 return 하도록 solution 함수를 완성해보세요.

<h4 class="sub-title">✅ 제한사항</h4>
- 10 ≤ `price` ≤ 1,000,000
  - `price`는 10원 단위로(1의 자리가 0) 주어집니다.
- 소수점 이하를 버린 정수를 return합니다.

<h4 class="sub-title">✅ 입출력 예</h4>

|n|result|
|:--:|:--:|
|150,000|142,500|
|580,000|464,000|

<h4 class="sub-title">✅ 입출력 예 설명</h4>
`입출력 예 #1`
- 150,000원에서 5%를 할인한 142,500원을 return 합니다.

`입출력 예 #2`
- 580,000원에서 20%를 할인한 464,000원을 return 합니다.

<hr class="sub">

### 💡 풀이
<h4 class="sub-title">✅ 나의 풀이</h4>

```javascript
function solution(price) {
  if(price < 100000) return price;
  
  let n = 0;
  if(price >= 500000) n = 20;
  else if(price >= 300000) n = 10;
  else if(price >= 100000) n = 5;
  return ~~(price - (price * (n / 100)));
}
```

<h4 class="sub-title">✅ 다른 사람 풀이</h4>

```javascript
const discounts = [
    [500000, 20],
    [300000, 10],
    [100000,  5],
]

const solution = (price) => {
  for (const discount of discounts){
    if (price >= discount[0]) return Math.floor(price - price * discount[1] / 100)
  }
  return price
}
```
조건 더 추가하기 좋아보임

<hr>

## 6. 나이 출력
> 머쓱이는 40살인 선생님이 몇 년도에 태어났는지 궁금해졌습니다. 나이 `age`가 주어질 때, 2022년을 기준 출생 연도를 return 하는 solution 함수를 완성해주세요.

<h4 class="sub-title">✅ 제한사항</h4>
- 0 < `age` ≤ 120
- 나이는 태어난 연도에 1살이며 1년마다 1씩 증가합니다.

<h4 class="sub-title">✅ 입출력 예</h4>

|age|result|
|:--:|:--:|
|40|1983|
|23|2000|

<h4 class="sub-title">✅ 입출력 예 설명</h4>
`입출력 예 #1`
- 2022년 기준 40살이므로 1983년생입니다.

`입출력 예 #2`
- 2022년 기준 23살이므로 2000년생입니다.

<hr class="sub">

### 💡 풀이
<h4 class="sub-title">✅ 나의 풀이</h4>

```javascript
const solution = (age) => 2022 - age + 1;
```

<h4 class="sub-title">✅ 다른 사람 풀이</h4>

```javascript
function solution(age) {
    return new Date().getFullYear() - age + 1;
}
```
Date함수 활용

<hr>

## 7. 배열 뒤집기
> 정수가 들어 있는 배열 `num_list`가 매개변수로 주어집니다. `num_list`의 원소의 순서를 거꾸로 뒤집은 배열을 return하도록 solution 함수를 완성해주세요.

<h4 class="sub-title">✅ 제한사항</h4>
- 1 ≤ `num_list`의 길이 ≤ 1,000
- 0 ≤ `num_list`의 원소 ≤ 1,000

<h4 class="sub-title">✅ 입출력 예</h4>
|num_list|result|
|:--:|:--:|
[1, 2, 3, 4, 5] |	[5, 4, 3, 2, 1]
[1, 1, 1, 1, 1, 2] |	[2, 1, 1, 1, 1, 1]
[1, 0, 1, 1, 1, 3, 5] |	[5, 3, 1, 1, 1, 0, 1]

<h4 class="sub-title">✅ 입출력 예 설명</h4>
`입출력 예 #1`
- `num_list`가 [1, 2, 3, 4, 5]이므로 순서를 거꾸로 뒤집은 배열 [5, 4, 3, 2, 1]을 return합니다.

`입출력 예 #2`
- `num_list`가 [1, 1, 1, 1, 1, 2]이므로 순서를 거꾸로 뒤집은 배열 [2, 1, 1, 1, 1, 1]을 return합니다.

`입출력 예 #3`
- `num_list`가 [1, 0, 1, 1, 1, 3, 5]이므로 순서를 거꾸로 뒤집은 배열 [5, 3, 1, 1, 1, 0, 1]을 return합니다.


<hr class="sub">

### 💡 풀이
<h4 class="sub-title">✅ 나의 풀이</h4>

```javascript
const solution = (num_list) => num_list.reverse();
```

<h4 class="sub-title">✅ 다른 사람 풀이</h4>

```javascript
// for문, pop() 활용
function solution(num_list) {
  var answer = [];
  var j = num_list.length
  for(var i = 1; i <= j; i++){
    answer.push(num_list.pop())
  }
  return answer;
}
```
```javascript
// sort 활용
function solution(num_list) {
  return num_list.sort((a, b) => -1);
}
```