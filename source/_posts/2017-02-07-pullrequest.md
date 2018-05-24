---
layout: post
title:  "pull request"
tags: github pr
published: false
---

github 사용할때 내가 반영한 변경사항을 업스트림으로 보내고 싶을때, 이런 동작을 pull request, 즉 PR 이라고 한다. PR시 유용한 것들을 정리해 본다.

## PR 중간에 rebase해야 할 경우

기본적은 workflow는 [git basic]({% post_url 2015-06-30-using-github %})에서 설명을 했고, 문제가 되는 경우는 PR을 하려고 업스트림을 rebase하여 PR을 올렸고, PR이 길어져서 conflict changes가 있어 다시 한번 rebase 를 해야 할 경우이다. 이런 경우 rebase 커밋을 하면 중간에 업스트림의 커밋이 들어오는데 커밋 그래프가 이쁘지가 않다.

이럴 경우 `git push -u origin -f <topic>` 이렇게 하면 강제로 origin의 커밋 히스토리를 바꾼다.



```
	c1 <- c2 <- c5 <- c6     <upstream>           : upstream
           |- c3 <- c4 <topic>

```
