## private 접근자

이펙티브 자바의 아이템 17 "변경 가능성을 최소화하라"의 본문에 다음과 같은 예제가 있습니다.


```
public final class Complex {
    private final double re;
    private final double im;

    public Complex(double re, double im) {
        this.re = re;
        this.im = im;
    }

    public Complex plus(Complex c) {
        return new Complex(re + c.re, im + c.im);
    }

    // 생략...
}
```

예제를 살펴보던중 plus 메서드에 매개변수로 넘겨진 객체에서 멤버변수에 직접 접근하는 것을 발견했습니다. 

저는 private로 선언된 멤버변수는 "c<z>.re"와 같은 형식으로 호출하지 못하는걸로 알고있었기 때문에 private 접근자에 대해 다시 알아보기로 했습니다.(알아본 결과 제가 private에 대해 잘못 이해하고 있었음..)

알아 본 결과 다음과 같은 정보를 얻을 수 있었습니다.

[Determining Accessibility](https://docs.oracle.com/javase/specs/jls/se8/html/jls-6.html#jls-6.6.1)

> Otherwise, the member or constructor is declared private, and access is permitted if and only if it occurs within the body of the top level class that encloses the declaration of the member or constructor.

> 멤버(클래스, 인터페이스, 필드, 메서드) 또는 생성자가 private로 선언되면 멤버 또는 생성자의 선언을 둘러싸는 탑 레벨 클래스의 본문 내에서만 액세스가 허용됩니다.

plus 메서드의 매개변수로 넘겨진 객체(Complex c)는 탑 레벨 클래스의 본문내에서 접근하기 때문에 멤버변수에 직접 접근할 수 있었습니다.

