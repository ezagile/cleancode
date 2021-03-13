## Stream

**Stream** 은 Java 8 에 새롭게 추가된 특징들 중의 하나이다.

제공되는 기능들을 살펴보고, 예제를 통해 좀더 깊이있는 이해를 해보자.

!!! info

    Stream 에 대한 이해를 위해서는 Lambda, Optional, Method reference 에 대한 사전 지식이 필요하다.
    
    참고>

    [Java 8 Tutorial](https://docs.oracle.com/javase/tutorial/collections/streams/reduction.html)

    [A Guide to Java Streams in Java 8: In-Depth Tutorial With Examples](https://stackify.com/streams-guide-java-8/)


먼저, Stream 을 Java I/O 의 Stream(예> *FileInputStream* )과 혼동해서는 안된다.
Stream 은 다수의 자료들을 편하고 빠른게 처리하는 방법을 제공하기 위해 자료 소스(data source)를 둘러싸는 *Wrapper* 이다.

Stream 은 자료를 저장하지 않으며, 자료 구조도 아니다. 또한, 자료 소스를 수정하지도 않는다.
Stream 의 요소(element)들에 대한 함수형 동작을 지원한다. (예> 컬렉션들에 대한 *map-reduce transformation*)


### Stream 생성

1 *Stream* 의 *of()* 메소드의 *Collection* 객체를 인자로 넘겨주면, Stream 을 얻을 수 있다.

    - 다음은 *Array* 로 부터 *Stream* 을 생성하는 예제다.

``` java
Person[] arrayOfPersons = {
            new Person("홍길동", 20),
            new Person("김선생", 40),
            new Person("이용중", 36)
};

Stream prsonStream = Stream.of(arrayOfPersons);

// 개별 객체들을 인자로 넘겨서 생성할 수 있다.
prsonStream = Stream.of(arrayOfPersons[0], arrayOfPersons[1], arrayOfPersons[2]);
```

2 *List* 에서 *stream()* 메소드로 Stream을 얻어오는 예제이다.

    - Java 8 은 *Collection Interface* 에 *stream()* 이라는 새로은 메소드를 추가했다.


``` java
List<Person> listOfPersons = Arrays.asList(arrayOfPersons);
prsonStream = listOfPersons.stream();
```

3 *Stream.builder()* 통해서 얻을 수 있다.

- 원하는 타입의 builder 를 *Stream.builder()* 메소드로 생성한다.
- builder 가 자료들의 객체들을 받아들인다.
- *build()* 메소드로 Stream 을 얻어온다.

``` java
Stream.Builder<Person> personStreamBuilder = Stream.builder();
personStreamBuilder.accept(arrayOfPersons[0]);
personStreamBuilder.accept(arrayOfPersons[1]);
personStreamBuilder.accept(arrayOfPersons[2]);

prsonStream = personStreamBuilder.build();
```

앞서 언급한 방법 외에 Stream 을 얻어오는 방법들이 있다. 

### Stream Operation

다음은 Stream 을 사용하는 기본적인 방법들이다.

#### *forEach*

*forEach()* 는 Stream 의 각 요소들에 대해 반복적인 supplier function 호출을 하는 가장 간단한 방법이다.

``` java
listOfPersons.stream().forEach(person -> System.out.println(person.name));
```
*forEach()* 는 터미널 오퍼레션이다. 이는, 메소드가 실행되면, 스트림의 파이프라인이 소비되었고, 더 이상 사용될 수 없음을 의미한다.


#### *map*

*map()* 은 최초 stream의 각 요소에 함수가 적용된 이후에 새로운 stream 을 생성한다.

``` java
String[] names = {"홍길동", "이용중"};
List<Person> newList = Stream.of(names) // PersonList 클래스에 Person 객체들의 리스트 가지고 있음.
                             .map(PersonsList::findByName)      
                             .collect(Collectors.toList());

System.out.println(newList.size());
newList.stream().forEach(person -> System.out.println(person.name));
```

#### *collect*

*collect()* 의 일반적인 사용법은 모든 처리가 끝난 이후의 스트림의 결과들을 수집하는 것이다.

``` java

```
스트림의 요소들에 대해 어떤 자료구조로 재구성하거나, 추가적인 로직을 수행하는 **mutable fold operation* 을 수행한다.

#### *filter*

*filter* 는 기존 스트림의 요소들에서 주어진 테스트(predicate)를 통과하는 요소들의 새로운 스트림을 생성한다.

``` java
List<Person> middleAge = Stream.of(names)
                               .map(Persons::findByName)
                               .filter(e -> e.age > 30)
                               .collect(Collectors.toList());;
```

### Method Type and Pipeline

- *Stream* 의 동작은 *intermediate* 와 *termina* 의 두 가지 유형으로 볼 수 있다.

    - *intermediate* : filter()와 같이 기존의 스트림에서 새로운 스트림을 생성하는 동작
    - *terminal* : forEach() 와 같이 스트림이 소비되는 동작

- *Stream pipeline* : *Stream Source* -> *intermediate operation* -> *terminal operation*

- *Short-circuiting operation* 은 **무한** Stream에 **유한한** 횟수내에 작업을 허용한다.

``` java
 Stream<Integer> infiniteStream = Stream.iterate(2, i -> i * 2);

        List<Integer> collect = infiniteStream
                                .skip(3)
                                .limit(5)
                                .collect(Collectors.toList());

        for(int val: collect)
            System.out.println(val);
```

--------

Java Stream 의 중요한 특징 중의 하나가 **Lazy Evaluation** 을 통한 최적화이다.

- Source Data 에 대한 작업은 *terminal operation* 에 의해 수행되고, 각 요소들은 필요할 경우에 소비되어 진다.

- 모든 *intermediate operation* 들은 실제 결과가 필요할 때 까지 실행되지 않는다.


### Comparison-based Stream Operation

- *Sorted()*
- *min()* , *max()*
- *distinct()*
- *allMatch()*, *anyMatch()*, *noneMatch()*


### Stream Serialization


### Advanced *collect*



