## Cloud Firestore 사용하기  

1. 앱,웹을 만들었을 때 그 안에 정보들을 저장하기 위해 데이터 베이스를 만들어야 된다.  
<img src="https://user-images.githubusercontent.com/97012561/197724013-25a3ca9e-7929-43ef-8a0d-eeb03488f1aa.png" width="500" height="200">  

2. 데이터 베이스를 만들고 테스트 모드에서 시작 (프로덕션 모드는 보안규칙상 일단 사용x)  
<img src="https://user-images.githubusercontent.com/97012561/197724691-43d8ea0d-6901-46ba-8e89-ddf0b58df8a7.png" width="500" height="270">  

3.  Cloud Firestore 위치는 asia-northeast1 선택 (생성하는데 시간 소요될 수 있음)  
<img src="https://user-images.githubusercontent.com/97012561/197724985-cba8206e-85fd-46d8-9ad2-f43d443a71cb.png" width="500" height="270">    

4. 생성되면 아래와 같은 화면이 나타나며 컬렉션 시작(=폴더)을 눌러 폴더를 만든다.   
   컬렉션 ID도 설정. id는 자동 생성됨. 텍스트 필드,값에는 임의의 텍스트 입력.    
<img src="https://user-images.githubusercontent.com/97012561/197725576-d32ecff8-0201-44f7-b45d-a1582c47d3d0.png" width="500" height="280">     
<img src="https://user-images.githubusercontent.com/97012561/197725770-64cbbe6d-d2b4-4d4f-a4d7-ee7bc645db4f.png" width="500" height="280">  

5. 생성하면 아래와 같은 화면이 나타남.  
<img src="https://user-images.githubusercontent.com/97012561/197726132-c1c219d2-016a-4bce-846d-9adaf2081fef.png" width="500" height="280">    

6. 데이터 베이스를 사용하기 위해 개발환경설정을 해 줘야 된다. 아래와 같은 import를 복붙하여 fbase 컴포넌트에 입력해줌.  
<img src="https://user-images.githubusercontent.com/97012561/197726570-53066a50-7790-4ed1-af44-5652efe8efab.png" width="500" height="280">    

7. 또한 초기화, 즉 정보를 내보내기를 하기 위해 아래의 코드도 복붙하여 fbase 컴포넌트 아래에 export 해준다.  
<img src="https://user-images.githubusercontent.com/97012561/197727078-3e56ffae-fcae-4816-878c-2472c6825328.png" width="500" height="280">    

8. 데이터 베이스에서 자동으로 ID를 생성하기 위해 다음과 같은 코드를 Home 컴포넌트에 삽입하고 수정한다.   
<img src="https://user-images.githubusercontent.com/97012561/197727725-a4f4e36d-2d56-4fda-bd47-ff5554ab325c.png" width="500" height="280">    

다음과 같이 수정 해 줬다.  

```
const onSubmit = async(e) => {
    e.preventDefault();
    await addDoc(collection(db, "tweets"), { //tweets라는 이름이 addDoc에 의해 자동으로 문서 추가됨
      text: tweet,     //객체 하나가 문서임
      createAt: Date.now(), 
      createId: userObj.uid,
    });
    setTweet(""); //입력과 동시에 비워줌
  }

```

9. 로그인을 하고 화면에 입력을 해 주면 입력한 값이 `createId: userObj.uid,` 에 의해 데이터 베이스에 전달되어 문서로 저장되는 것을 확인할 수 있다.  
<img src="https://user-images.githubusercontent.com/97012561/197729171-3093f7fb-1cd4-4a1f-950d-2c54527fa13e.png" width="500" height="200">    
<img src="https://user-images.githubusercontent.com/97012561/197729281-92bf595d-d071-4336-bb18-9a2132b63db3.png" width="600" height="220">   

10. 여기까지는 새로고침을 계속 해 주어야 화면에 입력한 값들이 뜬다. 자동으로 새로고침이 되는 방법을 해 보자.  
    데이터 읽기로 들어가 실시간 업데이트 수신 대기로 접속한다.     
    import 에서 onSnapshot 를 Home 컴포넌트에 넣어주고(이는 문서를 사진형태로 내보내는 역할을 한다.)    
    const 다음부분부터 복붙하여 삽입.   
<img src="https://user-images.githubusercontent.com/97012561/197731532-2bb0ceaf-cdb5-4087-83f6-ae0f0ea3e7b1.png" width="600" height="250"> 

아래와 같은 형태로 바꿔줄 수 있다.  
```
  useEffect(() => {
   //getTweets();
    const q = query(collection(db, "tweets"));
    const unsubscribe = onSnapshot(q, (querySnapshot) => {
      const newArray = [];
      querySnapshot.forEach((doc) => {
        newArray.push(doc.data());
      });
       //console.log(newArray);
       setTweets(newArray);
    });
  },[]);
```
 








