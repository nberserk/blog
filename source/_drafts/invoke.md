---
title: invoke
tags: invoke
---

rake 같은 것을 python세계에서 fabric을 사용하고 있었는데, fabric이 invoke로 이름이 바뀌었다. 앞으로 이것으로 사용해 봐야겠다.

설치는 아래처럼 하면 되고

> pip install invoke

```python
from invoke import task

@task
def co(c, branch):
    c.run("git checkout -b {}".format(branch))
```