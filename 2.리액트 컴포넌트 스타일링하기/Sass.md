## 리액트 컴포넌트 스타일링하기  

리액트에서 컴포넌트를 스타일링 하는 가장 기본적인 방법은 css 파일을 만들어서 컴포넌트에서 import 해서 사용하는 것이다.  이 방법도 있지만, 컴포넌트를 스타일링 할 때 다른 도구들을 사용하면 훨씬 더 편하게 작업 할 수 있다.   

1. SCSS
2. CSS Module
3. styled-components. 

<hr>

## 1.Sass.     

Sass(syntactically awesome style sheets) 는 css pre-processor 로서, 복잡한 작업을 쉽게 할 수 있게 해주고, 코드의 재활용성을 높여줄 뿐 만 아니라, 코드의 가독성을 높여주어 유지보수를 쉽게 해 준다.   


scss 에서는 두 가지의 확장자(.scss/.sass)를 지원한다. scss가 처음 나왔을 땐 sass 만 지원 되었고, sass는 문법이 다르다.  

* sass.   
```
$font-stack:    Helvetica, sans-serif
$primary-color: #333

body
  font: 100% $font-stack
  color: $primary-color
```

* scss.  
```
$font-stack:    Helvetica, sans-serif;
$primary-color: #333;

body {
  font: 100% $font-stack;
  color: $primary-color;
}
```

* Scss (ex).    
```
$blue: #228be6; // 주석 선언

.Button {
  display: inline-flex;
  color: white;
  font-weight: bold;
  outline: none;
  border-radius: 4px;
  border: none;
  cursor: pointer;

  height: 2.25rem;
  padding-left: 1rem;
  padding-right: 1rem;
  font-size: 1rem;

  background: $blue; // 주석 사용
  &:hover {
    background: lighten($blue, 10%); // 색상 10% 밝게
  }

  &:active {
    background: darken($blue, 10%); // 색상 10% 어둡게
  }
}
```

`$blue: #228be6;` 이런 식으로 스타일 파일에서 사용 할 수 있는 변수를 선언 할 수도 있고 `lighten()` 또는 `darken()` 과 같이  
색상을 더 밝게하거나 어둡게 해주는 함수도 사용 할 수 있다.   


* 버튼 사이즈 조정하기    
  * large, medium, small 를 설정해줄 수 있도록 구현해보자
  * Button.js 에서 다음과 같이 defaultProps 를 통하여 size 의 기본값을 medium 으로 설정하고,  이 값은 button 의 className 에 넣자 

```
import React from 'react';
import './Button.scss';

function Button({ children, size }) {
  return <button className={['Button', size].join(' ')}>{children}</button>;
}

Button.defaultProps = {
  size: 'medium'
};

export default Button;
```

* outline 옵션 만들기.   

```
import React from 'react'
import './Button.scss';
import classNames from 'classnames';

function Button({children,size,color,outline}) {
  return (
    <button className={classNames('Button',size, color, {outline})}>{children}</button>
  )
}
Button.defaultProps = {
  size: 'medium',
  color:'blue'
}

export default Button;

```

여기서는 outline 값을 props로 받아와서 객체 안에 집어 넣은 다음에 classnames() 에 포함 시켜줬다.   이렇게 하면 outline 값이 true 일 때에만 button 에 outline css 클래스가 적용된다.   


* 전체 너비 차지하는 옵션  
  * 이번에는 fullWidth 라는 옵션이 있으면 버튼이 전체 너비를 차지하도록 구현    
  * 구현 방식은 outline과 굉장히 유사   


* ...rest props 전달하기    
  * ex)컴포넌트에 onClick 을 설정해주고 싶다면 ?   
  * 필요한 이벤트를 매번 넣어주는게 번거롭다면 `spread 와 rest` 를 사용한다.  
  * 이 문법은 주로 배열과 객체, 함수의 파라미터, 인자를 다룰 때 사용하는데, 컴포넌트에서도 사용    

```
function Button({children,size,color,outline,fullWidth,...rest}) {
  return (
    <button className={classNames('Button',size, color, {outline,fullWidth})}
     onClick={onClick}
     onMouseMove={onMouseMove}
     >{children}</button>
  )
}
Button.defaultProps = {
  size: 'medium',
  color:'blue'
}

export default Button;

```
* 이벤트  대신 {...rest} 값 넣어주기
```
function Button({children,size,color,outline,fullWidth,...rest}) {
  return (
    <button className={classNames('Button',size, color, {outline,fullWidth})}
     {...rest}
     >{children}</button>
  )
}
Button.defaultProps = {
  size: 'medium',
  color:'blue'
}

export default Button;

```

* 그리고 App.js 에서 버튼에 onClick 설정 해 주기   

` <Button size="large" onClick={() => console.log('클릭됐다!')}>`
