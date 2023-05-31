## API 연동하기   

리액트 애플리케이션에서 API 를 연동하는 방법을 배운다. 웹 애플리케이션을 만들 때 데이터를 브라우저에서만 들고 있는  
것이 아니라, 데이터를 보존시키고, 다른 사람들도 조회 할 수 있게 하려면 서버를 만들고 서버의 API 를 사용해서    
데이터를 읽고, 써야 된다.    

프로젝트에서는 주로 Redux 라는 라이브러리와 Redux 미들웨어를 함께 사용하여 구현을 하는 경우가 많다.(나중에 공부)   
이 도구들은 API 연동을 할 때 필수적인 요소는 아니다. 이번 API 연동을 배우게 될 때에는 Redux 없이, 그냥 컴포넌트에서   
API 연동을 하게 될 때 어떻게 해야 하는지 알아보고, 더 깔끔한 코드로 구현하는 방법도 다뤄보도록 하겠다.   

자바스크립트의 비동기 처리 에 대한 기본적인 개념을 숙지하고 있어야 한다. `Promise` 와 `async/await` 를 잘 알아야된다.  

<hr>

## API 연동의 기본  

API 호출 하기 위해서 axios 라는 라이브러리를 설치한다. `npm install axios`   
axios를 사용해서 GET, PUT, POST, DELETE 등의 메서드로 API 요청을 할 수 있다.   

* REST API 를 사용 할 때에는 하고 싶은 작업에 따라 다른 메서드로 요청을 할 수 있는데 메서드들은 다음 의미를 가지고 있다.  
  * GET: 데이터 조회  
  * POST: 데이터 등록   
  * PUT: 데이터 수정   
  * DELETE: 데이터 제거   

* axios 의 사용법  
```
import axios from 'axios';

axios.get('/users/1');
```

`get` 이 위치한 자리에는 메서드 이름을 소문자로 넣는다. 예를 들어서 새로운 데이터를 등록하고 싶다면 `axios.post()` 사용  
그리고 파라미터에는 API 의 주소를 넣는다.   

`axios.post()` 로 데이터를 등록 할 때에는 두번째 파라미터에 등록 하고자 하는 정보를 넣을 수 있다.   
```
axios.post('/users', {
 username: 'blabla',
 name: 'blabla'
});
```

이번 API 연동 실습을 할 때에는 JSONPlaceholder 에 있는 연습용 API 를 사용할 것이다.  
[API주소](https://jsonplaceholder.typicode.com/users)  

<hr>

## useState 와 useEffect 로 데이터 로딩하기  

`useState` 를 사용하여 요청 상태를 관리하고, `useEffect` 를 사용하여 컴포넌트가 렌더링되는 시점에 요청을 시작하는  
작업을 해 보겠다.  

* 요청에 대한 상태를 관리 할 때엔는 다음과 같이 총 3가지 상태를 관리 헤주어야 한다.  
  * 요청의 결과  
  * 로딩상태  
  * 에러  

```
import React, { useState, useEffect } from 'react';
import axios from 'axios';

function Users() {
  const [users, setUsers] = useState(null);
  const [loading, setLoading] = useState(false);
  const [error, setError] = useState(null);

  useEffect(() => {
    const fetchUsers = async () => {
      try {
        // 요청이 시작 할 때에는 error 와 users 를 초기화하고
        setError(null);
        setUsers(null);
        // loading 상태를 true 로 바꿉니다.
        setLoading(true);
        const response = await axios.get(
          'https://jsonplaceholder.typicode.com/users'
        );
        setUsers(response.data); // 데이터는 response.data 안에 들어있습니다.
      } catch (e) {
        setError(e);
      }
      setLoading(false);
    };

    fetchUsers();
  }, []);

  if (loading) return <div>로딩중..</div>;
  if (error) return <div>에러가 발생했습니다</div>;
  if (!users) return null;
  return (
    <ul>
      {users.map(user => (
        <li key={user.id}>
          {user.username} ({user.name})
        </li>
      ))}
    </ul>
  );
}

export default Users;
```

`useEffect` 에 첫번째 파라미터로 등록하는 함수에는 `async` 를 사용 할 수 없기 때문에 함수 내부에서 `async` 를   
사용하는 새로운 함수를 선언해주어야 한다.  

로딩 상태가 활성화 됐을 땐 `로딩중..` 이라는 문구를 보여준다.  

`users` 값이 아직 없을 때에는 `null` 을 보여주도록 처리했다.  

마지막에는 `users` 배열을 렌더링하는 작업을 해 주었다.  

<hr>

## 에러 발생 확인하기  

주소를 이상하게 바꿔서 에러가 발생하는지도 확인 헤 보자  

![image](https://user-images.githubusercontent.com/97012561/217148272-436a8c34-7297-4e6e-967c-556b36839215.png)

<hr>

## 버튼을 눌러서 API 재요청하기  
버튼을 눌러서 API 를 재요청하는 기능을 구현 해 보자  
그렇게 하려면, 구현했던 `fetchUsers` 함수를 바깥으로 꺼내주고, 버튼을 만들어서 해당 함수를 연결 해 주면 된다.   

```
import React, { useState, useEffect } from 'react';
import axios from 'axios';

function Users() {
  const [users, setUsers] = useState(null);
  const [loading, setLoading] = useState(false);
  const [error, setError] = useState(null);
  
  const fetchUsers = async () => {
    try {
      // 요청이 시작 할 때에는 error 와 users 를 초기화하고
      setError(null);
      setUsers(null);
      // loading 상태를 true 로 바꿉니다.
      setLoading(true);
      const response = await axios.get(
        'https://jsonplaceholder.typicode.com/users'
      );
      setUsers(response.data); // 데이터는 response.data 안에 들어있습니다.
    } catch (e) {
      setError(e);
    }
    setLoading(false);
  };

  useEffect(() => {
    fetchUsers();
  }, []);

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





 
