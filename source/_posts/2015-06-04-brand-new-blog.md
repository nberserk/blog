---
layout: post
title:  "Jekyll + github = brand new blog"
tags: jekyll github blog
---


개발자들의 친구 github와 jekyll 이 만나니 이 홈페이지가 탄생했도다.
이를 기념하기 위한 포스트.

이제 알찬 컨텐츠를 넣는 일만 남았다.


여러가지 링크 예제

```
{% raw %}
[bash script]({% post_url 2015-06-11-hadoop-intro %})
![My helpful screenshot]({{ site.url }}/assets/screenshot.jpg)
{{ site.baseurl }}{% post_url 2010-07-21-name-of-post %}
{% endraw %}
```

gist 는 아래처럼 삽입할 수 있음.

```
{% raw %}
{% gist parkr/c08ee0f2726fd0e3909d %}
{% endraw %}
```

escape는 raw/endraw block으로 감싸주면 된다.

```
{{ "{% raw" }} %}

{{ "{% endraw"}} %}
```

References:

- 지원하는 code highlight 언어 리스트 : [pygment 홈페이지](http://pygments.org/languages/)
- markdown formatting : [rendered page](http://demo.getpoole.com/page2/) , [raw page](https://raw.githubusercontent.com/poole/poole/master/_posts/2014-01-01-example-content.md)





