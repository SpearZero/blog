## 자바 파일 업로드시 권한 설정하기
현재 진행중인 프로젝트에 이미지를 업로드하는 기능이 있습니다. 기능을 테스트하는 도중 이미지 업로드가 되지 않는다고해서 관련 코드를 살펴봤는데 이상이 없었습니다. 리눅스 서버에서만 업로드가 되지 않는것 같아서 관련된 부분에 대해 로그를 살펴보니 다음과 같은 로그가 표시되고있었습니다.

```
// access denied
[XXXRestController:184] - java.io.FileNotFoundException: /example/abc/images/abc/example/xyz/abc.jpeg (허가 거부)
```

리눅스 디렉터리에 대한 권한문제인것 같아서 인터넷에 검색을 해보니 다음과 같은 해결법을 찾을 수 있었습니다.

> https://stackoverflow.com/questions/6233541/java-set-file-permissions-to-777-while-creating-a-file-object

저는 이 문제를 [java.io.File](https://docs.oracle.com/javase/8/docs/api/java/io/File.html)의 setReadable, setExecutable, setWritable 메서드를 이용해서 해결했습니다.

```
String path = "/path";
File dir = new File(path);

if (!dir.exists()) {
    dir.mkdir(); 
    dir.setReadable(true, false); 
    dir.setExecutable(true, false); 
    dir.setWritable(true, false); 
}
```

디렉터리를 생성후에 권한을 부여하니 이미지를 업로드 할 수 있었습니다.

각 메서드에 대한 설명은 다음과 같습니다.

```
public boolean setReadable(boolean readable, boolean ownerOnly)
소유자 또는 모두에게 경로에 대한 읽기 권한을 설정한다. readable이 true인 경우 읽기 권한을 허용한다. ownerOnly가 false인 경우 모두에게 읽기를 허용한다.

public boolean setExecutable(boolean executable, boolean ownerOnly)
소유자 또는 모두에게 경로에 대한 실행 권한을 설정한다. executable이 true인 경우 실행 권한을 허용한다. ownerOnly가 false인 경우 모두에게 실행을 허용한다.

public boolean setWritable(boolean writable, boolean ownerOnly)
소유자 또는 모두에게 경로에 대한 쓰기 권한을 설정한다. writable이 true인 경우 쓰기 권한을 허용한다. ownerOnly가 false인 경우 모두에게 쓰기를 허용한다.
```

---

참고

- https://stackoverflow.com/questions/6233541/java-set-file-permissions-to-777-while-creating-a-file-object
- https://docs.oracle.com/javase/8/docs/api/java/io/File.html


