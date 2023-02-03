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

### Button 만들기  

Sass 를 배울 때 만들었던 재사용성 높은 Button 컴포넌트를 styled-components 로 구현을 해보겠다.   

### polished의 스타일 관련 유틸 함수 사용하기  

Sass 를 사용 할 때에는 lighten() 또는 darken() 과 같은 유틸 함수를 사용하여 색상에 변화를 줄 수 있었는데, CSS in JS 에서도 비슷한 유틸 함수를 사용하고 싶다면 polished 라는 라이브러리를 사용하면 된다.     

`npm install polished` 설치    

* 기존에 색상 부분을 polished 의 유틸 함수들로 대체 해 보겠다.   

```
import React from 'react';
import styled from 'styled-components';
import { darken, lighten } from 'polished';

const StyledButton = styled.button`
  /* 공통 스타일 */
  display: inline-flex;
  outline: none;
  border: none;
  border-radius: 4px;
  color: white;
  font-weight: bold;
  cursor: pointer;
  padding-left: 1rem;
  padding-right: 1rem;

  /* 크기 */
  height: 2.25rem;
  font-size: 1rem;

  /* 색상 */
  background: #228be6;
  &:hover {
    background: ${lighten(0.1, '#228be6')};
  }
  &:active {
    background: ${darken(0.1, '#228be6')};
  }

  /* 기타 */
  & + & {
    margin-left: 1rem;
  }
`;

function Button({ children, ...rest }) {
  return <StyledButton {...rest}>{children}</StyledButton>;
}

export default Button;
```

색상 코드를 지닌 변수를 Button.js 에서 선언을 하는 대신에 ThemeProvider 라는 기능을 사용하여 styled-components 로 만드는   
모든 컴포넌트에서 조회하여 사용 할 수 있는 전역적인 값을 설정해보겠다.    

```
import React from 'react';
import styled, { ThemeProvider } from 'styled-components';
import Button from './components/Button';

const AppBlock = styled.div`
  width: 512px;
  margin: 0 auto;
  margin-top: 4rem;
  border: 1px solid black;
  padding: 1rem;
`;

function App() {
  return (
    <ThemeProvider
      theme={{
        palette: {
          blue: '#228be6',
          gray: '#495057',
          pink: '#f06595'
        }
      }}
    >
      <AppBlock>
        <Button>BUTTON</Button>
      </AppBlock>
    </ThemeProvider>
  );
}

export default App;
```

이렇게 에서 theme 을 설정하면 ThemeProvider 내부에 렌더링된 styled-components 로 만든 컴포넌트에서 palette 를 조회하여 사용 할 수 있다.   
한번 Button 컴포넌트에서 우리가 방금 선언한 palette.blue 값을 조회해 보자.    

```
import React from 'react';
import styled, { css } from 'styled-components';
import { darken, lighten } from 'polished';

const StyledButton = styled.button`
  /* 공통 스타일 */
  display: inline-flex;
  outline: none;
  border: none;
  border-radius: 4px;
  color: white;
  font-weight: bold;
  cursor: pointer;
  padding-left: 1rem;
  padding-right: 1rem;

  /* 크기 */
  height: 2.25rem;
  font-size: 1rem;

  /* 색상 */
  ${props => {
    const selected = props.theme.palette.blue;
    return css`
      background: ${selected};
      &:hover {
        background: ${lighten(0.1, selected)};
      }
      &:active {
        background: ${darken(0.1, selected)};
      }
    `;
  }}

  /* 기타 */
  & + & {
    margin-left: 1rem;
  }
`;

function Button({ children, ...rest }) {
  return <StyledButton {...rest}>{children}</StyledButton>;
}

export default Button;
```

ThemeProvider 로 설정한 값은 styled-components 에서 props.theme 로 조회 할 수 있다. 지금은 selected 값을 무조건 blue 값을 가르키게 했는데,   
이 부분을 Button 컴포넌트가 color props 를 를 통하여 받아오게 될 색상을 사용하도록 수정.    

```
import React from 'react';
import styled, { css } from 'styled-components';
import { darken, lighten } from 'polished';

const StyledButton = styled.button`
  /* 공통 스타일 */
  display: inline-flex;
  outline: none;
  border: none;
  border-radius: 4px;
  color: white;
  font-weight: bold;
  cursor: pointer;
  padding-left: 1rem;
  padding-right: 1rem;

  /* 크기 */
  height: 2.25rem;
  font-size: 1rem;

  /* 색상 */
  ${props => {
    const selected = props.theme.palette[props.color];
    return css`
      background: ${selected};
      &:hover {
        background: ${lighten(0.1, selected)};
      }
      &:active {
        background: ${darken(0.1, selected)};
      }
    `;
  }}

  /* 기타 */
  & + & {
    margin-left: 1rem;
  }
`;

function Button({ children, ...rest }) {
  return <StyledButton {...rest}>{children}</StyledButton>;
}

Button.defaultProps = {
  color: 'blue'
};

export default Button;

```

* App 컴포넌트에서 회색,핑크색 버튼을 렌더링 하자.   

```
import React from 'react';
import styled, { ThemeProvider } from 'styled-components';
import Button from './components/Button';

const AppBlock = styled.div`
  width: 512px;
  margin: 0 auto;
  margin-top: 4rem;
  border: 1px solid black;
  padding: 1rem;
`;

function App() {
  return (
    <ThemeProvider
      theme={{
        palette: {
          blue: '#228be6',
          gray: '#495057',
          pink: '#f06595'
        }
      }}
    >
      <AppBlock>
        <Button>BUTTON</Button>
        <Button color="gray">BUTTON</Button>
        <Button color="pink">BUTTON</Button>
      </AppBlock>
    </ThemeProvider>
  );
}

export default App;
```

<img width="493" alt="스크린샷 2023-02-03 오후 2 14 18" src="https://user-images.githubusercontent.com/97012561/216518079-31ed21ee-eb69-43c2-ad07-e76aee707b76.png">






