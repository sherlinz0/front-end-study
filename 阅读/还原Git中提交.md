<!--
 * @Author: sherlyzz
 * @Date: 2022-01-29
 * @LastEditTime: 2022-01-29
 * @LastEditors: sherlyzz
 * @Description: 部分还原 Git 中提交的方法
-->

# 部分还原 Git 中提交

恢复提交使用 `git revert` 命令, 这个命令将恢复整个提交.

- 如果提交粒度很大, 只需要恢复其中一部分, 这个办法不那么有效.
- 如果提交粒度很小, 这种方法很有效.
- 但实际情况并非都是小粒度的提交

来看下面这个例子:

![例子1](https://img-1305590520.cos.ap-shanghai.myqcloud.com/%E9%83%A8%E5%88%86%E8%BF%98%E5%8E%9FGit%E4%B8%AD%E6%8F%90%E4%BA%A4-%E4%BE%8B%E5%AD%901.png)

Git 仓库包含 3 个提交, 分别为 A, B, C. 提交 B 包含对 file1, file2, file3, file4 的更改. 提交 C 基于提交 B 并且已经推送到远程库.

现在, 如果想恢复对 file2 和 file4 提交 B, 不能简单的执行 `git revert B`. 但是如果可以将更改隔离到单个提交中, 可以使用 ` git revert`. 所以问题时如何将更改隔离为单个提交或将提交拆分.

由于提交 B 和 C 已经被推送, 不应该重写它们。

来看下列操作:

1. 签出要恢复的更改的提交

    `git checkout B`

    ![步骤1](https://link-intersystems.com/wp-content/uploads/2015/04/git-revert-partially-checkout-detached.png)

2. 将索引重置为上一次提交

    `git reset HEAD~`

    现在索引时上一次提交的状态, 也就是 A, 但是工作目录仍然包含提交 B 的文件. 这意味着 `git status` 命令将显示提交 B 为非暂存更改. 这意味着能够创建一个新的提交, 只包含我们想要在提交 C 之上恢复的更改.

3. 提交要还原的更改

    由于工作目录包含提交 B 的状态, 并且索引被重置为 A, `git status` 将显示提交 C 包含但为提交的更改. 


**参考**

[1] **How to partially revert a commit in git?** https://link-intersystems.com/blog/2015/04/19/how-to-partially-revert-a-commit-in-git/

[2] **Git恢复之前版本的两种方法reset、revert（图文详解）** https://blog.csdn.net/yxlshk/article/details/79944535

[3] **git 理解 HEAD^与HEAD~** https://blog.csdn.net/claroja/article/details/78858411
