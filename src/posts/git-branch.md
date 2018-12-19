---
path: /blog/git-branch
date: 2018-12-19
title: git branch 확인
---

# 시작하기

git 에서 branch는 이미 완성된 코드에 영향을 주지 않고 자기의 dev box에서 안전하게 소스코드를 건드려 볼 수 있는 환경을 제공한다. 모든 것이 테스트 완료되었다면 Branch를 게시해서 다른 사람에게 review를 받을 수 있고, Review가 끝나면 나의 변경사항을 이미 완성된 코드(master)에 반영할 수 있는 방법을 제공한다. 그리고 임시 브렌치는 깔끔히 삭제 될 수 있다.

반드시 유지해야 하는 브렌치로는 master 브렌치 하나이다. 여기에 완성된 코드를 계속 반영하면서 작업한다.

## 현재 repository 의 모든 브렌치 확인

```
// gb: git branch

zonek@JADU-DESKTOP MINGW64 /D/projects/hello-world (master)
$ gb
  add-markdown
* master
```

## git branch 이동

- 주의: 이동 전에 현재의 브렌치에서 수정한 내용이 없어야 함
- 수정사항이 있다면 commit을 하거나 임시저장을 하여 수정사항을 마무리해야 함.

```
// gco [기존 브렌치 이름]: git checkout

zonek@JADU-DESKTOP MINGW64 /D/projects/hello-world (master)
$ gco add-markdown
Switched to branch 'add-markdown'
Your branch is up to date with 'origin/add-markdown'.

zonek@JADU-DESKTOP MINGW64 /D/projects/hello-world (add-markdown)
$

```

## 새로운 branch 만듬 (꼭 master에서 만든다)

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

## 기존 branch 삭제

- 어떤 branch 가 있는지 보고 (`gb`) 삭제할 branch 확인 할 것
- branch 일일히 삭제 안해도 되나... 가끔씩 정리해 줌.

```
// gbD [삭제할 branch 명]

zonek@JADU-DESKTOP MINGW64 /D/projects/hello-world (add-markdown)
$ gbD new-test-branch
Deleted branch new-test-branch (was a88aff2).
```
