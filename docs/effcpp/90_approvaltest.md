# Approval Test

- *Approval Test* 는 *assertions* 의 다른 한가지 방법이다.
- *Approval Test* 는 복잡한 객체들을 테스트하는 보다 단순하고 우아한 방법이다.

> 전통적인 Assertions 

- 전통적인 *Assertion* 은 테스트에 필요한 입력 생성과 결과 검증에 동일한(?) 시간이 소요된다.
- 많은 멤버를 가지는 객체들에 대해서 테스트 코드가 복잡해진다.


```cpp
// Arrange, Act
Sandwich s = createSandwichForTest();
// Assert
REQUIRE("Sourdough" == s.getBread());
REQUIRE(s.getCondiments().contains("Mayo"));
REQUIRE(s.getCondiments().contains("Pepper"));
REQUIRE(s.getCondiments().contains("Olive Oil"));
REQUIRE(s.getFillings().contains("Tomato"));
REQUIRE(s.getFillings().contains("Lettuce"));
REQUIRE(s.getFillings().contains("Cheddar"));
```

> Approval Tests

- *Approval Test* 는 결과 검증을 단순화한다.
- 이는 결과를 파일에 일단 저장하는 방법을 사용한다.


```cpp
// Arrange, Act
Sandwich s = createSandwichForTest();
// Assert
Approvals::verify(s)
```

- 위 작업으로 *approval file* 이 생성된다. 

```json
andwich {
    bread: "Sourdough",
    condiments: ["Mayo", "Pepper", "Olive Oil"],
    fillings: ["Tomato", "Lettuce", "Cheddar"]
}
```

- 이 파일은 당신을 위해 생성되었으나, 당신에 의해 *approve* 되야 한다.

## ApprovalTesting

- ==snapshot testing==, ==golden master testing==

- 대부분 테스트는 코드를 작성하기 전에 기대(예상)하는 것을 명시적으로 기술한다.
- 반면, *ApprovalTesting* 은 체크하고자 하는 데이타를 기술하고, 만족할 때까지 수동으로 체크하도록 한다. 차후, 일관성(consisitency)이 유지되도록 보장해준다.

- 결과의 차이를 더 쉽게 이해하고 처리하도록 돕는다.


> *Approval Testing* 의 기본적인 예로, `REQUIRE(42 == 6 * 9)` 가 실행되면 `42 != 54` 라는 결과를 얻게되고, `REQUIRE(54 == 6 * 9)` 로 변경한다.

> 이것은 일반적으로 보는 과정이다.

- Verfiy numbers : *array* 같은 많은 숫자들
- Strings : 숫자들을 문자열로 변환
- Files - Golden Master : Managing them - coming up with namaes
- Some sort of naming convention
- Start using diff tools : 파일의 내용을 이해하기 어렵다.
- verifyThing : Start creating custom methods



## Tutorial

- `tutorial.cpp` 테스트 파일을 작성한다. 

```cpp
TEST_CASE("HelloApprovals")
{
    ApprovalTests::Approvals::verify("Hello Approvals");
}
```

- *ApprovingTest* 를 실행한다.
- ==xxxxx.received.txt== 의 내용을 ==xxxxxx.approved.txt== 에 복사
- 이때, 테스트는 반드시 통과해야하고, **diff** 도구는 


> 무슨 일이 발생하나?

- *Approval Tests* 는 기대되는 결과를 외부 파일에 저장한다.
- 테스트 실행시, 파일을 읽어서 검증을 수행한다.
- 만약, 일치하지 않으면 ==Reporter== (Diff 도구) 가 실행된다.
- *Approval Test* 의 첫 번째 실행은 반드시 **fail** 된다.


### Approval Files


- *Approvals* 은 다수의 ==.received.txt==, ==.approved.txt== 파일들을 생성
- ==.received.txt== 파일들은 테스트가 통과하면 삭제된다.
- ==*.receved.*== 를 `.gitignore` 파일에 추가하는 것을 제안한다.

> Convention --> `FileName.TestName.approved.txt`

- 파일들은 테스트와 동일한 디렉토리에 위치한다.


### ApprovalTests namespace

- 다음 코드를 포함한다.

```cpp
using namespace ApprovalTests;
```

### ApprovalTests objects

```cpp
class LibraryBook
{
public:
    LibraryBook(std::string title_,
                std::string author_,
                int available_copies_,
                std::string language_,
                int pages_,
                std::string isbn_)
        : title(title_)
        , author(author_)
        , available_copies(available_copies_)
        , language(language_)
        , pages(pages_)
        , isbn(isbn_)
    {
    }
    // Data public for simplicity of test demo case.
    // In production code, we would have accessors instead.
    std::string title;
    std::string author;
    int available_copies;
    std::string language;
    int pages;
    std::string isbn;
};
```

```cpp
libraryBook harry_potter(
    "Harry Potter and the Goblet of Fire", "J.K. Rowling",
    30, "English", 752, "978-0439139595");

Approvals::verify(harry_potter); // 이 것은 컴파일 되지 않는다.
```

- 문제는 `LibraryBook` 을 문자열 표현으로 변경할 방법이 없다는 것이다.
- 람다 함수를 추가한다.

```cpp
Approvals::verify(harry_potter, [](const LibraryBook& b, std::ostream& os) {
        os << "title: " << b.title << "\n"
           << "author: " << b.author << "\n"
           << "available_copies: " << b.available_copies << "\n"
           << "language: " << b.language << "\n"
           << "pages: " << b.pages << "\n"
           << "isbn: " << b.isbn << "\n";
    });
```

- Lambda: [](const LibraryBook& b, std::ostream& os){} 
    - call-back function to convert your object to a string
    - can also write this as [](auto b, auto& os){}

- toString: os << "title: " << b.title; 
    - this is the bit of code that actually turns our object in to a string.

### Dealing with test failures

- ???

## Set up


1. 파일을 다운로드 한다. https://github.com/approvals/ApprovalTests.cpp/releases
2. *wrapper header* 를 추가하거나 파일의 이름을 `ApprovedTests.hpp` 로 변경한다. 
3. `main()` 함수를 작성한다. 이는 테스트 프레임워크에 따라 다르다.
4. 지원하는 **Diff** 도구를 다운로드 한다.





