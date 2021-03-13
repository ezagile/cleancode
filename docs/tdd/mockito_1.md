# Mockito

- **Expect-Run-Verify**
- BDD( **Arrange-Action-Assert** ) --> **record-replay-verify**
- **Mockito** 는 *Gerard Meszaros* 가 **Test Spy** 라 부르는 것을 구현한 것이다.
- Mockito is about focusing on interactions between objects, which is the most essential part of Object Oriented Programming (or messaging).

**Mockito** 라이브러리에 정의된 다음 어노테이션들에 대해서 살펴보자.

- __@Mock__ 
- __@Spy__
- __@Captor__
- __@InjectMock__

--------------

Mockito annotation 들을 사용하기 위해서는, **JUnit** 의 어노테션과 *MockitoJUnitRunner* 클래스로 다음과 같이 할 수 있다.

``` java
@RunWith(MockitoJUnitRunner.class)
public class MockitoAnnotationTest {
    ...
}
```

다른 방법은 *MockitoAnnotations.initMocks()* 메소드를 호출해서 가능하다.

``` java
@Before
public void init() {
    MockitoAnnotations.initMocks(this);
}
```

마지막으로 *MockitoJUnit.rule()* 을 사용하는 것이다.

``` java
public class MockitoInitWithMockitoJUnitRuleUnitTest {

    @Rule
    public MockitoRule initRule = MockitoJUnit.rule();

    ...
}
```


### __@Mock__

가장 널리 사용되는 어노테이션중의 하나가 __@Mock__ 이다. __@Mock__ 은 **Mockito.mock** 을 호출하지 않고 mock instance를 생성한다.

다음은 __@Mock__ 어노테이션을 사용하지 않는 예제이다.

``` java
@Test
public void whenNotUseMockAnnotation_thenCorrect() {
    List mockList = Mockito.mock(ArrayList.class);
    
    mockList.add("one");
    Mockito.verify(mockList).add("one");
    assertEquals(0, mockList.size());

    Mockito.when(mockList.size()).thenReturn(100);
    assertEquals(100, mockList.size());
}
```

__@Mock__ 을 사용하면 다음과 같이 작성한다.

``` java
@Mock
List<String> mockedList;

@Test
public void whenUseMockAnnotation_thenMockIsInjected() {
    mockedList.add("one");
    Mockito.verify(mockedList).add("one");
    assertEquals(0, mockedList.size());

    Mockito.when(mockedList.size()).thenReturn(100);
    assertEquals(100, mockedList.size());
}
```

위 2개의 예제 코드는 동일하게 **Mock** 과 서로 상호동작과 검증을 수행한다.

### __@Spy__

기존의 인스턴스에 __@Spy__ 어노테이션을 사용하는 방법이다.


``` java
@Test
public void whenNotUseSpyAnnotation_thenCorrect() {
    List<String> spyList = Mockito.spy(new ArrayList<String>());
    
    spyList.add("one");
    spyList.add("two");

    Mockito.verify(spyList).add("one");
    Mockito.verify(spyList).add("two");

    assertEquals(2, spyList.size());

    Mockito.doReturn(100).when(spyList).size();
    assertEquals(100, spyList.size());
}
```
``` java
@Spy
List<String> spiedList = new ArrayList<String>();

@Test
public void whenUseSpyAnnotation_thenSpyIsInjectedCorrectly() {
    spiedList.add("one");
    spiedList.add("two");

    Mockito.verify(spiedList).add("one");
    Mockito.verify(spiedList).add("two");

    assertEquals(2, spiedList.size());

    Mockito.doReturn(100).when(spiedList).size();
    assertEquals(100, spiedList.size());
}
```


!!! info

    - Mockito 어노테이션들을 사용하면 중복된 mock 객체 생성을 최소화한다.
    - 코드의 가독성이 좋아진다.
    - __@InjectMocks__ 은 __@MOCK__ 과 __@Spy__ 에 주입(inject)하기 위해 필요하다.
