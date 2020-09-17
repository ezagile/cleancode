# JUnit 기본

!!! info
    - Java 의 **Annotation** 에 관해서는 간략한 [소개글](../effjava/annotation.md) 을 참고하시오.

## 어노테이션들 (Annotations)


| Annotions                   |    설명                                                  |
|:----------------------------|:---------------------------------------------------------|
| __@Test__                   | 테스트 메소드임을 알린다.                                  |
| __@ParameterizedTest__                   |                                   |
| __@RepeatedTest__                   |                                   |
| __@TestFactory__                   |                                   |
| __@TestTemplate__                   |                                   |
| __@TestMethodOrder__                   |                                   |
| __@TestInstance__                   |                                   |
| __@DisplayName__                   |                                   |
| __@DisplayNameGeneration__                   |                                   |
| __@BeforeEach__                   |                                   |
| __@AfterEach__                   |                                   |
| __@BeforeAll__                   |                                   |
| __@AfterAll__                   |                                   |
| __@Nested__                   |                                   |
| __@Tag__                   |                                   |
| __@Disabled__                   |                                   |
| __@Timeout__                   |                                   |
| __@ExtendWith__                   |                                   |
| __@RegisterExtension__                   |                                   |
| __@TempDir__                   |                                   |



### 클래스와 메소드 테스트

- **Test Class** : 최상위 수준의 클래스, ``static`` 멤버 클래스, 또는 하나의 테스트 메소드를 포함하는 ``@Nested`` 클래스
    - 테스트 클래스는 반드시 하나의 생성자를 가져야 하고, ``abstract`` 클래스는 사용할 수 없다.

- **Test Method** : ``@Test``, ``@RepeatedTest``, ``@ParameterizedTest``, ``@TestFactory``, ``@TestTemplate`` 과 같은 어노테이션이 붙은 메소드들

- **Lifecycle Method** : ``@BeforAll``, ``@BeforeEach``, ``@AfterAll``, ``@AfterEach`` 의 어노테이션이 붙은 메소드


!!! info
    - Test Class 와 Test Method 는 반드시 **public** 일 필요는 없지만 **private** 이면 안된다.
    - Test method 와 lifecycle method 는 ``abstract`` 일 수 없고, 반환값을 가질 수 없다.

=== "java"
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
=== "C++"
    ```cpp
    
    ```
=== "python"
    ```python
    
    ```

## 테스트 실행



## 확장 모델

## 고급 토픽들

## Appendix


> 참고
[^1]:
    JUnit5 User Guide : https://junit.org/junit5/docs/current/user-guide/

