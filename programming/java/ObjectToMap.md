## Java Object를 Map으로 바꾸기

같이 프로젝트하는 프리랜서분이 Java Object를 Map으로 변환하는 기능을 만들어달라고 요청하셨다. 인터넷을 검색하던 중 [Apache Commons BeanUtils](https://commons.apache.org/proper/commons-beanutils/)을 이용하면 쉽게 변환할 수 있다고 해서 프로젝트 내부에서 찾아보니 Apache Commons BeanUtils 라이브러리를 찾을 수 없었다. 다른 방법을 찾던중 Jackson의 ObjectMapper를 이용하면 Java Object를 쉽게 Map 으로 변경할 수 있다는 것을 알게 되었다. 다행히 프로젝트 내부에서 Jackson 라이브러리를 찾을 수 있었고 Jackson의 ObjectMapper를 이용하여 기능을 만들었다.

ObjectMapper를 사용하기 위해 먼저 Jackson 라이브러리를 추가해야한다.
```
// Maven
<dependency>
    <groupId>com.fasterxml.jackson.core</groupId>
    <artifactId>jackson-databind</artifactId>
    <version>2.11.0</version>
</dependency>

// Gradle
compile group: 'com.fasterxml.jackson.core', name: 'jackson-databind', version: '2.11.0'
```

예시를 위해 Person 클래스를 작성

```
public class Person {

    public Person() {}

    public Person(int age, String name) {
        this.age = age;
        this.name = name;
    }

    private int age;

    private String name;

    public int getAge() {
        return age;
    }

    public String getName() {
        return name;
    }

    @Override
    public String toString() {
        return "Person {" + "age=" + age + ", name='" + name + '}';
    }
}
```

Main 클래스에서 ObjectMapper를 이용해 Object를 Map으로 변경

```
import com.fasterxml.jackson.databind.ObjectMapper;

import java.util.Map;

public class Main {
    public static void main(String[] args) {
        ObjectMapper objectMapper = new ObjectMapper();
        Person person = new Person(12, "kim");

        Map<String, Object> map = objectMapper.convertValue(person, Map.class);

        // [실행 결과]
        // age : 12
        // name : kim
        for(String key : map.keySet()) {
            System.out.println(key + " : " + map.get(key));
        }
    }
}
```

반대로 Map을 Object로 변경하는 것도 가능하다.

```
import com.fasterxml.jackson.databind.ObjectMapper;

import java.util.Map;

public class Main {
    public static void main(String[] args) {
        ObjectMapper objectMapper = new ObjectMapper();
        Person person = new Person(12, "kim");

        Map<String, Object> map = objectMapper.convertValue(person, Map.class);

        Person person2 = objectMapper.convertValue(map, Person.class);

        // [실행 결과]
        // Person {age=12, name='kim}
        System.out.println(person2);
    }
}
```
