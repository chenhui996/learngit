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

- 局限很大(未完善版本)：
  - 首先，需要当前命令行窗口没关;
  - 然后往上查找到之前版本的 commit id;
  - 然后通过 git reset --hard 1095a 还原;
- 版本号没必要写全，前几位就可以了，Git 会自动去找:
  - 也不能只写前一两位，因为 Git 可能会找到多个版本号，就无法确定是哪一个了;
- 完善版本：
  - 在 Git 中，总是有后悔药可以吃的;
  - git reflog 用来记录你的每一次命令：

# git reflog

- 用来记录你的每一次命令：
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
- 把 readme.txt 文件在工作区的修改全部撤销,这里有两种情况：
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
- 命令git rm用于删除一个文件;
  - 如果一个文件已经被提交到版本库，那么你永远不用担心误删;
  - 但是要小心，你只能恢复文件到最新版本:
    - 你会丢失最近一次提交后你修改的内容。

# git checkout

- git checkout -b dev
- git checkout命令加上-b参数表示创建并切换，相当于以下两条命令：
  - $ git branch dev
  - $ git checkout dev

# git branch

- 查看当前分支;
- 当前分支前面会标一个*号;

# git merge

- 用于合并指定分支到当前分支
