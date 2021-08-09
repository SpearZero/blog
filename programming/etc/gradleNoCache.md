## 오프라인 상태에서 no cached version available for offline mode 문제해결

현재 파견나와서 근무하고 있는곳은 폐쇄망을 사용하기 때문에 외부에서 gradle을 이용해 라이브러리를<br> 가져올 수 없다.<br>
그래서 필요한 라이브러리들이 포함된 폴더(modules-2)를 반입한 후, GRADLE_USER_HOME을 설정하고<br> caches 폴더에 modules-2 폴더를 넣었는데 프로젝트에서 라이브러리를 받아올 수 없었다.

이것저것 해보다가 해결할 수 없어서 인터넷을 검색해보니 다음과 같은 글을 발견하게 되었다.

> https://discuss.gradle.org/t/copying-the-gradle-cache-to-another-machine/7546

이 글을 읽다보니 다음과 같은 답변을 찾을 수 있었다.<br>

> I’m guessing that ‘gradleHome’ on the second PC doesn’t have exactly the same path<br> as the first PC? In that case you’re hitting a limitation on the portability of our cache:<br> all cached files are referenced by absolute path.

만약 원래 PC에서 /Users/abc/.gradle 경로를 사용했다면, 복사된 modules-2를 가져온 PC에서도 <br>.gradle의 경로를 /Users/abc/.gradle로 설정해야 한다.<br>
위의 답변을 보고 경로를 똑같이 설정했더니 라이브러리를 가져 올 수 있었다.
