## Java Optional


Java 8 에서 새로 추가된 *Optional* 클래스에 대해 소개한다.

Optional 클래스의 목적은 *null* 참조 대신에 선택적인 값들을 표현하기 위한 **type-level** 솔루션을 제공한다.

Optional는 존재할 수도 있지만 안 할 수도 있는 객체로 볼 수 있으며, *null* 일 수 있는 객체를 감싸는 래퍼 클래스입니다. 원소가 없거나 최대 하나 밖에 없는 *Collection* 이나 *Stream* 으로 볼 수 있다.


#### Optional 객체 생성하기

**Optional** 클래스의 정적 메소드를 통해 객체를 생성한다.

1. *empty()* 정적 메소드를사용할 수 있다.

``` Java
@Test
public void CreatesEmptyObject() {
    Optional<String> empty = Optional.empty();
    assertFalse(empty.isPresent());
}
```

2. *of()* 정적 메소드
3. *ofNullable()* 정적 메소드

#### 값이 있는지 체크하기

1. *isPresent()*
2. *isEmpty()*


#### 조건부 액션

- *ifPresent()*
- 전형적인 함수형 프로그래밍 스타일

``` java
@Test
public void givenOptional_whenIfPresentWorks_thenCorrect() {
    Optional<String> opt = Optional.of("baeldung");
    opt.ifPresent(name -> System.out.println(name.length()));
}
```
#### Optional 의 객체 접근하기


1. *get()* : null일때 예외 발생. isPresent() 와 같이 사용
2. *orElse(T other)* : 하나의 매개변수를 가지고, 디폴트 값으로 사용된다.
3. *orElseGet(Supplier<? extends T> other)* : 함수형 인터페이스를 파라미터로 가지고, 호출 값을 반환한다.
4. *orElseThrow(Supplier<? extends X> exceptionSupplier)* : 값이 없다면 디폴트 값을 반환하는 대시 예외를 던진다.


#### *orElse()* 와 *orElseGet()* 의 차이


