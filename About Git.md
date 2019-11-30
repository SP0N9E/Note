# About Git

[TOC]

### 【Git】创建一个新分支

```powershell
// 创建一个baseline1的空分支，这个分支不继承父分支，是独立的
$ git checkout --orphan baseline1
Switched to a new branch 'baseline1'
(base)
// 清空当前分支下的所有文件
$ git rm -rf .
// 查看当前分支 （但是这个时候你是看不到当前分支的）
$ git branch -a
//// 添加一个readme.md后 查看文件状态
$ git status
// 存入工作区
$ git add readme.md
// commit一下  添加评论
$ git commit -a -m "initialize"
// 然后sublime merge push上去（偷懒了
```

