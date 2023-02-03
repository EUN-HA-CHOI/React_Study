## CSS Module.   

리액트 프로젝트에서 컴포넌트를 스타일링 할 때 CSS Module 이라는 기술을 사용하면, CSS 클래스가 중첩되는 것을 완벽히 방지 할 수 있다.   

CRA 로 만든 프로젝트에서 CSS Module 를 사용 할 때에는, CSS 파일의 확장자를 .module.csss 로 하면 된다. 예를 들어 다음과 같이 Box.module.css 라는 파일을 만들게 된다면   

```
.Box {
  background: black;
  color: white;
  padding: 2rem;
}
```

리액트 컴포넌트 파일에서 해당 CSS 파일을 불러올 때 CSS 파일에 선언한 클래스 이름들이 모두 고유해진다. 고유 CSS 클래스 이름이 만들어지는  
과정에서는 파일경로, 파일 이름, 클래스 이름, 해쉬값 등이 사용 될 수 있다.   

예를 들어 Box 컴포넌트를 만든다면 다음과 같은 코드를 작성한다.  

```
import React from "react";
import styles from "./Box.module.css";

function Box() {
  return <div className={styles.Box}>{styles.Box}</div>;
}

export default Box;
```

className 을 설정 할 때에는 styles.Box 이렇게 import 로 불러온 styles 객체 안에 있는 값을 참조 해야 한다.   

클래스 이름에 대하여 고유한 이름들이 만들어지기 때문에, 실수로 CSS 클래스 이름이 다른 관계 없는 곳에서 사용한 CSS 클래스 이름과 중복되는   
일에 대하여 걱정 할 필요가 없다.  

* 이 기술은 다음과 같은 상황에 사용하면 유용하다.  
  * 레거시 프로젝트에 리액트를 도입할 때(기존 프로젝트에 있던 CSS 클래스와 이름이 중복되어도 스타일이 꼬이지 않게 해 준다.)  
  * CSS 클래스를 중복되지 않게 작성하기 위하여 CSS 클래스 네이밍 규칙을 만들기 귀찮을 때    

* 리액트 컴포넌트를 위한 클래스를 작성 할 때 CSS 클래스 네이밍 규칙은 다음과 같다.  
  * 컴포넌트의 이름은 다른 컴포넌트랑 중복되지 않게 한다.  
  * 컴포넌트의 최상단 CSS 클래스는 컴포넌트의 이름과 일치시킨다. (ex. .Button).   
  * 컴포넌트 내부에서 보여지는 CSS 클래스는 CSS Selector 를 잘 활용한다. (ex. .MyForm .my-input).    

만약 CSS 클래스 네이밍 규칙을 만들고 따르기 싫다면, CSS Module 을 사용하면 된다.  

<hr>

### CSS Module 기술을 사용하여 커스텀 체크박스 컴포넌트를 만드는 방법.   

```
import React from 'react';

function CheckBox({ children, checked, ...rest }) {
  return (
    <div>
      <label>
        <input type="checkbox" checked={checked} {...rest} />
        <div>{checked ? '체크됨' : '체크 안됨'}</div>
      </label>
      <span>{children}</span>
    </div>
  );
}

export default CheckBox;
```

여기서 `...rest` 를 사용한 이유는, CheckBox 컴포넌트에게 전달하게 될 `name, onChange` 같은 값을 그대로 `input` 에게 넣어주기 위함 이다  
  
 <img width="200" alt="스크린샷 2023-02-03 오후 12 17 49" src="https://user-images.githubusercontent.com/97012561/216504425-70c9b505-bf3a-42e3-a6dc-2afba4566fb2.png">  <img width="174" alt="스크린샷 2023-02-03 오후 12 19 23" src="https://user-images.githubusercontent.com/97012561/216504605-c0323c4e-c8a4-43ce-8516-d7e6880eb3ed.png">


input 이 아닌 텍스트 부분을 선택 했는데도 값이 바뀌는 이유는 현재 우리가 해당 내용을 `label` 태그로 감싸줬기 때문이다.  

스타일링을 하기 전에 `react-icons` 라이브러리를 설치 해 준다.  
이 라이브러리를 사용하면  Font Awesome, Ionicons, Material Design Icons, 등의 아이콘들을 컴포넌트 형태로 쉽게 사용 할 수 있습니다.  

**리액트 아이콘 라이브러리 [이동](https://react-icons.github.io/react-icons/#/)**

* 라이브러리에서 원하는 아이콘들을 불러온다.   

```
import React from 'react';
import { MdCheckBox, MdCheckBoxOutlineBlank } from 'react-icons/md';

function CheckBox({ children, checked, ...rest }) {
  return (
    <div>
      <label>
        <input type="checkbox" checked={checked} {...rest} />
        <div>{checked ? <MdCheckBox /> : <MdCheckBoxOutlineBlank />}</div>
      </label>
      <span>{children}</span>
    </div>
  );
}

export default CheckBox;
```

이렇게 수정을 하면, 텍스트 대신 아이콘이 나타나게 될 것이다.   

<img width="210" alt="스크린샷 2023-02-03 오후 12 46 37" src="https://user-images.githubusercontent.com/97012561/216507907-b25ed5f2-b397-4771-930e-de493bac6a67.png">


