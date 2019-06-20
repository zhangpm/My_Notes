---
title: GitHub_Learn
---
# GitHub_Learn
https://git-scm.com/doc
https://www.liaoxuefeng.com/
git bash 基本命令同linux bash命令，如ls，mkdir，rm等
## 0. 三个区域
* 工作区（working Directory）：简单的理解你在**电脑里能看到的目录**。
* 暂存区（stage）：Git的版本库里存了很多东西，其中最重要的就是称为stage（或者叫index）的暂存区，还有Git为我们自动创建的第一个分支master，以及指向master的一个指针叫HEAD。
* 版本库（Repository）：工作区有一个隐藏目录.git，这个不算工作区，而是Git的版本库。
![Pic_dlog2](/pic/git_3part.png "dlog")

## 1. 创建版本库（仓库）repository
1. 创建空目录，然后cd到此目录下 git init 
```bash
$ mkdir learngit
$ cd learngit
$ pwd
/Users/michael/learngit
```
```bash
$ git init

Initialized empty Git repository in /Users/michael/learngit/.git/
```
2. 目录下创建文件，如readme.txt

## 2. 文件放入仓库
两步
1. 用命令git add告诉Git，把文件添加到仓库
```bash
$ git add readme.txt
```
2. 用命令git commit告诉Git，把文件提交到仓库
```bash
$ git commit -m "wrote a readme file"
[master (root-commit) eaadf4e] wrote a readme file
 1 file changed, 2 insertions(+)
 create mode 100644 readme.txt
```
* git commit命令，-m后面输入的是本次提交的说明

* 为什么Git添加文件需要add，commit一共两步呢？因为commit可以一次提交很多文件，所以你可以多次add不同的文件，比如：
```bash
$ git add file1.txt
$ git add file2.txt file3.txt
$ git commit -m "add 3 files."
```

`git add . `全部添加

## 3. 版本回退
1. git log 查看日志
2. 用HEAD表示当前版本，上一个版本就是HEAD^，上上一个版本就是HEAD^^，当然往上100个版本写100个^比较容易数不过来，所以写成HEAD~100
3. git reset命令 退回上一版本
```bash
$ git reset --hard HEAD^
```
慎用hard
![Pic_dlog2](/pic/git_reset.jpg "dlog")
4. 回到未来版本：按照log中的版本号 `git reset --hard 1094a`
5. git reflog 查看命令历史
6. git checkout其实是用版本库里的版本替换工作区的版本，无论工作区是修改还是删除，都可以“一键还原”。
7. 命令git rm用于从版本库删除一个文件。如果一个文件已经被提交到版本库，那么你永远不用担心误删，但是要小心，你只能恢复文件到最新版本，你会丢失最近一次提交后你修改的内容。

## 4. 远程仓库连接，创建远程仓库
### 4.1 先有本地仓库，后连接远程仓库
1. 本地版本库创建：建文件夹
2. 登录github，Create a new repo，不要readme
3. 本地连接和推送到远程
```bash
git remote add origin https://github.com/zhangpm/git_note.git
git push -u origin master
```
4. origin 是远程默认名，master本地名
5. 查看远程，`git remote (-v)`

### 4.2 先有远程，后克隆
1. github 创建新repository
2. clone or download 复制地址（http或ssh）“https://github.com/zhangpm/New_try.git”
3. git clone https://github.com/zhangpm/New_try.git

## 5. 分支
### 5.1 查看
1. 查看分支`git branch `
2. 查看远程分支`git branch -r `
3. 查看全部分支`git branch -a`
4. 查看分支合并情况`git log --graph --pretty=oneline --abbrev-commit`

### 5.2 添加 **"dev 为分支名"**
1. 创建本地分支`git branch dev`
2. 切换到新创建的分支`git checkout dev`切换后，文件中的目录即检出未当前分支下的内容
3. 在当前分支下add 和commit文件
4. 将新分支push到github`git push origin dev`
5. **`git checkout -b dev`  -b参数表示创建并切换，相当于git branch dev + git checkout dev

### 5.3 删除
1. 删除本地：`git branch -d dev`
2. 未合并强制删除：`git branch -D dev`
3. 删除远程分支`git push origin --delete dev`

### 5.3.5 本地和远程分支的各种关联增删总结
1. 远程仓库有master 和dev 分支，要拉到本地
   * `git clone XXX`,克隆后本地master 和远程origin/master默认关联，但是dev默认不关联
   * `git checkout -b dev origin/dev`本地创建dev，关联origin/dev
   * 上述语句等同于`git checkout -b dev `+`git pull origin dev`??
2. 远程仓库只有master，本地分支push到远程
   * `git push origin dev`
3. 建立本地分支后，关联远程分支：
   * `git branch --set-upstream-to=origin/remote_branch  your_branch`
   * eg. `git branch --set-upstream-to=origin/dev dev`
其中，origin/remote_branch是你本地分支对应的远程分支；your_branch是你当前的本地分支。

### 5.4 合并分支
1. git merge dev
2. 合并成功是fast foward 模式，合并后无分支记录
3. 合并的两个分支同时改动同一位置，会产生合并冲突，先解决冲突，在文件中修改后，add，commit，然后即合并完成，可删除分支
4. no-ff 非fast foward模式，合并后有分支记录`git merge --no-ff -m "merge with no-ff" dev`

### 5.5 分支策略
1. master 应为稳定版，平时不在上面干活
2. 在dev分支上干活，发布版本时merge到master上
3. 各部分工作各做分支，先合并到dev，再合并到master

### 5.6 保持工作状态stash
应用: 工作一半，不想提交，但是要去作别的
1. 保存现场`git stash`
2. 查看保存`git stash list`
3. 恢复现场：
   * 恢复` git stash apply `，删除记录 `git slash drop`
   * 恢复并删除记录` git stash pop`

### 5.7 分支远程推送策略：
1. 远程推送：`git push origin dev`
2. 策略：
   * master 推送，保持同步
   * dev 开发分支（develop），保持同步
   * bug分支，本地修复，无需推送到远程
   * feature，视情况而定
总之，就是在Git中，分支完全可以在本地自己藏着玩，是否推送，视你的心情而定！

## 6. 多人协作
1. 首先，可以试图用git push origin \<branch-name>推送自己的修改
2. 若失败，则git pull先拉取最新版本。
3. pull 中会merge本地和拉下来的版本，容易产生冲突，需要手动解决，然后add，commit
4. 没有冲突或者解决掉冲突后，再用git push origin \<branch-name>推送就能成功
5. 如果git pull提示no tracking information，则说明本地分支和远程分支的链接关系没有创建，用命令git branch --set-upstream-to <branch-name> origin/<branch-name>

## 7. tag标签
1. `git tag v1.0`当前commit打标签“v1.0”
2. `git tag`查看全部标签
3. `git tag v0.9 f52c633`给commit号为f...打标签v0.9
4. commit id 号查看：`git log --pretty=oneline --abbrev-commit`
5. `git tag -a v0.1 -m "version 0.1 released" 1094adb` -m 添加说明
6. `git show v0.1` 显示说明
7. 标签总是和某个commit挂钩
8. `git tag -d v0.1`删除标签
9. `git push origin v1.0`标签推送到远程
10. `git push origin --tags`全部标签推送
11. 删除已推送的标签：先删本地`git tag -d v0.1`，再`git push origin :refs/tags/<tagname>`,登录github查看删除结果

## 8. 参与（下载）开源项目
1. 在GitHub上，可以任意Fork开源仓库；
2. clone到本地
3. 自己随便用，push到自己的仓库
4. 参与开源，则发起pull request，等待原作者审核通过

--End--
