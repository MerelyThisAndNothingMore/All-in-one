#develop_util
# Introduction
# Best Practice
## clear branches
```bash
  git branch --merged |grep -v "\*" | grep -v "release" |xargs git branch -D

```

