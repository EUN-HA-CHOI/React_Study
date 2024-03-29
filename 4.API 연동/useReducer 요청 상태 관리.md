## useReducer 로 요청 상태 관리하기   

`useState` 대신에 `useReducer` 를 사용해서 구현을 해보도록 하겠다.  
`useReducer`를 사용하여 LOADING, SUCCESS, ERROR 액션에 따라 다르게 처리를 해 보자.    

```
import React, { useReducer, useEffect } from 'react';
import axios from 'axios';

function reducer(state, action) {
  switch(action.type) {
    case 'LOADING' :
      return {
        loading: true,
        data: null,
        error:null
      };
    case 'SUCCESS':
      return {
        loading:false,
        data: action.data,
        error: null
      };
    case 'ERROR' :
      return {
        loading: false,
        data: null,
        error: action.error
      };
      default:
        throw new Error(`Unhandled action type: ${action.type}`);
  }
}

function Users() {
  const [state, dispatch] = useReducer(reducer, {
    loading: false,
    data: null,
    error: null
  });
  
  const fetchUsers = async () => {
    dispatch({ type: 'LOADING' });
    try {
      const response = await axios.get(
        'https://jsonplaceholder.typicode.com/users'
      );
      dispatch({ type: 'SUCCESS', data: response.data });
    } catch (e) {
      dispatch({ type: 'ERROR', error: e });
    }
  };

  useEffect(() => {
    fetchUsers();
  }, []);
  
  const { loading, data: users, error } = state; // state.data 를 users 키워드로 조회

  if (loading) return <div>로딩중..</div>;
  if (error) return <div>에러가 발생했습니다</div>;
  if (!users) return null;
  return (
    <>
    <ul>
      {users.map(user => (
        <li key={user.id}>
          {user.username} ({user.name})
        </li>
      ))}
    </ul>
    <button onClick={fetchUsers}>다시 불러오기</button>
    </>
  );
}

export default Users;
```

`useReducer` 로 구현했을 때 장점은 `useState` 의 `setState` 함수를 여러번 사용하지 않아도 된다는점과,  
리듀서로 로직을 분리했으니 다른곳에서도 쉽게 재사용을 할 수 있다는 점 이다.   

`useState` 로 구현해도 무방하다.  
