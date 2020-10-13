## GTest basic

!!!note "What & How Test?"

    - **independent and repeatable** : 각 테스트들은 다른 객체들에서 동작하고 서로 독립적이고, 실패 시 빠른 디버깅
    - **well organized and reflect the structure of the tested code**
    - **portable and reusable**
    - **provide much imormation about the problem as possible** 
    - **should be fast**
    - **test writers focus on test content**

> GTest 는 연관되는 테스트들의 그룹에 대해 **Test Case** 라는 용어를 사용하지만, `ISTQB`:(https://www.istqb.org/) 를 포함해서 소프트웨어 품질과 관련한 도서들에서는 **Test Suite** 를 사용한다.
> GTest 의 **Test** 는 ISTQB 의 **Test Case** 에 해당한다.


### Basic Concepts

- Google test를 시작할때, *assertion* 을 사용하면서 시작한다.
- *assertion* 은 조건이 참인지 검사하는 명령문이다.
- *assertion* 의 결과는 *success*, *nonfatal failure*, 또는 *fatal failure* 이다.
- *fatal failure* 가 발생하면, 현재 함수가 중단된다. 
- *Tests* 는 코드의 행위(behavior) 를 검증하기 *assertion* 을 사용한다.
- *Test suite* 는 하나 이상의 *Test* 를 포함한다. *Test suite* 의 다수의 *test* 들이 객체들과 서브루틴들을 공유하기 위해서는 **test fixture class** 를 생성한다.
- *Test probram* 은 다수의 *test suite* 를 포함할 수 있다.


### Assertions

- 구글 테스트의 **assertion** 들은 함수 호출과 유사한 **macro** 들이다. 
- 클래스나 함수를 테스하기 위해, 클래스나 함수의 행위에 대해 **assertion** 을 사용한다.
- **assertion** 실패하면, 구글 테스트는 소스의 줄넘버와 메시지를 출력한다.
- ``ASSERT_*`` 은 실패시 *fatal failure* 를 발생시키고 함수를 중단한다.
- ``EXPECT_*`` 은 실패시 *nonfatal failure* 를 발생시킨다.

```C++
ASSERT_EQ(x.size(), y.size()) << "Vectors x and y are of unequal length";

for (int i = 0; i < x.size(); ++i) {
  EXPECT_EQ(x[i], y[i]) << "Vectors x and y differ at index " << i;
}
```

#### Basic Assertions

- 기본적인 *true/false* 조건을 테스트한다.


Fatal assertion            | Nonfatal assertion         | Verifies
-------------------------- | -------------------------- | --------------------
`ASSERT_TRUE(condition);`  | `EXPECT_TRUE(condition);`  | `condition` is true
`ASSERT_FALSE(condition);` | `EXPECT_FALSE(condition);` | `condition` is false


#### Binary Comparision

- ASSERT_EQ( **actual**, **exptected** )

Fatal assertion          | Nonfatal assertion       | Verifies
------------------------ | ------------------------ | --------------
`ASSERT_EQ(val1, val2);` | `EXPECT_EQ(val1, val2);` | `val1 == val2`
`ASSERT_NE(val1, val2);` | `EXPECT_NE(val1, val2);` | `val1 != val2`
`ASSERT_LT(val1, val2);` | `EXPECT_LT(val1, val2);` | `val1 < val2`
`ASSERT_LE(val1, val2);` | `EXPECT_LE(val1, val2);` | `val1 <= val2`
`ASSERT_GT(val1, val2);` | `EXPECT_GT(val1, val2);` | `val1 > val2`
`ASSERT_GE(val1, val2);` | `EXPECT_GE(val1, val2);` | `val1 >= val2`

- C 문자열(예> ``const chahr*`` )들의 값을 비교한다면, **ASSERT_STREQ()**를 사용하라.
- C 문자열이 **NULL** 인지 테스트할 때 **ASSERT_STREQ(c_string, NULL)** 을 사용하라. 
- C++ 11 라면, **ASSERT_EQ(c_string, nullptr)** 을 고려해보라.

#### String Copmparision

| Fatal assertion                | Nonfatal assertion             | Verifies                       |
| --------------------------     | ------------------------------ | ------------------------------ |
| `ASSERT_STREQ(str1,str2);`     | `EXPECT_STREQ(str1,str2);`     | 두 문자열이 동일한가             |
| `ASSERT_STRNE(str1,str2);`     | `EXPECT_STRNE(str1,str2);`     | 두 문자열이 다른가?	             |
| `ASSERT_STRCASEEQ(str1,str2);` | `EXPECT_STRCASEEQ(str1,str2);` | 대소문자 관계없이 동일한가?      |
| `ASSERT_STRCASENE(str1,str2);` | `EXPECT_STRCASENE(str1,str2);` | 대소문자 관계없이 다른가?        |

#### 간단한 테스트 작성

> Creat a test

1. 테스트 함수를 정의하기 위해 ``TEST()`` 매크로를 사용한다. 반환값이 없는 보통의 C++ 함수이다.
2. 함수 내부에 값을 체크하기 위해 다양한 구글 테스트의 *assertion* 들을 사용할 수 있다. 포함시키고 싶은 C++ 명령문도 가능한다.
3. 테스트의 결과는 *assertion* 에 의해 결정된다. 테스트에 있는 *assertion* 이 실패하면, 전체 테스트가 실패한다.


```cpp
TEST(TestSuiteName, TestName) {
    //... test body ...
}
```
- ``TEST()`` 의  첫 번째 인자는 *test suite* 의 이름이고, 두 번째는 *test* 의 이름이다. 
- 이름들은 유효한 C++ 식별자여야 하고, `_` (underscore) 는 포함할 수 없다. 


> A tested function

```cpp
int Factorial(int n);       // n! 값을 반환
```

> A test suite for the factorial function

```cpp
// 0 입력 테스트
TEST(FactorialTest, ZeroInput){
    EXPECT_EQ(Fatorial(0), 1)
}

// 양수 입력 테스트
TEST(FactorialTest, PositiveInput){
    EXPECT_EQ(Fatorial(1), 1)
    EXPECT_EQ(Fatorial(2), 2)
    EXPECT_EQ(Fatorial(3), 6)
}
```

- 논리적으로 연관된 *test* 들은 동일한 *test suite* 내에 포함되어야한다. 즉, **TEST()** 의 첫 번째 인자들은 동일해아 한다.
- 앞의 예는 2개의 **test**, *ZeroInput* 과 *PositivieInput* 가 있고, *FactorialTest* 라는 **Test suite** 에 속한다.



### Test Fixtures

- 다수의 테스트들에 대해 동일한 데이터 설정을 적용하려면 *test fixture* 를 사용한다.
- 이는 다수의 테스트들에 대해 동일한 객체들의 설정을 재사용하도록 해준다.

> Fixture 생성하기

- `::testing::Test` 에서 파생된다. `protected:` 로 시작한다. 
- 클래스 내부에서 사용하길 원하는 객체들을 선언한다.
- 필요 시, 디폴트 생성자나 `SetUp()` 함수를 작성한다. `Setup()` 이라고 작성하는 흔한 실수를 하지 않으려면, C++ 11 의 `override` 를 사용하라.
- 필요 시, 소멸자나 `TearDown()` 함수를 작성하라. 생성자나 `SetUp()` 에서 할당한 자원을 반환한다.
- 필요 하다면 테스트들이 공유하는 서브루틴을 정의하라.

**Fixture** 를 사용할 때는, `TEST_F())` 를 사용한다. 
```cpp
TEST_F(TestFixtureName, TestName) {
    // ... test body ...
}
```

- `TEST_F()` 매크로를 사용하려면, 사전에 *test fixture* 를 정의해야만 한다. 그렇지 않으면, ==virtual outside calss declaration== 컴파일 에러를 볼 것이다.
- `TEST_F()` 와 정의되는 각 테스트에 대해, 구글 테스트는 런타임에 새로운 *test fixture* 를 생성하고, `SetUp()`에 의해 초기화되고, 테스트를 실행하고, `TearDown()` 에 의해 정리되고, *test fixture* 를 삭제한다.
- 같은 *test suite* 에 속한 다른 *test* 들은 다른 *test fixture* 객체를 가진다.
- 구글 테스트는 *test fixture* 를 삭제하고 다음 *test fixture* 를 생성한다. 다수의 테스트들에 대해서 동일한 *test fixture* 를 **재사용하지 않는다**.



- Queue class 예제의 interface 
```cpp
template <typename E>   // E is the element type.
class Queue {
 public:
  Queue();
  void Enqueue(const E& element);
  E* Dequeue();         // Returns NULL if the queue is empty.
  size_t size() const;
  ...
};
```

- 먼저, *fixture class* 를 정의한다.
- 클래스 `Foo` 에 대한 테스트는 `FfooTest` 로 사용해야 한다.
```cpp
class QueueTest : public ::testing::Test {
 protected:
  void SetUp() override {
     q1_.Enqueue(1);
     q2_.Enqueue(2);
     q2_.Enqueue(3);
  }

  //void TearDown() override {}

  Queue<int> q0_;
  Queue<int> q1_;
  Queue<int> q2_;
};
```

- 작성한 *fixture* 에 대한 테스트 코드를 `TEST_F()` 를 사용해서 작성한다.

```cpp
TEST_F(QueueTest, IsEmptyInitially) {
  EXPECT_EQ(q0_.size(), 0);
}

TEST_F(QueueTest, DequeueWorks) {
  int* n = q0_.Dequeue();
  EXPECT_EQ(n, nullptr);

  n = q1_.Dequeue();
  ASSERT_NE(n, nullptr);
  EXPECT_EQ(*n, 1);
  EXPECT_EQ(q1_.size(), 0);
  delete n;

  n = q2_.Dequeue();
  ASSERT_NE(n, nullptr);
  EXPECT_EQ(*n, 2);
  EXPECT_EQ(q2_.size(), 1);
  delete n;
}
```

> 테스트 실행 시,

1. 구글 테스트는 `QueueTest` 객체(`t1`)를 생성한다.
2. `t1.SetUp()` 이 `t1` 을 초기화 한다.
3. 첫 번째 테스트 `IsEmptyInitially` 가 `t1` 에 대해 동작한다.
4. `t1.TearDown()` 정리 작업을 하고 테스트가 끝난다.
5. `t1` 이 소멸한다.
6. 위 단계들이 또 다른 `QueueTest` 객체에 대해 반복된다. 


### Invoking the Tests

> `RUN_ALL_TESTS()` 매크로  실행 시,

- 모든 구글 테스트 플래그들의 상태를 저장.
- 첫 번째 테스트에 대한 *test fixture* 객체를 생성.
- `SetUp()` 에 의한 초기화.
- *fixture object* 에서 테스트 실행.
- `TearDown()` 에 의한 정리 작업.
- *fixture* 삭제.
- 모든 구글 테스트 플래그들 복구.
- 모든 테스트가 끝날 때가지, 테스트 마다 위의 작업을 반복.

!!!importance
    - `RUN_ALL_TESTS()` 의 반환 값에 주의해야 한다. 
    - 따라서, `main()` 함수는 `RUN_ALL_TESTS()` 의 반환 값을 반환한다.
    - 반드시 `RUN_ALL_TESTS()` 는 한번만 호출한다. 여러 번 호출하면 구글 테스트의 고급 특성(e.g., thread-safe death tests)과 충돌이 발생한다. 

### main() 함수 작성
Most users should not need to write their own main function and instead link with gtest_main (as opposed to with gtest), which defines a suitable entry point. See the end of this section for details. The remainder of this section should only apply when you need to do something custom before the tests run that cannot be expressed within the framework of fixtures and test suites.

If you write your own main function, it should return the value of RUN_ALL_TESTS().

```cpp
#include "this/package/foo.h"
#include "gtest/gtest.h"

namespace my {
    namespace project {
        namespace {

            // Foo 클래스를 테스트하기 위한 FooTest Fixture
            class FooTest : public ::testing::Test {
                protected:                

                FooTest() { }

                ~FooTest() override { }

                // 생성자와 소멸자 외에 추가적으로 필요한 작업
                void SetUp() override {
                    // 생성자 이후에 바로 호출된다. 
                    // 각각의 테스트 전에 실행된다.
                }

                void TearDown() override {
                    // 각각의 테스트 이후에 호출된다.
                    // 소멸자 호출전에 실행된다.
                }

                // Test suite에 포함된 모든 test 들에 의해 사용될 수 있는 멤버들이 선언된다.
            };

            // Foo::Bar() 메소드 테스트
            TEST_F(FooTest, MethodBarDoesAbc) {
                const std::string input_filepath = "this/package/testdata/myinputfile.dat";
                const std::string output_filepath = "this/package/testdata/myoutputfile.dat";
                Foo f;
                EXPECT_EQ(f.Bar(input_filepath, output_filepath), 0);
            }

            // Foo 가 Xyz 하는 것을 테스트
            TEST_F(FooTest, DoesXyz) {
                // Exercises the Xyz feature of Foo.
            }

        }  // namespace
    }  // namespace project
}  // namespace my

int main(int argc, char **argv) {
    ::testing::InitGoogleTest(&argc, argv);
    return RUN_ALL_TESTS();
}
```

The ::testing::InitGoogleTest() function parses the command line for googletest flags, and removes all recognized flags. This allows the user to control a test program's behavior via various flags, which we'll cover in the AdvancedGuide. You must call this function before calling RUN_ALL_TESTS(), or the flags won't be properly initialized.

On Windows, InitGoogleTest() also works with wide strings, so it can be used in programs compiled in UNICODE mode as well.

But maybe you think that writing all those main functions is too much work? We agree with you completely, and that's why Google Test provides a basic implementation of main(). If it fits your needs, then just link your test with the gtest_main library and you are good to go.

NOTE: ParseGUnitFlags() is deprecated in favor of InitGoogleTest().