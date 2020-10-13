## 구글 테스트 빌드하기

!!! note    "사전 준비"

    - **MinGW-w64** 설치
    - **MSYS2** 설치
    - **CMake** 설치
    - 설치된 패키지의 ``bin`` 폴더 ==path== 환경 변수에 추가

- 명령 프롬프트 창에서 원하는 디렉토리를 생성한다.
- **VS Code** 를 실행한다.
``` console
cdoe . 
```

- ``googletest`` 리포지터리를 복제한다.
``` console
git clone https://github.com/google/googletest.git
```

- 복제된 ``googletest`` 폴더로 이동한다.
- **CMake** 를 실행한다.
``` console
cmake CMakeLists.txt -G "MinGW Makefiles"
```

- **VS Code** 에 ``Cmake tool`` 확장이 설치하고 빌드한다.


## 구글 테스트 어댑터 사용하기
