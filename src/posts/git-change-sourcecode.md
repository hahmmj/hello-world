---
path: /blog/git-change-sourcecode
date: 2019-12-19
title: 소스코드에 새로운 코드 수정하기
---

# 시작하기

소스코드를 수정하려면 정확히 어떤 기능을 위해 코드를 수정해야 하는지 수정범위를 확실히 해야 한다. 이러한 설명은 개발팀에서 사용하는 Issue Tracker[^1]에 정의가 되어 있는데, 이것을 먼저 확인한 수 설명이 부족하거나 애매한 부분이 있으면 반드시 그것을 작성한 사람과 확실히 해야 한다. (그래야 정확한 수정을 했다고 말할 수 있다.)

물론 혼자 개발하는 프로잭트에서는 이런 것을 생략할 수 있겠으나, 혼자 수정을 하더라도 Issue Tracker를 기록하길 바란다. 내가 수정하려는 코드가 무엇을 하기 위해 변경하는 것일까 적고, 거기에 맞게 수정되었는지 확인하는 습관이 중요하다. 이것이 코드리뷰에 필수적이다.

다음과 같은 단계를 거쳐서 소스 코드가 수정된다.

1. Issue Tracker에서 수용기준(Acceptance Criteria) 파악 (혹은 작성)
2. master 브렌치에서 신규 브렌치 준비 (브렌치 명은 issue번호-issue이름을 따름)
3. Editor에서 Source Code 변경 및 테스트 코드 추가
4. 테스트가 올바로 동작하는지 확인
5. 변경된 파일들을 자체적으로 리뷰
6. Commit 예비단계인 Stage에 추가
7. 변경된 소스코드를 Local Reposiroty에 Commit 하기
8. 최종 Branch를 Remote Repository에 Push 하기
9. 코드리뷰 지적사항을 반영한다면 코드 수정 -> 3번 단계로

여기서는 Git 조작을 중심으로 5. 6. 7. 8 번만 설명한다.

## 변경된 사항을 자체적으로 리뷰

새로 만든 Branch에서 코드를 수정하였고 변경된 사항들을 확인하자 (컴퓨터가 바뀌는 바람에 프롬프트가 모양이 약간 바뀌었음)

```
~/projects/hello-world on  add-markdown [!?] is 📦  v1.0.0 via ⬢ v10.14.1 at ☸️ minikube
•100% ➜ g status
On branch add-markdown
Changes not staged for commit:
  (use "git add <file>..." to update what will be committed)
  (use "git checkout -- <file>..." to discard changes in working directory)

	modified:   package-lock.json
	modified:   package.json
	modified:   src/posts/git-command.md
	modified:   src/posts/shell-setup.md
	modified:   src/templates/blogTemplate.js

Untracked files:
  (use "git add <file>..." to include in what will be committed)

	src/posts/git-branch.md
	src/posts/git-change-sourcecode.md
	src/posts/git-review-changes.md

no changes added to commit (use "git add" and/or "git commit -a")
```

`-s` (short)을 뺀 `git status` 명령은 다음에 할 수 있는 명령어 후보까지 상세히 설명해 준다. 그러나 그러한 명령어도 곧 익숙해질 테니 다음 단축 상황을 보는 것을 익혀두자.

```
// gss : git status -s

~/projects/hello-world on  add-markdown [!?] is 📦  v1.0.0 via ⬢ v10.14.1 at ☸️ minikube
•100% ➜ gss
 M package-lock.json
 M package.json
 M src/posts/git-command.md
 M src/posts/shell-setup.md
 M src/templates/blogTemplate.js
?? src/posts/git-branch.md
?? src/posts/git-change-sourcecode.md
?? src/posts/git-review-changes.md

~/projects/hello-world on  add-markdown [!?] is 📦  v1.0.0 via ⬢ v10.14.1 at ☸️ minikube
•100% ➜
```

- 오른쪽에 `M`이 정렬된 것은 기존의 내용이 변경된 것을 의미.

```
 M package-lock.json
 M package.json
 M src/posts/git-command.md
 M src/posts/shell-setup.md
 M src/templates/blogTemplate.js
```

- `??`는 한번도 커밋되지 않은(untracted) 파일들을 표시

```
?? src/posts/git-branch.md
?? src/posts/git-change-sourcecode.md
?? src/posts/git-review-changes.md
```

- 변경내용확인은 Editor로 보면 편하다. 아니면 `git diff`로 터미널로 확인할 수 도 있다.

## Commit 예비단계인 Stage에 추가

git의 `add` 명령어로 변경된 내용을 stage에 올려 커밋을 준비한다.

```
// gaa: git add -all (`git add .` 와 동일)

~/projects/hello-world on  add-markdown [!?] is 📦  v1.0.0 via ⬢ v10.14.1 at ☸️ minikube
•100% ➜ gaa
```

다시 status 명령을 실행해 보면 `M`이 왼쪽으로 올라가 있고, 새롭게 추가된다는 표시인 `A`가 M과 같이 Stage영역으로 올라간 표시를 볼 수 있다.

```
~/projects/hello-world on  add-markdown [+] is 📦  v1.0.0 via ⬢ v10.14.1 at ☸️ minikube
•100% ➜ gss
M  gatsby-node.js
M  package-lock.json
M  package.json
A  src/posts/git-branch.md
A  src/posts/git-change-sourcecode.md
M  src/posts/git-command.md
A  src/posts/git-review-changes.md
A  src/posts/images/vscode-diff.png
M  src/posts/shell-setup.md
```

## 변경된 소스코드를 Local Reposiroty에 Commit 하기

stage에 있는 파일 변경이 Repository에 반영된다. 반영되는 단위는 commit 이고 특정 commit은 source code의 특정 형상을 시간순으로 타임머신과 같이 돌아 다닐 수 있다. (돌아 다니지는 않아도 된다. 예전 코드를 참조할 수 있다는 의미이다.)

아래는 --wip-- add git command 라는 메시지로 local repository에 commit을 하였다.

```
// git commit -m [commit message]

~/projects/hello-world on  add-markdown [+] is 📦  v1.0.0 via ⬢ v10.14.1 at ☸️ minikube
•100% ➜ gcmsg '--wip-- add git-command'
[add-markdown a43b2b4] --wip-- add git-command
 9 files changed, 512 insertions(+), 130 deletions(-)
 create mode 100644 src/posts/git-branch.md
 create mode 100644 src/posts/git-change-sourcecode.md
 create mode 100644 src/posts/git-review-changes.md
 create mode 100644 src/posts/images/vscode-diff.png
```

바로 status 를 확인하면 방금 commit 하였으므로 commit 이후 수정사항이 없이 깨끗하다

```
~/projects/hello-world on  add-markdown is 📦  v1.0.0 via ⬢ v10.14.1 at ☸️ minikube
•100% ➜ gss

~/projects/hello-world on  add-markdown is 📦  v1.0.0 via ⬢ v10.14.1 at ☸️ minikube
•100% ➜
```

### (참고) commit 이력 보기

참고로 내 커밋을 확인할 필요하 있다. 다음과 같이 확인하자.

1. tree의 맨 위에 `a43b2b4`의 고유 번호로 커밋이 반영된 것을 알 수 있다.
2. 바로 밑에 `f6be877`가 있는데 이 소스코드를 원본으로 하여 방금 하였던 commit이 작성되었다.
3. tree의 `*`는 이 브렌츠에 반영된 커밋을 보여준다.
4. `a88aff2` 는 다른 곳 (master) 에서 생성된 커밋이 `f6be877`에 의해 지금의 브렌치로 반영(merge) 되었다.
5. `*`로 이동하여 예전 변경사항을 볼 수 도 있음 (생략)

```
~/projects/hello-world on  add-markdown [!] is 📦  v1.0.0 via ⬢ v10.14.1 at ☸️ minikube
•100% ➜ glog

* a43b2b4 (HEAD -> add-markdown) --wip-- add git-command
*   f6be877 (origin/add-markdown) Merge branch 'master' into add-markdown
|\
| * a88aff2 (origin/master, origin/HEAD, master) Add markdown (#2)
* | 6393fae --wip-- add git command page
* | c4e61b6 add more pages and fix layout
* | ae3fcd1 add linux shell help page
* | 7d6687a #1 add markdown page
|/
* 6d045fa first commit
(END)
```

## 최종 Branch를 Remote Repository에 Push 하기

모든 커밋이 완료가 되었으면 이제 Code Review를 위해 다른 사람에게 공유하자. `ggpush` 명령어를 치기 전에 혹시 commit해야 할 변동사항이 늘어났든지, 함께 commit해야 할 파일을 빼먹은 것은 아닌지 확인하자. (뭔가 더 추가 해야 한다면 `add` 하고 `commit`을 다시하자. 이럴때를 대비해서 `--amend`란 옵션을 쓸 수 있다. 지금은 생략)

```
// ggpush // git push origin [현재 브렌치 명], ggpush 단축키는 자동으로 현재 브렌치명이 기입된다

~/projects/hello-world on  add-markdown is 📦  v1.0.0 via ⬢ v10.14.1 at ☸️ minikube took 3s
•100% ➜ ggpush
Counting objects: 14, done.
Delta compression using up to 8 threads.
Compressing objects: 100% (13/13), done.
Writing objects: 100% (14/14), 466.98 KiB | 16.10 MiB/s, done.
Total 14 (delta 7), reused 0 (delta 0)
remote: Resolving deltas: 100% (7/7), completed with 7 local objects.
To https://github.com/hahmmj/hello-world.git
   f6be877..426443f  add-markdown -> add-markdown

~/projects/hello-world on  add-markdown is 📦  v1.0.0 via ⬢ v10.14.1 at ☸️ minikube took 56s
•100% ➜
```

Push가 완료되면 해당 Push로 Github사이트로 이동해서 Pull Request를 만들 수 있다.
