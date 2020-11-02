---
title: 基于 dep 依赖管理的 Golang Gitlab CI 配置方法
date: 2020-02-29
---

`.gitlab-ci.yml`

```yml
image: golang:latest

variables:
  REPO_NAME: {Your Repo}

before_script:
  - mkdir -p $GOPATH/src/$(dirname $REPO_NAME)
  - ln -svf $CI_PROJECT_DIR $GOPATH/src/$REPO_NAME
  - cd $GOPATH/src/$REPO_NAME
  - go get -u github.com/golang/dep/cmd/dep


stages:
  - test
  - build

format:
  stage: test
  script:
    - dep ensure
    - go test

compile:
  stage: build
  script:
    - dep ensure
    - go build -o main
    - mkdir mybinary && cp main mybinary/
  artifacts:
    paths:
      - mybinary

```

<!-- more -->
