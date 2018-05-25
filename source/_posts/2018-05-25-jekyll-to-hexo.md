---
title: Jekyll to hexo
tags:
  - hexo
date: 2018-05-25 12:08:28
---


원래 jekyll을 사용하고 있었는데 딱 하나 맘에 들지 않는것이 ruby를 사용한다는 것이었다. 그래서 최근에 [hexo](https://hexo.io/) 라는 것을 보게 되었는데 node로 되어 있어서 migration을 해보기로 했다.

## post

우선 post는 jekyll과 hexo모두 markdown포맷을 사용하고 있기 때문에 마크다운 포맷을 복사하는 수준에서 마이그레이션이 된다. `jekyll/_posts/`에 있는 *.md파일을 `hexo/source/_posts/`로 카피만 해주면 된다.

asset들은  `jekyll/assets/`하위 파일들을 `hexo/source/images/`로 복사를 하고. 아래처럼 링크를 걸면 된다.

> `![my image](/images/my-image.png)`

## layout

레이아웃은 jekyll과 비슷한 템플릿 방식으로 되어 있어서 비교적 기계적으로 적용하면 된다. 나의 경우는 disqus comment를 적용했다. root의 _config.yml에 `disqus_shortname`을 선언하여 변수를 채워준 다음 `layout/_partial/article.ejs`의 끝에 아래 내용을 채워주면 된다.

```js
<% if (!index && page.comments && config.disqus_shortname){ %>
<section id="comments">
  <div id="disqus_thread"></div>
  <script type="text/javascript">
    /* * * DON'T EDIT BELOW THIS LINE * * */
    (function () {
      var dsq = document.createElement('script'); dsq.type = 'text/javascript'; dsq.async = true;
      dsq.src = '//' + '<%= config.disqus_shortname %>'  + '.disqus.com/embed.js';
      (document.getElementsByTagName('head')[0] || document.getElementsByTagName('body')[0]).appendChild(dsq);
    })();
  </script>
  <noscript>Please enable JavaScript to view the
    <a href="//disqus.com/?ref_noscript">comments powered by Disqus.</a>
  </noscript>
</section>
<% } %>
```


## theme


