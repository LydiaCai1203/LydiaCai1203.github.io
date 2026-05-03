+++
title = "GIT 常用命令查阅手册"
description = "技术"
tags = [
    "2020",
    "技术",
]
date = "2020-12-07"
categories = [
    "技术",
]
type = "blog"
+++

**[练习平台 - 强烈建议新手通关](https://learngitbranching.js.org/?locale=zh_CN)**


#### git commit
```markdown
    将当前版本和仓库的上一版本进行对比，将所有的差异打包到一起作为一个提交记录。
```

#### git branch <branch_name> 创建分支

#### git merge <branch_name>
    
![](https://lydiacai1203.github.io/images/merge.jpg)
    
#### git rebase master
```markdown
    将 bugFix 里面的 commit 按照顺序复制到 master 下。也就是说 bugFix 在 master 分支的最顶端了。
    git checkout master
    git rebase bugFix  # 相当于将 master 的 commit 复制到 fixBug 下，master 会在 bugFix 的顶端，所以 master 只会往下移一格。
```
  
![](https://lydiacai1203.github.io/images/rebase.jpg) 

#### HEAD 
```markdown
    总是指向当前分支上最近一次的提交记录。
    HEAD 通常情况下指向分支名的。 
    也就是说 master 分支其实是 master 指针，head 指针是单独的 head 指针。
    `git checkout <commit-sha>` 便可以移动 head 指针。
```

![](https://lydiacai1203.github.io/images/head.jpg)

```sh
    cat .git/HEAD
    git symbolic-ref HEAD 
    这两句可以查看当前的 HEAD 指针指向
```

#### ^ 相对引用，n 个 ^ 或者 ~n 就是向上 n 个 commit `git checkout master^` 或者 `git checkout HEAD~4`
![](https://lydiacai1203.github.io/images/相对引用.jpg)

#### git branch -f master HEAD~3
```markdown
    强制修改分支位置，将 master 分支强制指向 HEAD 的第三个父级提交。
```

![](https://lydiacai1203.github.io/images/branch_f.jpg)

#### git reset HEAD~1
```markdown
    通过分支记录回退几个提交记录来实现撤销改动。
```

![](https://lydiacai1203.github.io/images/reset.png)

#### git revert HEAD
```markdown
    通过在要撤销的提交记录后面新增一个提交，然后在新提交里面引入一些更改，这些更改便是用于撤销记录的。
```

![](https://lydiacai1203.github.io/images/revert.png)

#### git cherry-pick <commit-sha>
```markdown
    将一些提交复制到 HEAD 下。
```

![](https://lydiacai1203.github.io/images/cherry_pick.png)

#### 交互式的 rebase
```markdown
    如果你不知道要提交的 sha 值，就可以使用交互式的 rebase. `git rebase -i HEAD~4` or `git rebase -i branchName~3`
```

+ UI 界面出现时可以调整提交记录的顺序
+ 删除不想要的提交(通过切换 pick 的状态来完成，关闭就意味着不想要这个记录)
+ 允许你把多个提交记录合并成一个。

![](https://lydiacai1203.github.io/images/rebase_i.png)

#### 提交的技巧 #1
```markdown
    背景：你之前在 newImage 分支上进行了一次提交，然后基于它创建了 caption 分支，然后又提交了一次。此时如果想要对以前的提交记录进行一些小小的调整，该如何呢？
    (不过可能会两次调整顺序而导致冲突)
```
+ `git rebase -i` 将提交重新排序，将我们想要修改的提交记录挪到最前面
+ `git commit --amend` 进行一些小修改
+ `git rebase -i` 将他们调回到原来的顺序
+ 最后将 master 移动到最前端

#### 提交的技巧 #2
+ `git cherry-pick`
+ `git commit --amend`
+ `git cherry-pick`

#### git tag v1 C1
```markdown
    分支很容易会被人为移动，当有新的提交时，也会有移动。
    git tag 是可以永远指向某个提交记录的表示，比如软件发布新的大版本，或者修正了一些重要的 bug。
    tag 并不会随着新的提交而移动，你也不能检出某个标签进行修改提交。
```

![](https://lydiacai1203.github.io/images/tag.png)

#### git describe <ref>
```markdown
    用于描述离你最近的 tag, 能帮你在提交历史中移动了多次以后找到方向，当你用 `git bisect`（一个查找产生 BUG 的提交记录的制定）查找某个提交记录时。
```
+ <ref> 可以是 commit 或者 引用，若没有指定则是 HEAD 所在之处。
+ output: <tag>_<numCommits>_g<hash> tag 表示的是离 ref 最近的标签，numCommits 表示的是这个 ref 与 tag 相差多少个提交记录，hash 表示你所给定的 ref 所表示的提交记录哈希值的前几位。
+ 当 ref 提交记录上有某个标签时，则只输出标签名称。

![](https://lydiacai1203.github.io/images/describe.png)

#### 多分支 rebase 要求多个分支上的提交都合并到 master, 并且展示有序。

#### 选择父提交记录
```markdown
    操作符 ^ 和 ~ 一样都可以跟一个数字。但是 ^ 后面跟数字并不是用来指定向上返回几代。而是指定合并提交记录的某个父提交。(没懂这个表述)
    git 默认选择合并提交的 "第一个" 父提交，在操作符 ^ 后面跟一个数字可以改变这个默认行为。
```
+ `git checkout master^`
![](https://lydiacai1203.github.io/images/^_number.png)
+ `git checout master^2`
![](https://lydiacai1203.github.io/images/^_number_2.png)
+ `git checkout HEAD~^2~2`
![](https://lydiacai1203.github.io/images/链式操作.png)

#### <remote_name>/<branch_name>
```markdown
    前面是远程仓库的名字，后面是分支名。为什么我们平时都是叫 origin, 是因为当我们使用 git clone 时，git 已经帮你把远程仓库的名称变为 origin。
```

#### git fetch
```markdown
    实际上将本地仓库中的远程分支更新成了远程仓库相应分支的最新状态
    git fetch 并不会更改你本地仓库的状态，它也不会更新你的 master 分支，也不会修改你磁盘上的文件。也就是说并不是执行了 git fetch 之后，你的本地仓库就和远程仓库同步了。所以可以将 git fetch 单纯地理解为是一个下载操作。
```
+ 从远程仓库下载本地仓库中缺失的提交记录
+ 更新远程分支指针（origin/master）
+ fetch 通过互联网，使用 http:// 或者 git:// 协议与远程仓库进行通信

#### git pull = `git fetch + git merge`
```markdown
    如何将 git fetch 获取到的远程数据更新到本地分支上呢？
    + git cherry-pick origin/master
    + git rebase origin/master
    + git merge origin/master
    由于先抓取后合并的这个流程很常用，因此 git 专门提供了一个命令来完成这两步操作，就是 git pull
``

#### git push
```markdown
    可以将您的变更上传到远程指定的仓库中，并在远程仓库上合并你的新提交记录。
```

#### 偏离的提交历史

```markdown
    假设你周一克隆了一个仓库，研发了某个新功能。到周五的时候，新功能开发测试完毕，可以发布了，但是这一天你的同事写了一堆代码，还改了你的 API，且这些变动会让你的新功能变得不可用。
    而且他们已经将他们的代码提交到了远程仓库，这使得你的代码与远程仓库的最新代码不匹配了。

    这种情况，git 会强制你合并远程最新代码，然后才能 push.
```
![](https://lydiacai1203.github.io/images/refuse.png)

```sh
    git fetch
    git rebase o/master
    git push
```
![](https://lydiacai1203.github.io/images/refuse_2.png)

```sh
    git fetch
    git merge o/master
    git push
```
![](https://lydiacai1203.github.io/images/refuse_3.png)

```sh
    git pull = git fetch + git merge
    git pull --rebase = git fetch + git rebase 假如使用的是这个，则效果如下图
```
![](https://lydiacai1203.github.io/images/refuse_4.png)

#### 远程服务器拒绝(Remote Rejected)
```markdown
    ! [远程服务器拒绝] master -> master (TF402455: 不允许推送(push)这个分支; 你必须使用pull request来更新这个分支.)
    远程服务器拒绝直接推送提交到 master, 因为策略配置要求 pull requests 来提交更新。

    git reset HEAD^
    git checkout feature c2
    git push origin feature
```
![](https://lydiacai1203.github.io/images/rejected.png)

#### 为什么不用 merge 呢？
```markdown
rebase:
优点：
+ 让你的提交树变得很干净，所有的提交都在一条线上
缺点：
+ rebase 修改了提交树的历史
```

#### 远程跟踪分支
```markdown
有两种方法可以自定义指定任意分支指向 o/mater
+ `git checkout -b mymaster o/master` 但是这样的话 master 就不再指向 o/master 了
+ `git branch -u o/master foo` 
```

#### git push <remote> <place>
```markdown
example:
    git push origin master
    切换本地仓库中的 master 分支，获取所有的提交，在远程仓库 origin 中找到 master 分支，将远程仓库中没有的提交记录都添加上去，搞定之后告诉我
    c0 -> c1 -> c2
```
```sh
    # 提交失败，因为没有指定参数，则以 HEAD 所在的位置为准
    git checkout c0
    git push

    # 提交成功
    git checkout c0
    git push origin master
```

#### <place> 参数
```markdown
    如果同时要为源和目的指定 <place> 的话，只需要用冒号将两者连接起来。
    git push origin <source>:<destination>
    git push origin foo^:master 结果如下图
    git push origin master:new_branch
```
![](https://lydiacai1203.github.io/images/place.jpg)

#### git fetch 参数
```sh
    # 会到远程仓库的 foo 分支上，获取所有本地不存在的提交，然后放到本地的 o/foo 上
    git fetch origin foo
    # 当然一样可以直接更新在本地的 foo 分支上, source 就是远程仓库中的位置，destination 本地仓库的位置
    git fetch origin <source>:<destination>
    git fetch origin foo~:bar
```
![](https://lydiacai1203.github.io/images/fetch_args.jpg)
![](https://lydiacai1203.github.io/images/fetch_args2.jpg)

#### <source> 为空
```sh
    # 删除远程 side 分支
    git push origin :side
    # 在本地创建 bugFix 分支
    git fetch origin :bugFix
```

#### git pull 参数
```sh
    git pull origin master
    git pull origin master:foo 操作结果见下图
```
![](https://lydiacai1203.github.io/images/pull_args.jpg)

