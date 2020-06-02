# Agile Programming

참고: [mkdocs.org](https://www.mkdocs.org).

## 명령어

* `mkdocs new [dir-name]` - 새 프로젝트 생성
* `mkdocs serve` - live-reloading 서버 시작
* `mkdocs build` - 문서 생성
* `mkdocs -h` - 도움말 출력

## 작업 순서

* `mkdocs gh-deploy`
* `git add .`
* `git commit -m "코멘트"`
* `git push`

## Project layout

    mkdocs.yml    # The configuration file.
    docs/
        index.md  # The documentation homepage.
        ...       # Other markdown pages, images and other files.



## Git에 배포하기

> 웹문서를 생성하고 **git** 에 업로드한다.

```sh
$ mkdocs build
$ mkdocs gh-deploy
```

> 로컬 저장소에 저장한다.

```sh
$ git add .
$ git commit -m "코멘트"
$git push
```


