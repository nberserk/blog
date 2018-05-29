---
title: gerrit-workflow
date: 2018-05-29 14:43:58
tags:
- gerrit
- git
---

gerrit을 사용할때의 workflow를 정리해 본다.

## 준비

아래처럼 git hook을 설정.

> scp -p -P 29418 darren.ha@<git>:hooks/commit-msg <local git dir>/.git/hooks/

이것은 git commit message에 아래와 같이 change-id를 삽입하게 되는데, 이 change-id가 리뷰의 단위가 된다.

```
commit af6e0970c05cde2f3a40f1b89374360aaf3bb972
Author: Darren Ha <>
Date:   Tue May 29 17:22:00 2018 +0900

    my changes

    Change-Id: I6af3efd2b39707bcd0ec9b7815fe8ba656858456
```

## 변경사항 반영 및 리뷰 요청



```bash
git checkout -b topic
# submit some changes
git commit -m "my change"
git push origin HEAD:refs/for/master  # request gerrit review
```

## 수정

위 상황에서 리뷰를 통해 더 수정해야 할 사항이 생기면  `git commit --amend` 를 통해서 수정해서 리뷰를 계속한다.

## 마무리

모든 리뷰가 끝나고 이제 반영해야 된다고 하면 gerrit에서 merge를 하고 master 브랜치로 돌아와서 origin과 싱크를 맞추면 된다.

```bash
git checkout master
git pull --rebase
git branch -d topic # (optional)
```


## reference

- https://tech.evojam.com/2016/05/19/gerrit-based-workflow-complete-developers-walkthrough/

