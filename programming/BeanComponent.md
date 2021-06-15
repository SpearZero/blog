## @Bean vs @Component

스프링 코어 강의를 보던중 스프링에서 자바 클래스를 빈(Bean) 객체로 등록하기 위해 사용하는 두 가지 어노테이션이 있다고 했다. @Bean과 @Component 어노테이션인데, 강의에서는 어노테이션 사용법만 알려주고 두 어노테이션의 차이점을 알려주지 않아서 차이점을 알아보기로 했다.

### @Bean
```
@Configuration
public class TestConfig {
  
  @Bean
  public ArrayList<Integer> listBean() {
    return new ArrayList<Integer>();
  }
}
```

### @Component
```
@Configuration
@ComponentScan(basePackages = "com.springex.college")
public class TestConfig {
	
}
```

```
@Component
public class College {
	
}
```

### 차이점
- @Component
    - 컴포넌트 스캔을 통해 자동으로 빈이 탐색되고 등록된다.
    - @Component 어노테이션이 선언된 클래스와 빈 사이의 묵시적인 매핑이 있다
- @Bean
    - 자동으로 빈 설정을 해주는 @Component 어노테이션과 달리 빈을 명시적으로 선언한다.
    - 클래스 정의와 빈 설정을 분리하기 때문에 개발자가 빈 객체를 생성하고 설정하는것을 선택할 수 있다. 
    - @Component 어노테이션을 사용할 수 없는 클래스(개발자가 통제할 수 없는 외부 라이브러리 등)에 사용한다.
    - 상황에 따라 구현 클래스를 변경해야 할 때 사용(Repository를 다른 Repository로 변경 할 때)
---

참고

- https://stackoverflow.com/questions/10604298/spring-component-versus-bean
- [스프링 입문 - 코드로 배우는 스프링 부트, 웹 MVC, DB 접근기술, 섹션4 자바 코드로 직접 스프링 빈 등록하기](https://www.inflearn.com/course/%EC%8A%A4%ED%94%84%EB%A7%81-%EC%9E%85%EB%AC%B8-%EC%8A%A4%ED%94%84%EB%A7%81%EB%B6%80%ED%8A%B8)
