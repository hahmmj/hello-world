---
path: '/blog/shell-setup'
date: '2018-12-17'
title: 리눅스 shell 환경설정
---

### 시작하기

개발과재를 진행하면서 대부분의 코딩 작업은 Visual Code 와 같은 GUI기반의 Editor를 사용하여 작성하게 된다. 그런데 소스코드를 작성하기 전에 프로젝트를 설정하기 위해 Terminal을 열고 상황에 맞게 명령어를 입력하는 작업이 필수적이다. 예를 들면, 소스코드작성을 도와주는 NodeJS package를 찾아서 미리 설치해 준다든지, 작성한 코드의 Unittest와 작성한 코드를 확인하기 위해 Browser에서 접속할 수 있도록 Local web server를 실행하여 준다는지, 소스 코드를 remote repository로 push하여 다른 사람들과 공유하는 일련의 모든 작업들은 Terminal 사용이 필수적이다.

Windows 머신에서는 Terminal 로 `Git for bash`라는 app을 사용하면 편리하다. 이 app은 Linux/Unix OS의 Bash 환경을 Windows에서 에뮬레이팅한 것이다. 즉 100% 똑같이 Bash와 동일하게 동작하지는 않지만 개발에 사용되는 Windows용으로 설치된 유틸리티 Tool들을 (npm, ssh, git...) Bash 환경에서 사용하는 식으로 실행할 수 있다.

물론 Windows 에서 제공하는 PowerShell이나 cmd 유틸리티를 사용하여 위에서 말한 Tool들을 실행해가며 개발작업을 진행할 수 있다. 그런데 그렇게 하지 말고 Linux bash에 익숙해 지는것이 필요하다. 스스로 Windows에 갇혀서 고립되지 말고 웹 개발자라면 Linux/Unix의 Operation정도의 실력을 갖추는 것이 좋다. 거의 대부분의 웹서버들이 Linux/Unix OS를 사용하는 서버로 배포가 되며, 개발작업을 하다보면 Bash Terminal에서 확인하는 작업이 얼마나 편리한지 알게된다.

Bash를 사용하는 Terminal에서 조금 편리하게 작업할 수 있도록 사전 setup이 필요하다. 나중에 좀더 bash 환경에 대해 잘 알게 되면 해당 설정파일을 자신이 편한 방식대로 수정하여 사용할 수 있다.

### 글꼴설치 (Optional)

PowerLine 글꼴은 개발자들이 선호하는 코딩 글꼴들이 있고 Terminal 에서도 사용 가능하다. 설치방법은 다음의 링크를 참조.

[PowerLine 설치하기]([https://medium.com/@slmeng/how-to-install-powerline-fonts-in-windows-b2eedecace58)

요약하면,

1. Github에서 zip 형태의 master branch 소스코드를 Download 받음
2. Download가 완료되면 파일명이 fonts-master.zip인데 이것의 압축을 푼다.
3. PowerShell을 관리자 모드로 실행 (windows-x 키 눌러서 찾아보기)

아래는 Power Shell에서 실행해 줌

4. 압축을 푼 Directory로 이동. 예) cd "D:\Downloads\fonts-master\fonts-master"
5. power script를 실행하기 위해 실행 규칙 변경 예) Set-ExecutionPolicy Bypass
6. 압축푼 디렉토리에서 스크립트 실행 예) .\install.ps1
7. 실행 규칙을 원래대로 변경 예) Set-ExecutionPolicy Bypass

글꼴 설치가 끝났으면, `Git for bash`의 Options 창을 열고 글꼴, 테마, 기본 창 크기등을 조절해 본다. 선호하는 글꼴은 Source Code Pro 혹은 Fria Code 등이고, Editor와 같은 글꼴로 설치하면 Terminal 환경의 Text도 보다 익숙해 질 수 있다.

### ~/.bash_profile

.bash_profile은 사용자 로그인이 최초로 일어났을때 한번만 실행

```
# Source bashrc
if [ -f ~/.bashrc ]; then
    . ~/.bashrc
fi
```

### ~/.bashrc

.bashrc 는 새로운 Terminal이 열릴때마다 실행 (터미널 환경 셋업)

```
alias l="ls -al"

# directory
# 디렉토리 단축키 실행
# alias p='cd /D/projects'
# alias cdh='cd /D/projects/hello-world'

# Outputs the name of the current branch
# Usage example: git pull origin $(git_current_branch)
# Using '--quiet' with 'symbolic-ref' will not cause a fatal error (128) if
# it's not a symbolic ref, but in a Git repo.
function git_current_branch() {
  local ref
  ref=$(command git symbolic-ref --quiet HEAD 2> /dev/null)
  local ret=$?
  if [[ $ret != 0 ]]; then
    [[ $ret == 128 ]] && return  # no git repo.
    ref=$(command git rev-parse --short HEAD 2> /dev/null) || return
  fi
  echo ${ref#refs/heads/}
}


alias g='git'

alias ga='git add'
alias gaa='git add --all'
alias gapa='git add --patch'
alias gau='git add --update'
alias gav='git add --verbose'
alias gap='git apply'

alias gb='git branch'
alias gba='git branch -a'
alias gbd='git branch -d'
alias gbda='git branch --no-color --merged | command grep -vE "^(\*|\s*(master|develop|dev)\s*$)" | command xargs -n 1 git branch -d'
alias gbD='git branch -D'
alias gbl='git blame -b -w'
alias gbnm='git branch --no-merged'
alias gbr='git branch --remote'
alias gbs='git bisect'
alias gbsb='git bisect bad'
alias gbsg='git bisect good'
alias gbsr='git bisect reset'
alias gbss='git bisect start'

alias gc='git commit -v'
alias gc!='git commit -v --amend'
alias gcn!='git commit -v --no-edit --amend'
alias gca='git commit -v -a'
alias gca!='git commit -v -a --amend'
alias gcan!='git commit -v -a --no-edit --amend'
alias gcans!='git commit -v -a -s --no-edit --amend'
alias gcam='git commit -a -m'
alias gcsm='git commit -s -m'
alias gcb='git checkout -b'
alias gcf='git config --list'
alias gcl='git clone --recurse-submodules'
alias gclean='git clean -fd'
alias gpristine='git reset --hard && git clean -dfx'
alias gcm='git checkout master'
alias gcd='git checkout develop'
alias gcmsg='git commit -m'
alias gco='git checkout'
alias gcount='git shortlog -sn'
alias gcp='git cherry-pick'
alias gcpa='git cherry-pick --abort'
alias gcpc='git cherry-pick --continue'
alias gcs='git commit -S'

alias gd='git diff'
alias gdca='git diff --cached'
alias gdcw='git diff --cached --word-diff'
alias gdct='git describe --tags `git rev-list --tags --max-count=1`'
alias gds='git diff --staged'
alias gdt='git diff-tree --no-commit-id --name-only -r'
alias gdw='git diff --word-diff'

#function gdv() { git diff -w "$@" | view - }

alias gf='git fetch'
alias gfa='git fetch --all --prune'
alias gfo='git fetch origin'

#function gfg() { git ls-files | grep $@ }

alias gg='git gui citool'
alias gga='git gui citool --amend'

function ggf() {
  [[ "$#" != 1 ]] && local b="$(git_current_branch)"
  git push --force origin "${b:=$1}"
}

function ggfl() {
  [[ "$#" != 1 ]] && local b="$(git_current_branch)"
  git push --force-with-lease origin "${b:=$1}"
}

function ggl() {
  if [[ "$#" != 0 ]] && [[ "$#" != 1 ]]; then
    git pull origin "${*}"
  else
    [[ "$#" == 0 ]] && local b="$(git_current_branch)"
    git pull origin "${b:=$1}"
  fi
}

function ggp() {
  if [[ "$#" != 0 ]] && [[ "$#" != 1 ]]; then
    git push origin "${*}"
  else
    [[ "$#" == 0 ]] && local b="$(git_current_branch)"
    git push origin "${b:=$1}"
  fi
}

function ggpnp() {
  if [[ "$#" == 0 ]]; then
    ggl && ggp
  else
    ggl "${*}" && ggp "${*}"
  fi
}

function ggu() {
  [[ "$#" != 1 ]] && local b="$(git_current_branch)"
  git pull --rebase origin "${b:=$1}"
}

alias ggpur='ggu'
alias ggpull='git pull origin $(git_current_branch)'
alias ggpush='git push origin $(git_current_branch)'
alias ggsup='git branch --set-upstream-to=origin/$(git_current_branch)'
alias gpsup='git push --set-upstream origin $(git_current_branch)'

alias ghh='git help'

alias gignore='git update-index --assume-unchanged'
alias gignored='git ls-files -v | grep "^[[:lower:]]"'
alias git-svn-dcommit-push='git svn dcommit && git push github master:svntrunk'

alias gk='\gitk --all --branches'
alias gke='\gitk --all $(git log -g --pretty=%h)'

alias gl='git pull'
alias glg='git log --stat'
alias glgp='git log --stat -p'
alias glgg='git log --graph'
alias glgga='git log --graph --decorate --all'
alias glgm='git log --graph --max-count=10'
alias glo='git log --oneline --decorate'
alias glol="git log --graph --pretty='%Cred%h%Creset -%C(auto)%d%Creset %s %Cgreen(%cr) %C(bold blue)<%an>%Creset'"
alias glols="git log --graph --pretty='%Cred%h%Creset -%C(auto)%d%Creset %s %Cgreen(%cr) %C(bold blue)<%an>%Creset' --stat"
alias glod="git log --graph --pretty='%Cred%h%Creset -%C(auto)%d%Creset %s %Cgreen(%ad) %C(bold blue)<%an>%Creset'"
alias glods="git log --graph --pretty='%Cred%h%Creset -%C(auto)%d%Creset %s %Cgreen(%ad) %C(bold blue)<%an>%Creset' --date=short"
alias glola="git log --graph --pretty='%Cred%h%Creset -%C(auto)%d%Creset %s %Cgreen(%cr) %C(bold blue)<%an>%Creset' --all"
alias glog='git log --oneline --decorate --graph'
alias gloga='git log --oneline --decorate --graph --all'
alias glp="_git_log_prettily"

alias gm='git merge'
alias gmom='git merge origin/master'
alias gmt='git mergetool --no-prompt'
alias gmtvim='git mergetool --no-prompt --tool=vimdiff'
alias gmum='git merge upstream/master'
alias gma='git merge --abort'

alias gp='git push'
alias gpd='git push --dry-run'
alias gpf='git push --force-with-lease'
alias gpf!='git push --force'
alias gpoat='git push origin --all && git push origin --tags'
alias gpu='git push upstream'
alias gpv='git push -v'

alias gr='git remote'
alias gra='git remote add'
alias grb='git rebase'
alias grba='git rebase --abort'
alias grbc='git rebase --continue'
alias grbd='git rebase develop'
alias grbi='git rebase -i'
alias grbm='git rebase master'
alias grbs='git rebase --skip'
alias grh='git reset'
alias grhh='git reset --hard'
alias grm='git rm'
alias grmc='git rm --cached'
alias grmv='git remote rename'
alias grrm='git remote remove'
alias grset='git remote set-url'
alias grt='cd $(git rev-parse --show-toplevel || echo ".")'
alias gru='git reset --'
alias grup='git remote update'
alias grv='git remote -v'

alias gsb='git status -sb'
alias gsd='git svn dcommit'
alias gsh='git show'
alias gsi='git submodule init'
alias gsps='git show --pretty=short --show-signature'
alias gsr='git svn rebase'
alias gss='git status -s'
alias gst='git status'
alias gsta='git stash save'
alias gstaa='git stash apply'
alias gstc='git stash clear'
alias gstd='git stash drop'
alias gstl='git stash list'
alias gstp='git stash pop'
alias gsts='git stash show --text'
alias gstall='git stash --all'
alias gsu='git submodule update'

alias gts='git tag -s'
alias gtv='git tag | sort -V'

alias gunignore='git update-index --no-assume-unchanged'
alias gunwip='git log -n 1 | grep -q -c "\-\-wip\-\-" && git reset HEAD~1'
alias gup='git pull --rebase'
alias gupv='git pull --rebase -v'
alias gupa='git pull --rebase --autostash'
alias gupav='git pull --rebase --autostash -v'
alias glum='git pull upstream master'

alias gwch='git whatchanged -p --abbrev-commit --pretty=medium'
alias gwip='git add -A; git rm $(git ls-files --deleted) 2> /dev/null; git commit --no-verify -m "--wip-- [skip ci]"'

# docker
alias d="winpty docker"
alias dex="winpty docker exec"
alias dps="docker ps -a"
alias di="docker images"

# yarn
alias y="yarn"
alias ys="yarn start"
```

### vim setup (Optional)

vim은 Terminal 기반의 에디터인데 초기 설정을 해 주면 조금 편하게 사용할 수 있다.

[여기 따라하기](https://github.com/amix/vimrc)
