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

- JUnit 4 의 assertoin 메소드들을 포함해서 Java 8 람다를 사용하는 메소드들이 추가되었다.

| 메소드들                        |    설명                                                      |
|:---------------------------------|:-------------------------------------------------------------|
| assertEquals(), assertTrue()     | Standard Assertions                                           |
| assertAll(Executable...), assertAll(String, Executable...), assertAll(Collection), assertAll(String, Collection), assertAll(Stream)                     | Group assertions - 모든 assertions 실행, 모든 faili일 보고된다. |
| \<T extends Throwable> T assertThrows​(Class<T> expectedType, Executable executable)| |
| assertTimeout(ofMinutes(2), () -> {  Perform task that takes less than 2 minutes. }); || 

- Third-party Assertion Libraries

    - AsertJ, Hamcrest, Truth


## Assumptions
- org.junit.jupiter.api.Assumptions 클래스의 **static** 메소드들

| 메소드들                        |    설명                                                      |
|:---------------------------------|:-------------------------------------------------------------|
| assumeTrue(assumption), assumeFalse() | |
| assumeTrue(assumption or BooleanSupplier, message or messageSupplier), assumeFalse() | |
| assumingThat(assumption or BooleanSupplier) | | 
| assumingThat(assumption or BooleanSupplier, executable) | | 


## Disabling Tests

- 전체 테스트 클래스나 개별 테스트 메소드들에 ``@Disalbed`` 어노테이션을 사용하면 불능상태가 된다.

``` Java
import org.junit.jupiter.api.Disabled;
import org.junit.jupiter.api.Test;

class DisabledTestsDemo {

    @Disabled("Disabled until bug #42 has been resolved")
    @Test
    void testWillBeSkipped() {
    }

    @Test
    void testWillBeExecuted() {
    }

}
```

## Conditional Test Execution

#### Operating System Conditions

- ``@EnabledOnOs`` , ``@DisaabledOnOs``

``` Java
@Test
@EnabledOnOs(MAC)
void onlyOnMacOs() {
    // ...
}

@TestOnMac
void testOnMac() {
    // ...
}

@Test
@EnabledOnOs({ LINUX, MAC })
void onLinuxOrMac() {
    // ...
}

@Test
@DisabledOnOs(WINDOWS)
void notOnWindows() {
    // ...
}

@Target(ElementType.METHOD)
@Retention(RetentionPolicy.RUNTIME)
@Test
@EnabledOnOs(MAC)
@interface TestOnMac {
}
```

#### JRE conditions

- ``@EnabledOnJre`` , ``@DisabledOnJre`` , ``@EnabledForJreRange`` , ``@DisabledForJreRange``

``` Java
@Test
@EnabledOnJre(JAVA_8)
void onlyOnJava8() {
    // ...
}

@Test
@EnabledOnJre({ JAVA_9, JAVA_10 })
void onJava9Or10() {
    // ...
}

@Test
@EnabledForJreRange(min = JAVA_9, max = JAVA_11)
void fromJava9to11() {
    // ...
}

@Test
@EnabledForJreRange(min = JAVA_9)
void fromJava9toCurrentJavaFeatureNumber() {
    // ...
}

@Test
@EnabledForJreRange(max = JAVA_11)
void fromJava8To11() {
    // ...
}

@Test
@DisabledOnJre(JAVA_9)
void notOnJava9() {
    // ...
}

@Test
@DisabledForJreRange(min = JAVA_9, max = JAVA_11)
void notFromJava9to11() {
    // ...
}

@Test
@DisabledForJreRange(min = JAVA_9)
void notFromJava9toCurrentJavaFeatureNumber() {
    // ...
}

@Test
@DisabledForJreRange(max = JAVA_11)
void notFromJava8to11() {
    // ...
}
```

#### System Property Conditions

- ``@EnabledIfSystemProperty`` ,  ``@DisabledIfSystemProperty``

``` Java
@Test
@EnabledIfSystemProperty(named = "os.arch", matches = ".*64.*")
void onlyOn64BitArchitectures() {
    // ...
}

@Test
@DisabledIfSystemProperty(named = "ci-server", matches = "true")
void notOnCiServer() {
    // ...
}
```

#### Environment Varialbe Conditions

- ``@EnabledIfEnvironmentVariable`` ,  ``@DisabledIfEnvironmentVariable``

``` Java
@Test
@EnabledIfEnvironmentVariable(named = "ENV", matches = "staging-server")
void onlyOnStagingServer() {
    // ...
}

@Test
@DisabledIfEnvironmentVariable(named = "ENV", matches = ".*development.*")
void notOnDeveloperWorkstation() {
    // ...
}
```

## Tagging and Filtering

- 테스트 클래스들과 메소드들은 ``@Tag`` 어노테이션들로 태그될 수 있다.
- 태그들은 필터에 사용될 수 있다.



## Test Exectuion Order
- 실행 순서를 제어하기 위해 클래스나 인터페이스에 ``@TestMethodOrder`` 를 사용하거나 원하는 **MethodOrderer** Implementation 을 명시할 수 있다.
- 내장된 MethodOrderer 를 사용하거나 자신만의 MethodOrderer 를 구현할 수 있다.

> Built-in MethodOrderer

- DisplayName
- MethodName
- OrderAnnotation : ``@Order``
- Random
- Alphaaaanumeric

``` Java
import org.junit.jupiter.api.MethodOrderer.OrderAnnotation;
import org.junit.jupiter.api.Order;
import org.junit.jupiter.api.Test;
import org.junit.jupiter.api.TestMethodOrder;

@TestMethodOrder(OrderAnnotation.class)
class OrderedTestsDemo {

    @Test
    @Order(1)
    void nullValues() {
        // perform assertions against null values
    }

    @Test
    @Order(2)
    void emptyValues() {
        // perform assertions against empty values
    }

    @Test
    @Order(3)
    void validValues() {
        // perform assertions against valid values
    }

}
```

## Test Instance Lifecycle

!!! info 

    - ``@BeforeAll`` , ``@Afterall`` 을 사용한 테스트 메소드들은 반드시 **static** 이어야 한다.
    - ``@BeforeEach``, ``@AfterEach`` 에 static 이면 안된다.

## Nested Tests

## Dependecncy Injection for Constructors and Methods

## Test Interfaces and Default Methods

## Repeated Tests

- 원하는 만큼 메소드를 반복 실행하려면 ``@RepeatedTest`` 와 횟수를 기술한다.

``` Java
import static org.junit.jupiter.api.Assertions.assertEquals;

import java.util.logging.Logger;

import org.junit.jupiter.api.BeforeEach;
import org.junit.jupiter.api.DisplayName;
import org.junit.jupiter.api.RepeatedTest;
import org.junit.jupiter.api.RepetitionInfo;
import org.junit.jupiter.api.TestInfo;

class RepeatedTestsDemo {

    private Logger logger = // ...

    @BeforeEach
    void beforeEach(TestInfo testInfo, RepetitionInfo repetitionInfo) {
        int currentRepetition = repetitionInfo.getCurrentRepetition();
        int totalRepetitions = repetitionInfo.getTotalRepetitions();
        String methodName = testInfo.getTestMethod().get().getName();
        logger.info(String.format("About to execute repetition %d of %d for %s", //
            currentRepetition, totalRepetitions, methodName));
    }

    @RepeatedTest(10)
    void repeatedTest() {
        // ...
    }

    @RepeatedTest(5)
    void repeatedTestWithRepetitionInfo(RepetitionInfo repetitionInfo) {
        assertEquals(5, repetitionInfo.getTotalRepetitions());
    }

    @RepeatedTest(value = 1, name = "{displayName} {currentRepetition}/{totalRepetitions}")
    @DisplayName("Repeat!")
    void customDisplayName(TestInfo testInfo) {
        assertEquals("Repeat! 1/1", testInfo.getDisplayName());
    }

    @RepeatedTest(value = 1, name = RepeatedTest.LONG_DISPLAY_NAME)
    @DisplayName("Details...")
    void customDisplayNameWithLongPattern(TestInfo testInfo) {
        assertEquals("Details... :: repetition 1 of 1", testInfo.getDisplayName());
    }

    @RepeatedTest(value = 5, name = "Wiederholung {currentRepetition} von {totalRepetitions}")
    void repeatedTestInGerman() {
        // ...
    }

}
```

## Parameterized Tests

- 다른 인자 값들을 사용해서 테스트를 여러번 수행하기 위해 사용한다.
-  ``@ParameterizedTest`` 어노테이션을 사용.

``` Java
@ParameterizedTest
@ValueSource(strings = { "racecar", "radar", "able was I ere I saw elba" })
void palindromes(String candidate) {
    assertTrue(StringUtils.isPalindrome(candidate));
}
```

- ==junit-jupiter-params== artifact 에 대한 의존성(dependency)을 추가해야 한다.

#### Consuming Arguments
    - Zero or more indexed arguments must be declared first.
    - Zero or more aggregators must be declared next.
    - Zero or more arguments supplied by a ParameterResolver must be declared last.

### Source of Arguments
-  ``@ValueSource`` 

    - short, byte, int, long, flaot, double, char, boolean, java.lang.String, lava.lang.Class

``` Java
@ParameterizedTest
@ValueSource(ints = { 1, 2, 3 })
void testWithValueSource(int argument) {
    assertTrue(argument > 0 && argument < 4);
}
```
> NULL and Empty Sources

- ``@NullSource``
- ``@EmptySource``
- ``@NullAndEmptySource``

``` Java
@ParameterizedTest
@NullSource
@EmptySource
@ValueSource(strings = { " ", "   ", "\t", "\n" })
void nullEmptyAndBlankStrings(String text) {
    assertTrue(text == null || text.trim().isEmpty());
}
```

> @EnumSource


> @MethodSource

> @CsvSource

> @CsvFileSource

> @ArgumentSource


#### Argument Covnersion

- Implicit Conversion


- Explicit Conversion


#### Argument Aggregation

## Dynamic Tests

## Timeouts

- ``@Timeout`` 어노테이션은 test, tst factory, test template, 또는 lifecycle 메소드들이 주어진 시간을 초과하면 fail이 되게 한다.

``` java
class TimeoutDemo {

    @BeforeEach
    @Timeout(5)
    void setUp() {
        // fails if execution time exceeds 5 seconds
    }

    @Test
    @Timeout(value = 100, unit = TimeUnit.MILLISECONDS)
    void failsIfExecutionTimeExceeds100Milliseconds() {
        // fails if execution time exceeds 100 milliseconds
    }

}
```

- 전역(global)으로 timeout 을 설정하려면 다음 설정 파라미터들을 사용한다.

    junit.jupiter.execution.timeout.default
    Default timeout for all testable and lifecycle methods

    junit.jupiter.execution.timeout.testable.method.default
    Default timeout for all testable methods

    junit.jupiter.execution.timeout.test.method.default
    Default timeout for @Test methods

    junit.jupiter.execution.timeout.testtemplate.method.default
    Default timeout for @TestTemplate methods

    junit.jupiter.execution.timeout.testfactory.method.default
    Default timeout for @TestFactory methods

    junit.jupiter.execution.timeout.lifecycle.method.default
    Default timeout for all lifecycle methods

    junit.jupiter.execution.timeout.beforeall.method.default
    Default timeout for @BeforeAll methods

    junit.jupiter.execution.timeout.beforeeach.method.default
    Default timeout for @BeforeEach methods

    junit.jupiter.execution.timeout.aftereach.method.default
    Default timeout for @AfterEach methods

    junit.jupiter.execution.timeout.afterall.method.default
    Default timeout for @AfterAll methods

## Parallel Execution

- 기본적으로, Junit Jupiter 테스트들은 하나의 스레드에 의해 순차적으로 실행된다.
- 병렬 실행으로 위해서 **junit.jupiter.execution.parallel.enabled** 파라미터를 **True** 로 설정한다.



## Built-in Extensions


> 참고
[^1]:
    JUnit5 User Guide : https://junit.org/junit5/docs/current/user-guide/

