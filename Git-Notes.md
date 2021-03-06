### Git 笔记

# pwd

- 命令用于显示当前目录;

# git init

- 把这个目录变成 Git 可以管理的仓库;
  - 当前目录下多了一个.git 的目录;
  - 如果你没有看到.git 目录，那是因为这个目录默认是隐藏的;
    - 用 ls -ah 命令就可以看见;

# git diff

- 顾名思义就是查看 difference;
- 显示的格式是 Unix 通用的 diff 格式;

# git log

- 命令显示从最近到最远的提交日志;
- --pretty=oneline 参数:
  - 单行显示提交日志;
- git log --graph
  - 可以看到分支合并图;

# 回退版本

- 首先，Git 必须知道当前版本是哪个版本:
  - 在 Git 中，用 HEAD 表示当前版本;
    - 也就是最新的提交 1094adb...（注意我的提交 ID 和你的肯定不一样）:
    - 上一个版本就是 HEAD^;
    - 上上一个版本就是 HEAD^^;
    - 当然往上 100 个版本写 100 个^比较容易数不过来:
      - 所以写成 HEAD~100;
- 现在，我们要把'当前版本'回退到上一个版本:
  - 可以使用 git reset 命令;

# git reset

```
$git reset --hard HARD^
```

- --hard 参数有啥意义？这个后面再讲，现在你先放心使用;

# 版本前进

- 局限很大(未完善版本):
  - 首先，需要当前命令行窗口没关;
  - 然后往上查找到之前版本的 commit id;
  - 然后通过 git reset --hard 1095a 还原;
- 版本号没必要写全，前几位就可以了，Git 会自动去找:
  - 也不能只写前一两位，因为 Git 可能会找到多个版本号，就无法确定是哪一个了;
- 完善版本:
  - 在 Git 中，总是有后悔药可以吃的;
  - git reflog 用来记录你的每一次命令:

# git reflog

- 用来记录你的每一次命令:
  - 从单行日志中找到 HEAD 指针指向变动的行:
    - 查询对应 commit id;

# 指针

- git 版本回退的本质是指针;
  - 将 git 内部的 HARD 指针，指向对应版本，即可实现回退;

## 小结

- HEAD 指向的版本就是当前版本;
  - 因此，Git 允许我们在版本的历史之间穿梭:
    - 使用命令 git reset --hard commit_id;
  - 穿梭前，用 git log 可以查看提交历史:
    - 以便确定要回退到哪个版本;
  - 要重返未来，用 git reflog 查看命令历史:
    - 以便确定要回到未来的哪个版本;

# git checkout

- git checkout -- readme.txt
- 把 readme.txt 文件在工作区的修改全部撤销,这里有两种情况:
  - 一种是 readme.txt 自修改后还没有被放到暂存区:
    - 现在，撤销修改就回到和'版本库'一模一样的状态;
  - 一种是 readme.txt 已经添加到暂存区后，又作了修改:
    - 现在，撤销修改就回到添加到'暂存区'后的状态;
- 总之，就是让这个文件回到最近一次 git commit 或 git add 时的状态;
- git checkout -- file 命令中的--很重要:
  - 没有--，就变成了“切换到另一个分支”的命令;
  - 我们在后面的分支管理中会再次遇到 git checkout 命令;

# git reset HEAD <file>

- 可以把暂存区的修改撤销掉（unstage），重新放回工作区;

# git rm

- 从版本库中删除该文件;
- 命令 git rm 用于删除一个文件;
  - 如果一个文件已经被提交到版本库，那么你永远不用担心误删;
  - 但是要小心，你只能恢复文件到最新版本:
    - 你会丢失最近一次提交后你修改的内容。

# git checkout

- git checkout -b dev
- git checkout 命令加上-b 参数表示创建并切换，相当于以下两条命令:
  - \$ git branch dev
  - \$ git checkout dev

# git branch

- 查看当前分支;
- 当前分支前面会标一个\*号;

# git merge

- 用于合并指定分支到当前分支

# git switch

- 切换分支这个动作，用 switch 更科学;
- \$ git switch -c dev
  - 创建并切换到新的 dev 分支

## 小结

- Git 鼓励大量使用分支:
  - 查看分支:git branch
  - 创建分支:git branch <name>
  - 切换分支:
    - git checkout <name>
    - git switch <name>
  - 创建+切换分支:
    - git checkout -b <name>
    - git switch -c <name>
  - 合并某分支到当前分支:git merge <name>
  - 删除分支:git branch -d <name>

# --no-ff

- 参数就可以用普通模式合并，合并后的历史有分支，能看出来曾经做过合并:
  - 而 fast forward 合并就看不出来曾经做过合并;

## 分支策略

- 在实际开发中，我们应该按照几个基本原则进行分支管理:
  - 首先，master 分支应该是非常稳定的:
    - 也就是仅用来发布新版本，平时不能在上面干活;
  - 那在哪干活呢？干活都在 dev 分支上:
    - 也就是说，dev 分支是不稳定的:
      - 到某个时候，比如 1.0 版本发布时，再把 dev 分支合并到 master 上;
      - 在 master 分支发布 1.0 版本;
  - 你和你的小伙伴们每个人都在 dev 分支上干活:
    - 每个人都有自己的分支，时不时地往 dev 分支上合并就可以了;

## 小结

- Git 分支十分强大，在团队开发中应该充分应用;
- 合并分支时，加上--no-ff 参数就可以用普通模式合并;
  - 合并后的历史有分支，能看出来曾经做过合并:
  - git log --graph --pretty=oneline --abbrev-commit
  - 而 fast forward 合并就看不出来曾经做过合并;

## Bug 分支

- 软件开发中，bug 就像家常便饭一样;
- 有了 bug 就需要修复;
- 在 Git 中:
  - 由于分支是如此的强大:
    - 所以，每个 bug 都可以通过一个新的临时分支来修复;
    - 修复后，合并分支，然后将临时分支删除;

# git branch -d

- 删除分支;

# git stash

- 把当前工作现场“储藏”起来，等以后恢复现场后继续工作：

# git stash list

- Git 把 stash 内容存在某个地方了;
- 用 git stash list 查看目录;

# 恢复 stash

- git stash apply 恢复：
  - 恢复后，stash 内容并不删除;
  - 还需要用 git stash drop 删除;
  - 恢复指定的 stash:
    - git stash apply stash@{0}
- git stash pop 恢复;
  - 恢复的同时把 stash 内容也删了

# git cherry-pick

- 能复制一个特定的提交到当前分支

## 小结

- 修复 bug 时，我们会通过创建新的 bug 分支进行修复;
  - 然后合并，最后删除;
- 当手头工作没有完成时，先把工作现场 git stash 一下;
  - 然后去修复 bug;
  - 修复后，再 git stash pop，回到工作现场;
- 在 master 分支上修复的 bug:
  - 想要合并到当前 dev 分支:
    - 可以用 git cherry-pick <commit>命令;
    - 把 bug 提交的修改“复制”到当前分支，避免重复劳动;

## 多人协作

- 当你从远程仓库克隆时:
  - 实际上 Git 自动把本地的 master 分支和远程的 master 分支对应起来;
  - 远程仓库的默认名称是 origin;

# git remote

- 查看远程库的信息
- git remote -v
  - 查看更详细的信息;
    - 上面显示了可以抓取和推送的 origin 的地址;
    - 如果没有推送权限，就看不到 push 的地址;

# 推送分支

- 推送分支，就是把该分支上的所有本地提交推送到远程库;
- 推送时，要指定本地分支:
  - 这样，Git 就会把该分支推送到远程库对应的远程分支上：

```
$git push origin master
```

- 如果要推送其他分支，比如 dev，就改成：

```
$git push origin dev
```

- 并不是一定要把本地分支往远程推送:
  - 那么，哪些分支需要推送，哪些不需要呢？
    - master分支是主分支;
      - 因此要时刻与远程同步;
    - dev分支是开发分支，团队所有成员都需要在上面工作;
      - 需要与远程同步;
    - bug分支只用于在本地修复bug;
      - 就没必要推到远程了，除非老板要看看你每周到底修复了几个bug;
    - feature分支
      - 是否推到远程，取决于你是否和你的小伙伴合作在上面开发;

总之，就是在Git中，分支完全可以在本地自己藏着玩，是否推送，视你的心情而定！

# git checkout -b dev origin/dev

- 现在，你的小伙伴要在dev分支上开发，就必须创建远程origin的dev分支到本地，于是他用这个命令创建本地dev分支;
- 现在，他就可以在dev上继续修改，然后，时不时地把dev分支push到远程
