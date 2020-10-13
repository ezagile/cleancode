## CMake Tutorial

- C++ 소스 파일을 빌드하는 환경은 설정된 상태에서 시작한다.
- MinGW + CMake 또는 CygWin 사용


## Step 1

- `CMakeLists.txt` 파일을 생성하고, 다음 내용을 작성한다. 
- 아래 내용을 베이스로 튜토리얼을 진행한다.
``` cmake
cmake_minimum_required(VERSION 3.10)

# 프로젝트 이름 설정
project(Tutorial)

# 실행화일 추가
add_executable(Tutorial tutorial.cxx)
```

### 프로젝트와 실행 파일의 버전 추가

- `project()` 명령을 사용해서 프로젝트 이름과 버전 넘버를 수정한다.
``` cmake
# 프로젝트 이름 설정
project(Tutorial VERSION 1.0)
```

- 버전 넘버를 소스코드에 전달하도록 헤더파일을 설정(configure)한다.
``` cmake
configure_file(TutorialConfig.h.in TutorialConfig.h)
```

- 설정된 파일을 이진트리로 쓰여지고, **include** 할 파일을 찾을 경로들의 리스트에 디렉토리를 추가해야 한다.
``` cmake
target_include_directories(Tutorial PUBLIC 
                            "${PROJECT_BINARY_DIR}"
                        )
```

- 편집기를 사용해서 `TutorialConfig.h.in` 파일을 소스 디렉토리에 생성하고, 다음 내용을 작성한다.
``` cpp
// Tutorial 프로젝트에 대한 옵션 및 설정
#define Tutorial_VERSION_MAJOR @Tutorial_VERSION_MAJOR@
#define Tutorial_VERSION_MINOR @Tutorial_VERSION_MINOR@
```

- `CMake` 가 헤더파일을 처리하면 `@Tutorial_VERSION_MAJOR@`, `@Tutorial_VERSION_MINOR@` 가 값으로 대치된다.

- 다음으로 `tutorial.cxx` 가 `TutorialConfig.h` 를 **include** 하도록 한다.
- `tutorial.cxx` 에서 버전 넘버를 출력해본다.
``` cpp
if (argc < 2>){
    std::cout << argv[0] << " 버전" << Tutorial_VERSION_MAJOR << "." << Tutorial_VERSION_MINOR << std::endl;
}
```

- C++ 11 표준을 지원하도록 명시하고, 테스트 해보자.
- `CMAKE_CXX_STANDARD` 변수를 사용하는 것이 가장 쉬운 방법이다.
``` cmake
# specify the C++ standard
set(CMAKE_CXX_STANDARD 11)
set(CMAKE_CXX_STANDARD_REQUIRED True)
```

### 빌드 및 테스트

- `cmake` 또는 `cmake-gui` 를 실행시켜서 빌드한다.
- 명령창에서 `Help/guide/tutorial` 디렉토리에 가서 다음과 같이 해볼 수 있다.
``` console
mkdir step1_build
cd step1_build
cmake ../step1
cmake --build
```

- 실행창에서 글자가 깨져서 출력되면 다음을 추가한다.
``` cmake
set(CMAKE_CXX_FLAGS "-fexec-charset=CP_949")
```

## Step 2 라이브러리 추가

- 제곱근(square root)을 계산한 라이브러리를 추가해보자.
- 실행 화일은 표준 라이브러리의 제곱근 함수흘 사용한다.
- `MathFunctions` 하위 디렉토리를 라이브러리를 둘 것이다.
- 여기예, `MathFunctions.h` 와 `mysqrt.cxx` 가 있으며, `mysqrt` 함수를 포함한다.

- `CMakeLists.txt` 에 다음과 같이 `MathFunctions` 디렉토리를 탑레벨(최상위)에 추가한다.
```console
add_library(MathhFunctions mysqrt.cxx)
```

- 새로운 라이브러리를 사용하기 위해 **add_subdirectory()** 를 `CMakeLists.txt` 의 앞쪽에 추가하면, 라이브러리가 빌드된다.
- `include` 디렉토리로 `MathFunctions` 을 추가해서 `mqsqrt.h` 파일을 찾도록 한다.

```console
add_subdirectory(MathFunctions)

add_executable(Tutorial tutorial.cxx)

target_link_libraries(Tutorial PUBLIC MathFunctions)

target_include_directories(Tutorial PUBLIC
                            "${PROJECT_BINARY_DIR}"
                            "${PROJECT_BINARY_DIR}/MathFunctions"

                    )
```

```console
option(USE_MYMATH "Use tutorial provided math implementation" ON)

# configure a header file to pass some of the CMake settings
# to the source code
configure_file(TutorialConfig.h.in TutorialConfig.h)
```

```console
if(USE_MYMATH)
  add_subdirectory(MathFunctions)
  list(APPEND EXTRA_LIBS MathFunctions)
  list(APPEND EXTRA_INCLUDES "${PROJECT_SOURCE_DIR}/MathFunctions")
endif()

# add the executable
add_executable(Tutorial tutorial.cxx)

target_link_libraries(Tutorial PUBLIC ${EXTRA_LIBS})

# add the binary tree to the search path for include files
# so that we will find TutorialConfig.h
target_include_directories(Tutorial PUBLIC
                           "${PROJECT_BINARY_DIR}"
                           ${EXTRA_INCLUDES}
                           )
```

## Step 3

- 사용 요구사항(usage requirements)은 라이브러리나 실행 링크에 대한 더 나은 컨트롤을 가능하게 한다.
- 주요 명령들은 다음과 같다.

    - target_compile_definitions()
    - target_compile_options()
    - target_include_directories()
    - target_link_libraries()

