# Module

!!! info
    Mark Reinhold 의 [The State of the Module System](http://openjdk.java.net/projects/jigsaw/spec/sotms/#defining-modules)을 참고해서 작성.


Java 9 에서 새로 소개된 **모듈 시스템** 에 대해 소개한다.

모듈성([Project Jigsaw](http://openjdk.java.net/projects/jigsaw/)) 은 개발자들이 S/W 시스템의 빌드, 유지, 보수 시 생산성을 높이는데 도움이 된다.


## 모듈이란

- 모듈성은 다수의 패키지(packages)들에 대한 좀 더 높은 수준의 집합으로 볼 수 있다.
- 자바의 **module** 은 유일한 이름, 패키지들, 리소스들(이미지 또는 XML 파일등), 그리고 하나의 **module descriptor** 로 구성된다.
- `module descriptor`에는 다음 내용이 기술된다.
    - 모듈 이름(module name)
    - 모듈들의 의존성(module dependencies)
    - 제공하는 서비스들    
    - 소비하는 서비스들
    - 리플렉션(reflection) 을 허용하는 다른 모듈들


- JDK 의 모듈들 확인하기

```
    java --list-modules
```

- Module declarations

=== "module"
    ```java
    module com.foo.bar { }
    ```
=== "requires"
    ```java
    module com.foo.bar {
        requires org.baz.qux;
    }
    ```
=== "exports"
    ```java
    module com.foo.bar {
        requires org.baz.qux;
        exports com.foo.bar.alpha;
        exports com.foo.bar.beta;
    }
    ```

- Module artifacts

```
    META-INF/
    META-INF/MANIFEST.MF
    module-info.class
    com/foo/bar/alpha/AlphaFactory.class
    com/foo/bar/alpha/Alpha.class
    ...
```

- Platform module



## 호환성



## 서비스들

## Reflection


> 참고
[^1]:
    The State of the Module System : http://openjdk.java.net/projects/jigsaw/spec/sotms/#defining-modules

[^2]:
    Open JDK Project Jigsaw : http://openjdk.java.net/projects/jigsaw/


