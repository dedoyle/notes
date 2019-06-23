# git 使用笔记

## 优势

    * 快
    * 完全的分布式
    * 轻量级的分支操作
    * Git 已经成为现实意义上的标准
    * 社区成熟活跃

## 命令

  git help

  git config --global user.name ""
  git config --global user.email ""

    * --local [默认，高优先级] 只影响本仓库 .git/config
    * --global [中优先级] 影响所有当前用户的 git 仓库 ~/.gitconfig
    * --system [低优先级] 影响全系统的 git 仓库 /etc/gitconfig

  git init

  git status
    * 未跟踪 <-> 跟踪
    * 工作目录 <-> 暂存区
    * 暂存区 <-> 最新提交

  git add 添加文件到暂存区

  .gitignore
    * 在添加时忽略匹配的文件
    * 仅作用于未追踪的文件

  git rm 从暂存区删除
    * git rm --cached 仅从暂存区删除
    * git rm 从暂存区与工作目录删除
    * git rm $(git ls-file --deleted) 删除所有被追踪，但是在工作目录被删除的文件

  git commit 根据暂存区内容创建一个提交记录

  git commit -a 等同于 git add 和 git commit

  git log

  git config alias.shortname `<fullCommand>`

  git diff
    * 工作目录与暂存区的差异
  git diff --cached [`<reference>`]
    * 暂存区与某次提交差异，默认为 HEAD
  git diff `<reference>`
    * 工作目录与某次提交的差异

  git checkout -- `<file>`
    * 将文件内容从暂存区复制到工作目录

  git checkout HEAD -- `<file>`
    * 将内容从提交区复制到工作目录

  git reset HEAD `<file>`
    * 将文件内容从提交区复制到暂存区

  ![命令流程图](/./git_commands.png)

## 分支操作

  git branch `<branchname>` 创建分支
  git branch -d `<branchname>` 删除分支
  git branch -v 显示分支

  git commit 命令后，会生成一个对象，有一个引用指向前一个提交。默认会有一个 master 分支，会指向当前最新的提交。HEAD 默认和 master 指向同个位置。

  git branch next 会在 HEAD 指向的对象上创建一个引用，创建了一个 next 分支

  git checkout 通过移动 HEAD 检出版本，可用于分支切换

    git checkout `<branchname>` 切换分支
    git checkout -b `<branchname>` 创建一个分支，然后切换到该分支
    git checkout `<reference>` 切换到指定引用上
    git checkout - 切换回上一个分支

    HEAD 和 master 分开时，最好不要提交操作，只是读取内容就行了

  git reset 将分支回退到某个历史版本
    * git reset --mixed `<commit>` 默认
    * git reset --soft `<commit>` // 复制到工作目录
    * git reset --hard `<commit>` // 复制到暂存区

  git reflog 回退指针后，新的提交就没用了，在 log 看不到，只能用 reflog 查看

  回退版本的捷径
    * A^: A 上的父提交
    * A~n: 在 A 之前的第 n 次提交
    master^  === master~1

  ![reset vs checkout](/./reset_vs_checkout.png)

  git checkout next

  git stash 保存目前工作目录和暂存区状态，并返回到干净的工作空间
    git stash save 'push to stash area'
    git status
    git stash list
    git stash apply stash@{0} // 从 stash 区恢复
    git stash drop stash@{0} // 删除 stash 记录
    git stash pop === stash apply + stash drop

  git merge 合并分支
    git cat-file -p HEAD 可以查看某个节点的指向信息

    解决冲突
      git merge next master
      git status 

    git fast-forward
      git merge next
      git merge next --no-ff // 不要 fast forward

  git rebase 修剪提交历史基线，俗称“变基”

  git remote -v

  git push

  git fetch + merge 可以用来合并冲突

  git pull === git fetch + git merge

  git clone
