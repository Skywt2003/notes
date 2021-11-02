# Git 学习笔记

[toc]

## 基本介绍

- Git 是最流行的分布式版本控制程序。
- 开发 Linux 时，由于和免费向社区提供版本管理的 BitMover 公司闹翻，Linus 花了两周时间自己用 C 写了一个分布式版本控制系统，这就是 Git。

### 注意事项

- 版本控制系统只能控制文本文件的改动，诸如图片、Word 文档之类的二进制文件无法监测改动。
- **编码：** 如果没有历史遗留问题，强烈建议使用标准的 UTF-8 编码。
- **GitHub 默认分支：** 2020 年 10 月 1 日后，GitHub 将新建仓库的默认分支名从 master 改为 main。

### 分布式的版本控制

- **去中心化。** 分布式版本控制系统根本没有「中央服务器」，每个人的电脑上都是一个完整的版本库，所以工作时无需联网。
- **安全性更高。** 任何用户的文件损坏不影响其他用户。

## 基础概念

- **工作区（Working Directory）** ：在电脑本地的这个工作目录。
- **版本库（Repository）** ：指的是工作区文件夹中的 `.git` 文件夹。
  - **暂存区（Stage/Index）：** 更改被 `add` 后先放到暂存区。`commit` 会一次性提交暂存区的所有修改。
  - **分支（Branch）：** 仓库默认创建分支 master。
  - **指针：** 指向当前分支的指针 HEAD。

## 基本用法

- **创建仓库：** 进入一个目录，通过 `git init` 命令把这个目录变成 Git 可以管理的仓库。创建完成后，目录会出现 `.git` 隐藏目录。
- **添加改动到仓库：** `git add <file>` 添加改动。将修改从 Untracked、Unstaged  状态添加到暂存区，成为 Staged 状态。
  - **删除文件：** `git rm <file>`  使一个文件的删除改动暂存。其实效果和 `git add <file>` 一样。
  - **添加所有：** 使用 `-A` 参数来添加所有更改（包括删除）。
- **提交文件到仓库：** `git commit -m <messsge>` 提交改动到某个分支。
- **查看当前仓库状态：** `git status` 显示目前待提交的改动等信息。
- **查看改动：** `git diff <file> `。

## 版本控制

- **版本表示：**
  - **相对表示：** 在 Git 中，用 `HEAD` 表示当前版本，也就是最新的提交。上一个版本就是 `HEAD^`，上上一个版本就是 `HEAD^^`，也可以用数字表示，例如往上 100 个版本写成 `HEAD~100`。
  - **绝对表示：** 也可以用 commit id 表示版本号。commit id 是一个 SHA1 计算出的十六进制字符串，唯一地标记这个 commit。使用时无需写全，可以只用前缀表示，确保无歧义即可。
- **查看日志：** `git log` 命令显示从最近到最远的提交日志，`--pretty=oneline`  参数使其一行显示一个。
- **查看历史命令：** `git reflog` 显示历史的每一次命令。可以找到某个想要去的版本的 commit id。
- **版本回退：** `git reset --hard HEAD^` 命令可以回退版本。也可以回退后用版本号回到新的版本。
- **撤销修改：**
  - **撤销暂存区的修改：** `git reset HEAD <file>` 可以把暂存区的修改撤销（Unstage），重新放回工作区。
  - **撤销工作区的修改：** `git checkout -- <file>` 把文件在工作区的修改撤销。让这个文件回到最近一次 `commit` 或 `add` 时的状态。

## 远程仓库

- **克隆远程仓库：** `git clone git@github.com:michaelliao/gitskills.git `。
- **关联远程仓库：** `git remote add origin https://github.com/Skywt2003/notes.git` 。关联一个远程库时必须给远程库指定一个名字，origin 是默认习惯命名。关联后，第一次推送使用 `git push -u origin master` 可以关联本地与远程的 master 分支。
  - 克隆和关联支持多种协议，但是 ssh 最快。
- **查看远程库信息：** `git remote -v`。使用 `-v` 为显示更详细的信息。
- **推送到远程：** `git push`。
  - 使用 `git push origin master` 将推送 master 分支的修改。
- **从远程拉取：** `git pull`，需要当前分支与远程某个分支链接。可以用 `git branch --set-upstream-to=origin/dev dev` 链接。
- **删除远程库：** `git remote rm <name>`。name 指的是仓库名（如 origin）。此处的「删除」其实是解除了本地和远程的绑定关系，并不是物理上删除了远程库。

## 分支 Branch 管理

### 基础概念

- `HEAD` 指针指向的就是当前分支。分支指向该分支下最新的提交。
- 对于新创建的分支 `dev`，切换后 `HEAD` 指向这个分支，这个分支指向最新的提交。
- 将 `dev` 合并到 `master` 时，只要使 `master` 指向 `dev` 的最新提交即可。所以合并往往很迅速。
- **分支策略最佳实践：** 在 master 上保持稳定，仅用于发布新版本，在 dev 等分支上进行开发。
  - **dev 分支：** 团队协作时也需要远程同步。
  - **bug 分支：** 本地开发，一般无需推到远程。
  - **feature 分支： ** 视情况而定。

### 基本分支管理

- **创建并切换分支：** `git switch -c dev` 或者 `git checkout -b dev`。相当于以下两条命令。
  - **创建分支：** `git branch dev`。
  - **切换分支：** `git switch dev` 或者 `git checkout dev`。
- **查看当前分支：** `git branch`。列出的分支列表中，当前的分支前有星号 `*`。
- **合并分支：** `git merge dev` 表示合并指定分支到当前的分支。
  - **冲突：** 如果两个分支各自有了新的提交则合并时会出现冲突。`git status  `可以告诉我们冲突的文件。需要自行编辑文件解决冲突后才能进行合并操作。
  - **分支合并图：** 用 `git log --graph` 可以看到分支合并图。
  - **合并方式：**
    - **Fast forward 模式：** 通常合并分支时 Git 会用 Fast forward 模式，但这种模式下删除分支后会丢掉分支信息。
    - **普通模式：** 强制禁用 Fast forward 模式，Git 会在 merge 时生成一个新的 commit，可以记录分支信息。`git merge --no-ff -m "merge with no-ff" dev`。
- **推送分支：** `git push origin dev`。如果推送失败，先用 `git pull` 抓取远程的新提交。
- **创建和远程分支对应的分支：** `git checkout -b dev origin/dev`。相当于创建分支+链接分支。
- **链接分支： ** `git branch --set-upstream-to=origin/dev dev`，指定本地创建的 dev 分支和远程 origin/dev 分支链接。
- **删除分支：** `git branch -d dev`。

### 其他应用

- **暂存工作现场：** `git stash` 暂存工作现场，方便临时突然开始进行短期的紧急更改。
- **恢复工作现场： ** `git stash list` 查看列表，`git stash pop` 恢复并删除，相当于两条命令：
  - **恢复： ** `git stash apply`，也可以恢复指定的 stash：`git stash apply stash@{0}`。`pop` 同理。
  - **删除： ** `git stash drop`，亦可删除指定的 stash。
- **Cherry Pick：** `git cherry-pick 4c805e2`，将（其他分支上）特定的 commit 复制到当前分支。
- **Rebase： ** `git rebase`，把树状图中分叉的历史提交合并为一条线。使更为易读。

## 标签 Tag 管理

- tag 就像版本库的快照。其实它就是指向某个 commit 的指针。
- tag 可以命名，和某个 commit 绑定在一起，免去了一个十六进制字符串的繁琐。
- **创建标签：** `git tag <tagname>`。默认创建在 HEAD（当前分支的最新 commit）。`git tag <tagname> <commitid>` 可以给指定 commit 打标签。
  - **标签名：** 也可以用 `-a <tagname>` 来指定。
  - **说明文字：** `-m <message>`。
  - 标签总是和某个 commit 挂钩。如果这个 commit 出现在多个分支，这些分支上都可以看到这个标签。
- **查看所有标签：** `git tag`。标签不是按时间顺序列出，而是按字母排序的。
- **查看标签信息：** `git show <tagname>`。
- **删除标签：** `git tag -d <tagname>`。
- **推送标签到远程：** `git push origin <tagname>`。一次性推送全部可以用 `git push origin --tags`。
- **删除远程标签：** 先在本地删除，然后 `git push origin :refs/tags/<tagname>`。

## 自定义配置

### GIt 配置文件

- 每个 git 仓库的配置文件都放在 `.git/config` 中。用户配置文件在 `~/.gitconfig`。
- 配置时，加上 `--global` 表示配置对当前用户生效，否则对当前仓库生效。

### 忽略文件 .gitignore

- 在 Git 工作区的根目录下创建一个特殊的 `.gitignore` 文件，然后把要忽略的文件名填进去，Git 就会自动忽略这些文件。
- 感叹号 `!` 开头的是例外规则，将指定文件排除在  `.gitignore` 之外（例子：不要忽略 `.gitignore` 文件本身！）。
- 被忽略的文件无法通过 `git add` 添加。可以用 `-f` 强制添加，或者用 `git check-ignore -v <filename>` 查看为什么不能添加。
- `.gitignore` 也可以被版本管理！
- 井号 `#` 开头的是注释。

### 常用配置

- **配置别名：** `git config --global alias.st status` 告诉 Git，以后用 st 表示 status。
- **终端代理：** `git config --global http.proxy http://127.0.0.1:10800`，`git config --global https.proxy http://127.0.0.1:10800`。

