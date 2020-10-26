# VS Code Configuration

- 작업 영역 `name.code-workspace` 파일

```json
{
    "version": "0.2.0",
    "configurations": [
        {
            "name": "gcc.exe build and debug active file",
            "type": "cppdbg",
            "request": "launch",
            "program": "${fileDirname}\\${fileBasenameNoExtension}.exe",
            "args": [],
            "stopAtEntry": false,
            "cwd": "${workspaceFolder}",
            "environment": [
                {
                    "name": "PATH",
                    "value": "%PATH%;z:\\cygwin64\\bin"
                }
            ],
            "externalConsole": false,
            "MIMode": "gdb",
            "miDebuggerPath": "C:\\cygwin64\\bin\\gdb.exe",
            "setupCommands": [
                {
                    "description": "Enable pretty-printing for gdb",
                    "text": "-enable-pretty-printing",
                    "ignoreFailures": true
                }
            ],
            "logging": { "engineLogging": true }, //optional
            "preLaunchTask": "gcc.exe build active file"
        }
    ]
}
```

- 사용자는 `settings.json` 파일
- 작업 영역에은 `.vsocde` 아래에 각종 `*.json` 파일들이 생성됨
- 예를 들어, *c/c++ extension* 을 사용하는 경우 실행 구성 설정은 `c_cpp_properties.json` 파일을 생성함.
