# 学习如何使用git
<div style="color:white; background-color:#354538; border: 1px solid #FFE0C3; border-radius: 10px; margin-bottom:0rem">
    <p style="margin:1rem; padding-left: 1rem; line-height: 2.5;">
        ©️ <b><i>Copyright 2023 @ Authors</i></b><br/>
        <i>Author：
            <b>
            <a href="mailto:qhyu@stu.suda.edu.com">Trinity 📨 </a>
            </b>
        </i>
        <br/>
        <i>Date：2023-09-11</i><br/>
        <i>Need more help? </a>See more information in <a rel="license" href="https://github.com/Ternity/Trinity.github.io/">My GitHub Homepage </a>or send me email. Thanks!</i><br/>
    </p>
</div>

<br/>
学习资料源自廖雪峰老师博客[Git教程](https://www.liaoxuefeng.com/wiki/896043488029600)

## 目录
* [背景](#charpt1)
* [DeePSPIN用于构建正极材料的机器学习势函数](#practice)
* [总结](#conclusion)
* [参考文献](#reference)

## 创建版本库 <a id ='charpt1'></a>
将目录变为Git可管理的仓库：
```bash
git init
```
输出：
```log
Initialized empty Git repository in /Users/michael/learngit/.git/
```
目录下多一个`.git`目录，没事别找他麻烦。

## 添加一个文件到仓库
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
## 修改文件
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
### 第四步 添加至暂存区
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



使用命令提交至远程仓库：
```bash
git push origin master
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
