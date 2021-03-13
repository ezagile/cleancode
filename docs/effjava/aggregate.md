# Aggregate Operation

!!! info

    Lambda Expressoin 과 Method Reference 에 대한 이해가 필요하다.

    참고>
    [Java Tutorial - Aggregate Operations](https://docs.oracle.com/javase/tutorial/collections/streams/index.html)



``` java
public class Person {

    public enum Sex {
        MALE, FEMALE
    }

    String name;
    LocalDate birthday;
    Sex gender;
    String emailAddress;
    
    public int getAge() { /* ... */ }
    public String getName() { /* ... */ }
}
```

``` java
// roster => Person 객체들을 가지는 Collection 
for (Person p : roster) {
    System.out.println(p.getName());
}

roster.stream()
      .forEach(e -> System.out.println(e.getName());
```

### Pipelines 과 Streams

``` java
roster
    .stream()
    .filter(e -> e.getGender() == Person.Sex.MALE)
    .forEach(e -> System.out.println(e.getName()));

// ====================================================

for (Person p : roster) {
    if (p.getGender() == Person.Sex.MALE) {
        System.out.println(p.getName());
    }
}
```

파이프 라인은 다음 구성요소들을 포함한다.

- A source :
- Zero or more *intermediate operatoin* :
- A *terminal operation* : 



### Aggregate Operations 과 Iterators 의 차이점

Aggregate operation 은,

- 내부 iteration 을 사용한다.
- Stream 의 요소들을 처리한다.
- 파라미터들을 통해 동작(behavior)을 지원한다.

### Reduction

``` java
Integer totalAge = roster
                .stream()
                .mapToInt(Person::getAge)
                .sum();

// =============================

Integer totalAgeReduce = roster
                .stream()
                .map(Person::getAge)
                .reduce(0, (a, b) -> a + b);
```
> reduce()
The reduce operation in this example takes two arguments:

identity: The identity element is both the initial value of the reduction and the default result if there are no elements in the stream. In this example, the identity element is 0; this is the initial value of the sum of ages and the default value if no members exist in the collection roster.

accumulator: The accumulator function takes two parameters: a partial result of the reduction (in this example, the sum of all processed integers so far) and the next element of the stream (in this example, an integer). It returns a new partial result. In this example, the accumulator function is a lambda expression that adds two Integer values and returns an Integer value:

(a, b) -> a + b
The reduce operation always returns a new value. 

> collect()

Unlike the reduce method, which always creates a new value when it processes an element, the collect method modifies, or mutates, an existing value.


### Parallelism

#### Executing Streams in Parallel

#### Concurrent Reduction

#### Ordering

