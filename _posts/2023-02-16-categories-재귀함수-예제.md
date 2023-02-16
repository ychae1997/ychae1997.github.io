---
title: "[알고리즘] 코플릿 fibonacci 예제"
excerpt: "재귀함수로 피보나치 문제 풀기"

categories:
  - Algorithm
tags:
  - [JavaScript, 알고리즘]

permalink: /Algorithm/재귀함수 예제/

toc: true
toc_sticky: true

date: 2023-02-16
last_modified_at: 2023-02-16
---
## 문제
아래와 같이 정의된 피보나치 수열 중 n번째 항의 수를 리턴해야 합니다.
- 0번째 피보나치 수는 0이고, 1번째 피보나치 수는 1입니다. 그 다음 2번째 피보나치 수부터는 바로 직전의 두 피보나치 수의 합으로 정의합니다.
- 0, 1, 1, 2, 3, 5, 8, 13, 21, 34, 55, ...

> 
- 입력
   - 인자 1 : n
   - number 타입의 n (n은 0 이상의 정수)
- 출력
   - number 타입을 리턴해야합니다.
- 주의사항
   - 재귀함수를 이용해 구현해야 합니다.
   - 반복문(for, while) 사용은 금지됩니다.
   - 함수 fibonacci가 반드시 재귀함수일 필요는 없습니다.

<hr>

## 입출력 예시
```javascript
let output = fibonacci(0);
console.log(output); // --> 0

output = fibonacci(1);
console.log(output); // --> 1

output = fibonacci(5);
console.log(output); // --> 5

output = fibonacci(9);
console.log(output); // --> 34
```

<hr>

## 문제해결
### 1. recursive 
```
피보나치 n번째 수는 직전의 두 수의 합과 같다.
피보나치 4번째 수는 피보나치 3번째 수와 2번째 수의 합과 같다.

fibonacci(4) = fibonacci(3) + fibonacci(2) === (1 + 0 + 1) +  (1 + 0)
fibonacci(3) =  fibonacci(2) + fibonacci(1) === (1 + 0) + 1
fibonacci(2) =  fibonacci(1) + fibonacci(0) === 1 + 0

피보나치 1번째 수는 = 1, 0번째 수는 0 <--- base case

fibonacci(num) = fibonacci(num-1) + fibonacci(num-2) 
```
### 2. base case
0번 째 피보나치 수는 0, 1번 째 피보나치 수는 1
```javascript
if(num <= 1) return num;
```

### 3. 처음 입력한 코드
```javascript
function fibonacci(num) {
  if(num <= 1) return num;
  return fibonacci(num - 1) + fibonacci(num - 2);
}
```
이렇게 했는데 문제가 통과가 안됐다. 알고보니 아래쪽에 Advanced가 있었다.

<hr>

## Advanced
피보나치 수열을 구하는 효율적인 알고리즘(O(N))이 존재합니다. 재귀함수의 호출을 직접 관찰하여 비효율이 있는지 확인해 보시기 바랍니다.
<br><br>
위의 코드는 새로운 수가 들어오면 처음부터 다시 계산을 해줘야해서 비효율적이다. <br>
그래서 새로운 함수를 만들어서 이미 해결한 문제는 `memo`라는 배열에 넣어서 기록해 두고, 아직 풀지 않은 문제는 풀이 후에 `memo`에 기록해둔다.

### 🌟 최종코드
```javascript
function fibonacci(n) {
  const memo = [0, 1]; // 기존의 if문에서 배열로 바꿔준다.
  const aux = (n) => {
    // 1. base case
    // 이미 해결한 적 있으면, 메모해둔 정답을 리턴
    if(memo[n] !== undefined) return memo[n];
    
    // 2. recursive case
    // 새롭게 풀어야하는 경우, 문제를 풀고 메모한다.
    // 기존코드와 달리 fibonacci함수가 아닌 aux함수를 호출.
    return memo[n] = aux(n - 1) + aux(n - 2);
  }
  return aux(n);
}
```


