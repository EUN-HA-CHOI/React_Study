## styled-components.     

styled-components 는 현존하는 CSS in JS 관련 리액트 라이브러리 중에서 가장 인기 있는 라이브러리이다.    
CSS in JS 기술은 JS 안에 CSS 를 작성하는 것을 의미한다.  

styled-components 를 사용하기 전에, Tagged Template Literal 이라는 문법에 대하여 짚고 넘어가면,   
styled-components 가 내부적으로 어떻게 작동하는지 이해 할 수 있다.    

Template Literal 은 문자열 조합을 더욱 쉽게 할 수 있게 해주는 ES6 문법이다.  
 
### styled-components 사용하기   

`npm install styled-components` 설치   

```
import React from 'react';
import styled from 'styled-components';

const Circle = styled.div`
 width: 5rem;
 height:5rem;
 background: black;
 border-radius: 50%;
`; 

function App() {
  return <Circle />;
}

export default App;
```

<img width="186" alt="스크린샷 2023-02-03 오후 1 14 25" src="https://user-images.githubusercontent.com/97012561/216511030-6c430068-a02b-467f-9d43-bdc3c958aa3f.png">


styled-components 를 사용하면 이렇게 스타일을 입력함과 동시에 해당 스타일을 가진 컴포넌트를 만들 수 있다. 만약에 div 를 스타일링 하고 싶으면 `styled.div`,`input` 을 스타일링 하고 싶으면 `styled.input` 이런식으로 사용하면 된다.   

* Cicle 컴포넌트에 color 라는 props 를 넣어줘보겠다.   

```
import React from 'react';
import styled from 'styled-components';

const Circle = styled.div`
 width: 5rem;
 height:5rem;
 background: ${props => props.color || 'black'};
 border-radius: 50%;
`; 

function App() {
  return <Circle color="blue" />;
}

export default App;

```

Circle 컴포넌트에서는 color props 값을 설정해줬으면 해당 값을 배경색으로 설정하고, 그렇지 않으면 검정색을 배경색으로 사용하도록 설정되었다.  


* huge 라는 props 를 설정됐을 때 크기를 더 키워서 보여주도록 작업   

```
import React from 'react';
import styled, { css } from 'styled-components';

const Circle = styled.div`
 width: 5rem;
 height:5rem;
 background: ${props => props.color || 'black'};
 border-radius: 50%;
 ${props => 
   props.huge &&
   css`
     width: 10rem;
    height: 10rem;
  `}
`; 

function App() {
  return <Circle color="red" huge />;
}

export default App;

```

이런식으로 여러 줄의 CSS 코드를 조건부로 보여주고 싶다면 css 를 사용해야한다. css 를 불러와서 사용을 해야 그 스타일 내부에서도 다른 props 를 조회 할 수 있다.    

* Button 만들기  

Sass 를 배울 때 만들었던 재사용성 높은 Button 컴포넌트를 styled-components 로 구현을 해보겠다.   

