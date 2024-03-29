## 여러개의 input 상태관리하기   

```
import React, { useState } from 'react';

function InputSample() {
  const onChange = (e) => {
  };

  const onReset = () => {
  };


  return (
    <div>
      <input placeholder="이름" />
      <input placeholder="닉네임" />
      <button onClick={onReset}>초기화</button>
      <div>
        <b>값: </b>
        이름 (닉네임)
      </div>
    </div>
  );
}

export default InputSample;
```

input 의 개수가 여러개가 됐을때는, 단순히 useState를 여러번 사용하고 onChange 도 여러 개 만들어서 구현할 수 있지만   
가장 안 좋은 방법이다! 좋은 방법은 , input에 name을 설정하고 이벤트가 발생했을 때 이 값을 참조하는 것이다.  

그리고, useState 에서는 문자열이 아니라 객체 형태의 상태를 관리해주어야 한다.    

```
import React, { useState } from 'react'

function InputSample() {
  const [inputs, setInputs] = useState({
    name: '',
    nickname:''
  });

  const {name, nickname} = inputs //비구조화 할당을 통해 값 추출  
  
  const onChange = (e) => {
   const {value, name} = e.target; //우선 e.target 에서 name과 value를 추출
   setInputs({
    ...inputs, //기존의 input 객체를 복사한 뒤
    [name] : value //name 키를 가진 값을 value로 설정
   });
  };

  const onReset = () => {
   setInputs({
    name: '',
    nickname:'',
   })
  };

  return (
    <div>
      <input name='name' placeholder='이름' onChange={onChange} value={name} />
      <input name='nickname' placeholder='닉네임' onChange={onChange} value={nickname} />
      <button onClick={onReset}>초기화</button>
      <div>
        <b>값: </b>
        {name} ({nickname})
      </div>
    </div>
  )
}

export default InputSample

```

`...` 이 문법은 spread 문법이다. 객체의 내용을 모두 펼쳐서 기존 객체를 복사해준다.    
이러한 작업을, '불변성을 지킨다' 라고 부른다. 이를 해야 이랙트 컴포넌트에서 상태가 업데이트가 됐음을 감지 할 수 있고 이에 따라 필요한 리렌더링이 진행된다.   
추가적으로 리액트에서는 불변성을 지켜주어야만 컴포넌트 업데이트 성능 최적화를 제대로 할 수 있다.  

**리액트에서 객체를 업데이트하게 될 때에는 기존 객체를 직접 수정하면 안되고, 새로운 객체를 만들어서, 새 객체에 변화를 주어야 한다.**  


