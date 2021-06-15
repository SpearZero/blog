## java.lang.NoSuchMethodException

프로그램을 실행하다가 다음과 같은 에러가 발생했다.

```
java.lang.NoSuchMethodException : 패키지경로.클래스이름.<init>()
    at java.lang.Class.getConstructor0
    at java.lang.Class.getDeclaredConstructor
```

이 문제를 해결하기 위해서 에러가 표시된 콘솔창을 찾아봤더니 다음과 같은 문구가 보였다.

```
No primary or default constructor found for class 패키지명.클래스이름
```

그래서 에러가 발생한 클래스에 public 기본 생성자를 추가했더니 에러가 해결됐다.

NoSuchMethodException에러가 발생하는 이유를 찾아보니 리플렉션이 존재하지 않는 메서드에 접근할 때<br> 나는 에러라고한다. 그리고 getConstuctor에 대한 설명을 찾아보니 다음과 같은 문구를 볼 수 있었다.

> Returns a Constructor object that reflects the specified public constructor of the class represented<br> by this Class object.

getConstructor 메서드는 public 생성자들만 리턴한다.
