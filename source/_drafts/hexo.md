---
title: migrating to hexo
tags:
- hexo
---

원래 jekyll을 사용하고 있었는데 딱 하나 맘에 들지 않는것이 ruby를 사용한다는 것이었다. 그래서 최근에 [hexo](https://hexo.io/) 라는 것을 보게 되었는데 node로 되어 있어서 migration을 해보기로 했다.

## migration

우선 post는 jekyll과 hexo모두 markdown포맷을 사용하고 있기 때문에 `jekyll/_posts/`에 있는 *.md파일을 `hexo/source/_posts/`로 카피만 해주면 된다.

이미지는 링크는 `hexo/source/images/`하위에 카피한뒤 아래처럼 `images/<xx>`로 링크를 걸면 된다. 아래처럼

> `![my image](/images/image.png)`

마찬가지로 기타 파일들은 `hexo/source/assets/` 폴더를 만든 후 `assets/<xx>`로 링크를 걸면 된다.

##
