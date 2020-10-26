# gooletest + approval


- `main.cpp` 파일을 생성하고 다음 두 줄을 추가한다..

```cpp
#define APPROVALS_GOOGLETEST // This tells Approval Tests to provide a main() - only do this in one cpp file
#include "ApprovalTests.hpp"
```

- `main()` 함수를 제공하는 `gtest_main` 라이브러를 사용하는 경우, *output* 파일의 이름을 얻을 수 없다.

```cpp
// main.cpp:

// 1. Add these two lines to your main:
#define APPROVALS_GOOGLETEST_EXISTING_MAIN
#include "ApprovalTests.hpp"

int main(int argc, char** argv)
{
    ::testing::InitGoogleTest(&argc, argv);

    // 2. Add this line to your main:
    ApprovalTests::initializeApprovalTestsForGoogleTests();

    return RUN_ALL_TESTS();
}
```

- 대부분 테스트 프레임워크는 `SourceFileName` 과 `TestName` 정보가 있다.
- 구글 테스트는 추가로 `TestCaseName`을 가진다.
```
TEST(TestCaseName, TestName)
```
- 구글 테스트에서는 다음으로 시작하는 파일을 생성한다.

```
SourceFileName.TestCaseName.TestName
```

-  기본적으로, *Approval Tests* 는 `SourceFileName` 내부에 `TestCaseName` 이 완전히 포함되어 있을 경우 
- `SourceFileName` 과 `TestCaseName` 은 중복되는 경우가 빈번하다.

- *test fixture* 에서 `TestClassNmaeFixture` 포맷의 클래스 이름들을 가질수 있다.
```cpp
// main.cpp
auto customization = GoogleConfiguration::addIgnorableTestCaseNameSuffix("Fixture");
```


## Testing Single Objects

## Testing Conainders

## Testing Combinations

## Testing Exceptions

## String conversions

## Tips for Designing Strings

