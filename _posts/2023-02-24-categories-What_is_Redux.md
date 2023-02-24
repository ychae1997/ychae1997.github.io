---
title: "[React] Redux란?"
excerpt: "Redux에 대해 알아보자"

categories:
  - React
tags:
  - [React, 코드스테이츠]

permalink: /React/Redux/

toc: true
toc_sticky: true

date: 2023-02-24
last_modified_at: 2023-02-24
---
<hr>

리덕스란 무엇일까? 먼저 [공식문서](https://ko.redux.js.org/tutorials/essentials/part-1-overview-concepts#what-is-redux){:target="_blank"}를 살펴보자.
> Redux는 "액션"이라는 이벤트를 사용하여 애플리케이션 상태를 관리하고 업데이트하기 위한 패턴 및 라이브러리입니다. 상태가 예측 가능한 방식으로만 업데이트될 수 있도록 하는 규칙과 함께 전체 애플리케이션에서 사용해야 하는 상태에 대한 중앙 집중식 저장소 역할을 합니다.

Redux는 React의 관련 라이브러리, 혹은 하위 라이브러리라는 대표적인 오해가 있는데, 전혀 그렇지 않다.
Redux는 React 없이도 사용할 수 있는 상태 관리 라이브러리이다.

React에서는 상태와 속성(props)을 이용한 컴포넌트 단위 개발 아키텍처를 배웠다면, Redux에서는 컴포넌트와 상태를 분리하는 패턴을 배운다.
그동안에는 상태를 다루기 위해 컴포넌트 안에서 상태 변경 로직이 복잡하게 얽혀있는 경우가 많았다.
그러나 상태 변경 로직을 컴포넌트로부터 분리하면 표현에 집중한, 보다 단순한 함수 컴포넌트로 만들 수 있게 된다.

<hr>

## Redux를 언제, 왜 사용해야 할까?

Redux는 애플리케이션의 여러 부분에서 필요한 상태인 "전역" 상태를 관리하는 데 도움이 된다. 아래 사진과 같은 구조의 React 애플리케이션이 있다고 한 번 가정해보자.
이 애플리케이션은 컴포넌트 3, 컴포넌트 6에서만 사용되는 상태가 있다. 이런 경우에는 최상위 컴포넌트에 상태를 위치시키는 것이 적합하다.

<figure>
  <img src="/assets/images/posts_img/What_is_Redux/img1.gif">
  <figcaption>출처: 코드스테이츠</figcaption>
</figure>

하지만 이러한 배치는 다음과 같은 이유로 비효율적이다.
>
- 해당 상태를 직접 사용하지 않는 최상위 컴포넌트, 컴포넌트1, 컴포넌트2도 상테 데이터를 가진다. <br>
➡️ **불필요하게 관여된 컴포넌트들 또한 리렌더링 되므로 웹성능에 악영향을 미친다.**
- 상태 끌어올리기, Props 내려주기를 여러 번 가져야한다.
- 애플리케이션이 복잡해질수록 데이터 흐름도 복잡해진다. <br>
➡️ **코드의 가독성이 나빠진다.**
- 컴포넌트 구조가 바뀐다면, 지금의 데이터 흐름을 완전히 바꿔야 할 수도 있음 <br>
➡️ **코드의 유지보수가 힘들어진다.**

Redux는 전역 상태를 관리할 수 있는 `Store`를 제공함으로써 이러한 문제들을 해결해준다. 아래의 사진을 보면 데이터 흐름이 간결해진 것을 알 수 있다.

<figure>
  <img src="/assets/images/posts_img/What_is_Redux/img2.gif">
  <figcaption>출처: 코드스테이츠</figcaption>
</figure>

<!-- Redux는 다음과 같은 경우에 더 유용합니다.
- 앱의 여러 위치에서 필요한 많은 양의 애플리케이션 상태가 있다.
- 해당 상태를 업데이트하는 로직이 복잡하다.
- 앱에 중간 또는 큰 규모의 코드베이스가 있어 협업에 유리하다.
- 규모가 큰 프로젝트에서 상태를 더 체계적으로 관리할 수 있다.
- 코드의 유지보수성을 높여준다.
- 단방향 데이터 흐름으로 버그의 예측이 쉽다. -->

<hr>

## Redux 구조
<figure>
  <img src="/assets/images/posts_img/What_is_Redux/img3.gif">
  <figcaption>출처: 코드스테이츠</figcaption>
</figure>

Redux는 다음과 같은 순서로 상태를 관리한다.

1. 상태가 변경되어야 하는 이벤트가 발생하면, 변경될 상태에 대한 정보가 담긴 `Action 객체`가 생성된다.
2. 이 Action 객체는 `Dispatch 함수`의 인자로 전달된다.
3. Dispatch 함수는 Action 객체를 `Reducer 함수`로 전달해준다.
4. Reducer 함수는 Action 객체의 값을 확인하고, 그 값에 따라 전역 상태 저장소 `Store`의 상태를 변경한다.
5. 상태가 변경되면, React는 화면을 다시 렌더링 한다.

즉, Redux에서는 `Action` → `Dispatch` → `Reducer` → `Store` 순서로 데이터가 단방향으로 흐르게 된다. <br>
예시를 보면서 좀 더 자세히 알아보자!

### Store
Store는 상태가 관리되는 유일한 저장소의 역할을 한다. 즉, Redux 앱의 state가 저장되어 있는 공간이다.
아래 코드와 같이 createStore 메서드를 활용해 Reducer를 연결해서 Store를 생성할 수 있다.
```javascript
import { Provider } from 'react-redux';
import { legacy_createStore as createStore } from 'redux';

const roootReducer = () => {};
const store = creatStore(rootReducer);

root.render(
  <Provider store={store}>
    <App />
  </Provider>
);

```

### Reducer
Reducer은 Dispatch에서 전달받은 Action객체의 `type`값에 따라 상태를 변경시키는 함수이다.
```javascript
const count = 1

// Reducer를 생성할 때에는 초기 상태를 인자로 요구합니다.
const counterReducer = (state = count, action) => {

  // Action 객체의 type 값에 따라 분기하는 switch 조건문입니다.
  switch (action.type) {

    //action === 'INCREASE'일 경우
    case 'INCREASE':
			return state + 1

    // action === 'DECREASE'일 경우
    case 'DECREASE':
			return state - 1

    // action === 'SET_NUMBER'일 경우
    case 'SET_NUMBER':
			return action.payload

    // 해당 되는 경우가 없을 땐 기존 상태를 그대로 리턴
    default:
      return state;
	}
}
// Reducer가 리턴하는 값이 새로운 상태가 됩니다.
```
외부 요인으로 인해 기대한 값이 아닌 엉뚱한 값으로 상태가 변경되는 일이 없어야하기 때문에 이때 Reducer는 **순수함수**여야 한다.


만약 여러 개의 Reducer를 사용하는 경우, Redux의 `combineReducers` 메서드를 사용해서 하나의 Reducer로 합쳐줄 수 있다.
```javascript
import { combineReducers } from 'redux';

const rootReducer = combineReducers({
  counterReducer,
  anyReducer,
  ...
});
```

### Action
Action은 말 그대로 어떤 액션을 취할 것인지 정의해 놓은 객체이다.
```javascript
// payload가 필요 없는 경우
{ type: 'INCREASE' }

// payload가 필요한 경우
{ type: 'SET_NUMBER', payload: 5 }
```
여기서 type 은 필수로 지정해줘야한다. 해당 Action 객체가 어떤 동작을 하는지 명시해주는 역할을 하기 때문이다.
type은 대문자와 Snake Case로 작성하고, 여기에 필요에 따라 payload 를 작성해 구체적인 값을 전달할 수 있다.

보통 Action을 직접 작성하기보다는 Action 객체를 생성하는 함수를 만들어 사용하는 경우가 많다. 이러한 함수를 액션 생성자(Action Creator)라고도 한다.
```javascript
// payload 가 필요 없는 경우
export const increase = () => {
  return {
    type: 'INCREASE',
  };
};
export const decrease = () => {
  return {
    type: 'DECREASE',
  };
};

// payload 가 필요한 경우 
const setNumber = (num) => {
  return {
    type: 'SET_NUMBER',
    payload: num
  }
}
```

### Dispatch
Dispatch는 Reducer로 Action을 전달해주는 함수이다. Dispatch의 전달인자로 Action 객체가 전달된다. <br>
Action 객체를 전달받은 Dispatch함수는 Reducer를 호출한다.
```javascript
// Action 객체를 직접 작성하는 경우
dispatch( { type: 'INCREASE' } );
dispatch( { type: 'SET_NUMBER', payload: 5 } );

// 액션 생성자(Action Creator)를 사용하는 경우
dispatch( increase() );
dispatch( setNumber(5) );
```

### Redux Hooks
Redux Hooks는 React-Redux에서 Redux를 사용할 때 활용할 수 있는 Hooks 메서드를 제공한다.

#### - useDispatch()
`useDispatch()` 는 Action 객체를 Reducer로 전달해 주는 Dispatch 함수를 반환하는 메서드이다.
위에서 사용한 dispatch 함수도 useDispatch()를 사용해서 만든 것이다.

```javascript
import { useDispatch } from 'react-redux';
import { increase, decrease } from './index.js';

const plusNum = () => {
  dispatch(increase());
};
const minusNum = () => {
  dispatch(decrease());
};
```

#### - useSelector()
`useSelector()`는 컴포넌트와 state를 연결하여 Redux의 state에 접근할 수 있게 해주는 메서드이다. 
```javascript
// Redux Hooks 메서드는 'redux'가 아니라 'react-redux'에서 불러옵니다.
import { useSelector } from 'react-redux'
const state = useSelector((state) => state);
```

<hr>

## Redux의 세가지 원칙
1. Single source of truth
동일한 데이터는 항상 같은 곳에서 가지고 와야 한다는 의미이다. <br>
➡️ Redux에는 데이터를 저장하는 `Store`라는 단 하나뿐인 공간이 있음

2. State is read-only
상태는 읽기 전용이라는 뜻으로, React에서 상태갱신함수로만 상태를 변경할 수 있었던 것처럼, Redux의 상태도 직접 변경할 수 없음을 의미한다. <br>
➡️ `Action` 객체가 있어야만 상태를 변경할 수 있음

3. Changes are made with pure functions
변경은 순수함수로만 가능하다는 뜻이다. <br>
➡️ 상태가 엉뚱한 값으로 변경되는 일이 없도록 순수함수로 작성되어야하는 `Reducer`와 연결

## 리팩토링
여러 역할을 하는 코드들을 한 곳에 작성하는 것은 바람직하지 않다. 대표적인 이유로는 코드의 가독성이 나빠진다는 점이다.
<div><iframe height="500px" width="100%" src="https://stackblitz.com/edit/react-11p978?file=README.md&view=editor"></div>