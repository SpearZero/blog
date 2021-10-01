## 빌더 패턴(Effective Java)
이펙티브 자바를 읽던중 생성자에 "매개변수가 많다면 빌더를 고려하라"는 글을 보고 최근 프로젝트에서 lombok의 [@Builder](https://projectlombok.org/features/Builder) 어노테이션을 사용해 빌더 패턴을 사용한 기억이 났습니다. 빌더 패턴을 사용하더라도 왜 사용하는지 알고 써야 할것같아서 정리해봅니다.

> 인터넷을 찾아보니 GoF의 빌더 패턴과는 다른 접근방법 같지만 저는 이펙티브 자바의 빌더 패턴을 설명하겠습니다.

이 책에서는 객체를 생성하는 세 가지 방법을 소개합니다.

### 점층적 생성자 패턴(telescoping constructor pattern)
선택적 매개변수가 많을 때 보통 개발자들은 점층적 사용자 패턴(telescoping constructor pattern)을 사용했습니다.

점층적 생성자 패턴 작성 방법
- 필수 매개변수를 받는 생성자 생성
- 필수 매개변수와 선택 매개변수를 1개 받는 생성자 생성
- ...반복
- 필수 매개변수와 선택 매개변수를 n개 받는 생성자 생성

```
public class Book {
    // 필수 
    private final String isbn;
    private final String title;

    // 선택
    private final int page;
    private final int price;
    private final int weight;

    public Book(String isbn, String title) {
        this(isbn, title, 0);
    } 

    public Book(String isbn, String title, int page) {
        this(isbn, title, page, 0);
    }

    public Book(String isbn, String title, int page, int price) {
        this(isbn, title, page, price, 0);
    }

    public Book(String isbn, String title, int page, int price, int weight) {
        this.isbn = isbn;
        this.title = title;
        this.page = page;
        this.price = price;
        this.weight = weight;
    }
}
```

객체는 다음과 같이 생성합니다.

```
Book book = new Book("xxx-xx-xxxx-xxx-x", "book", 0, 100, 22000);
```

단점
- 원치 않는 매개변수가 포함되기 쉽다.
    - price값을 지정하기 위해 page에 매개변수 값을 0으로 지정
- 코드를 작성하거나 읽기 어렵다.
    - 각 매개변수가 무엇을 의미하는지 알기 어렵다.
    - 매개변수를 헷갈려서 잘못 지정하면 런타임에 잘못된 동작을 한다.

다른 대안으로는 자바빈즈 패턴(JavaBeans Pattern)이 있습니다.

### 자바빈즈 패턴(JavaBeans Pattern)
자바빈즈 패턴은 세터(setter)메서드들을 호출해 원하는 매개변수의 값을 설정합니다.

```
Book book = new Book();
book.setIsbn("xxx-xx-xxxx-xxx-x");
book.setTitle("book");
book.setPage(1000);
book.setPrice(15000);
book.setWeight(300);
```

점층적 생성자 패턴에 비해 가독성이 좋아졌습니다.

단점
- 객체가 생성되기 전까지 일관성(consistency)이 무너진 상태에 놓이게 된다.
    - 객체를 한 번에 생성하지 못함
- 불변(immutable)으로 만들 수 없다
    - setter를 호출하면 값이 변경됨

다른 대안으로는 빌더 패턴(Builder Pattern)이 있습니다.

### 빌더 패턴(Builder Pattern)
위에서 설명한 두 패턴의 장점을 결합한 방법입니다.(안정성 + 가독성)

```
public class Book {
    private final String isbn;
    private final String title;
    private final int page;
    private final int price;
    private final int weight;

    public static class Builder {
        // 필수
        private final String isbn;
        private final String title;
        // 선택
        private int page = 0;
        private int price = 0;
        private int weight = 0;

        public Builder(String isbn, String title) {
            this.isbn = isbn;
            this.title = title;
        }

        public Builder page(int page) {
            this.page = page;
            return this;
        }

        public Builder price(int price) {
            this.price = price;
            return this;
        }

        public Builder weight(int weight) {
            this.weight = weight;
            return this;
        }


        public Book build() {
            return new Book(this);
        }
    }

    private Book(Builder builder) {
        this.isbn = builder.isbn;
        this.title = builder.title;
        this.page = builder.page;
        this.price = builder.price;
        this.weight = builder.weight;
    }
}
```

빌더패턴의 객체를 생성하는 방법입니다.

```
Book book = new Book.Builder("xxx-xx-xxxx-xxx-x", "book")
                .page(100).price(23000).weight(350).build();
```

장점
- 읽기 쉽고, 쓰기 쉽다
- 변경 불가능한 객체를 만들 수 있다.

단점
- 코드가 많아진다.