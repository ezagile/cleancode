# Labmda Expression

!!! info

    - Java Tutorial [Lambda Expressions](https://docs.oracle.com/javase/tutorial/java/javaOO/lambdaexpressions.html) 참고해서 작성함.

람다 수식에 대해서 살펴보자.

단 하나의 메소드만 포함하는 인터페이스와 같이 익명 클래스(anonymous class)가 매우 심플하면, 그 익명 클래스는 명확하지 않고 다루기가 까다롭다.

이런 경우, 다른 매소드의 인자로 함수(기능)를 전달하는 방법을 시도할 수 있다. 람다 수식들은 **함수 를 메소드 인자** 로, 또는 **코드를 데이타** 로 처리할 수 있게 한다.

## 이상적인 유스 케이스

Social Networking 앱을 작성한다고 하자. 

관리자가 멤버들에게 메시지를 전송하것과 같은 액션을 가능하게 하려고 한다. 이에 대한 유스케이스가 다음과 같다.

| Field                     |    Description                               |
|:---------------------------------|:--------------------------------------|
|Name                       | 선택된 멤버들에 대한 액션 수행                 |
|Primary Actor              | 관리자                                       |
|Preconditions              | 관리자가 로그인 된 상태                       |
|PostCodnitions             | 특정 기준에 부합하는 멤버들에게 액션이 실행됨 |
|Main Success Scenario      | 1. 관리자는 액션의 대상이 되는 멤버들에 대한 기준을 정한다. |
|                           | 2. 관리자는 선택된 멤버들에게 적용될 액션을 정한다.        |
|                           | 3. 관리자는 **Submit** 버튼을 선택한다.                  |
|                           | 4. 시스템은 기준에 부합되는 멤버들을 찾는다.              |
|                           | 5. 시슽메은 매칭된 멤버들에 대한 해당 액션을 수행한다.     |
|Extesnsions                | 1a. 관리자는 **Submit** 버튼을 선택하기 전에 기준에 부합되는 멤버들을 사전에 확인할 수 있다. |
|Extesnsions                | 1일에 빈번하게 수행된다.                       |

멤버들은 다음 **Person** 클래스로 표현할 수 있다.

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

    public void printPerson() { /* ... */ }
}
```

#### Approach1: 하나의 특성을 만족하는 멤버들에 대한 검색 메소드

#### Approach2: 좀더 일반화된 검색 메소드

#### Approach3: 하나의 지역 클래스 내부에 탐색 기준 코드 기술

#### Approach4: 익명 클래스 내부에 탐색 기준 코드 기술

#### Approach5: 람다 수식으로 탐색 기준 코드 기술

#### Approach6: 람다 수식을 가지는 표준 함수 인터페이스 사용

#### Approach7: 앱 전체에서 람다 수식 사용하기

#### Approach8: 보다 확장성을 가지는 제네릭 사용

#### Approach9: 파라미터로 람다 수식을 허용하는 Aggregat Operation 사용


## 람다 수식의 문법

람다 수식은 다음으로 구성된다.

- 콤마( **,** )로 구분되고, 괄호로 묶여진 **파라미터들**. 

- **->** , 화살표 토큰.

- 하나의 수식 또는 하나의 명령문 블록으로 구성된 **바디**.

``` java
// p는 Person 클래스의 인스턴스

p -> p.getGender() == Person.Sex.MALE 
    && p.getAge() >= 18
    && p.getAge() <= 25
```
수식을 기술하면 JRE 는 수식을 평가한 값을 되돌려 준다. 다음과 같은 표현을 사용할 수 있다.

``` java
p -> { 
    return p.getGender() == Person.Sex.MALE 
        && p.getAge() >= 18
        && p.getAge() <= 25
}
```
**return** 문은 수식이 아니므로 반드시 중괄호 **{}** 를 사용해야 한다.

- 람다 수식은 메소드 선언처럼 보일 수 있다. 담다 수식을 이름이 없는 **익명 메소드** 로 간주해도 된다. 
- 다음 예는 하나 이상의 파라미터를 가지는 람다 수식에 대한 예이다.

``` java
public class Calculator {
  
    interface IntegerMath {
        int operation(int a, int b);   
    }
  
    public int operateBinary(int a, int b, IntegerMath op) {
        return op.operation(a, b);
    }
 
    public static void main(String... args) {
    
        Calculator myApp = new Calculator();
        IntegerMath addition = (a, b) -> a + b;
        IntegerMath subtraction = (a, b) -> a - b;
        System.out.println("40 + 2 = " +
            myApp.operateBinary(40, 2, addition));
        System.out.println("20 - 10 = " +
            myApp.operateBinary(20, 10, subtraction));    
    }
}
```
- ``operateBinary()`` 메소드는 2개 정수 피연산자에 대한 연산을 수행한다. 실제 연산은 **IntegerMath** 의 인스턴스로 기술된다.
- **addition** 과 **substraction** 람다 수식을 가지는 2개의 연산을 정의한다. 

``` console
40 + 2 = 42
20 - 10 = 
```


## 닫힌 Scope 의 지역변수 접근



## Target Typing