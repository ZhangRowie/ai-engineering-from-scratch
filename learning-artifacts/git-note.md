# Git 学习笔记
- 我已经配置了Git的提交者姓名和邮箱
- 我创建并切换到了my-progress学习分支
- Git commit用来记录一次版本，git push用来上传提交
- 修改文件并保存，不会自动更新之前的提交；需要再次add和commit。

## 1. 先分清几个概念

| 概念 | 含义 |
|---|---|
| Git | 安装在电脑上的版本管理工具，用来记录修改、查看历史；本地提交不需要联网 |
| GitHub | 托管 Git 仓库的网站，用来共享代码、保存远程副本和协作 |
| 仓库（repository / repo） | 由 Git 管理的项目及其版本历史 |
| 工作区 | 当前能直接编辑的项目文件 |
| 暂存区（staging area） | 准备放进下一次提交的内容；`git add` 把选中文件当时的内容放入其中 |
| 提交（commit） | 一个可以追溯的项目版本，有自己的标识和说明 |
| 分支（branch） | 指向某次提交、可以随新提交前进的名字，方便沿不同方向继续工作 |
| 远程（remote） | 本地保存的一组远程仓库地址配置，例如 `origin`、`upstream` |
| Fork | 在 GitHub 上创建一份属于自己账号的仓库副本 |

保存文件、暂存修改、提交版本、上传提交是不同的动作：

1. 编辑并保存：文件内容写入电脑磁盘。
2. `git add`：选择这次准备记录的修改。
3. `git commit`：把暂存内容记录为本地版本。
4. `git push`：把本地提交上传到指定的远程仓库。

之前的提交不会因为我再次编辑文件而自动改变。如果 `git add` 后又修改了文件，需要再次 `git add`，才能把后来的修改也放进下一次提交。

## 2. 配置提交者身份：git config

下面是配置语法，姓名和邮箱需要替换成自己的信息；已经配置好就不用重复执行。

```bash
git config --global user.name "你的名字或昵称"
git config --global user.email "你的提交邮箱"
```

| 部分 | 含义 |
|---|---|
| `git config` | 读取或修改 Git 配置；注意不是 `git cofin` |
| `--global` | 为当前电脑用户设置默认配置，通常对其多个仓库生效 |
| `user.name` | 提交记录中的作者姓名或昵称 |
| `user.email` | 提交记录中的作者邮箱 |

查看当前仓库实际使用的配置：

```bash
git config --get user.name
git config --get user.email
```

这些信息用于标记提交作者，不是 GitHub 登录凭据。姓名和邮箱会写进之后的提交记录；可以使用 GitHub 提供的隐私邮箱。

## 3. 创建和切换分支：git checkout

```bash
git checkout -b my-progress
```

`-b` 表示创建新分支；这条命令从当前位置创建 `my-progress`，并立即切换过去。它不会创建另一个项目文件夹，也不会上传内容。

分支已经存在时，切换不再加 `-b`：

```bash
git checkout my-progress
git checkout main
```

上面两行分别表示切到学习分支、切到主分支，按需要选择执行。切换后可以通过 `git status` 查看当前分支。

分支可以包含不同的文件内容。笔记只提交在 `my-progress`、还没合并到 `main` 时，切到 `main` 会暂时看不到笔记；切回来就会恢复，原来的提交没有被删除。切换前先查看状态，避免把尚未提交的修改和分支里的已提交内容混淆。

## 4. 查看、暂存与提交修改

### git status：查看当前状态

```bash
git status
```

| 常见输出 | 含义 |
|---|---|
| `On branch my-progress` | 当前处于 `my-progress` 分支 |
| `Untracked files` | 有新文件尚未被 Git 跟踪 |
| `Changes not staged for commit` | 已跟踪文件有修改，但这些修改还没暂存 |
| `Changes to be committed` | 暂存区中有准备提交的修改 |
| `modified` | 文件相对于比较基准有改动 |
| `nothing to commit, working tree clean` | 当前没有需要提交的修改；不是文件被清空了，也不代表已经上传 |

在 `learning-artifacts` 文件夹内执行时，未跟踪目录可能显示为 `./`，表示当前目录。

### git add：暂存指定文件

在项目根目录执行：

```bash
git add learning-artifacts/git-note.md
```

如果当前已经在 `learning-artifacts` 目录中，则执行：

```bash
git add git-note.md
```

路径相对于终端当前所在目录。`git add` 不会替我创建文件。课程中的 `git add file.py` 使用了示例文件名，如果实际不存在这个文件，就会提示 `pathspec 'file.py' did not match any files`。

我的实际笔记名是 `git-note.md`，不是 `git-notes.md`；文件名必须对应。

### git commit -m：创建本地版本

```bash
git commit -m "docs: add first Git learning note"
```

- `git commit`：把暂存区中的修改记录为一次提交。
- `-m`：后面提供提交说明（message）。
- 引号中的文字：描述这次实际做了什么，可以写中文。
- `docs:`：表示文档类修改的命名约定，不是 Git 的特殊指令。

课程示例中的 `"Add perceptron implementation"` 意思是“添加感知机的实现”，只是提交说明。它不会替我实现感知机，也不需要照抄。

`git commit` 只记录本地版本，不会上传到 GitHub。

## 5. 查看差异与提交历史

### git diff：看还没暂存的修改

```bash
git diff
```

默认比较已跟踪文件的工作区内容与暂存区内容；不会显示未跟踪文件的内容。

本次新增笔记时看到了：

```diff
diff --git a/learning-artifacts/git-note.md b/learning-artifacts/git-note.md
index 6dd5aa03..11c580cf 100644
--- a/learning-artifacts/git-note.md
+++ b/learning-artifacts/git-note.md
@@ -2,3 +2,4 @@
 - 我已经配置了Git的提交者姓名和邮箱
 - 我创建并切换到了my-progress学习分支
 - Git commit用来记录一次版本，git push用来上传提交
+- 修改文件并保存，不会自动更新之前的提交；需要再次add和commit。
```

| 标记 | 含义 |
|---|---|
| `a/...`、`b/...` | 比较前后版本的标记，不是两个真实文件夹 |
| `---`、`+++` | 分别标明比较前和比较后的文件 |
| `@@ -2,3 +2,4 @@` | 此片段在旧内容中从第 2 行开始、包含 3 行；在新内容中从第 2 行开始、包含 4 行 |
| 内容行开头的 `+` | 新增的内容 |
| 内容行开头的 `-` | 删除的内容 |
| 内容行开头的空格 | 用来提供上下文的未变化内容 |
| `index ...` | Git 的文件内容标识等信息，初学时可以略过 |

笔记本身的 Markdown 列表符号是 `-`，因此新增该行会显示为 `+- ...`：第一个 `+` 是 Git 的标记，后面的 `-` 才是文件内容。

暂存后查看准备提交的修改：

```bash
git diff --staged
```

它比较暂存区与当前最新提交。如果所有修改都已暂存，普通 `git diff` 可能没有输出，但 `git diff --staged` 仍会显示修改。

查看差异或历史时，如果进入翻页界面，可以按 `q` 退出。

### git log：看已经提交的历史

```bash
git log -1 --oneline
git log -2 --oneline
```

| 部分 | 含义 |
|---|---|
| `git log` | 查看当前分支可追溯的提交历史 |
| `-1`、`-2` | 分别只显示最近 1 次、2 次提交 |
| `--oneline` | 每次提交用一行简短显示；前面有两个短横线 |

例如：

```text
acb1d3d8 docs: record Git add and commit workflow
f0b74392 docs: add first Git learning note
```

前面是提交的简短标识，后面是提交说明；新的提交通常显示在上面。查看历史不会修改文件。

## 6. Fork、origin 和 upstream

Fork 在 GitHub 网站上完成，不会自动修改本地仓库的远程配置。即使已经 Fork，本地最初的 `origin` 仍可能指向课程作者。

查看本地记录的远程地址：

```bash
git remote -v
```

`-v` 表示显示详细信息，包括远程地址。一个名称通常有两行：

- `(fetch)`：获取提交时使用的地址。
- `(push)`：上传提交时使用的地址。

两行地址相同时，表示同一个远程仓库的两种用途，不是两个 `origin`。

我已完成以下配置，后续不用重复添加：

```bash
git remote rename origin upstream
git remote add origin https://github.com/ZhangRowie/ai-engineering-from-scratch.git
```

第一条将原来的远程名称从 `origin` 改为 `upstream`，地址不变；第二条用 `origin` 这个名字记录我自己的仓库地址。它们修改的是本地配置，不会给 GitHub 上的仓库改名。

| 远程名称 | 对应地址 | 用途 |
|---|---|---|
| `origin` | `https://github.com/ZhangRowie/ai-engineering-from-scratch.git` | 上传自己的学习成果 |
| `upstream` | `https://github.com/rohitg00/ai-engineering-from-scratch.git` | 后续获取作者的课程更新 |

这些名称是约定，不是特殊权限。同一个本地仓库不能有两个同名远程配置。重命名前先看 URL：只有原 `origin` 指向作者仓库时，才按上面的步骤重命名。

`origin`、`upstream` 是远程仓库的名称；`main`、`my-progress` 是分支名称，两者不要混淆。

## 7. 上传提交与分支跟踪：git push -u

首次上传学习分支时使用：

```bash
git push -u origin my-progress
```

| 部分 | 含义 |
|---|---|
| `git push` | 上传本地提交，并更新远程分支 |
| `origin` | 指定目的地为我的 GitHub 仓库 |
| `my-progress` | 指定上传本地学习分支，本例对应远程同名分支 |
| `-u` | 成功推送后，记住本地分支与远程分支的跟踪关系 |

本例让本地 `my-progress` 跟踪 `origin/my-progress`。以后在这个分支上，采用当前配置时通常只需执行：

```bash
git push
```

`-u` 可以理解成“这次上传，并记住默认目的地”。它不会自动上传未来的修改，仍然需要我提交后再执行 `git push`。

每个分支分别设置跟踪关系。本地 `main` 上传到自己的仓库时使用：

```bash
git push -u origin main
```

这会让本地 `main` 跟踪 `origin/main`。推送 `my-progress` 不会自动更新远程 `main`。

“上游跟踪分支”（upstream tracking branch）中的 upstream 是关系名称，不等于一定要跟踪名叫 `upstream` 的远程仓库。我的分支可以跟踪 `origin` 中的分支。

## 8. 合并分支与 Fast-forward

合并时，当前分支是接收修改的分支。在项目根目录执行：

```bash
git checkout main
git merge my-progress
```

意思是先切换到 `main`，再把 `my-progress` 的提交历史合并进来。

如果之前在只存在于学习分支的 `learning-artifacts` 目录里，先执行 `cd ..` 回到项目根目录，再切换分支，避免当前目录在切换时消失。

本次 `main` 没有独立新增的提交，只落后于 `my-progress` 的两次笔记提交，因此可以快进（Fast-forward）：把 `main` 的位置推进到学习分支的最新提交，不需要额外生成一次合并提交。

`Fast-forward` 出现在执行 `git merge` 时的输出里，例如：

```text
Updating fe27da89..acb1d3d8
Fast-forward
```

`git status` 不会重复显示刚才的合并方式。在 `main` 上运行 `git log -2 --oneline`，可以确认那两次学习提交已包含进来。

合并不会自动删除 `my-progress`，也不会自动上传。合并完成后再用 `git push -u origin main`，才会更新自己 GitHub 仓库的 `main`。

## 9. 顺便学到的 Shell 语法

这些是终端 Shell 的功能，不属于 Git。

| 命令或符号 | 含义 |
|---|---|
| `cd ..` | 切换到当前目录的上一级 |
| `mkdir -p learning-artifacts` | 创建文件夹；必要时一并创建父目录，目录已存在时不报错 |
| `echo "文字"` | 输出文字 |
| `>` | 把输出写入文件；文件存在时先清空原内容，不存在时创建 |
| `>>` | 把输出追加到文件末尾；保留原内容，不存在时创建 |
| `./` | 当前目录 |

在 `learning-artifacts` 目录里给笔记增加一行，应使用追加：

```bash
echo '- 修改文件并保存，不会自动更新之前的提交；需要再次 add 和 commit。' >> git-note.md
```

如果把 `>>` 写成 `>`，就会覆盖原笔记。执行前也要核对文件名和所在目录。

## 10. 以后复习时的操作顺序

下面是一轮练习的参考流程。执行前先确认分支和文件状态，不需要为了复习重复创建已存在的分支或远程配置。以下路径以项目根目录为基准。

1. 用 `git status` 确认状态；开始新的学习分支工作时切到已有的 `my-progress`。
2. 编辑并保存 `learning-artifacts/git-note.md`。
3. 用 `git diff` 检查修改。
4. 用 `git add learning-artifacts/git-note.md` 暂存。
5. 用 `git diff --staged` 核对准备提交的内容。
6. 用 `git commit -m "docs: update Git learning notes"` 记录版本。
7. 用 `git log -1 --oneline` 查看提交，用 `git status` 检查剩余修改。
8. 在已经建立跟踪关系的学习分支上，用 `git push` 上传。
9. 需要把成果纳入主分支时，切到 `main`，执行 `git merge my-progress`，再将 `main` 推送到 `origin`。

这份整理保存到文件后，仍需要新的 `git add` 和 `git commit` 才会成为一个新版本。
