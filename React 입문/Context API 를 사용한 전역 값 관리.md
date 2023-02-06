## Context API 를 사용한 전역 값 관리.    

특정 함수를 특정 컴포넌트를 거쳐서 원하는 컴포넌트에게 전달하는 작업은 리액트로 개발을 하다보면 자주 발생 할 수 있는 작업이다.     
한 개의 컴포넌트를 거쳐서 전달하는 건 불편함이 없지만, 3-4개 이상의 컴포넌트를 거쳐서 전달을 해야 하는 일이 발생한다면 번거로울 것이다.  

그럴 땐, 리액트의 Context API 와 dispatch 를 함께 사용하면 해결 할 수 있다.  

리액트의 Context API 를 사용하면, 프로젝트 안에서 전역적으로 사용 할 수 있는 값을 관리 할 수 있다.   
이 값은 꼭 상태를 가르키지 않아도 된다. 이 값은 함수일수도 있고, 어떤 외부 라이브러리 인스턴스일 수도 있고 DOM일 수도 있다.  

물론, Context API 를 사용해서 프로젝트의 상태를 전역적으로 관리 할 수 있다.  
우선, Context API 를 사용하여 새로운 Context 를 만드는 방법을 알아보자.  

Context 를 만들 땐 다음과 같이 `React.createContext()` 라는 함수를 사용한다.  
`const UserDispatch = React.createContext(null);`  

`createContext` 의 파라미터에는 Context 의 기본값을 설정할 수 있다. 여기서 설정하는 값은 Context 를 쓸 때 값을 따로 지정 하지 않을 경우 사용되는 기본 값이다.    

Context 를 만들면, Context 안에 Provider 라는 컴포넌트가 들어 있는데 이 컴포넌트를 통하여 Context 의 값을 정할 수 있다.    
이 컴포넌트를 사용할 때, `value` 라는 값을 설정해주면 된다.  
`<UserDispatch.Provider value={dispatch}>...</UserDispatch.Provider>`   

이렇게 설정해주면 Provider 에 의하여 감싸진 컴포넌트 중 어디서든지 우리가 Context 의 값을 다른 곳에서 바로 조회해서 사용할 수 있다.    

1. App 컴포넌트 에서 Context 를 만들고, 사용하고, 내보내는 작업을 해 준다.  
지금은 우리가 UserDispatch 라는 Context 를 만들어서, 어디서든지 dispatch 를 꺼내 쓸 수 있도록 준비를 해준 것이다.  
그리고, UserDispatch 를 만들 때 다음과 같이 내보내주는 작업을 했다.      
`export const UserDispatch = React.createContext(null);`  

이렇게 내보내주면 나중에 사용하고 싶을 때 다음과 같이 불러와서 사용 할 수 있다.  
`import { UserDispatch } from './App';`   

2. App,UserList 에서 onToggle 과 onRemove 를 지우고, UserList 에게 props를 전달하는것도 지운다.   

```
import React from 'react';

const User = React.memo(function User({ user }) {
  return (
    <div>
      <b
        style={{
          cursor: 'pointer',
          color: user.active ? 'green' : 'black'
        }}
        onClick={() => {}}
      >
        {user.username}
      </b>
      &nbsp;
      <span>({user.email})</span>
      <button onClick={() => {}}>삭제</button>
    </div>
  );
});

function UserList({ users }) {
  return (
    <div>
      {users.map(user => (
        <User user={user} key={user.id} />
      ))}
    </div>
  );
}

export default React.memo(UserList);
```

3. User 컴포넌트에서 바로 `dispatch` 를 사용 할건데, 그렇게 하기 위해서는 `useContext` 라는 Hook 을 사용해서 우리가 만든 UserDispatch Context 를 조회해야한다.  
 이렇게 Context API 를 사용해서 dispatch 를 어디서든지 조회해서 사용해줄 수 있게 해주면 코드의 구조가 훨씬 깔끔해질 수 있다.   
 
 ```
 import React, { useContext } from 'react';
import { UserDispatch } from './App';

const User = React.memo(function User({ user }) {
  const dispatch = useContext(UserDispatch);

  return (
    <div>
      <b
        style={{
          cursor: 'pointer',
          color: user.active ? 'green' : 'black'
        }}
        onClick={() => {
          dispatch({ type: 'TOGGLE_USER', id: user.id });
        }}
      >
        {user.username}
      </b>
      &nbsp;
      <span>({user.email})</span>
      <button
        onClick={() => {
          dispatch({ type: 'REMOVE_USER', id: user.id });
        }}
      >
        삭제
      </button>
    </div>
  );
});

function UserList({ users }) {
  return (
    <div>
      {users.map(user => (
        <User user={user} key={user.id} />
      ))}
    </div>
  );
}

export default React.memo(UserList);
 ```

<hr>

### 정리    

이로써 useState 를 사용하는 것과 useReducer 를 사용하는 것의 큰 차이를 발견했다. useReducer 를 사용하면 이렇게 dispatch 를 Context API 를 사용해서
전역적으로 사용 할 수 있게 해주면   컴포넌트에게 함수를 전달해줘야 하는 상황에서 코드의 구조가 훨씬 깔끔해질 수 있다.    
만약에 깊은 곳에 위치하는 컴포넌트에게 여러 컴포넌트를 거쳐서 함수를 전달해야 하는 일이 있다면 이렇게 Context API 를 사용하면 된다.   
