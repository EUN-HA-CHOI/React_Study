### Storage 에서 사진이 삭제 될 수 있게 하는 방법  

<img src="https://user-images.githubusercontent.com/97012561/198050986-860fc0ae-3a76-4a02-bba5-b14aec9c342c.png" width="600" height="300">  

Tweet 컴포넌트에 `import { ref, deleteObject } from "firebase/storage";` import 한다.  

onDeleteClick  부분에서 deleteDoc 밑에 다음의 코드를 추가한다.   

```
if(tweetObj.attachmentUrl !== "") {
      const deleteRef = ref(storage, tweetObj.attachmentUrl);
      await deleteObject(deleteRef)
    } 
```

