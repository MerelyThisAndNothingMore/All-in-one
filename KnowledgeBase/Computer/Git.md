#develop_util
# Introduction
# Best Practice
## clear branches
```bash
  git branch --merged |grep -v "\*" | grep -v "release" |xargs git branch -D

```
## ignore
在igonre文件中添加后出现不生效的问题，原因是已经被记录过，处理方案如下：
```bash
git rm -r --cached .
git add .
git commit 
```
参考：[gitignore不生效问题修复](https://blog.csdn.net/xuxu_123_/article/details/131710549)
