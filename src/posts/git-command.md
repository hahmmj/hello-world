---
path: '/blog/git-command'
date: '2018-12-16'
title: git command 정리
---

# 시작하기

개발자는 Editor를 통해 소스코드를 수정한다. 자신만의 연습용 코드를 작성한다면 소스관리가 필요없겠지만, 대부분의 개발 프로잭트는 여려명이 하나의 과제를 동시에 작업하며 코드를 완성해 나간다. 그래서 개발자가 소스코드를 관리하는 방법은 필수로 익히고 있어야 한다. 많은 소스코드 관리 툴이 존재하지만 Git을 익히기를 추천한다.

Git을 어떻게 사용할지는 프로잭트를 진행하는 관리자가 정하게 되는데 프로잭트별로, 팀이 선호하는 방식으로 다양할 수 있다. 많은 깊이 있는 방법이 있겠으나 모든 내용을 다루지 않고 여기에서 설명하는 방식대로 먼저 익혀주었으면 좋겠다. 다른 프로젝트는 어떻게 Git을 사용하는지는 더 찾아보며 추후에 살펴 볼 수 있다. 개발하면서 git을 다루는 방식은 여기서 벗어나지 않으리라 생각한다.

참고로, 이 작업을 하기 전에 bash 환경 설정인 `~/.bash_profile`, `~/.bashrc`, 그리고 `~/.gitconfig` 가 적절히 셋업되어야 한다. 소스코드를 수정하기 전에 자신이 어떤 Branch에서 작업중인지 bash 프롬프트를 확인하자. Bash를 편하게 설정했기 때문에 Directory 정보와 함께 branch도 표시된다.

```
// 아래와 같이 비슷하게 출력되며 (master) 는 현재 master branch에 있다는 것을 표시
// bash 환경 설정에 따라 출력되는 글자나 형식은 틀릴 수 있음

zonek@JADU-DESKTOP MINGW64 /D/projects/hello-world (master)
$
```

## Repository 생성

아래와 같이 dev box (개발용으로 사용하는 노트북이나 데스크탑 PC)의 디렉토리에서 local repository를 생성해도 되나..,

```bash
// directory를 새로 만든 후 해당 디렉토리로 이동하여 다음의 명령어로 생성

$ git init
```

다른 사람과의 공유를 할 목적이라면 먼저 github같은 곳에서 remote repository를 먼저 생성한 후, Web Page의 안내에 따라 `git clone` 명령어로 renote repository를 내려받아 local repository를 셋업한다.

```bash
$ git clone git@github.com:hahmmj/hello-world.git
```

## 소스코드 수정전에 익혀야 할 명령어

가장 기본적으로 현재 디렉토리에서 수정된 사항이 있는지 확인하는 것이다.

- 마지막 소스코드 수정이후 현재 Repository의 변경사항 확인
  - 수정한 파일이 없으면 아무것도 출력 안됨

```
// gss: git status -s  (마지막 -s 의미는 short 즉, 단축된 형태로 표시)

zonek@JADU-DESKTOP MINGW64 /D/projects/hello-world (master)
$ gss

zonek@JADU-DESKTOP MINGW64 /D/projects/hello-world (master)
$
```

### 시나리오별로 Git 작업하기

1. [git 브렌치 확인](/blog/git-branch)
2. [소스코드에 새로운 코드 수정하기](/blog/git-change-sourcecode)
3. [다른사람이 게시한 Branch로 테스트 하기](/blog/git-review-changes)
