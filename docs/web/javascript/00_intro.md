## Javascript 시작하기

!!! note "Javascript 로 무엇을 할 수 있을까?"

    - 웹페이지 / 웹사이트 개발
    - 게임
    - 데스크톱 앱
    - 모바일 앱
    - 데이터베이스 관리
    - IOT 서비스

> 웹 서버 앱

- 2010년 까지는 Javascript로 웹 클라이언트 개발 이외에는 사용하지 않았으나, Node.js 가 등장하면서 웹 서버 애플리케이션도 개발이 가능하게 되었다.

- Node.js 는 웹 서버 앱을 개발하기 위해 필수적인 모듈만 제공하기 때문에 다른 스크립트 언어나 프레임워크에 비해 개발이 복잡할 수 있다.

- 그렇지만, Node.js 는 빠르기 때문에 서버 운영/유지 비용이 다른 방법들에 비해 적다는 이점이 있다.

> 모바일 앱

- 안드로이드와 아이폰에서 공통으로 동작하는 모바일 앱을 Javascript 언어 하나로 개발이 가능하다.

!!! note "React Native"

    - 페이스북에서 Javascript로 애플리케이션을 개발할 수 있는 React Native를 공개함.
    - Javascript 만으로 모든 운영체제에서 빠르게 동작하는 앱을 개발할 수 있다.
    - 안드로이드와 아이폰에서 동작하는 페이스북, 페이스북 그룹 등이 모두 Javascript로 작성된 앱이다.

> 데스크톱 앱

- **NW.js** 모듈 등으로 데스크톱 앱 개발에 Javascript를 활용하기 시작함.
- **Github** 에서 *Atom* 에디터와 일렉트론(Electron) 모듈을 공개.


> 게임 앱

- 개인 개발자도 저렴하게 사용할 수 있는 **Unity** 게임 엔진.
- 모노 플랫폼을 사용해 Javascript 기반의 유니티 스크립트 또는 C# 언어를 사용해서 다양한 플랫폼에 서 동작하는 게임 개발 가능.


> 데이터베이스

- 복잡하고 무거운 **SQL** 대신 간단하고 사용하기 쉬운 **NoSQL** 데이터베이스들 등장.
- **NoSQL** 을 사용하는 대표적인 DB는  **MongoDB** .


### Javascript 표준

- ECMAScript 라는 표준 용어
- 2016년 6월에 ECMAScript 7 발표.
- 최신 브라우저들은 7 까지 지원하지만 인터넷 익스플로러는 6 이상은 지원하지 않음.


### 간단한 HTML + Javascript 코드 작성

- `#!html <script> </script>` 태그를 사용해서 작성한다.

``` html

<!DOCTYPE html>
<html lang="en" xmlns="http://www.w3.org/1999/xhtml">
    <head>
        <meta charset="utf-8" />
        <title></title>
        
        <!-- 자바스크립트 코드 작성하는 부분 -->
        <script>
            alert('Hello World!!');
        </script>            
        
    </head>
    <body>
        
    </body>
</html>

```