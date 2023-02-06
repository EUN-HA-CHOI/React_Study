## Context API 를 활용한 상태 관리  

<img width="466" alt="스크린샷 2023-02-06 오후 11 57 35" src="https://user-images.githubusercontent.com/97012561/217005483-31893e64-664b-47cd-91eb-653815a65dad.png">

만든 투두 리스트 앱에서 , 상태 관리를 한다면 위와 같은 구조로 구현 할 수 있다.  
App 에서 todos 상태와 , onToggle, onRemove , onCreate 함수를 지니고 있게 하고, 해당 값들을 props 를 사용해서  
자식 컴포넌트들에게 전달해주는 방식으로 구현 할 수 있다.    

하지만, 프로젝트 규모가 커지게 되면 최상위 컴포넌트인 App 에서 모든 상태 관리를 하기엔 App 컴포넌트의 코드가 너무 복잡해질 수도 있고,  
props 를 전달해줘야 하는 컴포넌트가 너무 깊숙히 있을 수도 있다.(여러 컴포넌트를 거쳐서 전달해야 하는 경우)    

만약 Context API 를 활용한다면 다음과 같이 구현 할 수 있다.  

<img width="470" alt="스크린샷 2023-02-07 오전 12 01 32" src="https://user-images.githubusercontent.com/97012561/217006414-6d3de38c-1bb5-48b6-a365-b93a27a7bddf.png">

## 리듀서 만들기   

먼저, src 디렉터리에 TodoContext.js 파일을 생성하고, 그 안에 useReducer 를 사용하여 상태를 관리하는 TodoProvider 라는 컴포넌트를 만든다.   

