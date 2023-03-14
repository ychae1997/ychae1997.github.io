---
title: "[자료구조] Stack, Queue"
excerpt: "선형구조 중 Stack과 Queue에 대해 알아보자"

categories:
  - Data Structure
tags:
  - [Stack, Queue, 자료구조]

permalink: /Data Structure/Stack_and_Queue

toc: false
toc_sticky: false

date: 2023-03-15
last_modified_at: 2023-03-15
---
<hr>

## 📚 Stack
>
- **스택(Stack):** 쌓다, 쌓이다, 포개지다.
- 직역 그대로, **데이터(data)를 순서대로 쌓는 자료구조**를 말한다.

<img src="/assets/images/posts_img/stack_and_queue/stack1.jpg">

프링글스 통을 예로 들어 보자. 프링글스 통을 자료구조 Stack, 과자를 데이터(data)로 비유할 수 있다.
가장 먼저 들어간 과자는 가장 나중에 나올 수 있다. 또한 가장 나중에 넣었던, 원통 상단에 위치한 과자를 가장 먼저 뺄 수 있다.

<hr class="sub">

### ✅ Stack 구조
- 자료구조 Stack의 특징은 입력과 출력이 하나의 방향, 즉 스택의 최상단에서만 이루어지는 **제한적 접근**에 있다.
- 이런 Stack 자료구조의 정책을 **LIFO(Last In First Out)** 혹은 **FILO(First In Last Out)**이라고 부른다.
- Stack에 데이터를 넣는 것을 'PUSH' 데이터를 꺼내는 것을 'POP'이라고 한다.


<hr class="sub">

### ✅  Stack 특징
#### 1. LIFO(Last In First Out)
먼저 들어간 데이터는 제일 나중에 나오는 후입선출의 구조로 되어 있다.
```
예1) 1, 2, 3, 4를 스택에 차례대로 넣습니다.

stack.push(데이터)
|  4  | <- top
|  3  |
|  2  |
|  1  |
들어간 순서대로, 1번이 제일 먼저 들어가고 4번이 마지막으로 들어가게 됩니다.

예2) 스택이 빌 때까지 데이터를 전부 빼냅니다.

stack.pop()
|    |
|    |
|    |
|    |
4, 3, 2, 1
제일 마지막에 있는 데이터부터 차례대로 나오게 됩니다.
```
- 이러한 특성으로 인해 스택 구조 내에서 특정 데이터를 조회할 수 없다.
- 스택의 최상단에서만 데이터를 저자하고 꺼낼 수 있다.
- 데이터를 저장할 때나 검색할 때 항상 스택의 최상단에서만 행위가 이루어지기 때문에 저장, 검색 프로세스가 매우 빠르다.

<br>

#### 2. 하나의 입출력 방향을 가지고 있다.
- Stack 자료구조는 데이터를 넣고 뺄 수 있는 곳이 스택의 가장 최상단 한 곳 뿐이다.
- 즉, 데이터를 넣을 때도 스택의 가장 최상단으로 넣고(입력), 뺄 때 또한 스택의 가장 최상단으로 데이터를 뺄 수 있다(출력)

<br>

#### 3. 데이터는 하나씩 넣고 뺄 수 있다.
- Stack 자료구조는 데이터를 넣고 뺄 수 있는 경로가 스택의 최상단 뿐이므로 입출력 모두 최상단에서 이루어진다.
- 즉, 스택에 한개 씩 여러 번 데이터를 넣어 스택 내부에 데이터가 여러 개 쌓여 있다고 하더라도, 데이터를 뺄 때는 스택의 가장 최상단에서 한 번에 한 개의 데이터만을 뺄 수 있다.

<hr class="sub">

### ✅ Stack 실사용 예제
브라우저의 뒤로 가기, 앞으로 가기 기능을 구현할 때 Stack이 활용된다.
<figure>
  <img src="/assets/images/posts_img/stack_and_queue/stack2.gif">
  <figcaption>출처: 코드스테이츠</figcaption>
</figure>
브라우저에서 자료구조 Stack이 사용될 때에는 다음과 같은 순서를 거친다.
1. 새로운 페이지로 접속할 때, 현재 페이지를 Prev Stack에 보관한다.
2. 뒤로 가기 버튼을 눌러 이전 페이지로 돌아갈 때는, 현재 페이지를 Next Stack에 보관하고, Prev Stack에 가장 나중에 보관된 페이지를 현재 페이지로 가져온다.
3. 앞으로 가기 버튼을 눌러 앞서 방문한 페이지로 이동을 원할 때는, Next Stack의 가장 마지막으로 보관된 페이지를 가져온다.
4. 마지막으로 현재 페이지를 Prev Stack에 보관한다.

<hr>

## 🛣️ Queue
>
**큐(Queue):** 줄을 서서 기다리다, 대기행렬
<figure>
  <img src="/assets/images/posts_img/stack_and_queue/queue1.gif">
  <figcaption>출처: 코드스테이츠</figcaption>
</figure>
고속도로의 톨게이트를 예로 들어보자. 자동차는 톨게이트에 진입한 순서대로 통행료를 내고 톨게이트를 통과한다.
톨게이트를 Queue 자료구조, 자동차는 데이터(data)로 비유할 수 있다.

<hr class="sub">

### ✅ Queue 구조
- Queue는 먼저 들어간 데이터(data)가 먼저 나오는 FIFO(First In First Out) 혹은 LILO(Last In Last Out) 을 특징으로 가지고 있다.
- 입력의 방향과 출력의 방향이 각각 고정되어 있으며, 데이터를 입력할 시에는 큐의 끝에서(tail), 데이터를 출력할 때는 큐의 맨 앞에서(head) 진행된다.
- Queue에 데이터를 넣는 것을 'enqueue', 데이터를 꺼내는 것을 'dequeue'라고 한다.
- Queue는 데이터가 입력된 순서대로 처리할 때 주로 사용한다.

<hr class="sub">

### ✅ Queue 특징
#### 1. FIFO (First In First Out)
먼저 들어간 데이터가 제일 처음에 나오는 선입선출의 구조로 되어 있다.
```
예1) 1, 2, 3, 4를 큐에 차례대로 넣습니다.

						queue.enqueue(데이터)
출력 방향(head) 	<---------------------------< 입력 방향(tail)
					1 <- 2 <- 3 <- 4
				<---------------------------<
들어간 순서대로, 1번이 제일 먼저 들어가고 4번이 마지막으로 들어가게 됩니다.

예2) 큐가 빌 때까지 데이터를 전부 빼냅니다.

						queue.dequeue(데이터)
출력 방향(head)	 <---------------------------< 입력 방향(tail)

				 <---------------------------<
1, 2, 3, 4
제일 첫 번째 있는 데이터부터 차례대로 나오게 됩니다.
```
#### 2. 두 개의 입출력 방향을 가지고 있다.
- Queue 자료구조는 데이터의 입력, 출력 방향이 다르다.
- 데이터를 입력할 때는 큐의 맨 끝(tail)으로만 입력이 가능하며 데이터를 출력할 때는 큐의 맨 앞(head)으로만 출력이 가능하다.
- 즉, 큐는 데이터를 입력하는 곳과 출력하는 곳이 각각 정해져 있으며 이렇게 총 2개의 입출력 방향을 가지고 있다.
- 만약 입출력 방향이 같다면 Queue 자료구조라고 볼 수 없다.

#### 3. 데이터는 하나씩 넣고 뺄 수 있습니다.
- 앞서 말했듯, Queue 자료구조는 데이터를 넣을 때는 큐의 맨 뒷 부분에서 뺄 때는 큐의 맨 앞부분에서 처리를 진행한다.
- 각 처리 시마다 한 개의 데이터를 넣거나 뺄 수 있다.
- 즉, 큐에 한 개 씩 여러 번 데이터를 넣어 큐 내부에 데이터가 여러 개 쌓여 있다고 하더라도, 데이터를 뺄 때는 큐의 맨 앞에서 한 번에 한 개의 데이터만을 뺄 수 있다.

<hr class="sub">

### ✅ Queue 실사용 예제

Queue는 컴퓨터와 연결된 프린터에서 여러 문서를 순서대로 인쇄할 때 사용된다.

<figure>
  <img src="/assets/images/posts_img/stack_and_queue/queue2.gif">
  <figcaption>출처: 코드스테이츠</figcaption>
</figure>

1. 문서에 프린터 출력 버튼을 누르면 해당 문서는 인쇄 작업(임시 기억 장치의 Queue)에 들어간다.
2. 프린터는 인쇄 작업 Queue에 들어온 문서를 순서대로 인쇄한다.
- 컴퓨터(출력 버튼)
- (임시 기억 장치의) Queue에 하나씩 들어옴
- Queue에 들어온 문서를 순서대로 인쇄

<br>

✍️ 컴퓨터와 프린터 사이의 데이터(data) 통신
<blockquote class="ex">
<ul>
<li>일반적으로 프린터는 속도가 느리다.</li>
<li>CPU는 프린터와 비교하여, 데이터를 처리하는 속도가 빠르다.</li>
<li>CPU는 빠른 속도로 인쇄에 필요한 데이터(data)를 만든 다음, 인쇄 작업 Queue에 저장하고 다른 작업을 수행한다.</li>
<li>프린터는 인쇄 작업 Queue에서 데이터(data)를 받아 들어온 순서대로 일정한 속도로 인쇄한다.</li>
</ul>
</blockquote>

✍️ 유튜브와 같은 동영상 스트리밍 앱을 통해 동영상을 시청할 때
<blockquote class="ex">
다운로드 된 데이터(data)가 영상을 재생하기에 충분하지 않은 경우가 있다. <br>
이때 동영상을 정상적으로 재생하기 위해 Queue에 모아 두었다가 동영상을 재생하기에 충분한 양의 데이터가 모였을 때 동영상을 재생한다.
</blockquote>
