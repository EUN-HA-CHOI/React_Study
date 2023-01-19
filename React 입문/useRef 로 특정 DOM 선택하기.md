## useRef 로 특정 DOM 선택하기  

리액트에서 특정 엘리먼트의 크기를 가져와야 하거나, 스크롤바 위치를 가져오거나 설정해야 할 때 또는 포커스를 설정해 줘야할 때 등등 DOM 을 직접 선택해야 하는 상황이 발생할 때 리액트에서 `ref` 라는 것을 사용한다.    

함수형 컴포넌트에서 `ref` 를 사용할 때에는 useRef 라는 Hook 함수를 사용한다.     

이제 전에 만든 InputSample 에서 초기화 버튼을 누르면 포커스가 초기화 버튼에 그대로 남아있게 된다.   
초기화 버튼을 클릭했을 때 이름 input에 포커스가 잡히도록 `useRef` 를 사용하여 기능을 구현 해 보자 ~!  

```
import React, { useState, useRef } from 'react';

function InputSample() {
  const [inputs, setInputs] = useState({
    name: '',
    nickname: ''
  });
  const nameInput = useRef();

  const { name, nickname } = inputs; // 비구조화 할당을 통해 값 추출

  const onChange = e => {
    const { value, name } = e.target; // 우선 e.target 에서 name 과 value 를 추출
    setInputs({
      ...inputs, // 기존의 input 객체를 복사한 뒤
      [name]: value // name 키를 가진 값을 value 로 설정
    });
  };

  const onReset = () => {
    setInputs({
      name: '',
      nickname: ''
    });
    nameInput.current.focus();
  };

  return (
    <div>
      <input
        name="name"
        placeholder="이름"
        onChange={onChange}
        value={name}
        ref={nameInput}
      />
      <input
        name="nickname"
        placeholder="닉네임"
        onChange={onChange}
        value={nickname}
      />
      <button onClick={onReset}>초기화</button>
      <div>
        <b>값: </b>
        {name} ({nickname})
      </div>
    </div>
  );
}

export default InputSample;
```

`useRef()` 를 사용하여 Ref 객체를 만들고, 이 객체를 선택하고 싶은 DOM에 `ref` 값으로 설정해주어야 한다. 그러면,  Ref 객체의 .current 값을 우리가 원하는 DOM 을 가르키게 된다.  

위 예제에서는 onReset 함수에서 input 에 포커스를 하는 focus() DOM API 를 호출해주었다.     
