---
path: '/blog/git-command'
date: '2018-12-16'
title: git command 정리
---

# 시작하기

개발자는 Editor를 통해 소스코드를 수정한다. 자신만의 연습용 코드를 작성한다면 소스관리가 필요없겠지만, 대부분의 개발 프로잭트는 여려명이 하나의 과제를 동시에 작업하며 코드를 완성해 나간다. 그래서 개발자가 소스코드를 관리하는 방법은 필수로 익히고 있어야 한다. 많은 소스코드 관리 툴이 존재하지만 Git을 익히기를 추천한다.

Git을 사용하여 코드를 수정하는 일련의 개발자들 간의 약속이 있다. 이 룰에 맞게 설명하기로 한다.

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

## 소스코드 수정전에 익혀야할 명령어

이 작업을 하기 전에 `~/.bash_profile`, `~/.bashrc`, 그리고 `~/.gitconfig` 가 적절히 셋업되어야 한다. (다른 페이지 참조)

소스코드를 수정하기 전에 자신이 어떤 Branch에서 작업중인지 확인한다. Bash를 편하게 설정했기 때문에 Directory 정보와 함께 branch도 표시된다.

```
// 아래와 같이 비슷하게 출력되며 (master) 는 현재 master branch에 있다는 것을 표시
// bash 환경 설정에 따라 출력되는 글자나 형식은 틀릴 수 있음

zonek@JADU-DESKTOP MINGW64 /D/projects/hello-world (master)
$
```

- 마지막 소스코드 수정이후 현재 Repository의 변경사항 확인
  - 수정한 파일이 없으면 아무것도 출력 안됨

```
// gss: git status -s  (마지막 -s 의미는 short 즉, 단축된 형태로 표시)

zonek@JADU-DESKTOP MINGW64 /D/projects/hello-world (master)
$ gss

zonek@JADU-DESKTOP MINGW64 /D/projects/hello-world (master)
$
```

### git Branch 관련 명령어

- 현재 repository의 모든 브렌치 확인

```
// gb: git branch

zonek@JADU-DESKTOP MINGW64 /D/projects/hello-world (master)
$ gb
  add-markdown
* master
```

- git branch 이동
  - 주의: 이동 전에 현재의 브렌치에서 수정한 내용이 없어야 함
  - 수정사항이 있다면 commit을 하거나 임시저장을 하여 수정사항을 마무리해야 함. 하기참조

```
// gco [기존 브렌치 이름]: git checkout

zonek@JADU-DESKTOP MINGW64 /D/projects/hello-world (master)
$ gco add-markdown
Switched to branch 'add-markdown'
Your branch is up to date with 'origin/add-markdown'.

zonek@JADU-DESKTOP MINGW64 /D/projects/hello-world (add-markdown)
$

```

- 새로운 branch 만듬
  - 주의: 새로운 branch만드는 작업은 반드시 master branch로 이동(`gcm`)후에 명령어를 실행 함 (약속임)

```

// gcm
// gcb [새 branch 이름]: git checkout -b (-b는 새로운 branch 만드는 옵션)

zonek@JADU-DESKTOP MINGW64 /D/projects/hello-world (new-test-branch)
$ gcm
Switched to branch 'master'
Your branch is up to date with 'origin/master'.

zonek@JADU-DESKTOP MINGW64 /D/projects/hello-world (master)
$ gcb new-test-branch
Switched to a new branch 'new-test-branch'

zonek@JADU-DESKTOP MINGW64 /D/projects/hello-world (new-test-branch)
$

```

- 기존 branch 삭제
  - 어떤 branch 가 있는지 보고 (`gb`) 삭제할 branch 확인 할 것
  - branch 일일히 삭제 안해도 되나... 가끔씩 정리해 줌.

```
// gbD [삭제할 branch 명]

zonek@JADU-DESKTOP MINGW64 /D/projects/hello-world (add-markdown)
$ gbD new-test-branch
Deleted branch new-test-branch (was a88aff2).
```

### git에서 소스수정 이후 원격 Repository로 전송하기
