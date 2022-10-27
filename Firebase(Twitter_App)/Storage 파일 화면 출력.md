### Storage 에 있는 사진을 화면에 띄우기  

<img src="https://user-images.githubusercontent.com/97012561/198026479-1c6810f1-9ba1-4b33-9bd5-e9dd00c70227.png" width="600" height="230">

먼저 Home 컴포넌트에 getDownloadURL를 import 한다. 다음 onSubmit 파일 업로드 경로 코드 밑에 
```
getDownloadURL(ref(storage,response.ref)) 이거를 가져와서 사용한다. 코드에 맞게 변형을 해 줘야 되는데,
attachmentUrl = await getDownloadURL(ref(storage, response.ref)); 이렇게 바꿔준다. 

화면에 보이기 위해서는 
addDoc 에 attachmentUrl를 추가한다. 
```

```
전체코드
 const onSubmit = async(e) => {
    e.preventDefault(); 
    let attachmentUrl =""
     if (attachment !== "") {
      const storageRef = ref(storage, `${userObj.uid}/${uuidv4()}`);
      //ref(파일경로),로그인한 사용자정보를 폴더로 만들고,uuidv4 파일이름 id 생성 = 파일경로가됨 
      const response = await uploadString(storageRef,attachment ,'data_url'); //파일 업로드되는곳 , uploadString는 화면에 가져오는 역할
     
      attachmentUrl = await getDownloadURL(ref(storage, response.ref)) //response에 업로드된 파일이 ref속성에 다운로드 되어야됨
     }

    await addDoc(collection(db, "tweets"), { //tweets라는 이름이 addDoc에 의해 자동으로 문서 추가됨
      text: tweet, //객체 하나가 문서임
      createAt: Date.now(), 
      createId: userObj.uid,
      attachmentUrl,  //attachmentUrl :attachmentUrl 키와값이 값을 때 하나만 써준다 
    });
    setTweet(""); //입력과 동시에 비워줌
    setAttachment(""); 
  }

```
사진이 있을 때만 나타나는 것이 아니므로 사진이 없을 때의 코드도 만들어줘야 한다. 그래서 파일경로 코드를 ` if (attachment !== "") {} `로 감싸준다.  

**여기서 잠깐!**  
이 상태에서는 텍스트를 Tweet 하면 화면에 나타나지 않는다.   
사진은 화면에 뜨지만 텍스트만 입력했을 땐 화면에 나타나지 않는다. 그래서 **setAttachment("");** 빈 값을 추가 해 주고     
**let attachmentUrl =""**  로 바꿔야 텍스트 입력, 사진 입력 둘 다 가능하다.      


<img src="https://user-images.githubusercontent.com/97012561/198046863-40104d10-2776-41c4-94d0-906d5a623ab7.png" width="800" height="300">   

이렇게 텍스트 필드에 빈 값이 있는 것을 확인할 수 있다. 이것은 사진을 추가해서 빈 값이 된 것이다.   


    
마지막에 useEffect가 적용돼서 배열이 새로 바뀌면 렌더링이 되어 tweet 컴포넌트가 호출 된다. 트윗 컴포넌트에서    
tweetObj.attachmentUrl 가 있으면 이미지가 화면에 뜨게 해라!  
```
<h4>{tweetObj.text}</h4> 
        {tweetObj.attachmentUrl && (
          <img src={tweetObj.attachmentUrl} width="50" height="50" />
        )} 
```



