---
title: git常见命令
categories: 工具
tags: git 工具
---

1、创建分支的两种命令

- `git checkout -b test`
- `git branch test`

区别：

- `git checkout -b test`相当于两条命令：

  - `git branch test`
  - `git checkout test`

  所以这条命令的作用是：创建并切换分支

`git checkout -b test`和`git checkout -B test`区别？

- 如果当前仓库中，已经存在一个跟你新建分支同名的分支，那么使用普通的`git checkout -b test`这个命令，是会报错的，且同名分支无法创建。
- 如果使用`-B`参数，那么就可以强制创建新的分支，并会覆盖掉原来的分支

<!-- more -->

2、删除分支`git branch -b test`和`git branch -D test`区别？

- 如果删除的分支还没有被 `merge` 到其他分支，删除这样的分支会导致这个分支上所做的改动丢失，因此 `git branch -d` 命令会失败，提示你这样做会丢失信息。该分支必须完全和它的上游分支`merge`完成
- 如果你的确想删除这样的分支，不怕信息丢失，那么可以使用` git branch -D `命令，这个命令不会去判断分支的`merge`状态


3、合并分支 `git merge`的三种状态

- 默认的`--ff` ， 即 `fast-forward` 方式。`Git` 合并两个分支时，如果顺着一个分支走下去可以到达另一个分支的话，那么 `Git` 在合并两者时，只会简单地把指针右移，叫做“快进”`（fast-forward）`不过这种情况如果删除分支，则会丢失`merge`分支信息。

- `--no-ff`标志。将导致创建一个新的`commit`标志，即使合并可以使用`fast-forward`模式。这可以避免丢失功能分支的历史信息并将新特性和所有的提交合并到一起

- `—squash`标志。把一些不必要`commit`进行压缩，比如说，你的`feature`在开发的时候写的`commit`很乱，那么我们合并的时候不希望把这些历史`commit`带过来，于是使用`--squash`进行合并，此时文件已经同合并后一样了，但不移动`HEAD`，不提交。需要进行一次额外的`commit`来“总结”一下，然后完成最终的合并

  ![1-1](1-1.png)


4、删除远程分支两个命令

- `git push origin :test`
- `git push origin --delete test`

两者本质一样，推送本地空分支到远程，相当于远端分支被置为空，从而删除指定的远端分支。


5、`git fetch`和`git pull`的区别

- `git pull = git fetch + git merge `。`git fetch`是将远程主机的最新内容拉到本地，用户在检查了以后决定是否合并到工作本机分支中。`git pull` 则是将远程主机的最新内容拉下来后直接合并


6、`git fetch -p`作用

- 清理本地无效分支(远程已删除本地没删除的分支）


7、`git reset`和`git revert` 

- `git reset`
  - 回退到指定的`commit`版本，指定`commit`版本之后的`commit`都将被重置
- `git revert`
  - 撤销指定`commit`版本的操作，这个操作也会生成一个新`commit`，指定`commit`版本之前及之后的操作均不受影响
  - 支持同时撤销连续的几个`commit`的修改  `git revert (commit_older..commit_newer]`
- `git revert `和 `git reset`的区别
  - `git revert`是用一次新的`commit`来回滚之前的`commit`，`git reset`是直接删除指定的`commit`
  - `git reset` 是把`HEAD`向后移动了一下，而`git revert`是`HEAD`继续前进，只是新的`commit`的内容和要`revert`的内容正好相反，能够抵消要被`revert`的内容


8、同步远程分支

- `git checkout -b 本地分支名 origin/远程分支名` 拉取远程分支并同时创建对应的本地分支