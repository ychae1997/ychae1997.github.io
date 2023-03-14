---
title: "[알고리즘 / 자료구조] Queue"
excerpt: "Queue을 활용한 알고리즘 문제"

categories:
  - Algorithm
tags:
  - [Queue, 자료구조, 알고리즘]

permalink: /Algorithm/Queue

toc: true
toc_sticky: true

date: 2023-03-15
last_modified_at: 2023-03-15
---
<hr>

## 1. Implementation Queue
> Queue 구현을 위한 기본적인 코드가 작성되어 있습니다. Queue 자료구조의 특성을 이해하고 FILL_ME_IN 을 채워 테스트를 통과해주세요.

<h4 class="sub-title">✅ 멤버 변수</h4>
- 데이터를 저장할 `Object` 타입의 storage
- 큐의 가장 앞을 가리키는 `Number` 타입의 포인터 front
- 큐의 가장 뒤를 가리키는 `Number` 타입의 포인터 rear

<h4 class="sub-title">✅ 메서드</h4>
- size(): 큐에 추가된 데이터의 크기를 리턴해야 합니다.
- enqueue(): 큐에 데이터를 추가할 수 있어야 합니다.
- dequeue(): 가장 먼저 추가된 데이터를 큐에서 삭제하고 삭제한 데이터를 리턴해야 합니다.

<h4 class="sub-title">✅ 주의 사항</h4>

내장 객체 Array.prototype에 정의된 메서드는 사용하면 안 됩니다.
포인터 변수 front, rear의 초기값은 -1, 0, 1등 임의로 지정할 수 있지만 여기서는 0으로 합니다.

<h4 class="sub-title">✅ 사용 예시</h4>

```javascript
const queue = new Queue();

queue.size(); // 0
for(let i = 1; i < 10; i++) {
  	queue.enqueue(i);
}
queue.dequeue(); // 1
queue.dequeue(); // 2
queue.size(); // 7
queue.enqueue(10);
queue.size(); // 8
queue.dequeue(); // 3
queue.dequeue(); // 4
queue.size(); // 6
...
```

<hr class="sub">

### 💡 풀이

```javascript
class Queue {
  constructor() {
    this.storage = {};
    this.front = 0;
    this.rear = 0;
  }

  // queue는 추가될 때, rear의 값이 커지고 삭제 될 때, front가 변경이 때문에, 각 값을 알아야 size를 구할 수 있습니다.
  size() {
    return this.rear - this.front;
  }
	
  // 큐에 데이터를 추가 할 수 있어야 합니다.
  // 새롭게 추가될 요소의 인덱스를 나타내는 this.rear를 키로, 요소를 값으로 하여 storage에 할당합니다. this.rear은 다음 인덱스를 가리키게 하여 새로운 요소에 대비합니다.
  enqueue(element) {
    this.storage[this.rear] = element;
    this.rear += 1;
  }
	
  // 가장 먼저 추가된 데이터가 가장 먼저 추출되어야 합니다.
  dequeue() {
    // 빈 큐에 dequeue 연산을 적용해도 에러가 발생하지 않아야 합니다
    if (!this.size()) {
      return;
    }

    // queue에서 element를 제거 한 뒤 해당 element를 반환합니다.
    // this.front+1로 가장 앞에 있는 요소를 다시 설정한 후 변수에 저장하고, 큐에서 삭제합니다.
    const result = this.storage[this.front];
    delete this.storage[this.front];
    this.front += 1;

    return result;
  }
}
```

## 2. 박스 포장
>
마트에서 장을 보고 박스를 포장하려고 합니다. 박스를 포장하는 데는 폭이 너무 좁습니다. 그렇기에 한 줄로 서 있어야 하고, **들어온 순서대로 한 명씩 나가야 합니다.**
<br><br>
불행 중 다행은, 인원에 맞게 포장할 수 있는 기구들이 놓여 있어, 모두가 포장을 할 수 있다는 것입니다. 짐이 많은 사람은 짐이 적은 사람보다 포장하는 시간이 길 수밖에 없습니다.
<br><br>
**뒷사람이 포장을 전부 끝냈어도 앞사람이 끝내지 못하면 기다려야 합니다.** 앞사람이 포장을 끝내면, 포장을 마친 뒷사람들과 함께 한 번에 나가게 됩니다.
<br><br>
만약, 앞사람의 박스는 5 개고, 뒷사람 1의 박스는 4 개, 뒷사람 2의 박스는 8 개라고 가정했을 때, 뒷사람 1이 제일 먼저 박스 포장을 끝내게 되어도 앞사람 1의 포장이 마칠 때까지 기다렸다가 같이 나가게 됩니다.
<br><br>
이때, 통틀어 **최대 몇 명이 한꺼번에 나가는지 알 수 있도록** 함수를 구현해 주세요.


<h4 class="sub-title">✅ 입력</h4>
**인자 1 : boxes**
- `Number` 타입을 요소로 갖는, 포장해야 하는 박스가 담긴 배열
  - 1 ≤ 사람 수 ≤ 10,000
  - 1 ≤ 박스 ≤ 10,000

<h4 class="sub-title">✅ 출력</h4>
- `Number` 타입을 리턴해야 합니다.

<h4 class="sub-title">✅ 주의 사항</h4>
- 먼저 포장을 전부 끝낸 사람이 있더라도, 앞사람이 포장을 끝내지 않았다면 나갈 수 없습니다.

<h4 class="sub-title">✅ 입출력 예시</h4>
만약 5, 1, 4, 6이라는 배열이 왔을 때, 5개의 박스를 포장하는 동안 1, 4 개의 박스는 포장을 끝내고 기다리게 되고, 6 개의 박스는 포장이 진행 중이기 때문에, 5, 1, 4 세 개가 같이 나가고, 6이 따로 나가게 됩니다. 그렇기에 최대 3 명이 같이 나가게 됩니다.

```javascript
const boxes = [5, 1, 4, 6];
const output = paveBox(boxes);
console.log(output); // 3

const boxes2 = [1, 5, 7, 9];
const output2 = paveBox(boxes2);
console.log(output2); // 1
```

<hr class="sub">

### 💡 풀이

<h4 class="sub-title">✅ 나의 풀이</h4>

```javascript
function paveBox(boxes) {
  // TODO: 여기에 코드를 작성합니다.
  const queue = [];
  
  while(boxes.length > 0) {
    let rearIdx = boxes.findIndex(el => boxes[0] < el);

    // 앞사람이 제일 많으면 뒷사람 모두가 기다렸다가 같이 나감
    if(rearIdx === - 1) {
      queue.push(boxes.length)
      boxes.splice(0, boxes.length)
    }
    else {
      // 제거된 배열의 길이 = 한 번에 나간 사람의 수
      queue.push(rearIdx)
      boxes.splice(0, rearIdx).length;
    }    
  }
  return Math.max(...queue)
}
```

<h4 class="sub-title">✅ 레퍼런스</h4>

```javascript
function paveBox(boxes) {
  let answer = [];
  
  // boxes 배열이 0보다 클 때까지 반복합니다.
  while(boxes.length > 0){
    let finishIndex = boxes.findIndex(fn => boxes[0] < fn);
    
    if(finishIndex === -1){
      // 만약 찾지 못했다면 answer에 boxes 배열의 길이를 넣은 후, boxes 내부의 요소를 전부 삭제합니다.
      answer.push(boxes.length);
      boxes.splice(0, boxes.length);
    } else {
      // 만약 찾았다면, 해당 인덱스를 answer에 넣고, boxes에서 그만큼 제외합니다.
      answer.push(finishIndex);
      boxes.splice(0, finishIndex);
    }
  }
  // 결과물을 반환합니다.
  return Math.max(...answer);
}
```

