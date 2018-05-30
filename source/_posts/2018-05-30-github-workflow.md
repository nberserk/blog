---
title: github workflow
tags:
  - github
date: 2018-05-30 18:35:41
---


얼마전 [gerrit workflow](/2018/05/29/gerrit-workflow)에 이어서 이번에는 github의 workflow를 정리해 본다. github는 거의 pull request(이하 PR)이 전부다. 요즘 거의 모든 github를 사용하는 프로젝트들은 대개 이런 flow로 개발이 되기 때문에 처음에 조금 복잡해 보일지라도 익혀 두면 도움이 될 것이다.

## 준비

우선 설명을 쉽게 하기 위해서 아파치 오픈소스 프로젝트인 [Zeppelin](https://github.com/apache/zeppelin)을 기준으로 정리한다.

내가 이 Zeppelin프로젝트의 개발에 참여하기 위해서는 우선 이 프로젝트를 `fork`해야 한다. fork를 하게 되면 내 github의 계정에 fork된 repository가 생기게 된다. 나의 경우는 아래처럼 만들어졌다.

> https://github.com/nberserk/zeppelin

이렇게 포크된 git repository를 내 로컬에 클론한다.

> git clone git@github.com:nberserk/zeppelin.git

그리고 나중에 PR을 위해서 upstream 이라는 remote를 추가해준다.

> git remote add upstream git@github.com:apache/zeppelin.git

따라서 origin은 nberserk/zepplein.git 이고 upstream은 apache/zeppelin.git이 된다.

## PR

이제 PR을 날려보자. 우선 master branch로 변경하고 upstream의 최신 사항을 origin에 반영해 준다.

```bash
git checkout master
git fetch upstream # update apache remote
git rebase upstream/master # rebase upstream/master to current branch(master)
# make tracking branch 이렇게하면 upstream은 apache master와 싱크를 하게 되고, 로컬에 unpushed commit이 생기게 된다.
git push # git push로 origin의 git에 최신사항으로 업데이트 함.
```

우선 토픽 브랜치 만들고
> git checkout -b topic

변경사항을 커밋하고
> git add .
> git commit -m "desc"

토픽 브랜치를 origin으로 push

> git push -u origin topic

이 상태에서 github의 nberserk/zeppelin으로 가서 새 PR을 만들면 된다.

그러면 upstream의 maintainer가 리뷰하고 의견을 남겨주고, 몇번의 피드백이 오고간후 머지할 준비가 되면 머지가 된다. 그러면 업스트림에 나의 코드가 반영이 된 것이다.

## conflict이 난 경우

PR중간에 conflict changes가 upstream에 커밋이 되었다면 PR에 conflict이 나기 때문에 머지가 불가능해 진다. 이럴때는 upstream을 토픽 브랜치로 머지를 해야 한다.

우선 upstream을 가져오고
> git fetch upstream

topic 브랜치로 머지 한다.
> git checkout topic
> git merge upstream/master

conflict나는 부분을 해결한후 커밋을 하고 PR review를 다시 요청하면 된다.

