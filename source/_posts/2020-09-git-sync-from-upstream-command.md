---
title: Git进行fork后与原仓库同步方法
categories:
  - 学编程
tags:
  - 混技能
  - shell
toc: true
comments: true
keywords: '同步上游仓库'
description: '在你 `fork` 一个仓库之后， 往往上游的仓库又更新了。但 Git 不会自动帮你把上游的仓库同步给你 `fork` 后的仓库，有时候自己的一些更改也需要保留……此时，就需要学会与上游仓库合并更改及同步。'
date: 2020-09-09 15:50:03
updated: 2020-09-09 15:50:03
top:
---
# 前言
在你 `fork` 一个仓库之后， 往往上游的仓库又更新了。但 Git 不会自动帮你把上游的仓库同步给你 `fork` 后的仓库，有时候自己的一些更改也需要保留……此时，就需要学会与上游仓库合并更改及同步。

# 方法
最省事的办法可能是：
- 在你 fork 的仓库 `setting` 页翻到最下方，然后 `delete` 这个仓库
- 然后重新 fork 仓库，并 git clone 到你的本地

但在更多情况下，删掉自己fork的库，应该是你的最后选择，而不应该是首选。

可行的首选方法简单的说就是：
1. fork 你要的仓库到自己账号下
2. `git clone` fork后的仓库到本地
3. 打开 `Git bash` ，以下是需要输入的命令：
   1. `git remote -v` #查看远程状态
   2. `git remote add upstream https://github.com/OWNER/REPOSITORY.git` #配置完建议再次查看状态确认是否配置成功
   3. `git fetch upstream`  #从上游仓库 `fetch` 分支和提交点，提交给本地 `master`，并会被存储在一个本地分支 `upstream/master`
   4. `git checkout master` #切换到本地主分支(如果不在的话)
   5. `git merge upstream/master` #把 `upstream/master` 分支合并到本地 `master` 上，这样就完成了同步，并且不会丢掉本地修改的内容。 
   6. `git push origin master` #这一步就把合并后的内容推送到fork后的仓库里了

