---
title: "[알고리즘 / 자료구조] Stack"
excerpt: "Stack을 활용한 알고리즘 문제"

categories:
  - Algorithm
tags:
  - [Stack, 자료구조, 알고리즘]

permalink: /Algorithm/Stack

toc: true
toc_sticky: true

date: 2023-03-15
last_modified_at: 2023-03-15
---
<hr>

## 1. Implementation Stack
> Stack 구현을 위한 기본적인 코드가 작성되어 있습니다. Stack 자료구조의 특성을 이해하고 FILL_ME_IN을 채워 테스트를 통과해 주세요.

<h4 class="sub-title">✅ 멤버 변수</h4>
- 데이터를 저장할 `Object` 타입의 storage
- 마지막에 들어온 데이터를 가리키는 `Number` 타입의 포인터 top

<h4 class="sub-title">✅ 메서드</h4>
- size(): 스택에 추가된 데이터의 크기를 리턴해야 합니다.
- push(): 스택에 데이터를 추가할 수 있어야 합니다.
- pop(): 가장 나중에 추가된 데이터를 스택에서 삭제하고 삭제한 데이터를 리턴해야 합니다.

<h4 class="sub-title">✅ 주의 사항</h4>

- 내장 객체 Array.prototype에 정의된 메서드는 사용하면 안 됩니다.
- 포인터 변수 top의 초기값은 -1, 0, 1등 임의로 지정할 수 있지만, <b>
여기서는 빈 스택을 나타내는 -1으로 초기화되며 이후 push, pop에 따라 1씩 증감해주어 데이터가 추가될 인덱스의 위치를 가리키도록 합니다.

<h4 class="sub-title">✅ 사용 예시</h4>

```javascript
const stack = new Stack();

stack.size(); // 0
for(let i = 1; i < 10; i++) {
  	stack.push(i);
}
stack.pop(); // 9
stack.pop(); // 8
stack.size(); // 7
stack.push(8);
stack.size(); // 8
...
```

<hr class="sub">

### 💡 풀이

```javascript
class Stack {
  constructor() {
    this.storage = {};
    this.top = -1; // 스택의 가장 상단을 가리키는 포인터 변수를 초기화 합니다.
  }

  // this.top은 스택이 쌓일 때마다 하나씩 증가하기 때문에 top으로부터 size를 구할 수 있습니다.
  // this.top은 스택에 새롭게 추가될 요소의 인덱스를 나타냅니다. 빈 스택을 표현하는 -1부터 1씩 증감하며 표현하며 전체 요소의 개수를 추정할 수 있습니다
  size() {
    return this.top + 1;
  }

  // 현재 추가하는 element의 인덱스인 this.top을 키로, 요소를 값으로 하여 storage에 할당합니다.
  push(element) {
    this.top += 1;
    this.storage[this.top] = element;
  }
	
  // 가장 나중에 추가된 데이터가 가장 먼저 추출되어야 합니다.
  pop() {
    // 빈 스택에 pop 연산을 적용해도 에러가 발생하지 않아야 합니다
    // 만약 size가 0보다 작거나 같다면 이는 비어있는 스택을 의미하므로 아무 일도 일어나지 않습니다.
    if (!this.size()) {
      return;
    }

    // stack에서 현재 stack의 최상단에 있는 element를 변수에 저장합니다.
    // storage에서 해당 element를 제거합니다.
    // 하나를 제거했으니 방금 제거한 element의 인덱스를 나타내는 top 또한 감소시켜 업데이트 해줍니다.
    // 최상단에 있던 element가 저장된 변수 result를 반환합니다.
    const result = this.storage[this.top];
    delete this.storage[this.top];
    this.top -= 1;

    return result;
  }
}
```

## 2. 유효한 괄호쌍
> 입력된 괄호 값들이 모두 쌍이 맞게 올바른지를 판단해 모두 쌍이 맞으면 true 그렇지 않으면 false를 출력하세요.
<br>
입력된 괄호 값들이 유효한 경우들은 다음에 해당합니다.
1. 열린 괄호는 같은 타입의 닫힌 괄호로 닫혀있어야 한다.
2. 열린 괄호는 올바른 순서대로 닫혀야만 한다.
3. 모든 닫힌 괄호는 그에 상응하는 같은 타입의 열린 괄호를 갖고 있다.
<br>
입력값을 통해 들어오는 괄호는 ()[]{}로만 이루어져 있습니다.

<h4 class="sub-title">✅ 입력</h4>
**인자 1 : str**
- `string` 타입으로 된 문장

<h4 class="sub-title">✅ 출력</h4>
- boolean 타입을 리턴해야 합니다.

<h4 class="sub-title">✅ 주의 사항</h4>
- 입력값을 통해 들어오는 괄호는 ()[]{}로만 이루어져 있습니다.
- 입력값으로 들어오는 str의 길이는 0부터 10^4승 까지 입니다.

<h4 class="sub-title">✅ 입출력 예시</h4>

```javascript
const result1 = isValid('[]');
console.log(result1); // true

const result2 = isValid('[)');
console.log(result2); // false

const result3 = isValid('[]{}()');
console.log(result3); // true

const result4 = isValid('[]{)()');
console.log(result4); // false
```

<h4 class="sub-title">🔑 힌트</h4>
- 스택을 사용해 보세요.
- 열린 괄호인 경우 스택에 push합니다.
- 닫힌 괄호인 경우에는 스택의 top을 확인하고, 해당 닫힌 괄호와 짝이 맞는 열린 괄호인지를 확인합니다.

<hr class="sub">

### 💡 풀이

<h4 class="sub-title">✅ 나의 풀이</h4>

```javascript
const isValid = (str) => {
  const stack = [];
  let result = []
  
  // 한쌍이 아니거나 빈값일 때
  if(str.length < 1) return false;

  for(let i = 0; i < str.length; i++) {
    if(str[i] === '(' || str[i] === '[' || str[i] === '{'){
      stack.push(str[i])
    }else{
      // 스택의 top을 확인하고, 해당 닫힌 괄호와 짝이 맞는 열린 괄호인지를 확인
      switch(stack.pop()){
        case '(' : str[i] === ')' ? result.push(true) : result.push(false)
            break;
        case '[' : str[i] === ']' ? result.push(true) : result.push(false)
            break;
        case '{' : str[i] === '}' ? result.push(true) : result.push(false)
            break;
        default : return false;
      }
    }
  }
  
  // 열린 괄호가 연속으로 들어왔을 때 false
  // stack의 길이는 0이어야한다 -> 열린괄호가 연속으로 들어왔을 경우 stack의 길이는 0이 아니게 됨
  if (stack.length) return false;

  return result.includes(false) ? false : true
}
```

<h4 class="sub-title">✅ 레퍼런스</h4>

```javascript
const isValid = (str) => {
  // 최초 입력값이 빈 값이라면 유효하지 않은 괄호쌍으로 간주합니다.
  if (str.length === 0) return false;

  // 각각의 여는 괄호에 알맞는 닫는 괄호를 매칭시키기 위한 괄호맵 생성.
  const braketsMap = {
    '(': ')',
    '[': ']',
    '{': '}'
  };
  const arr = str.split('');
  const stack = [];
  for (let i = 0; i < arr.length; ++i) {
    // 1. 여는 괄호 -> stack에 push해야 하는 케이스
    if (arr[i] === '(' || arr[i] === '[' || arr[i] === '{') {
      stack.push(arr[i]);
    } else {
      // 2. 닫는 괄호 -> stack에서 pop해야 하는 케이스
      // 먼저 현재 stack의 가장 위에 위치한 괄호를 확인하고
      const lastElementOfStack = stack[stack.length - 1];
      
      // 지금 처리해야하는 닫힌 괄호인 arr[i]의 짝과 맞는지를 확인 후 맞지 않다면 false를 리턴하고 로직 전체를 종료
      if (braketsMap[lastElementOfStack] !== arr[i]) return false;
      
      // 짝이 맞다면 현재 stack의 가장 위에 위치한 괄호를 pop시켜서 stack에서 제거해줌
      stack.pop();
    }
  }
  // arr 배열전체를 돌면서 해당 로직을 이행한 후 stack에 어떠한 열린괄호라도 남아있다면 쌍이 맞지 않는 괄호들이 인풋으로 들어왔기 때문에 false를 반환,
  // 그렇지 않고 stack이 비어져 있다면 모든 괄호쌍이 문제없이 유효한 괄호쌍이므로 true를 반환
  return stack.length ? false : true;
};
```

