---
title: "[React] 상태관리"
excerpt: "상태관리에 대해 알아보자"

categories:
  - React
tags:
  - [React, 코드스테이츠]

permalink: /React/상태관리/

toc: true
toc_sticky: true

date: 2023-02-23
last_modified_at: 2023-02-23
---
<hr>

## 상태관리란 무엇일까?
State Management, 상태관리란 데이터를 관리하는 방법을 말한다. 그렇다면 프론트엔드에서 상태관리란 무엇일까?

>
- 데이터를 설계된 UI, UX에 맞게 설계하고 구현하는 일
- 네트워크를 통해 서버로 전달되는 클라이언트의 요청에 따라 변화하는 상태를 관리하는 일


<img src="/assets/images/posts_img/StateManagement/agora.png">
지난번 과제로 만든 아고라스테이츠를 보며 데이터를 살펴보자.

- 작성자
- 작성시간
- 답변유무

다시 본론으로 돌아와 '데이터를 설계된 UI, UX에 맞게 설계하고 구현하는 일'은 무슨 뜻일까?
이는 서버로부터 받아온 데이터를 클라이언트의 화면에 가공해 제공하는 일과 같다. <br>

위의 데이터 중 작성시간으로 예를 들어보자. <br>

작성시간은 언제 작성했는지를 나타내는 데이터이다. 원래 작성시간은 Date타입으로 `Thu Feb 23 2023 20:32:22 GMT+0900 (한국 표준시)` 와 같은 형식으로 나온다. 이런 데이터를 직관적으로 알아보기가 어렵다.
```javascript
// 작성시간 생성부분
const handleSubmit = () => {
    const addData = {
      createdAt: new Date().toISOString(), // 2023-02-23T11:32:04.345Z
      // ...생략
    }
    // ...생략
  }
```

위와 같은 데이터 형식을 `오후 8:32:04` 이런 형태로 바꿔주면 훨씬 더 보기 좋아진다. 

```javascript
// 작성시간 출력부분
new Date(createdAt).toLocaleTimeString() // 오후 8:32:04
```
<img src="/assets/images/posts_img/StateManagement/date.png">


또한 글쓰기, 답변 등과 같이 사용자가 UI를 통해 데이터를 변경할 수 있도록 환경을 마련하는 것과,
사용자의 눈에는 보이지 않는 네트워크 요청 등에 대한 상태관리 역시 프론트엔드 개발자가 고려해야할 부분이다.

<hr>

## Props Drilling
`Props Drilling`란 상위 컴포넌트의 state를 props를 통해 전달하고자 하는 컴포넌트로 전달하기 위해 그 사이는 props를 전달하는 용도로만 쓰이는 컴포넌트들을 거치면서 데이터를 전달하는 현상을 뜻한다.

<img src="/assets/images/posts_img/StateManagement/propsDrilling.png">

위 그림처럼 컴포넌트 A의 state를 컴포넌트 D로 전달하기 위해선 사이에 있는 컴포넌트 B, C를 거쳐야한다. props의 전달 횟수가 5회 미만으로 적다면 문제가 되진 않겠지만, 규모가 커지고 구조가 복잡해지면 다음과 같은 무제점들이 나타난다
>
- 코드의 가독성이 나빠진다
- 코드의 유지보수가 힘들어진다
- state변경 시 Props전달 과정에서 불필요하게 관여된 컴포넌트들 또한 리렌더링 된다. -> 웹성능에 악영향!🤯

<hr>

## 📚 과제 - Cmarket Hooks 
과제를 통해 `Props Drilling`의 문제를 직접 경험해보자!


<b><span>initialState.js</span></b>

```javascript
export const initialState =
{
  "items": [
    {
      "id": 1,
      "name": "노른자 분리기",
      "img": "../images/egg.png",
      "price": 9900
    }
    // .. 생략
  ],
  "cartItems": [
    {
      "itemId": 1,
      "quantity": 1
    },
    // .. 생략
  ]
}
```
<b><span>App.js</span></b>

`items`, `cartItems`은 여러 컴포넌트에서 사용해야하기 때문에 각각의 상태를 변경하는 핸들링 함수를 내려줬다.
(물론 `setCartItems` 같은 상태변경함수 자체를 내려줘도 상관없음!)

- `onAddCart={handleAddCart}` : 장바구니에 선택한 상품을 추가하는 함수 
- `onDelete={handleDelete}` : 장바구니 페이지에서 선택한 상품을 함수
- `onQuantityChange={handleQuantityChange}` : 장바구니에 담긴 상품의 수량을 변경하는 함수

```javascript
function App() {
  const [items, setItems] = useState(initialState.items);
  const [cartItems, setCartItems] = useState(initialState.cartItems);

  return (
    <Router>
      <Nav cartCount={cartItems.length}/>
      <Routes>
        <Route
          path="/"
          element={<ItemListContainer
            items={items}
            cartItems={cartItems}
            onAddCart={handleAddCart} />} />
        <Route
          path="/shoppingcart"
          element={<ShoppingCart
            items={items}
            cartItems={cartItems}
            onDelete={handleDelete}
            onQuantityChange={handleQuantityChange} />}
        />
      </Routes>
      <img
        id="logo_foot"
        src={`${process.env.PUBLIC_URL}/codestates-logo.png`}
        alt="logo_foot"
      />
    </Router>
  );
}

export default App;
```

<hr>

### 1. 장바구니 추가
> 메인 화면에서 &#91;장바구니 담기&#93; 버튼을 누른 후, 장바구니 페이지로 이동하면 상품이 담겨있어야 합니다.

<b><span>Item.js</span></b>

`hanleClick` 함수 : `ItemListContaine.js`파일에서 받아온 아이템의 아이디 중 선택된 아이디와 클릭이벤트를 리턴한다.

```javascript
export default function Item({ item, handleClick }) {
  return (
    <div key={item.id} className="item">
      <img className="item-img" src={item.img} alt={item.name}></img>
      <span className="item-name">{item.name}</span>
      <span className="item-price">{item.price}</span>
       <button
         className="item-button"
         onClick={(e) => handleClick(e, item.id)}>장바구니 담기</button>
    </div>
  )
}
```

<b><span>ItemListContaine.js</span></b>

`handleClick`, `onAddCart(id)` 함수 : 자식 컴포넌트인 `Item.js`의 `handleClick`에서 리턴한 클릭된 상품의 아이디를 `App.js`로 올려보내 준다. (Props전달을 위한 과정이며, 사실상 불필요하게 관여된 컴포넌트)


```javascript
function ItemListContainer({ items, onAddCart }) {
  const handleClick = (e, id) => onAddCart(id); // 🌟 Props전달
  
  return (
    <div id="item-list-container">
      <div id="item-list-body">
        <div id="item-list-title">쓸모없는 선물 모음</div>
        {items.map((item, idx) => <Item item={item} key={idx} handleClick={handleClick} />)}
      </div>
    </div>
  );
}

export default ItemListContainer;

```
<b><span>App.js</span></b>

올려받은 아이디와 카트아이템의 아이디를 비교해 조건에 따라(카트에 담겨 있는지) 로직을 구현한다.

```javascript
const handleAddCart = (id) => {
  let addItem = { itemId : id, quantity : 1 };
  const target = [...cartItems].find(el => el.itemId === id);
  if(target){
    // 수량만 증가
    target.quantity++;
    setCartItems([...cartItems]);
  }else {
    // 장바구니에 담기
    setCartItems([...cartItems, addItem]);
    }
}
```

<hr>

### 2. 장바구니 상품 수량 증가

>장바구니 페이지에서 장바구니에 담긴 각 아이템의 개수를 변경할 수 있어야 합니다.

<b><span>CartItem.js</span></b>

`CartItem`중 number 타입 input 부분을 보면, `ShoppingCart.js`에서 내려받은 `quantity`를 `value`값으로 주고 있고, 이 `value`값을 핸들링하는 함수인 `handleQuantityChange`를 통해 아이디와 함께 리턴하고 있다.

```javascript
export default function CartItem({
  item,
  checkedItems,
  handleCheckChange,
  handleQuantityChange, // 🌟
  handleDelete,
  quantity // 🌟
}) {
  
  return (
    
    // 생략...

    <input
      type="number"
      min={1}
      className="cart-item-quantity"
      value={quantity} // 🌟
      onChange={(e) => {
        handleQuantityChange(Number(e.target.value), item.id) // 🌟
      }}>
    </input>

    // 생략...
  )
}
```

<b><span>ShoppingCart.js</span></b>

- `renderItems` : `states.js` 파일을 보면 `cartItems`과 `Itmes` 데이터 형식이 다르다. 우리가 장바구니에서 보고 싶은 화면은 `Item`데이터와 같이 상품명과 이미지 등이 보이는 형태이므로 화면에 출력해 줄 상품들을 필터링(아이디가 같은 상품)해서 `Items`파일의 데이터 형식으로 매핑하여 보여준다.

- `quantity` : `cartItem`과 `renderItems`의 아이디를 비교해서 `cartItem`의 수량을 변수에 할당한다.

- `handleQuantityChange`, `onQuantityChange`: `CartItem.js`파일에서 받아온 변경된 수량 즉, `value`와 변경된 아이디를 상위 컴포넌트인 `App.js`로 올려보낸다.

```javascript
export default function ShoppingCart({ items, cartItems, onDelete, onQuantityChange }) {
  // 생략...

  const handleQuantityChange = (quantity, itemId) => {
    onQuantityChange(quantity, itemId) // 🌟
  }
  
  // 생략...
  const renderItems = items.filter((item) => cartItems.map((el) => el.itemId).indexOf(item.id) > -1) // 🌟
  return (
    // 생략...
    <div id="cart-item-list">
      {renderItems.map((item, idx) => {
        const quantity = cartItems.filter(el => el.itemId === item.id)[0].quantity // 🌟
        return <CartItem
          key={idx}
          handleCheckChange={handleCheckChange}
          handleQuantityChange={handleQuantityChange} // 🌟
          handleDelete={handleDelete}
          item={item}
          checkedItems={checkedItems}
          quantity={quantity} // 🌟
        />
      })}
    </div>
  )
}
```

<b><span>App.js</span></b>

전달받은 상품아이디를 이용해 변경된 상품을 찾은 후, 그 상품의 수량을 전달받은(업데이트 된) 수량으로 바꿔준다.

```javascript
const handleQuantityChange = (quantity, itemId) => {
    const target = [...cartItems].find(el => el.itemId === itemId);
    target.quantity = quantity;
    setCartItems([...cartItems]);
  }
```

* 레퍼런스 코드 : `map`을 이용해 상태를 변경해줬다.

```javascript
setCartItems(cartItems.map(el => {
  if(el.itemId === itemId) {
    return {
      "itemId": itemId,
      "quantity": quantity
    }
  }else {
    return el
  }
}))
```
<hr>

### 3. 장바구니 상품 삭제
> 장바구니 페이지에서 &#91;삭제&#93; 버튼을 누른 후, 해당 상품이 목록에서 삭제되어야 합니다.

<b><span>CartItem.js</span></b>

`handleDelete(item.id)` : 클릭된 상품의 아이디를 전달한다.

```javascript
export default function CartItem({
  item,
  checkedItems,
  handleCheckChange,
  handleQuantityChange,
  handleDelete, // 🌟
  quantity 
}) {
  
  return (
    
    // 생략...

    <button className="cart-item-delete" onClick={() => { handleDelete(item.id) }}>삭제</button> // 🌟
    
    // 생략...
  )
}
```

<b><span>ShoppingCart.js</span></b>

`onDelete(itemId)` : `CartItem.js`에서 전달받은 클릭한 상품 아이디를 `App.js`파일로 전달한다.

```javascript
export default function ShoppingCart({ items, cartItems, onDelete, onQuantityChange }) {
  // 생략...
  const handleDelete = (itemId) => {
    setCheckedItems(checkedItems.filter((el) => el !== itemId)) // 삭제 누른 아이디 찾아서 체크 해제
    onDelete(itemId) // 🌟
  }
  
  // 생략...
  const renderItems = items.filter((item) => cartItems.map((el) => el.itemId).indexOf(item.id) > -1) 
  return (
    // 생략...
    <div id="cart-item-list">
      {renderItems.map((item, idx) => {
        const quantity = cartItems.filter(el => el.itemId === item.id)[0].quantity 
        return <CartItem
          key={idx}
          handleCheckChange={handleCheckChange}
          handleQuantityChange={handleQuantityChange} 
          handleDelete={handleDelete} // 🌟
          item={item}
          checkedItems={checkedItems}
          quantity={quantity}
        />
      })}
    </div>
  )
}
```

<b><span>App.js</span></b>

`handleDelete` : 전달받은 아이디를 `CartItems`의 아이디와 비교해서 선택된 상품이 아닌 나머지 상품들만 필터링 해서 리턴한다.

```javascript
const handleDelete = (itemId) => {
    setCartItems(cartItems.filter(el => el.itemId !== itemId))
  }
```

<hr>

### 4. Nav에 상품 개수 표시
> 내비게이션 바에 상품 개수가 즉시 표시되어야 합니다.

<b><span>Nav.js</span></b>

전달받은 `cartCount`을 표시

```javascript
function Nav({ cartCount }) {
  return (
    <div id="nav-body">
      // 생략
      <div id="menu">
        <Link to="/">상품리스트</Link>
        <Link to="/shoppingcart">
          장바구니<span id="nav-item-counter">{cartCount}</span> 
        </Link>
      </div>
    </div>
  );
}
```

<b><span>App.js</span></b>

`cartItems.length`를 Props로 전달

```javascript
<Nav cartCount={cartItems.length} />
```

<hr>

## 💡 해결방법
과제를 통해서 `Props Drilling`문제를 직접 경험해 보았다. 많이 복잡한 과제는 아니었지만, Props전달과정에서 불필요하게 관여된 컴포넌트들이 생기는 것을 볼 수 있다.
위와같은 문제를 해결하기 위해 아래와 같은 방법들이 있다.

>
- state는 될 수 있으면 가까이 유지하기
- **상태관리 라이브러리** 사용하기
 
상태관리 라이브러리 같은 경우에는 전역으로 관리하는 저장소에서 직접 state를 꺼내 쓸 수 있기 때문에 `Props Drilling`방지에 매우 효과적이다. 
상태관리 라이브러리에는 Redux, Context api, Mobx, Recoil 등이 있으며, 다음 포스트에서는 Redux사용법에 대해 알아보자! 