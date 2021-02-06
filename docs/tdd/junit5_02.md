# JUnit 기본

!!! info
    - Java 의 **Annotation** 에 관해서는 간략한 [소개글](../effjava/annotation.md) 을 참고하시오.
    - JUnit 의 내용은 [Junit User Guide](https://junit.org/junit5/docs/current/user-guide) 를 참고하여 작성하였다.

## 어노테이션들 (Annotations)

- JUnit jupter 는 다음 어노테이션들을 지원한다.
- 거의 모든 핵심 어노테이션들은 ==junit-jupiter-api== 모듈의 ==org.junit.jupiter.api== 패키지에 위치한다.

| Annotions                   |    설명                                                  |
|:----------------------------|:---------------------------------------------------------|
| __@Test__                   | 테스트 메소드임을 알린다. 오버라이딩 되지 않으면 상속된다.    |
| __@ParameterizedTest__      | 메소드가 parameterized test 임을 알린다. 오버라이딩 되지 않으면 상속된다. |
| __@RepeatedTest__           | 메소드가 repeated test를 위한 템플릿임을 알린다.            |
| __@TestFactory__            | 메소드가 dynamic test를 위한 테스트 팩토리임을 알린다.          |
| __@TestTemplate__           | 멤소드가 test case 를 위한 템플릿임을 알린다.                |
| __@TestMethodOrder__        | 테스트 메소드의 실행 순서를 설정하기 위해 사용.               |
| __@TestInstance__           | 테스트 인스턴스의 라이프 싸이클을 설정하기 위해 사용.   |
| __@DisplayName__            | 테스트 클래스와 테스트 메소드의 표시명을 선언한다.   |
| __@DisplayNameGeneration__  | 테스트 클래스에서 display name generator를 선언한다.   |
| __@BeforeEach__             | 메소드가 각 현재 클래스의 ``@Test`` , ``@RepeatedTest``, ``@ParameterizedTst``, 또는 ``@TestFactory`` 메소드 전에 실행할 것을 알린다.   |
| __@AfterEach__              | 이후 실행  |
| __@BeforeAll__              | 모든 테스트 관련 메소드보다 먼저 실행된다. **per-class** 가 아니면 **static** 이어야 한다.   |
| __@AfterAll__               | 모든 테스트 메소드 이후에 실행된다. **per-class** 가 아니면 **static** 이어야 한다.   |
| __@Nested__                 | non-static 네스티드 테스트 클래스.   |
| __@Tag__                    | 필터링(filtering) 테스트를 위한 태그 선언. (클래스나 테스트 레벨에서)   |
| __@Disabled__               | 테스트 클래스나 메소드를 비활성화.   |
| __@Timeout__                | 테스트, 테스트 팩토리, 테스트 템플릿, 라이프싸이클 메소드가 시간을 초과하면 실패.  |
| __@ExtendWith__             | Used to register extensions declaratively. |
| __@RegisterExtension__      | Used to register extensions programmatically via fields.   |
| __@TempDir__                |   |


!!! info

    - JUnit Jupiter 어노테이션들은 **meta-annotaions** 으로 사용될 수 있다.
    - 따라서, meta-annotation의 시맨틱을 상속하는 composed annotation 을 정의할 수 있다.

    ``` java
    import java.lang.annotation.ElementType;
    import java.lang.annotation.Retention;
    import java.lang.annotation.RetentionPolicy;
    import java.lang.annotation.Target;

    import org.junit.jupiter.api.Tag;

    @Target({ ElementType.TYPE, ElementType.METHOD })
    @Retention(RetentionPolicy.RUNTIME)
    @Tag("fast")
    public @interface Fast {
    }
    ```

    - 다음 ``@Test`` 메소드에  ``@Fast`` 어노테이션을 사용한다.
    
    ``` java
    @Fast
    @Test
    void myFastTest() {
        // ...
    }
    ```


## 테스트 클래스와 테스트 메소드

- **Test Class** : 최상위 수준의 클래스, ``static`` 멤버 클래스, 또는 하나의 테스트 메소드를 포함하는 ``@Nested`` 클래스
    - 테스트 클래스는 반드시 하나의 생성자를 가져야 하고, ``abstract`` 클래스는 사용할 수 없다.

- **Test Method** : ``@Test``, ``@RepeatedTest``, ``@ParameterizedTest``, ``@TestFactory``, ``@TestTemplate`` 과 같은 어노테이션이 붙은 메소드들

- **Lifecycle Method** : ``@BeforAll``, ``@BeforeEach``, ``@AfterAll``, ``@AfterEach`` 의 어노테이션이 붙은 메소드


!!! info
    - Test Class 와 Test Method 는 반드시 **public** 일 필요는 없지만 **private** 이면 안된다.
    - Test method 와 lifecycle method 는 ``abstract`` 일 수 없고, 반환값을 가질 수 없다.


```java
import static org.junit.jupiter.api.Assertions.fail;
import static org.junit.jupiter.api.Assumptions.assumeTrue;

import org.junit.jupiter.api.AfterAll;
import org.junit.jupiter.api.AfterEach;
import org.junit.jupiter.api.BeforeAll;
import org.junit.jupiter.api.BeforeEach;
import org.junit.jupiter.api.Disabled;
import org.junit.jupiter.api.Test;

class StandardTests {

    @BeforeAll
    static void initAll() {
    }

    @BeforeEach
    void init() {
    }

    @Test
    void succeedingTest() {
    }

    @Test
    void failingTest() {
        fail("a failing test");
    }

    @Test
    @Disabled("for demonstration purposes")
    void skippedTest() {
        // not executed
    }

    @Test
    void abortedTest() {
        assumeTrue("abc".contains("Z"));
        fail("test should have been aborted");
    }

    @AfterEach
    void tearDown() {
    }

    @AfterAll
    static void tearDownAll() {
    }
}
```

## Display Names (표시명)

``` java
import org.junit.jupiter.api.DisplayName;
import org.junit.jupiter.api.Test;

@DisplayName("A special test case")
class DisplayNameDemo {

    @Test
    @DisplayName("Custom test name containing spaces")
    void testWithDisplayNameContainingSpaces() {
    }

    @Test
    @DisplayName("╯°□°）╯")
    void testWithDisplayNameContainingSpecialCharacters() {
    }

    @Test
    @DisplayName("😱")
    void testWithDisplayNameContainingEmoji() {
    }

}
```

### Display Name Generators

- JUnit Jupiter는 ``@DisaplayNameGeneration`` 으로  custom 표시명 생성기를 지원한다.
- Generator 들은 ``DisplayNameGenerator`` 를 implement 해서 생성.

| Display Name Generators     |   동작                                                  |
|:----------------------------|:---------------------------------------------------------|
| Standard                    | 표준 표시명 생성. |
| Simple                      | 매개변수가 없는 메소드의 괄호를 제거. |
| ReplaceUnderScore           | 공백은 ``_`` (underscore)로 교체. |
| IndicativeSentence          | 테스트 이름과 클래스 이름을 연결해서 문장(sentence)으로 생성. |

- ``@IndicativeSentencesGeneration`` 을 사용해서 분리자(separator)와 생성기(gerator)를 커스터마이징한 예제.

``` java
import org.junit.jupiter.api.DisplayName;
import org.junit.jupiter.api.DisplayNameGeneration;
import org.junit.jupiter.api.DisplayNameGenerator;
import org.junit.jupiter.api.IndicativeSentencesGeneration;
import org.junit.jupiter.api.Nested;
import org.junit.jupiter.api.Test;
import org.junit.jupiter.params.ParameterizedTest;
import org.junit.jupiter.params.provider.ValueSource;

class DisplayNameGeneratorDemo {

    @Nested
    @DisplayNameGeneration(DisplayNameGenerator.ReplaceUnderscores.class)
    class A_year_is_not_supported {

        @Test
        void if_it_is_zero() {
        }

        @DisplayName("A negative value for year is not supported by the leap year computation.")
        @ParameterizedTest(name = "For example, year {0} is not supported.")
        @ValueSource(ints = { -1, -4 })
        void if_it_is_negative(int year) {
        }

    }

    @Nested
    @IndicativeSentencesGeneration(separator = " -> ", generator = DisplayNameGenerator.ReplaceUnderscores.class)
    class A_year_is_a_leap_year {

        @Test
        void if_it_is_divisible_by_4_but_not_by_100() {
        }

        @ParameterizedTest(name = "Year {0} is a leap year.")
        @ValueSource(ints = { 2016, 2020, 2048 })
        void if_it_is_one_of_the_following_years(int year) {
        }
    }
}
```
- 출력 결과

``` console
+-- DisplayNameGeneratorDemo [OK]
  +-- A year is not supported [OK]
  | +-- A negative value for year is not supported by the leap year computation. [OK]
  | | +-- For example, year -1 is not supported. [OK]
  | | '-- For example, year -4 is not supported. [OK]
  | '-- if it is zero() [OK]
  '-- A year is a leap year [OK]
    +-- A year is a leap year -> if it is divisible by 4 but not by 100. [OK]
    '-- A year is a leap year -> if it is one of the following years. [OK]
      +-- Year 2016 is a leap year. [OK]
      +-- Year 2020 is a leap year. [OK]
      '-- Year 2048 is a leap year. [OK]
```


### Setting the Default Display Name Generators

- ``junit.jupiter.displayname.generator.default`` 의 설정 파라미터(configuration parameter) 사용

- ``src/test/resources/junit-platform.properties``

``` 
junit.jupiter.displayname.generator.default = \
    org.junit.jupiter.api.DisplayNameGenerator$ReplaceUnderscores
```    

!!! info "display name 우선순위"

    1. 만약 있다면, ``@DisplayName`` annotation 의 값
    2. 만약 있다면, ``@DisplayNameGeneration`` annotation에 기술된 ``DisplayNameGenerator`` 를 호출
    3. 만약 있다면, configuration parameter 로 설정된 ``default DisplayNameGenerator`` 호출
    4. ``org.junit.jupiter.api.DisplayNameGenerator.Standard`` 호출


## Assertions

## Assumptions


## Disabling Tests

## Conditional Test Execution

## Tagging and Filtering

## Text Exectuion Order



## Test Instance Lifecycle

!!! info "@BeforeAll, @Afterall"

    - ``@BeforeAll`` , ``@Afterall`` 을 사용한 테스트 메소드들은 반드시 **static** 이어야 한다.
    - ``@BeforeEach``, ``@AfterEach`` 에 static 이면 안된다.

## Nested Tests

## Dependecncy Injection for Constructors and Methods

## Repeated Tests

## Parameterized Tests

## Dynamic Tests

## Timeouts

## Parallel Execution

## Built-in Extensions


> 참고
[^1]:
    JUnit5 User Guide : https://junit.org/junit5/docs/current/user-guide/

