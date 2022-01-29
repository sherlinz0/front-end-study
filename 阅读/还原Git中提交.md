<!--
 * @Author: sherlyzz
 * @Date: 2022-01-29
 * @LastEditTime: 2022-01-29
 * @LastEditors: sherlyzz
 * @Description: 介绍还原 Git 中提交的方法
-->

# 还原 Git 中提交

**概览:**

![概览](https://img-1305590520.cos.ap-shanghai.myqcloud.com/%E9%83%A8%E5%88%86%E8%BF%98%E5%8E%9FGit%E4%B8%AD%E6%8F%90%E4%BA%A4-%E6%A6%82%E8%A7%88.png)

目前遇到了三种情况下的提交还原, 其他情况待更新...

## 部分还原

场景: 提交粒度很大, 只需要恢复其中一部分.


来看下面这个例子:

![例子1](https://img-1305590520.cos.ap-shanghai.myqcloud.com/%E9%83%A8%E5%88%86%E8%BF%98%E5%8E%9FGit%E4%B8%AD%E6%8F%90%E4%BA%A4-%E4%BE%8B%E5%AD%901.png)

Git 仓库包含 3 个提交, 分别为 A, B, C. 提交 B 包含对 file1, file2, file3, file4 的更改. 提交 C 基于提交 B 并且已经推送到远程库.

现在, 恢复对 file2 和 file4 的更改.

来看下列操作:

1. ### 签出要恢复的更改的提交

    `git checkout B`

    ![步骤1](https://link-intersystems.com/wp-content/uploads/2015/04/git-revert-partially-checkout-detached.png)

2. ### 将索引重置为上一次提交

    `git reset HEAD~`

    现在索引时上一次提交的状态, 也就是 A, 但是工作目录仍然包含提交 B 的文件. 这意味着 `git status` 命令将显示提交 B 为非暂存更改. 这意味着能够创建一个新的提交, 只包含我们想要在提交 C 之上恢复的更改.

3. ### 提交要还原的更改

    由于工作目录包含提交 B 的状态, 并且索引被重置为 A, `git status` 将显示提交 B 中的文件更改.

   ```bash
   $ git status
   HEAD detached from 74b388e
   Changes not staged for commit:
   (use "git add <file>..." to update what will be committed)
   (use "git checkout -- <file>..." to discard changes in working directory)
   
           modified:   file1
           modified:   someDir/file2
           modified:   someDir/file3
           modified:   someDir/someOtherDir/file4
   
   no changes added to commit (use "git add" and/or "git commit -a")
   ```

   现在来创建一个新的提交

   ```bash
   $ git add someDir/file2 someDir/someOtherDir/file4
   $ git commit -m 'Changes to file2 and file4'
   [detached HEAD 823bd88] Changes to file2 and file4
   2 file changed, 5 insertions(+), 2 deletion(-)
   ```

4. ### 用 `master` 分支覆盖 `HEAD` 指针指向的 `游离分支`

   `git merge master`

5. ### 撤销 `第三步` 的 `新提交`

   `git revert [commit]`

6. ### 强制移动 `master` 位置至 `HEAD` 处

   `git branch master -f HEAD`

7. 检出 `master`

   `git checkout master`

   至此, 还原完成, 过程参考 [git-learn](https://github.com/Pony-Zhang/git-learn) `commit: f3524fc8425f16dc6e830eaf5dadb639b404e3b8` - `commit: 1aefec7325b1f5953246551e6ebd1d099e6abb00` 

## 整个还原

当提交粒度较小时, 要提交中仅有要还原的变更, 这时使用 `git revert [commit]` 命令即可.

## 直接操作提交信息

我们可以使用 `git rebase [commit]` 命令来直接操作提交信息

1. ### `git log` 查看提交记录

2. ### 记录要进行修改的提交的前一个提交哈希码

   注意, 可修改的列表不包括该提交

3. ### `git rebase -i [commit]`
   
   此处 `commit` 为目标提交的前一次提交

4. ### 修改提交列表

   如果想操作哪个提交, 就将该提交前的选项进行修改:

   - pick (p): 表明正在使用
   - reword (r): 表明仍然使用该提交对象, 但是需要修改提交信息
   - edit (e): 使用该提交对象, 但是不合并提交对象
   - squash (s): 使用该提交对象, 但是将此提交与上一次提交对象合并
   - fixup (f): 同 squash 值, 但是丢失此次提交的日志信息
   - exec (x): 后接特定脚本, 保存后将执行该脚本
   - drop (d): 移除该提交对象

5. ### 修改提交信息

   `git commit --amend` 显示提交信息, 可以进行修改, 不仅可以修改这里的信息, 还可以修改提交的文件.

6. ### `git rebase --continue`

**参考**

[1] **How to partially revert a commit in git?** https://link-intersystems.com/blog/2015/04/19/how-to-partially-revert-a-commit-in-git/

[2] **Git恢复之前版本的两种方法reset、revert（图文详解）** https://blog.csdn.net/yxlshk/article/details/79944535

[3] **git 理解 HEAD^与HEAD~** https://blog.csdn.net/claroja/article/details/78858411

[4] **git修改历史提交(commit)信息（超详细，图文并茂）** https://blog.csdn.net/qq_17011423/article/details/104648075
