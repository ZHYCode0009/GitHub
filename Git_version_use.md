
[toc]  
声明：本文摘录：廖雪峰的《Gig教程》。

# Git简介
## 安装Git


## 创建版本库
- 指令：mkdir；创建一个目录
- 指令：pwd；显示当前目录
- 指令：git init；初始化一个人Git仓库
- 指令：ls -ah；显示当前目录的文件
- 指令：git add < file >；把文件添加到仓库
- 指令：git commit - < message >；把文件提交到仓库

# 时光穿梭机
- 指令：git statusl；查看仓库的当前状态
- 指令：git diff；查看文件的difference，修改的内容
## 版本回退

## 工作区和暂存区

## 管理修改

## 撤销修改

## 删除修改

# 远程仓库

# 分支管理
## 创建与合并分支  
- git branch：查看当前分支  
命令会罗列所有的分支，并在当前分支前用“*”指出  
- git branch<name>：创建name分支  
- git checkout <name>：切换到name分支    

- git checkout -b <name>：创建并切换到name分支  
- git merge <name>：name合并到当前分支  
- git branch -d <name>：删除name分支      


## 解决冲突 
当我们在使用：git merge < name >合并分支时，有可能由于两个分支中都存在相应的修改，而出现冲突（conflict）,此时我们需要手动修改文件在提交。  
可以使用带参数的git log查看分支的合并情况：
git log --graph --pretty=oneline --abbrev-commit。
## 分支管理策略  
之前合并分支使用：git merge < name >，Git会使用：Fast forward模式，在这种模式中，删除分支后，会丢掉分支信息。我们可以将Fast foward模式禁用，Git就会在merge时生成一个新的commit，就可以从历史中看出分支信息。  
- git merge --no-ff -m"merge with no-ff" <name>  
参数：--no-ff，表示禁用Fast forward  
在实际开发中，应该按照几个基本原则进行分支管理：  
1. master分支应该是非常稳定，也就是仅用来发布新版本，不在上面干活；  
2. 在分支dev中干活，他是一个不稳定的分支，每个人在dev分支中创建自己的分支。然后，在工作完成之后合并到DEV分支中，最后才合并到master。  

##  Bug分支 
在软件设计在修复BUG中，可以通过一个新的临时分支来修复，修复后合并分支，然后删除临时分支。  
但是在工作进行到一半时，还没做完，无法提交，此时可以使用stash功能，他可以把现场储藏起来，等以后恢复现场后继续工作。  
- git stash：保存工作现场  
- git stash lsit：查看工作现场的位置  
- git stash apply：恢复现场，但不会删除stash内容
- git stash drop：删除stash内容  
- git stash pop：恢复现场的同事删除stash内容  

## Feature分支  
添加新功能时，不想因为一些实验性质的代码，将主分支搞乱。可以将每个新功能都创建新的feature分支，当功能完善后再合并及删除。  
- git branch -D <name>：强制删除未合并的分支  

## 多人协作  
当从远程仓库克隆时，实际上Git自动把本地的master分支和远程的master分支对应了，并且远程仓库的默认名称是origin。  
- git remote：查看远程库的信息  
- git remote -v：显示更详细远程库的信息  
系统会回复可以抓取和推送的origin的地址，如果没有推送权限则看不到push的地址。  

### 推送分支
- git push origin master：把master分支推送到远程仓库origin  
git push origin dev；把dev分支推送到origin远程仓库。  
- 需要推送的分支  
    1.master分支时主分支，要时刻与远程同步  
    2.dev：开发分支，团队所有成员都需要在上面工作，需要与远程同步  
    3.bug：只用于本地修复BUG,没必要推送到远程  
    4.feature：功能分支，推送与否取决于和你的小伙伴在上面开发    

### 抓取分支  
多人协作时，大家都会往master与dev分支推送各自的修改。  
可以通过在同一台电脑的另外一个目录下克隆：
```
git clone git@github.com:manmanluxi/learngit.git
```  
当你的小伙伴从远程仓库clone时，默认情况下，小伙伴只能看到本地的master分支，使用：git branch查看。  
- git checkout -b dev origin/dev：创建远程orign的dev分支到本地  
如果你们来两个人要在dev分支上开发，就必须创建远程仓库dev分支push到远程仓库。  
- git pull：从远程抓取分支  
当你的小伙伴已经向origin/dev分支推送了他的提交，而你也碰巧也对同样的文件进行了修改，并试图推送。推送会失败，因为你们的推送有冲突，我们可以先用git pull把最新的提交从origin/dev分支抓下来，然后再本地合并，解决冲突之后在推送。  

### 总结  
多人协作的工作模式：
- 可以先试图用git pushorigin <branch-name>推送自己的个修改  
- 如果推送失败，则因为远程分支比你的本地更新，需要先用git pull试图合并；  
- 如果合并有冲突，则解决冲突，并在本地提交；
- 没有冲突或者解决冲突后，再用git push origin <brabch-name>推送  
如果git pull提示no tracking information，则说明本地分支和远程分支的链接关系没有创建，用命令：git branch --set-upstream-to <branch-name> origin/<brabch-name>。  

## Rebase  
变基：把分叉的提交历史“整理”成一条直线，看上去更直观，缺点：本地的分叉提交已经被修改过。
# 标签管理  
标签(tag)便于我们查找对应的版本。
## 创建标签  
- git tag <tag>：创建一个新的标签  
- git tag：查看所有的标签
- git tag <name> commit-id：在指定的commit id位置打标签  
- git show <tagname>：查看标签信息  
- -a：指定标签名  
- -m：指定说明文字  

## 操作标签  
创建的标签都只存储在本地，不会自动推送到远程，所以打错的标签在本地安全删除。  
- git tag -d <tagname>：删除标签  
- git push origin <tagname>：推送到远程  
- git push origin --tage：一次性推送全部未推送到远程的本地标签  
- git push origin :refs/tags/<tagname>：删除一个远程标签  
在删除远程标签之前，先删除本地的标签  

# 使用GitHub  
# 使用码云  
创建账号。。。。
- git remote add origin git@gitee.com:<gitee-id>/<仓库名字>：将本地仓库和码云上的仓库关联  
如果已经关联了其他的远程仓库，将无法关联。  
- git remote -v：查看远程库信息  
- git remote rm <origin-name>：删除已有的远程库  
- git remote add github git@github.com:manmanluxi/learngit.git：先关联GitHub的远程库，库名字：github  
- git remote add gitee git@gitee.com:manmanluxi/learngit.git：先关联GitHub的远程库，库名字：gitee  
如此，可以将本地库同时与多个远程库同步。  

# 自定义Git  
- git config --global color.ui true：配置随机颜色  

## 忽略特殊文件
- 

# 问题汇总  
### fatal: remote origin already exists  
我在将本地的仓库与码云上的仓库进行关联时提示的错误提示。  
- 解决方案：  
    1. 先输入：git remote rm origin  
    2. 接着输入：git remote add origin git@gitee.com:manmanluxi/learngit.git  
    3. 如果第一步输入还是错误，并提示：error: Could not remove config section 'remote.origin' 我们需要修改gitconfig文件的内容
    4. 找到你的github的安装路径，我的是C:\Users\ASUS\AppData\Local\GitHub\PortableGit_ca477551eeb4aea0e4ae9fcd3358bd96720bb5c8\etc  
    5. 找到一个名为gitconfig的文件，打开它把里面的[remote "origin"]那一行删掉就好  
参考：[git常见错误](https://blog.csdn.net/dengjianqiang2011/article/details/9260435)



# 帮助
## git --help
```
usage: git [--version] [--help] [-C <path>] [-c <name>=<value>]
           [--exec-path[=<path>]] [--html-path] [--man-path] [--info-path]
           [-p | --paginate | -P | --no-pager] [--no-replace-objects] [--bare]
           [--git-dir=<path>] [--work-tree=<path>] [--namespace=<name>]
           <command> [<args>]

These are common Git commands used in various situations:

start a working area (see also: git help tutorial)
   clone      Clone a repository into a new directory
   init       Create an empty Git repository or reinitialize an existing one

work on the current change (see also: git help everyday)
   add        Add file contents to the index
   mv         Move or rename a file, a directory, or a symlink
   reset      Reset current HEAD to the specified state
   rm         Remove files from the working tree and from the index

examine the history and state (see also: git help revisions)
   bisect     Use binary search to find the commit that introduced a bug
   grep       Print lines matching a pattern
   log        Show commit logs
   show       Show various types of objects
   status     Show the working tree status

grow, mark and tweak your common history
   branch     List, create, or delete branches
   checkout   Switch branches or restore working tree files
   commit     Record changes to the repository
   diff       Show changes between commits, commit and working tree, etc
   merge      Join two or more development histories together
   rebase     Reapply commits on top of another base tip
   tag        Create, list, delete or verify a tag object signed with GPG

collaborate (see also: git help workflows)
   fetch      Download objects and refs from another repository
   pull       Fetch from and integrate with another repository or a local branch
   push       Update remote refs along with associated objects

'git help -a' and 'git help -g' list available subcommands and some
concept guides. See 'git help <command>' or 'git help <concept>'
to read about a specific subcommand or concept.

           
```

- start a working area (see also: git help tutorial)  
1. clone----Clone a repository into a new directory  
2. init----Create an empty Git repository or reinitialize an existing one  

- work on the current change (see also: git help everyday)  
1. add----Add file contents to the index  
2. mv----Move or rename a file, a directory, or a symlink  
3. reset----Reset current HEAD to the specified state  
4. rm--------Remove files from the working tree and from the index  

- examine the history and state (see also: git help revisions)
1. bisect     Use binary search to find the commit that introduced a bug
2. grep       Print lines matching a pattern
3. log        Show commit logs
4. show       Show various types of objects
5. status     Show the working tree status

- grow, mark and tweak your common history
1. branch     List, create, or delete branches
2. checkout   Switch branches or restore working tree files
3. commit     Record changes to the repository
4. diff       Show changes between commits, commit and working tree, etc
5. merge      Join two or more development histories together
6. rebase     Reapply commits on top of another base tip
7. tag        Create, list, delete or verify a tag object signed with GPG

- collaborate (see also: git help workflows)
1. fetch      Download objects and refs from another repository
2. pull       Fetch from and integrate with another repository or a local branch
3. push       Update remote refs along with associated objects  
## git checkout -help  
```
usage: git checkout [<options>] <branch>
   or: git checkout [<options>] [<branch>] -- <file>...

    -q, --quiet           suppress progress reporting
    -b <branch>           create and checkout a new branch
    -B <branch>           create/reset and checkout a branch
    -l                    create reflog for new branch
    --detach              detach HEAD at named commit
    -t, --track           set upstream info for new branch
    --orphan <new-branch>
                          new unparented branch
    -2, --ours            checkout our version for unmerged files
    -3, --theirs          checkout their version for unmerged files
    -f, --force           force checkout (throw away local modifications)
    -m, --merge           perform a 3-way merge with the new branch
    --overwrite-ignore    update ignored files (default)
    --conflict <style>    conflict style (merge or diff3)
    -p, --patch           select hunks interactively
    --ignore-skip-worktree-bits
                          do not limit pathspecs to sparse entries only
    --ignore-other-worktrees
                          do not check if another worktree is holding the given ref
    --recurse-submodules[=<checkout>]
                          control recursive updating of submodules
    --progress            force progress reporting

```
1. -q, --quiet  
suppress progress reporting  
2. -b <branch>  
create and checkout a new branch  
3. -B <branch>  
create/reset and checkout a branch  
4. -l  
create reflog for new branch  
5. --detach  
detach HEAD at named commit  
6. -t, --track  
set upstream info for new branch  
7.  --orphan <new-branch>  
new unparented branch  
8. -2, --ours  
checkout our version for unmerged files  
9. -3, --theirs  
checkout their version for unmerged files  
10. -f, --force  
force checkout (throw away local modifications)  
11. -m, --merge  
perform a 3-way merge with the new branch  
12. --overwrite-ignore  
update ignored files (default)  
13. --conflict <style>    
conflict style (merge or diff3)  
14. -p, --patch  
select hunks interactively  
15. --ignore-skip-worktree-bits  
do not limit pathspecs to sparse entries only  
16. --ignore-other-worktrees  
do not check if another worktree is holding the given ref  
17. --recurse-submodules[=<checkout>]  
control recursive updating of submodules  
18. --progress  
force progress reporting  



# 标签管理

# 使用GitHub

# 使用码云

# 自定义Git

# 总结



参考资料：
[廖雪峰的《Gig教程》](https://www.liaoxuefeng.com/wiki/0013739516305929606dd18361248578c67b8067c8c017b000)  
