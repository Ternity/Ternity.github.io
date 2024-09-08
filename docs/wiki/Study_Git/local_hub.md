---
draft: true 
date: 2024-09-08 
categories:
  - AAA
  - BBB
---
# 学习如何使用git
<div style="color:white; background-color:#354538; border: 1px solid #FFE0C3; border-radius: 10px; margin-bottom:0rem">
    <p style="margin:1rem; padding-left: 1rem; line-height: 2.5;">
        ©️ <b><i>Copyright 2024 @ Authors</i></b><br/>
        <i>Author：
            <b>
            <a href="mailto:qhyu@stu.suda.edu.com">Trinity 📨 </a>
            </b>
        </i>
        <br/>
        <i>Date：2024-09-08</i><br/>
        <i>Need more help? </a>See more information in <a rel="license" href="https://ternity.github.io/">My GitHub Homepage </a>or send me email. Thanks!</i><br/>
    </p>
</div>

<br/>
学习资料源自廖雪峰老师博客[Git教程](https://www.liaoxuefeng.com/wiki/896043488029600)

## 目录
* [创建版本库](#charpt1)
* [添加一个文件到仓库](#charpt2)
* [修改文件](#charpt3)
* [同步至远程仓库](#charpt4)
* [将本地master分支推送到远程main分支](#charpt5)
* [多分支协作流程](#charpt6)
  * [创建新分支](#charpt6.1)
  * [提交远端](#charpt6.2)
  * [同步远端](#charpt6.3)
  * [合并修改](#charpt6.4)
  * [删除分支](#charpt6.5)

## 1. 创建版本库 <a id ='charpt1'></a>
将目录变为Git可管理的仓库：
```bash
git init
```
输出：
```log
Initialized empty Git repository in /Users/michael/learngit/.git/
```
目录下多一个`.git`目录，没事别找他麻烦。

## 2. 添加一个文件到仓库 <a id ='charpt2'></a>
### 第一步 添加文件
编写一个README.md文件，注意要放到`.git`同目录下，使用命令`git add`添加到仓库：
```bash
git add README.md
```
没有输出？这就对了。没有就是好消息。
### 第二步 提交更改至仓库
使用命令`git commit`将文件提交至仓库
```bash
git commit -m "add readme file"
```
## 3. 修改文件 <a id ='charpt3'></a>
### 第一步 修改文件
向README.md文件中添加：
```log
Git is a version control system.
```
### 第二步 查看仓库状态
使用命令`git status`查看modified，输出为：
```log
On branch master
Changes not staged for commit:
  (use "git add <file>..." to update what will be committed)
  (use "git checkout -- <file>..." to discard changes in working directory)

        modified:   README.md

Untracked files:
  (use "git add <file>..." to include in what will be committed)

        Study_Git/

no changes added to commit (use "git add" and/or "git commit -a")
```
这样我们可以实时掌控仓库的状态，监控什么被修改过，但是还没有准备提交。
### 第三步 查看修改内容
命令`git status`只能查看修改了什么文件，但是看不出修改内容，使用命令`git diff`查看。
```bash
git diff README.md
```
输出：
```log
diff --git a/README.md b/README.md
index 8df08ff..b8b3205 100644
--- a/README.md
+++ b/README.md
@@ -2,3 +2,17 @@
 Welcome to my  persional Homepage！
 ## Let‘s strart together!
 **The webpage is currently under construction**
+
+<div style="color:white; background-color:#354538; border: 1px solid #FFE0C3; border-radius: 10px; margin-bottom:0rem">
+    <p style="margin:1rem; padding-left: 1rem; line-height: 2.5;">
+        ©️ <b><i>Copyright 2023 @ Authors</i></b><br/>
+        <i>Author：
+            <b>
+            <a href="mailto:qhyu@stu.suda.edu.com">Trinity 📨 </a>
+            </b>
+        </i>
+        <br/>
+        <i>Date：2023-09-11</i><br/>
+        <i>Need more help? </a>See more information in <a rel="license" href="https://github.com/Ternity/Trinity.github.io/">My GitHub Homepage </a>or send me email. Thanks!</i><br/>
+    </p>
+</div>
\ No newline at end of file
```
这里的`diff`表示的是本地磁盘上的内容和本地git分支上内容的差异。
### 第四步 添加至暂存区
当确认`git diff`的自己修改的内容无误后，可以使用命令`git add`将修改的文件添加至暂存区：
```bash
git add READMD.md
```
同样没有任何输出，使用命令`git status`:
```log
On branch master
Changes to be committed:
  (use "git reset HEAD <file>..." to unstage)

        modified:   README.md

Untracked files:
  (use "git add <file>..." to include in what will be committed)

        Study_Git/
```
git告诉我们将要被提交的修改包括README.md文件，那我们就可以放心提交修改了：
```bash
git commit -m "add distributed"
```
输出提示：
```log
[master fc7c02d] add distributed
 1 file changed, 14 insertions(+)
```
提交后再用`git status`命令查看仓库状态：
提示：
```log
On branch master
nothing to commit, working tree clean
```
可以看到git表示当前没有在暂存区的文件，工作区是干净的。


## 4. 同步到远程仓库 <a id ='charpt4'></a>
使用命令提交至远程仓库：

```bash
# 1.首先需要添加远程仓库
git remote add origin https://github.com/Ternity/Ternity.github.io/tree/master
# 2.如果网络有问题，可以使用SSH而不是HTTPS
git remote set-url origin git@github.com:Ternity/Ternity.github.io.git
# 3.将master分支推送到远程仓库
git push origin master
# 4.也可以将所有分支和标签推送到远程仓库
git push new-origin --all
git push new-origin --tags
# 如果需要添加到新远程仓库
git remote add new-origin <新仓库的URL>
# 然后执行上面2-4，然后有需要可以删除旧的远程仓库，并重命名新仓库
git remote remove origin
git remote rename new-origin origin
```
当我们提交至remote 仓库时候，输出：
```log
Counting objects: 18, done.
Delta compression using up to 12 threads.
Compressing objects: 100% (17/17), done.
Writing objects: 100% (18/18), 198.42 KiB | 796.00 KiB/s, done.
Total 18 (delta 3), reused 0 (delta 0)
remote: Resolving deltas: 100% (3/3), done.
remote:
remote: Create a pull request for 'master' on GitHub by visiting:
remote:      https://github.com/Ternity/Trinity.github.io/pull/new/master
remote:
To github.com:Ternity/Trinity.github.io.git
 * [new branch]      master -> master
Branch 'master' set up to track remote branch 'master' from 'origin'.
```
## 5. 将本地master分支推送到远程main分支 <a id ='charpt5'></a>
本地master推送到远程main分支
```bash
git push origin master:main
```
如果是想将本地master分支重命名为main分支并推送到master，则
```bash
git branch -m master main
git push origin main
```

## 6. 多分支协作流程 <a id ='charpt6'></a>
**创建新分支-提交远端-同步远端-合并修改-删除分支**
### 6.1 创建新分支 <a id ='charpt6.1'></a>
无论是clone别人的代码还是自己本地的main(master)分支的代码，直接在main分支上修改是**不良行为**。一方面，对于多人协作仓库，创建新的分
支可以区分不同人的工作；另一方面，新的PR(pull request)也不会使之前的main的code失效。因此我们可以使用如下命令从原有的`main`分支复制一个新的分支,并重命名为`dev`分支：
```bash
git checkout -b dev
```
现在，我们有两个分支，`main`和`dev`，使用如下命令命令查看当前有哪些分支：
```bash
git branch
```
可以发现现在本地git中有两个git分支，`main`和`dev`，其中`*`表示当前所在的分支：
```log
* dev
  main
```
我们可以在dev分支上修改，添加修改文件到暂存区(`git add xxx`)，确认无误(`git diff`)，提交修改到本地仓库(`git commit -m "xxx"`)。
### 6.2 提交远端 <a id ='charpt6.2'></a>
将本地git的dev分支推送到远程仓库(`git push origin dev`)。这样就不会影响到远程仓库的main分支的代码。
### 6.3 同步远端 <a id ='charpt6.3'></a>
一种很常见的情况是，远程仓库中的`main`分支有了更新`update`，而我们的`dev`分支相对于init代码也有了更新`feature`，这时候我们需要测试我们的`feature`能否在main分支更新的`update`下工作。这需要将远程仓库的`main`分支的`update`更新同步到本地仓库的`dev`分支。
#### 6.3.1 远程仓库`main`分支最新的`update`同步到本地仓库的`main`分支
首先，我们需要切换当前分支`dev`到`main`分支：
```bash
git checkout main
``` 
将远程仓库的`main`分支更新同步到本地仓库的`main`分支
```bash
git pull origin main
```
这样我们本地git分支`main`就获得了远程仓库的`main`分支的`update`。
#### 6.3.2 将本地仓库的`main`分支的`update`同步到`dev`分支
接下来我回到`dev`分支，并将`main`分支的更新同步到`dev`分支：
```bash
git checkout dev
git rebase main   
```
`git rebase main`这个命令的作用：<br>
之前`dev`的`feature`先不管,将`mai`n的修改同步到`dev`,然后在此基础上将`feature`尝试合并。
然后再将本地仓库的`main`分支合并到本地仓库的`dev`分支。<br>
#### 6.3.3 解决冲突
上面这个过程可能会有 rebase conflict，需要手动选择冲突代码。
上一步的`git rebase main`命令会提示冲突的文件，也可使用`git status`命令查看冲突文件。
```bash
warning: squelched 2140 whitespace errors
warning: 2145 lines add whitespace errors.
Falling back to patching base and 3-way merge...
Auto-merging docs/index.md
CONFLICT (add/add): Merge conflict in docs/index.md
error: Failed to merge in the changes.
Patch failed at 0002 create web use mkdocs-material
Use 'git am --show-current-patch' to see the failed patch

Resolve all conflicts manually, mark them as resolved with
"git add/rm <conflicted_files>", then run "git rebase --continue".
You can instead skip this commit: run "git rebase --skip".
To abort and get back to the state before "git rebase", run "git rebase --abort".
```
找到相应的文件，这里是`docs/index.md`，打开文件分析选择需要保留的部分。
<br>使用`git add`命令将冲突文件标记为已解决，然后使用`git rebase --continue`继续rebase。
```bash
git add docs/index.md
git rebase --continue
```
重复上述步骤，直到rebase完成。

### 6.4 合并修改 <a id ='charpt6.4'></a>
更新完`dev`分支后，我们可以将本地`dev`分支更新到远程仓库的`dev`分支：
```bash
git push -f origin dev    # 由于rebase，需要强制推送
```
一切准备就绪，我们可以将GitHub的`dev`版本的代码合并到`main`分支，此事即为**Pull Request(PR)**：<br>
这里需要在GitHub上操作。<br>
![Commpare&pull_request](Commpare&pull_request.png)<br>
在确认New pull request后，代码管理员有三个选项可选：<br>
![merge](merge.png)<br>
`Merge pull request`：所有添加到`dev`分支的`commit`都会被添加到`main`分支，由于`commit`可能很多，不推荐。<br>
`Squash and merge`：将`dev`分支的所有`commit`合并为一个`commit`，然后再添加到`main`分支，推荐。<br>
`Rebase and merge`：将`dev`分支的所有`commit`合并为一个`commit`，并`rebase`到`main`分支。<br>

我们希望`main`分支里的commit尽可能简洁且代码可正常工作，因此推荐代码管理员选择`Squash and merge`操作，形成新的commit`uodate2`。
### 6.5 删除分支 <a id ='charpt6.5'></a>
当`dev`分支的代码合并到`main`分支后，`dev`分支就没有存在的必要了，可以删除。<br>
GitHub上有操作为`Delete branch`。这样远程仓库的`dev`分支就被删除了。<br>
![Delete branch](Delete_branch.png)<br>
然后本地的`dev`分支也可以删除：
```bash
git checkout main   # 本地分支切换到main
git branch -D dev   # 删除dev分支
```
将远程`main`分支最新更新`update2`同步到本地：
```bash
git pull origin main
```
这时候，远程、本地git的`main`分支内容就一致啦！
