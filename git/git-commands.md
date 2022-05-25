# git-commands-basic

## git 常用命令

`git init` 把目录转换为仓库 

> 通常有两种获取 Git 项目仓库的方式：
>
> 将尚未进行版本控制的本地目录转换为 Git 仓库；
>
> 从其它服务器 克隆 一个已存在的 Git 仓库。

`git add <name>` 添加想要commit的文件  `git add *` 添加所有修改过的文件

`git clone <url>`  克隆Git Repo  `git clone <url> <name>` 克隆并重命名

`git commit -m "MESSAGE"` 以MESSAGE为讯息commit文件

`git commit -a` 跳过add步骤，直接commit

`git commit --amend` 将这次的commit代替上一次的commit（修补）

`git status [-s]` 查看工作目录状态（modified/untracked/branch）`-s` for `--short`

`git diff` 查看修改过的与暂存的文件差异

`git diff --staged` 查看暂存的与最后一次commit的文件差异

`git rm [-f]` 从版本库和目录中删除文件 `-f` for `--force`

`git rm --cached <glob>`  保留文件，不继续跟踪

`git mv` 移动/重命名文件



`git log [-number] [-p] [--stat]` 查看日志

`[-number]`只显示n项 `--pretty=oneline --graph` 以ascii码展示，一行输出一个commit

`git show <commit>` 



`git reset [--hard] <commit>` 

**`<commit>` ：`HEAD`表示当前版本 `HEAD^`表示上个版本 `HEAD^^`上上个 `HEAD~n` 前n个版本**



`git checkout -- <file>` 撤销对文件的修改（到上一次提交的状态）

`git checkout -b <branch` 切换当前工作分支



`git remote [-v] [--show]` 查看远程仓库

`git remote add <name> <url>` 添加远程仓库URL地址

`git remote rename <original> <new>` 重命名远程仓库

`git remote remove <name> ` 移除远程仓库 



`git fetch <name/url>` 拉取所有本地没有的数据，但不自动合并

`git pull` 若本地分支跟踪了远程分支，可以拉取并自动合并

`git push` 推送commit到服务器



`git tag [-a] <tag> [<commit-hash>]`  创建标签（或在commit上创建标签

`git tag -d <tag>` 删除标签

`git push origin <tag> [--tags]` 推送标签

`git checkout <tag>` 查看标签



`git reflog` 查看历史命令

## .gitignore 的使用

使用标准 glob 模式匹配（即linux命令中使用的模式匹配，简化的正则表达式）

```gitignore
# *.c  // 注释
*.[osa]  // 忽略.o/.s/.a文件
!lib.a  // 忽略.a但是排除lib.a
/TODO  //　忽略
TODO/  // 忽略任何目录下名为TODO的文件夹
```

## git 分支

`git branch [-r]` 显示当前项目所有分支 `-r`：远程分支

`git branch <name>` 创建分支

`git checkout [-b] <name>` 切换分支

`git rev-parse <branch>` 查看分支SHA值 

### 分支合并

切换到主分支，用`git merge <name>`合并分支到当前分支。

分支合并冲突：

- 手动解决冲突

    去掉 `<<<<<<` / `=======` / `>>>>>>` 分割线再进行commit

- 放弃合并

    `git merge --abort` 放弃合并回滚改动

- mergetool

### 分支推送

`git push origin <branch>` 将branch推送到远程origin分支

`git push -u origin master` 当远程库为空时，第一次将master推送到origin加上-u参数可以将这两个分支联系起来，之后直接`git push`即可

`git clone`下来的库只能看到master分支，想要在别的分支上开发就要创建origin的其他分支：`git checkout -b <new> origin/<new>` 

`git pull` `git branch --set-upstream-to=origin/<branch> <branch>`

### 删除分支

`git branch -d <branch>` 删除分支

`git push origin --delete <branch>` 删除远程分支

### 重命名分支

`git branch -m <old> <new>`

```shell
git branch -m oldBranchName newBranchName   # 将本地的分支进行重命名
git push origin newBranchName               # 将新的分支推送到远程        
git push --delete origin oldBranchName      # 删除远程的旧的分支 
```

### 分支工作流

1. master分支应该是最稳定的，也就是仅用来发布新版本，平时不能直接在上面进行操作，应该保存在远程。

2. 短期分支是我们干活的分支，短期分支可以不用上传到远程，当我们完成了bug的修复，新功能的开发时才需要合并到主分支上。

3. 多使用分支来进行开发工作。

## 交互式暂存

`git add -i` 进入交互式暂存界面

`status`

`update` 暂存

`revert` 取消暂存

`diff` similar to `git diff --cached` 查看已暂存内容的区别

`patch` 补丁（暂存特定部分）

## 贮存与清理

`git stash` 想切换分支又不想放弃修改，将当前工作区入栈

`git stash pop`/`git stash apply [stash@{n}]` 恢复之前的工作区

`git stash list` 所有stash

## 清理工作目录

`git clean [-f] [-d] [--dry-run/-n] [-i]` 

删除所有未被跟踪的文件 `-f`强制 `-d`文件夹 `--dry-run/-n` 提示会删除什么 `-i` 交互式

默认不删除忽略文件

## 搜索

`git grep <pattern>`

`git log [-S/-L]`

## 子模块

maybe too advanced...

## TO BE ORGANIZED

工区（Working Directory）

就是你在电脑里能看到的目录

 

版本库（Repository）

工作区有一个隐藏目录.git，这个不算工作区，而是Git的版本库。

Git的版本库里存了很多东西，其中最重要的就是称为stage（或者叫index）的**暂存区**，还有Git为我们自动创建的第一个分支master，以及指向master的一个指针叫HEAD。

![git-repo](data:image/jpeg;base64,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)

 

第一步是用git add把文件添加进去，实际上就是把文件修改添加到暂存区；

第二步是用git commit提交更改，实际上就是把暂存区的所有内容提交到当前分支。

 

git checkout -- <file> / git restore <file>

一种是readme.txt自修改后还没有被放到暂存区，现在，撤销修改就回到和版本库一模一样的状态；

一种是readme.txt已经添加到暂存区后，又作了修改，现在，撤销修改就回到添加到暂存区后的状态。

 

git restore --staged <file>

如果已经提交到暂存区 用这个命令撤销到工作区

git restore <file>

将工作区里的修改撤销（文件意义上的）

命令git rm用于删除一个文件。如果一个文件已经被提交到版本库，那么你永远不用担心误删，**但是要小心，你只能恢复文件到最新版本，你会丢失最近一次提交后你修改的内容。**

 



由于远程库是空的，我们第一次推送master分支时，加上了-u参数，Git不但会把本地的master分支内容推送的远程新的master分支，还会把本地的master分支和远程的master分支关联起来，在以后的推送或者拉取时就可以简化命令。

从现在起，只要本地作了提交，就可以通过命令：

git push origin master

把本地master分支的最新修改推送至GitHub

 

删除远程库，可以用git remote rm <name>命令

git remote -v 查看远程库

 

git branch 查看分支

 

git merge <branch> 合并指定分支到当前分支



 

解决冲突

git log --graph --pretty=oneline --abbrev-commit 美观分支日志

合并分支时，加上--no-ff参数就可以用普通模式合并，合并后的历史有分支，能看出来曾经做过合并，而fast forward合并就看不出来曾经做过合并。

 

修复bug时，我们会通过创建新的bug分支进行修复，然后合并，最后删除；

当手头工作没有完成时，先把工作现场git stash一下，然后去修复bug，修复后，再git stash pop，回到工作现场；

在master分支上修复的bug，想要合并到当前dev分支，可以用git cherry-pick <commit>命令，把bug提交的修改“复制”到当前分支，避免重复劳动。

 

因此，多人协作的工作模式通常是这样：

1. 首先，可以试图用git     push origin <branch-name>推送自己的修改；
2. 如果推送失败，则因为远程分支比你的本地更新，需要先用git     pull试图合并
3. 如果合并有冲突，则解决冲突，并在本地提交
4. 没有冲突或者解决掉冲突后，再用git push origin     <branch-name>推送就能成功！

如果git pull提示no tracking information，则说明本地分支和远程分支的链接关系没有创建，用命令git branch --set-upstream-to <branch-name> origin/<branch-name>。

 

 

git rebase

rebase操作可以把本地未push的分叉提交历史整理成直线；

rebase的目的是使得我们在查看历史提交的变化时更容易，因为分叉的提交需要三方对比。