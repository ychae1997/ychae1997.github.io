---
title: "[React] fe-sprint-react-custom-component"
excerpt: "코드스테이츠 과제 - styled-components 이용해 UI 디자인 패턴 만들기"

categories:
  - React
tags:
  - [React, 코드스테이츠]

permalink: /React/fe-sprint-react-custom-component/

toc: true
toc_sticky: true

date: 2023-02-22
last_modified_at: 2023-02-22
---
## Modal
<img src="/assets/images/posts_img/react-custom-component-assignment/modal.gif">

<b><span style="font-size:1.225em">📝 요구사항</span></b>

- styled components 라이브러리를 이용해 CSS 구현
    - `ModalContainer` : Modal을 구현하는데 필요한 컴포넌트를 감싸주는 컨테이너 컴포넌트 역할
    - `ModalBackdrop` : Modal이 떴을 때의 배경을 깔아주는 역할
    - `ModalBtn` : Modal 창을 키고 끌 수 있는 버튼
    - `ModalView` : Modal 창 컴포넌트
    - `ModalBtn`클릭시 Modal이 열린 상태(isOpen)를 boolean 타입으로 변경하는 메서드 실행
- 조건부 렌더링 활용해서 Modal이 열린 상태(isOpen이 true)일 때만 모달창과 배경이 뜰 수있게 구현
- 조건부 렌더링 활용해서 Modal이 열린 상태(isOpen이 true)일 때는 `ModalBtn`의 내부 텍스트가 'Opened'로, 아닐때는 'Open Modal'이 되도록 구현


<br>

<b><span style="font-size:1.225em">💅 styled</span></b>

- `ModalBackdrop` : Modal이 활성화 됐을 때 배경 역할. 클릭하면 모달이 닫혀야한다. <br>
아래 코드처럼 left: 0, rigth:0 이런식으로 좌표를 설정하면 컴포넌트가 가로로 늘어난다. top과 bottom도 동일하게 세로로 늘려놓고, flex로 화면 가운데에 모달창을 띄어놓은 다음 fixed로 고정했다. 

```javascript
export const ModalBackdrop = styled.div`
  background-color: rgb(0, 0, 0, 0.5);
  display: flex;
  justify-content: center;
  align-items: center;
  position: fixed;
  left: 0; top: 0;
  right: 0; bottom: 0;
  z-index: 1;
`;
```

- `ModalView` : Modal창 컴포넌트 <br>
attrs 메소드를 이용해서 아래 코드와 같이 div 엘리먼트에 속성을 추가할 수 있다.

```javascript
export const ModalView = styled.div.attrs((props) => ({
  role: 'dialog',
}))`
  max-width: 400px;
  width: 90%;
  padding: 50px 0;
  background-color: white;
  border-radius: 15px;
  text-align: center;

  > div.desc {
    font-size: 20px;
  }
`;
```

이하 나머지 태그들 생략

<br>

<b><span style="font-size:1.225em">👩🏻‍💻 code</span></b>

```javascript
export const Modal = () => {
  const [isOpen, setIsOpen] = useState(false);

  const openModalHandler = () => {
    setIsOpen(!isOpen);
  };

  return (
    <>
      <ModalContainer>
        <ModalBtn onClick={openModalHandler}>
          {isOpen ? 'Opened!' : 'Open Modal'}
        </ModalBtn>
        {
          isOpen && <ModalBackdrop  onClick={openModalHandler}>
              <ModalView onClick={(e) => {e.stopPropagation()}}>
                <ModalClose onClick={openModalHandler}>
                  <i title='모달 창 닫기' className="fa-solid fa-xmark" />
                </ModalClose>
                <div className='desc'>HELLO CODESTATES!</div>
              </ModalView>
            </ModalBackdrop>
        }
      </ModalContainer>
    </>
  );
};
```
* `isOpen && <ModalBackdrop  onClick={openModalHandler}>` <br>
isOpen이 true 일때만 보여준다. (조건 ? 참일때 : null 과 같음) <br>

* `<ModalView onClick={(e) => {e.stopPropagation()}}>` <br>
`<ModalBackdrop>`에 onClick이벤트를 주면 그의 자식인 `<ModalView>`까지 영향을 미치게 된다. 즉, 모달 텍스트 부분을 클릭하면 모달이 닫히는 것이다. 이렇게 이벤트가 연속해서 발생하는 버블현상을 **이벤트 버블링**이라고 한다. 이렇게 의도하지 않은 동작을 막기 위해 `e.stopPropagation()`을 사용해주었다.

<hr>

## Toggle
<img src="/assets/images/posts_img/react-custom-component-assignment/toggle.gif">

<b><span style="font-size:1.225em">📝 요구사항</span></b>

- styled components 라이브러리를 이용해 CSS 구현
    - `ToggleContainer` : Toggle을 구현하는데 필요한 컴포넌트를 감싸주는 컨테이너 컴포넌트 역할
    - `Desc` : Toggle Switch의 상태를 설명하는 텍스트를 담는 컴포넌트
- `ToggleContainer` 내부에 `.toggle-container` `.toggle-circle` 클래스를 가진 `div` 요소를 각각 생성
- 성한 요소에 조건부 스타일링을 활용해 Toggle Switch가 ON인 상태일 경우에만 `toggle--checked` 클래스를 두 요소 모두에 추가
- 조건부 렌더링 활용해서 Toggle Switch가 ONdls tkdxodlf ruddn `Desc`컴포넌트 내부 텍스트를 'Toggle Switch ON'으로 아닐때는 'Toggle Switch OFF'로 변경

<br>

<b><span style="font-size:1.225em">💅 styled</span></b>

* `Wrap` : Toggle전체를 감싸는 div태그, 기존 과제 코드에서 가로 스크롤이 생기기에 `Fragment` 대신 사용했다.

```javascript
const Wrap = styled.div`
  display: flex;
  height: inherit;
  justify-content: center;
  align-items: center;
  flex-direction: column;
  // 가로스크롤 제거
`;
```

* `ToggleContainer` : 내부에 두개의 div 태그를 추가해 각각 컨테이너 부분과 써클부분 담당

```javascript
const ToggleContainer = styled.div`
  position: relative;
  cursor: pointer;
  > .toggle-container {
    width: 50px;
    height: 24px;
    border-radius: 30px;
    background-color: #8b8b8b;
    
    // TODO : .toggle--checked 클래스가 활성화 되었을 경우의 CSS를 구현합니다.
    transition-duration: 0.2s;
    &.toggle--checked{
      background-color: #fcc419;
    }
  }

  > .toggle-circle {
    position: absolute;
    top: 1px;
    left: 1px;
    width: 22px;
    height: 22px;
    border-radius: 50%;
    background-color: #ffffff;
    
    // TODO : .toggle--checked 클래스가 활성화 되었을 경우의 CSS를 구현합니다.
    transition-duration: 0.2s;
    &.toggle--checked{
      /* left: unset; */
      /* right: 1px; */
      transform: translateX(26px);
    }
  }
`;
```
* `Desc` : 설명 부분의 CSS를 구현

```javascript
const Desc = styled.div`
  margin-top: 0.8rem;
`;
```

<br>

<b><span style="font-size:1.225em">👩🏻‍💻 code</span></b>

```javascript
export const Toggle = () => {
  const [isOn, setisOn] = useState(false);

  const toggleHandler = () => {
    // TODO : isOn의 상태를 변경하는 메소드를 구현합니다.
    setisOn(!isOn);
  };

  return (
    <Wrap>
      <ToggleContainer onClick={toggleHandler}>
        <div className={`toggle-container ${isOn ? "toggle--checked" : ""}`} />
        <div className={`toggle-circle ${isOn ? "toggle--checked" : ""}`} />
      </ToggleContainer>
      <Desc>
        {isOn ? 'Toggle Switch ON' : 'Toggle Switch OFF'}
      </Desc> 
    </Wrap>
  );
};
```

<hr>

## Tab
<img src="/assets/images/posts_img/react-custom-component-assignment/tab.gif">

<b><span style="font-size:1.225em">📝 요구사항</span></b>

`Component`
* Styled Components 라이브러리를 활용해 CSS 구현
  * TabMenu : Tab을 구현하는데 필요한 컴포넌트를 감싸주는 컨테이너 컴포넌트 역할
  * Desc : Toggle Switch의 상태를 설명하는 텍스트를 담는 컴포넌트

`TabMenu`
* `TabMenu` 내부에 `.submenu` 클래스명을 가진 `li` 요소들을 `map` 을 이용한 반복을 통해 생성
* `TabMenu` 내부에 `.submenu` 클래스명을 가진 `li` 요소의 `textContent` 는 각 요소의 `name` 입니다.

`currentTab`
* 조건부 렌더링을 활용해서 Tab 메뉴가 선택된 상태일 때, 선택된 Tab 메뉴 `li` 요소의 클래스명만 `submenu focused` 가 되어야 하고, 선택되지 않은 나머지는 `submenu`가 되도록 구현
* `TabMenu` 를 클릭하면 현재 선택된 탭의 인덱스 값을 전달받아 `currentTab` 상태를 변경하는 `selectMenuHandler` 메서드가 실행
* `TabMenu` 를 클릭하면 현재 선택된 탭 메뉴만 `.focused` CSS 적용
* `TabMenu` 를 클릭하면 `Desc` 컴포넌트의 `content` 의 내용이 해당 탭의 `content`로 변경

<br>

<b><span style="font-size:1.225em">💅 styled</span></b>

```javascript
const TabMenu = styled.ul`
  // 생략 ...

  // 기본 탭메뉴
  .submenu {
    flex-grow: 1;
    text-align: center;
    background-color: inherit;
    color: #dee2e6;
    border-bottom: 2px solid #dee2e6;
    padding: 1rem 0;
    cursor: pointer;
  }

  // 선택된 탭메뉴
  .focused {
    color: var(--coz-purple-600);
    border-color: var(--coz-purple-600);
    transition: 0.3s;
  }
`;

const Desc = styled.div`
  text-align: center;
`;
```

<br>

<b><span style="font-size:1.225em">👩🏻‍💻 code</span></b>

```javascript
export const Tab = () => {
  const [currentTab, setCurrentTab] = useState(0);

  const menuArr = [
    { name: 'Tab1', content: 'Tab menu ONE' },
    { name: 'Tab2', content: 'Tab menu TWO' },
    { name: 'Tab3', content: 'Tab menu THREE' }
  ];

  const selectMenuHandler = (index) => {
    setCurrentTab(index);
  };

  return (
    <>
      <div>
        <TabMenu>
          {menuArr.map((menu, index) => {
            return (
              <li
                key={index}
                className={currentTab === index ? 'submenu focused' : 'submenu'}
                onClick={() => selectMenuHandler(index)}
              >
                {menu.name}
              </li>
            );
          })}
        </TabMenu>
        <Desc>
          <p>{menuArr[currentTab].content}</p>
        </Desc>
      </div>
    </>
  );
};

```
* `onClick={() => selectMenuHandler(index)}` <br>
`onClick={selectMenuHandler(index)}` 와 같이 축약해서 쓰면 해당 함수가 바로 실행된다. (이 때 함수에는 return 값이 없으므로 onClick 자리에는 undefined가 남게 됨) 위와 같이 콜백함수에 매개변수를 전달해줘야 할 때는 함수형태를 사용해야한다.

<hr>

## Tag
<img src="/assets/images/posts_img/react-custom-component-assignment/tag.gif">

<b><span style="font-size:1.225em">📝 요구사항</span></b>

input 기능
- `input` 창에 텍스트를 입력 후 Enter 키를 누르면 태그가 추가. 마우스 클릭이 아닌 Enter 키를 통해 태그가 추가되도록 하며, Enter 키가 눌리면 태그를 추가하는 `addTags` 메서드가 실행
- `addTags` 메서드는 기본적으로 태그를 추가하는 기능 이외에 아래 세 가지 기능도 수행할 수 있어야 한다.
  - 이미 입력되어 있는 태그인지 검사하여 이미 입력되어 있다면 추가하지 말아야 합니다.
  - 아무것도 입력하지 않은 상태에서는 Enter 키를 눌러도 `addTags` 메서드가 실행되지 않아야 합니다.
  - 태그가 추가되고 나면 `input` 창이 비워져야 합니다.

삭제 기능
- 기본적으로 `tags` 배열 안의 모든 태그들이 화면에 보여야 한다.
- 태그 이름 옆에 삭제 아이콘(`x`)이 표시되도록 하고, 아이콘을 클릭하면 해당 태그를 삭제하는 `removeTags` 메서드가 실행되어야 한다.
- `removeTags` 메서드가 삭제 아이콘(`x`)이 눌린 태그를 삭제하도록 `removeTags` 메서드를 완성해야 한다.

<br>

<b><span style="font-size:1.225em">💅 styled</span></b>

```javascript
export const TagsInput = styled.div`
  margin: 8rem auto;
  display: flex;
  align-items: flex-start;
  flex-wrap: wrap;
  min-height: 48px;
  width: 480px;
  padding: 0 8px;
  border: 1px solid rgb(214, 216, 218);
  border-radius: 6px;

  > ul {
    display: flex;
    flex-wrap: wrap;
    padding: 0;
    margin: 8px 0 0 0;

    > .tag {
      width: auto;
      height: 32px;
      display: flex;
      align-items: center;
      justify-content: center;
      color: #fff;
      padding: 0 8px;
      font-size: 14px;
      list-style: none;
      border-radius: 6px;
      margin: 0 8px 8px 0;
      background: var(--coz-purple-600);
      > .tag-title {
        color: white;
      }
      > .tag-close-icon {
        display: block;
        width: 16px;
        height: 16px;
        line-height: 16px;
        text-align: center;
        font-size: 14px;
        margin-left: 8px;
        color: var(--coz-purple-600);
        border-radius: 50%;
        background: #fff;
        cursor: pointer;
      }
    }
  }

  > input {
    flex: 1;
    border: none;
    height: 46px;
    font-size: 14px;
    padding: 4px 0 0 0;
    :focus {
      outline: transparent;
    }
  }

  &:focus-within {
    border: 1px solid var(--coz-purple-600);
  }
`;
```

<br>

<b><span style="font-size:1.225em">👩🏻‍💻 code</span></b>

```javascript
export const Tag = () => {
  const initialTags = ['CodeStates', 'kimcoding'];

  const [tags, setTags] = useState(initialTags);
  const removeTags = (indexToRemove) => {
    const filtered = tags.filter((el, i) => i !== indexToRemove);
    setTags(filtered);
  };

  const addTags = (event, value) => {
    if (event.type === 'keyup' && event.code === 'Enter' && value && !tags.includes(value)){
      setTags([...tags, value]);
      event.target.value = '';
    }
  };

  return (
    <>
      <TagsInput>
        <ul id="tags">
          {tags.map((tag, index) => (
            <li key={index} className="tag">
              <span className="tag-title">{tag}</span>
              <span className="tag-close-icon" onClick={() => removeTags(index)}>
                <i title='태그삭제' className="fa-solid fa-xmark" />
              </span>
            </li>
          ))}
        </ul>
        <input
          className="tag-input"
          type="text"
          onKeyUp={e => addTags(e, e.target.value)}
          placeholder="Press enter to add tags"
        />
      </TagsInput>
    </>
  );
};
```

<hr>

## Autocomplete
<img src="/assets/images/posts_img/react-custom-component-assignment/auto.gif">

<b><span style="font-size:1.225em">📝 요구사항</span></b>

input 기능
- `input` 요소에 onChange 이벤트 핸들러가 불려와야 합니다.
- `input` 값을 삭제할 수 있는 버튼(`div.delete-button`)이 있어야 합니다.
- 삭제 버튼 클릭 시 input value가 삭제되어야 합니다.

drop down 기능
- input 값이 포함된 자동 완성 추천 drop down 리스트가 보여야 합니다.
- drop down 항목을 마우스로 클릭 시, input 값 변경에 따라 drop down 목록이 변경되어야 합니다.
- drop down 항목을 마우스로 클릭 시, input 값이 변경되어야 합니다.
- drop down 항목을 마우스로 클릭 시, input 값이 이미 있어도 input 값이 drop down 항목의 값으로 변경되어야 합니다.

Advanced
- 키보드 방향키로 drop down 목록을 조정할 수 있도록 구현합니다.
- 키보드 엔터키를 누르면 선택한 추천 검색어가 input 값이 되도록 구현합니다.

<br>

<b><span style="font-size:1.225em">👩🏻‍💻 code</span></b>

```javascript
export const Autocomplete = () => {
  /**
   * Autocomplete 컴포넌트는 아래 3가지 state가 존재합니다. 필요에 따라서 state를 더 만들 수도 있습니다.
   * - hasText state는 input값의 유무를 확인할 수 있습니다.
   * - inputValue state는 input값의 상태를 확인할 수 있습니다.
   * - options state는 input값을 포함하는 autocomplete 추천 항목 리스트를 확인할 수 있습니다.
   */
  const [hasText, setHasText] = useState(false);
  const [inputValue, setInputValue] = useState('');
  const [options, setOptions] = useState(deselectedOptions);
  const [selected, setSelected] = useState(-1);

  useEffect(() => {
    if (inputValue === '') {
      setHasText(false);
    }else{
      const filtered = deselectedOptions.filter(option => option.includes(inputValue));
      setOptions(filtered);
    }
  }, [inputValue]);

  const handleInputChange = (event) => {
    setInputValue(event.target.value);
    setHasText(true);
  };
  const handleDropDownClick = (clickedOption) => {
    setInputValue(clickedOption);
  };

  const handleDeleteButtonClick = () => {
    setInputValue('');
  };

  // Advanced
  const handleKeyUp = (event) => {
    // https://developer.mozilla.org/en-US/docs/Web/API/KeyboardEvent/getModifierState#example
    // eslint-disable-next-line
    if (event.getModifierState("Fn") || event.getModifierState("Hyper") || event.getModifierState("OS") || event.getModifierState("Super") || event.getModifierState("Win")) return; if (event.getModifierState("Control") + event.getModifierState("Alt") + event.getModifierState("Meta") > 1) return;
    if (hasText) {
      if (event.code === 'ArrowDown' && options.length - 1 > selected) {
        setSelected(selected + 1);
      }
      if (event.code === 'ArrowUp' && selected >= 0) {
        setSelected(selected - 1);
      }
      if (event.code === 'Enter' && selected >= 0) {
        handleDropDownClick(options[selected]);
        setSelected(-1);
      }
    }
  };

  return (
    <div className='autocomplete-wrapper' onKeyUp={handleKeyUp}>
      <InputContainer>
        <input
          type='text'
          value={inputValue}
          onChange={handleInputChange}
        >
        </input>
        <div className='delete-button' onClick={handleDeleteButtonClick}>&times;</div>
      </InputContainer>
      {hasText && <DropDown
        options={options}
        handleComboBox={handleDropDownClick}
         selected={selected}/>}
    </div>
  );
};

export const DropDown = ({ options, handleComboBox, selected }) => {
  return (
    <DropDownContainer>
      {options.map((option, i) => {
        return (
          <li 
            key={i}
            onClick={() => handleComboBox(option)}
            className={selected === i ? 'selected' : ''}
            >{option}</li>
        );
      })}
    </DropDownContainer>
  );
};
```

<hr>

## Click To Edit
<img src="/assets/images/posts_img/react-custom-component-assignment/clicktoedit.gif">

<b><span style="font-size:1.225em">📝 요구사항</span></b>

MyInput 컴포넌트
- 조건부 렌더링을 활용하여 Edit가 가능한 상태일 경우에는 InputEdit 컴포넌트를 불러와야 합니다.
- input 요소에 onChange 이벤트 핸들러가 불러져 와야 합니다. onChange가 실행되면 저장된 value를 변경시켜 줍니다.
- input 요소에 onBlur 이벤트 핸들러가 불러져 와야 합니다. onBlur 이벤트 핸들러가 무슨 역할을 하는지 구글링을 통해서 찾아보세요!
- input 요소가 포커스를 잃었을 때, Edit가 불가능한 상태로 변경되어야 합니다.
- Edit가 불가능한 상태일 경우에는 저장된 value를 보여줄 수 있는 요소를 불러와야 합니다.
  - 이 요소를 클릭하면 Edit 가 가능한 상태로 변경되어야 합니다.

input 창 클릭 기능 테스트
- 입력 가능 상태로 변경할 수 있는 onClick 이벤트 핸들러가 span 요소에 있어야 합니다.
- 포커스가 제외되는 이벤트 onBlur의 핸들러가 input 요소에 있어야 합니다.
- 텍스트 영역을 클릭하면 입력 가능 상태로 변경되어야 합니다.
- 입력 가능 상태일 때 변화가 감지되면 새로운 값을 설정하는 메서드가 실행되어야 합니다.
- 입력 가능 상태일 때 input이 아닌 다른 곳을 클릭하면 입력 불가 상태가 되어야 합니다.
- 입력 가능 상태일 때 input이 아닌 다른 곳을 클릭하면 input의 값이 span에 담겨야 합니다.

<br>

<b><span style="font-size:1.225em">👩🏻‍💻 code</span></b>

```javascript
export const MyInput = ({ value, handleValueChange }) => {
  const inputEl = useRef(null);
  const [isEditMode, setEditMode] = useState(false);
  const [newValue, setNewValue] = useState(value);

  useEffect(() => {
    if (isEditMode) {
      inputEl.current.focus();
    }
  }, [isEditMode]);

  useEffect(() => {
    setNewValue(value);
  }, [value]);

  const handleClick = () => {
    setEditMode(true)
  };

  const handleBlur = () => {
    handleValueChange(newValue);
    setEditMode(false)
  };

  const handleInputChange = (e) => {
    setNewValue(e.target.value)
  };

  return (
    <InputBox>
      {isEditMode ? (<InputEdit
          type='text'
          value={newValue}
          ref={inputEl}
          onBlur={handleBlur}
          onChange={handleInputChange}
        />)
        : (<span onClick={handleClick}>{newValue}</span>)}
    </InputBox>
  );
}

const cache = {
  name: '김코딩',
  age: 20
};

export const ClickToEdit = () => {
  const [name, setName] = useState(cache.name);
  const [age, setAge] = useState(cache.age);

  return (
    <>
      <InputView>
        <label>이름</label>
        <MyInput value={name} handleValueChange={(newValue) => setName(newValue)} />
      </InputView>
      <InputView>
        <label>나이</label>
        <MyInput value={age} handleValueChange={(newValue) => setAge(newValue)} />
      </InputView>
      <InputView>
        <div className='view'>이름 {name} 나이 {age}</div>
      </InputView>
    </>
  );
};
```
