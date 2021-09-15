## 정규표현식 전방탐색과 후방탐색

프로그래머스 문제를 풀던 중 전화번호 뒷 4자리를 제외한 나머지를 *로 치환하는 문제를 접하게 되었습니다. 예전에 정규표현식 책에서 본 전방탐색을 이용해서 풀면 되겠다는 생각을 했지만 문법이 기억나지 않아서 전방탐색을 찾아본 후 문제를 풀 수 있었습니다. 

전방탐색과 후방탐색에 대해 알아본 것을 간단하게 정리하겠습니다.

### 전방탐색
패턴에 일치하는 텍스트 자체는 소비하지 않고, 일치하는 텍스트의 앞 부분을 탐색합니다. 패턴의 구문은 ?= 를 사용합니다.

```
String addr1 = "http://www.example.example";
String addr2 = "https://www.example.example";
String addr3 = "mailto:example@example.com";

// :의 앞부분을 탐색한다.
Pattern pattern = Pattern.compile(".+(?=:)");
Matcher matcher = pattern.matcher(addr1);

if (matcher.find()) {
    // http
    System.out.println(matcher.group());
}

matcher = pattern.matcher(addr2);
if (matcher.find()) {
    // https
    System.out.println(matcher.group());
}

matcher = pattern.matcher(addr3);
if (matcher.find()) {
    // mailto
    System.out.println(matcher.group());
}
```

### 후방탐색
전방탐색과 반대로 일치하는 텍스트의 뒷 부분을 탐색합니다. 패턴의 구문은 ?<= 을 사용합니다.

```
String money1 = "$1234";
String money2 = "$12.34";
String money3 = "$12345";

// $의 뒷부분을 탐색한다.
Pattern pattern = Pattern.compile("(?<=\\$)\\w+");
Matcher matcher = pattern.matcher(money1);

if (matcher.find()) {
    // $1234 -> 1234
    System.out.println(matcher.group());
}

matcher = pattern.matcher(money2);
if (matcher.find()) {
    // $12.34 -> 12
    System.out.println(matcher.group());
}

matcher = pattern.matcher(money3);
if (matcher.find()) {
    // $12345 -> 12345
    System.out.println(matcher.group());
}
```