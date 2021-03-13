# Annotations

!!! info
    - [Java 8 Langage 관련 튜토리얼 문서의 Annotations][java8_Tutorial_annotation]을 참고하였다.

[java8_Tutorial_annotation]: https://docs.oracle.com/javase/tutorial/java/annotations/index.html


**어노테이션(Annotation)** 은 프로그램의 일부는 아니지만 프로그램에 대한 정보를 제공하는 **메타데이타** 의 한 형태이다.
어노테이션이 추가된 코드의 동작에 어떤 영향도 주지 않는다.

사용예는 다음과 같다.

- 컴파일 정보 : 컴파일러가 에러(error)를 발견하거나 경고(warning)를 방지하기 위해 사용
- 컴파일 단계와 배포 단계 처리 : 소프트웨어 도구들이 코드, XML 파일등을 생성하기 위해 어노테이션 정보를 처리
- 런타임 처리 : 어떤 어노테이션들은 런타임에 사용된다.

Java 플랫폼(여기서는, Standard Edition)에 미리 정의된 어노테이션을 언제 사용하고, 어떻게 적용하는지에 대해 알아본다.
또헌, 강한 타입 체킹을 위해 타입 어노테이션(type annotations)들이 플러거블 타입 시스템(pluggable type system)과 어떤식으로 결합되고, 어떻게 중복 어노테이션(repeating annotation)을 구현하는지 살펴본다.

--------------

## 어노테이션 기본

어노테이션의 기본 형태는 다음과 같다.
```java
    @Entity
```

문자 ``@`` (at sign) 은 컴파일러에게 다음에 오는 내용이 어노테이션이라고 알린다. 예를 들어, 다음은 ``Override`` 라는 어노테이션에 대한 예제이다.
```java
    @Override
    Void OverrideMethod() { . . . }
```

어노테이션은 엘리먼트들을 포함할 수 있고, 엘리먼트들은 이름이 있거나 없을 수 있으며, 값도 가질 수 있다.
```java
    @Author(
        name = "Jongyun Jung"
        date = "7/01/2020"
    )
    class TestCalss() { . . . }
```

**name** 인 엘리먼트 하나만 있으면, 이름을 생략할 수 있고, 엘리먼트가 없으면 괄호를 생략할 수 있다.
```java
    @Author(name = "Jongyun Jung")
    class TestCalss() { . . . }
```

여러 엘리먼트들을 사용할 수도 있다.
```java
    @Author
    @Ebook
    class TestClass{ . . . }
```

같은 유형의 어노테이션들이 사용되면, 이를 중복 어노테이션이라 한다.
```java
    @Autho(name = "Kim")
    @Autho(name = "Lee")
    class TestClass{ . . . }
```

!!! note
    - 어노테이션 타입들은 **Java SE API** 의 ``java.lang`` 또는 ``java.lang.annotation`` 패키지에 정의된다.
    - 사용자 정의 어노테이션도 가능하다. 

--------------

## 어디에 사용하는가?

어노테이션은 **선언(declaration)** 에 많이 사용된다. (class,  field, method, 그외 다른 프로그램 요소들)

**Java SE 8** 에서는 타입들의 사용에 적용할 수 있다.

    - 객체 생성
        new @interned TestClass();

    - type cast
        myString = (@NonNull String) str;

    - implemets 문

이런 유형의 어노테이션들을 타입 어노테이션(type annotation)이라 한다.


## 어노테이션 타입 선언

많은 어노테이션들은 주석(comments) 을 대체한다.

```java
public class TestClass extends TestSuperClass{

    // Author: Kim
    // Date: 7/01/2020
    // Last Modified: 6/25/2020
    // By: Kim
    // Reviewers: Lee, Park

    // 코드
}
```
위 예제와 같이 주석으로 제공하는 정보들을 어노테이션 타입으로 정의하면 다음과 같다.

```java
@interface ClassPreamble{
    String author();
    String date();
    String lastModified() default "N/A";
    String lastModifiedBy() default "N/A";

    String[] reviewers();
}
```

어노테이션 타입을 정의하는 것은 ``interface`` 키워드 때문에 인터페이스 정의와 비슷하게 보인다. 어노테이션 타입은 인터페이스의 일종이다. 어노테이션 **body** 에는 메소드처럼 보이는 엘리먼트들에 대한 선언들을 포함한다. 엘리먼트들은 옵셔널 디폴트 값(optinal default value)을 정의할 수 있다.

어노테이션 타입을 정의하면, 엘리먼트들에 값을 지정해서 어노테이션을 사용할 수 있다.

```java
@ClassPreamble{
    author = "Kim",
    date = "7/01/2020",
    lastModified() = "6/25/2020",
    lastModifiedBy = "Kim",

    reviewers = {"Lee", "Park"}
}
```
!!! note
    **Javadoc** 이 생성하는 문서에 ``@ClassPreamble`` 이 나타나게 하려면, ``@Documented`` 어노테이션을 같이 사용해야 한다.
    
    ```java
        @Documented
        @interface ClassPreamble{
            // Annotation elements
        }
    ```

--------------

## 사전 정의 어노테이션

- __@Deprecated__

``` Java
// Javadoc comment follows
/**
    * @deprecated
    * explanation of why it was deprecated
    */
@Deprecated
static void deprecatedMethod() { }
```

- __@Override__

``` Java
// mark method as a superclass method
// that has been overridden
@Override 
int overriddenMethod() { }
```

- __@SupressWarnings__

``` Java
// use a deprecated method and tell 
   // compiler not to generate a warning
   @SuppressWarnings("deprecation")
    void useDeprecatedMethod() {
        // deprecation warning
        // - suppressed
        objectOne.deprecatedMethod();
    }
```

- __@SafeVarargs__
- __@Functionalface__

### 다른 어노테이션들에 적용되는 어노테이션들

다른 어노테이션에 적용되는 어노테이션들을 **meta-annotations** 이라 한다. 메타 어노테이션들은 ==java.lang.annoation== 에 정의되어 있다.

- __@Retention__ : 어노테이션이 어떻게 저장되는지를 기술한다.
    - RetentionPolicy.SOURCE
    - RetentionPolicy.CLASS
    - RetentionPolicy.RUNTIME

- __@Documented__ : 어노테이션들이 사용될 때마다, Javadoc 에 의해 문서화되어야 함을 지시한다.

- __@Target__ : 어노테이션이 적용되는 Java 요소들의 종류를 제한하기 위해 사용한다.

    - ElementType.ANNOTATION_TYPE
    - ElementType.CONSTRUCTOR
    - ElementType.FIELD
    - ElementType.LOCAL
    - ElementType.METHOD
    - ElementType.PACKAGE
    - ElementType.PARAMETER
    - ElementType.TYPE

- __@Inherited__ : 어노테이션 타입이 상속될 수 있음을 나타낸다. 클래스 선언에만 사용된다.

- __@Repeatable__ : 어노테이션이 여러번 적용될 수 있음을 나타낸다.

``` java
@Schedule(dayOfMonth="last")
@Schedule(dayOfWeek="Fri", hour="23")
public void doPeriodicCleanup() { ... }
//----------------------------------------
import java.lang.annotation.Repeatable;

@Repeatable(Schedules.class)
public @interface Schedule {
  String dayOfMonth() default "first";
  String dayOfWeek() default "Mon";
  int hour() default 12;
}

```

## 어노테이션 타입과 플러거블 타입 시스템

> 작성 필요함.
