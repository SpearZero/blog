## HTTP Method PUT, PATCH

일하는 곳에서 소스코드를 분석하던 중 HTTP 메서드로 PUT과 PATCH를 요청 받는곳을 발견했다. 여태까지<br> 사용해본 HTTP 메서드라고는 POST와 GET밖에 없기 때문에, 두 메서드가 정확히 무슨일을 하는지 정확히 몰라서<br> 두 메서드에 대해 찾아보게 되었다.

### PUT

요청 페이로드를 사용해 새로운 리소스를 생성하거나, 대상 리소스를 나타내는 데이터를 대체한다.<br> 여기서 요청 페이로드를 사용해 새로운 리소스를 생성한다는 뜻은 만약 PUT /v1/coffees/orders/1234 요청을<br> 보냈는데 대상 리소스를 나타내는 데이터가 없다면 리소스를 생성한다는 뜻이다. 대상 리소스를 나타내는<br> 데이터를 대체한다는 것은 PUT /v1/coffees/orders/1234 요청에 페이로드를 담아 보냈을 때 대상 리소스가<br> 존재한다면 리소스를 나타내는 데이터를 대체한다는 뜻이다.

PUT 메서드는 멱등성을 가지는데, 여기서 멱등성이란 몇 번을 호출하더라도 동일한 결과를 리턴한다는 뜻이다.

### PATCH

리소스의 부분적인 수정을 할 때 사용되며, 멱등성을 가지지 않는다.

---

참고

- https://developer.mozilla.org/ko/docs/Web/HTTP/Methods/PUT
- https://developer.mozilla.org/ko/docs/Web/HTTP/Methods/PATCH
- [RESTful 자바 패턴과 실전 응용 - 1장](https://book.naver.com/bookdb/book_detail.nhn?bid=8509490)
