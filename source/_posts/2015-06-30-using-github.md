---
layout: post
title:  "github tips"
tags: github git upstream
---

github 모르면 간첩.

## fork 된 저장소 upstream으로 부터 업데이트 하기

fork 한후 upstream에 있는 변경사항을 주기적으로 싱크해야 할때 이렇게 하면 된다.
upstream은 포크 하기전 원본 저장소를 칭하는 것

```bash
git checkout master
git remote add upstream git@github.com:whoever/whatever.git
git fetch upstream # update apache remote
git rebase upstream/master # rebase upstream/master to current branch(master)
# make tracking branch 이렇게하면 upstream은 apache master와 싱크를 하게 되고, 로컬에 unpushed commit이 생기게 된다.
git push # git push로 내 계정의 git에 최신사항으로 업데이트 함.


# merge upstream with master; 최신 변경 사항을 master로 가지고 온다.
#git checkout master
#git merge upstream

# make topic branch;
git checkout -b topic # make topic branch
git push -u origin topic # push topic branch to origin
# 이 상태에서 github상에서 pull request를 요청하면 된다.
```

## make topic branch

포크한 repo에서 바로 작업을 하게 되면 나중에 apache/master를 rebase하기가 힘들어 진다. 그래서 토픽 브랜치를 만들어서 거기서 태스크를 위한 커밋을 하고, 준비가 되면 거기서 바로 pull request를 하고, 업스트림에 머지가 되면 그 토픽브랜치는 이제 더이상 필요가 없다. 그후 업스트림으로 부터 다시 리베이스를 하면 마스터 브랜치는 최신으로 유지가 가능하다.


```bash
# assume current branch is master
git checkout -b topic # make topic branch
...
git commit -m "..."
git push -u origin topic # push topic branch to origin
# 이 상태에서 github상에서 pull request를 요청하면 된다.

git checkout master
git merge --squash topic  # merge to master ans make 1 commit
```


## undoing commits

git reset 명령을 하게 되면 커밋은 돌려지고 HEAD의 체인지가 워킹 디렉토리에 반영이 되지만(그래서 원복을 할 기회가 있음.), --hard를 붙이면 워킹 디렉토리의 변경사항은 없다. 따라서 --hard옵션은 조심해서 쓸것.

```bash
git reset --hard <dest_commithash> # move head to the designated commit
git reset (--hard) HEAD^1 # reset last commit
# HEAD^1 : parent of head
```

## pathspec

git에서 파일을 지칭하고 싶을때 매번 풀 패스를 써주면 길어서 번거러울때가 있다. 이럴때 pathspec을 이용해서 줄일 수 있다. 잘 사용하면 아주 유용하다. [pathspec](https://git-scm.com/docs/gitglossary.html#gitglossary-aiddefpathspecapathspec) 참고.

```
# java파일만 리스트 업
git log -p -- **/*.java

#abc folder하위 모든 파일
git log -p -- abc/**

```

## revision selection

- `HEAD^1` : parent of head
- `HEAD^2` : 2nd parent of head. only valid for merge commit
- `HEAD@{n}` : nth prior head

## cherrypick specific file from different branch

`git cherry-pick`은 커밋 단위로 가지고 오기때문에, 다른 파일의 변경사항도 같이 적용이 된다. 이럴때는 `checkout file`을 사용하면 된다.


```bash
git checkout <branch> -- <filename>
```

## ssh key를 등록했음에도 아이디와 패스워드를 물어볼때
git clone을 하고 난후 ssh키를 등록을 했음에도 매번 아이디와 패스워드를 물어본다면 git clone을 https로 해서 그렇다. 그럴때는 이렇게 하면 된다.

```bash
git remote set-url origin git@github.com:username/repo.git
```

## commit a file and ignore content changes

설정파일의 경우, gradle.propertes 라던가, 로컬에서 수정할 필요는 있으나, 변경사항을 업스트림으로 반영하면 안되는 경우가 있다. 이럴때는 .gitignore를 사용할 수도 없고 매번 수동으로 커밋되지 않게 했다가 커밋하고 나면 다시 수정을 해야 하니까 불편하다. 이럴때 아래 명령어를 사용하면 변경사항은 무시 된다.

from http://stackoverflow.com/questions/3319479/git-can-i-commit-a-file-and-ignore-the-content-changes

```bash
# <file> 변경사항 무시
git update-index --assume-unchanged <file>

# 원복할때
git update-index --no-assume-unchanged <file>
```

## upstream 복구 하기

실수로 upstream git을 지워버렸고, 로컬에는 포크된 git이 존재한다. 그렇다면 이상황에서 어떻게 복원할 수 있을까?
우선 같은 이름의 upstream git을 생성한후에 아래 명령어를 실행해주면 아주 쉽게 복원 가능

```bash
git push <upstream> --mirror
```

