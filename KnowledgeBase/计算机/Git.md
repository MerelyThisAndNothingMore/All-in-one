---
tags: 
alias:
---



## ignore
在igonre文件中添加后出现不生效的问题，原因是已经被记录过，处理方案如下：
```bash
git rm -r --cached .
git add .
git commit 
```
参考：[gitignore不生效问题修复](https://blog.csdn.net/xuxu_123_/article/details/131710549)


# Git 的数据模型

## 快照

Git 将顶级目录中的文件和文件夹作为集合，并通过一系列快照来管理其历史记录。
在Git的术语里，文件被称作Blob对象（数据对象），也就是一组数据。
目录则被称之为“树”，它将名字与 Blob 对象或树对象进行映射（使得目录中可以包含其他目录）。
快照则是被追踪的最顶层的树。例如，一个树看起来可能是这样的：

```
<root> (tree)
|
+- foo (tree)
|  |
|  + bar.txt (blob, contents = "hello world")
|
+- baz.txt (blob, contents = "git is wonderful")
```

这个顶层的树包含了两个元素，一个名为 “foo” 的树（它本身包含了一个blob对象 “bar.txt”），以及一个 blob 对象 “baz.txt”。

## 历史记录建模：关联快照

在 Git 中，历史记录是一个由快照组成的有向无环图。

Git 中的每个快照都有一系列的“父辈”，也就是其之前的一系列快照。注意，快照具有多个“父辈”而非一个，因为某个快照可能由多个父辈而来。例如，经过合并后的两条分支。

在 Git 中，这些快照被称为“提交”。

## 数据模型及其伪代码表示

以伪代码的形式来学习 Git 的数据模型，可能更加清晰：

```
// 文件就是一组数据
type blob = array<byte>

// 一个包含文件和目录的目录
type tree = map<string, tree | blob>

// 每个提交都包含一个父辈，元数据和顶层树
type commit = struct {
    parent: array<commit>
    author: string
    message: string
    snapshot: tree
}
```

这是一种简洁的历史模型。

## 对象和内存寻址

Git 中的对象可以是 blob、树或提交：

```
type object = blob | tree | commit
```

Git 在储存数据时，所有的对象都会基于它们的 [SHA-1 哈希](https://en.wikipedia.org/wiki/SHA-1) 进行寻址。

```
objects = map<string, object>

def store(object):
    id = sha1(object)
    objects[id] = object

def load(id):
    return objects[id]
```

Blobs、树和提交都一样，它们都是对象。当它们引用其他对象时，它们并没有真正的在硬盘上保存这些对象，而是仅仅保存了它们的哈希值作为引用。