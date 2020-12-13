# Git 基础

## 获取 Git 仓库

**在已存在目录中初始化仓库**

如果你有一个尚未进行版本控制的项目目录，想要用 Git 来控制它，那么首先需要进入该项目目录中。

```shell
$ cd ../my_project
$ git init
```

该命令将创建一个名为 `.git` 的子目录，这个子目录含有你初始化的 Git 仓库中所有的必须文件。但是，在这个时候，我们仅仅是做了一个初始化的操作，你的项目里的文件还没有被跟踪。

如果在一个已存在文件的文件夹 ( 而非空文件夹 ) 中进行版本控制，你应该开始追踪这些文件并进行初始提交。可以通过 `git add` 命令来指定所需的文件来进行追踪，然后执行 `git commit` ：

```shell
$ git add *.c
$ git add LICENSE
$ git commit -m 'initial project version'
```

**克隆现有的仓库**

当你执行 `git clone` 命令的时候，默认配置下远程 Git 仓库中的每一个文件的每一个版本都将被拉取下来。事实上，如果你服务器的磁盘坏掉了，你通常可以使用任何一个克隆下来的用户端来重建服务器上的仓库 ( 虽然可能会丢失某些服务器端的钩子 ( hook ) 设置，但是所有版本的数据仍在，详见 [在服务器上搭建 Git](https://git-scm.com/book/zh/v2/ch00/_getting_git_on_a_server)  ) 。

克隆仓库的命令是 `git clone <url>` 。比如，要克隆 Git 的链接库 `libgit2`，可以用下面的命令：

```shell
$ git clone https://github.com/libgit2/libgit2
# 自定义本地仓库的名字
# $ git clone https://github.com/libgit2/libgit2 mylibgit
```

这会在当前目录下创建一个名为 “libgit2” 的目录，并在这个目录下初始化一个 `.git` 文件夹， 从远程仓库拉取下所有数据放入 `.git` 文件夹，然后从中读取最新版本的文件的拷贝。

## 记录每次更新到仓库

请记住，你工作目录下的每一个文件都不外乎这两种状态：**已跟踪** 或 **未跟踪**。已跟踪的文件是指那些被纳入了版本控制的文件，在上一次快照中有它们的记录，在工作一段时间后，它们的状态可能是未修改，已修改或已放入暂存区。简而言之，已跟踪的文件就是 Git 已经知道的文件。

工作目录中除已跟踪文件外的其它所有文件都属于未跟踪文件，它们既不存在于上次快照的记录中，也没有被放入暂存区。初次克隆某个仓库的时候，工作目录中的所有文件都属于已跟踪文件，并处于未修改状态。

编辑过某些文件之后，由于自上次提交后你对它们做了修改，Git 将它们标记为已修改文件。在工作时，你可以选择性地将这些修改过的文件放入暂存区，然后提交所有已暂存的修改，如此反复。

![文件的状态变化周期](https://git-scm.com/book/en/v2/images/lifecycle.png)

### 检查当前文件状态

可以用 `git status` 命令查看哪些文件处于什么状态。如果在克隆仓库后立即使用此命令，会看到类似这样的输出：

```shell
$ git status
On branch main
Your branch is up to date with 'origin/main'.
nothing to commit, working directory clean
```

这说明你现在的工作目录相当干净。换句话说，所有已跟踪文件在上次提交后都未被更改过。此外，上面的信息还表明，当前目录下没有出现任何处于未跟踪状态的新文件，否则 Git 会在这里列出来。

现在，让我们在项目下创建一个新的 `README.md` 文件。 如果之前并不存在这个文件，使用 `git status` 命令，你将看到一个新的未跟踪文件：

```shell
$ echo 'My Project' > README.md
$ git status

Untracked files:
  (use "git add <file>..." to include in what will be committed)
        README.md

nothing added to commit but untracked files present (use "git add" to track)
```

在状态报告中可以看到新建的 `README` 文件出现在 `Untracked files` 下面。未跟踪的文件意味着 Git 在之前的快照 ( 提交 ) 中没有这些文件。

### 跟踪新文件

使用命令 `git add` 开始跟踪一个文件。

```shell
$ git add README.md
```

此时再运行 `git status` 命令，会看到 `README` 文件已被跟踪，并处于暂存状态：

```shell
Changes to be committed:
  (use "git restore --staged <file>..." to unstage)
        new file:   README.md
```

只要在 `Changes to be committed` 这行下面的，就说明是已暂存状态。如果此时提交，那么该文件在你运行 `git add <files>` 时的版本将被留存在后续的历史记录中。`git add` 命令使用文件或目录的路径作为参数；如果参数是目录的路径，将递归地跟踪该目录下的所有文件。

### 暂存已修改的文件

现在我们来修改一个已被跟踪的文件。 如果你修改了一个名为 `CONTRIBUTING.md` 的已被跟踪的文件，然后运行 `git status` 命令，会看到下面内容：

```shell
Changes to be committed:
  (use "git restore --staged <file>..." to unstage)
        new file:   README.md

Changes not staged for commit:
  (use "git add <file>..." to update what will be committed)
  (use "git restore <file>..." to discard changes in working directory)
        modified:   CONTRIBUTING.md
```

文件 `CONTRIBUTING.md` 出现在 `Changes not staged for commit` 这行下面，说明已跟踪文件的内容发生了变化，但还没有放到暂存区。要暂存这次更新，需要运行 `git add` 命令。这是个多功能命令：可以用它开始跟踪新文件，或者把已跟踪的文件放到暂存区，还能用于合并时把有冲突的文件标记为已解决状态等。

```shell
Changes to be committed:
  (use "git restore --staged <file>..." to unstage)
        modified:   CONTRIBUTING.md
        new file:   README.md
```

现在两个文件都已暂存，下次提交时就会一并记录到仓库。假设此时，你想要在 `CONTRIBUTING.md` 里再加条注释。重新编辑存盘后，准备好提交。不过且慢，再运行 `git status` 看看：

```shell
Changes to be committed:
  (use "git restore --staged <file>..." to unstage)
        modified:   CONTRIBUTING.md
        new file:   README.md

Changes not staged for commit:
  (use "git add <file>..." to update what will be committed)
  (use "git restore <file>..." to discard changes in working directory)
        modified:   CONTRIBUTING.md
```

现在 `CONTRIBUTING.md` 文件同时出现在暂存区和非暂存区。这怎么可能呢？ 实际上 Git 只不过暂存了你运行 `git add` 命令时的版本。如果你现在提交，`CONTRIBUTING.md` 的版本是你最后一次运行 `git add` 命令时的那个版本，而不是你运行 `git commit` 时。所以，运行了 `git add` 之后又作了修订的文件，需要重新运行 `git add` 把最新版本重新暂存起来。

### 状态简览

`git status` 命令的输出十分详细，但其用语有些繁琐。Git 有一个选项可以帮你缩短状态命令的输出，这样可以以简洁的方式查看更改。如果你使用 `git status -s` 命令或 `git status --short` 命令，你将得到一种格式更为紧凑的输出。

```shell
$ git status -s
 M README
MM Rakefile
A  lib/git.rb
M  lib/simplegit.rb
?? LICENSE.txt
```

新添加的未跟踪文件前面有 `??` 标记，新添加到暂存区中的文件前面有 `A` 标记，修改过的文件前面有 `M` 标记。输出中有两栏，左栏指明了暂存区的状态，右栏指明了工作区的状态。例如，上面的状态报告显示：*README* 文件在工作区已修改但尚未暂存，*lib/simplegit.rb* 文件已修改且已暂存。*Rakefile* 文件已修改，暂存后又做了修改。

### 忽略文件

可以创建一个名为 `.gitignore` 的文件，列出要忽略的文件的模式。

```shell
$ cat .gitignore
*.[oa]
*~
```

第一行告诉 Git 忽略所有以 `.o` 或 `.a` 结尾的文件。第二行告诉 Git 忽略所有名字以波浪符 ( ~ ) 结尾的文件。

**文件 `.gitignore` 的格式规范如下：**

- 所有空行或者以 `#` 开头的行都会被 Git 忽略；
- 可以使用标准的 glob 模式匹配，它会递归地应用在整个工作区中；
- 匹配模式可以以 （ `/` ) 开头防止递归；
- 匹配模式可以以（ `/` ) 结尾指定目录；
- 要忽略指定模式以外的文件或目录，可以在模式前加上叹号 ( `!` ) 取反。

所谓的 glob 模式是指 shell 所使用的简化了的正则表达式。星号 ( `*` ) 匹配零个或多个任意字符；`[abc]` 匹配任何一个列在方括号中的字符 ( 这个例子要么匹配一个 a，要么匹配一个 b，要么匹配一个 c )；问号 ( `?` ) 只匹配一个任意字符；如果在方括号中使用短划线分隔两个字符，表示所有在这两个字符范围内的都可以匹配 ( 比如 `[0-9]` 表示匹配所有 0 到 9 的数字 ) 。使用两个星号 ( `**` ) 表示匹配任意中间目录，比如 `a/**/z` 可以匹配 `a/z` 、`a/b/z` 或 `a/b/c/z` 等。

再看一个 `.gitignore` 文件的例子：

```shell
# 忽略所有的 .a 文件
*.a
# 但跟踪所有的 lib.a，即便你在前面忽略了 .a 文件
!lib.a
# 只忽略当前目录下的 TODO 文件，而不忽略 subdir/TODO
/TODO
# 忽略任何目录下名为 build 的文件夹
build/
# 忽略 doc/notes.txt，但不忽略 doc/server/arch.txt
doc/*.txt
# 忽略 doc/ 目录及其所有子目录下的 .pdf 文件
doc/**/*.pdf
```

> **提示**
>
> GitHub 有一个十分详细的针对数十种项目及语言的 [`.gitignore` 文件列表](https://github.com/github/gitignore) 。

> **注意**
>
> 在最简单的情况下，一个仓库可能只根目录下有一个 `.gitignore` 文件，它递归地应用到整个仓库中。然而，子目录下也可以有额外的 `.gitignore` 文件。子目录中的 `.gitignore` 文件中的规则只作用于它所在的目录中。

### 查看已暂存和未暂存的修改

如果 `git status` 命令的输出对于你来说过于简略，而你想知道具体修改了什么地方，可以用 `git diff` 命令。稍后我们会详细介绍 `git diff`，你通常可能会用它来回答这两个问题：当前做的哪些更新尚未暂存？有哪些更新已暂存并准备好下次提交？虽然 `git status` 已经通过在相应栏下列出文件名的方式回答了这个问题，但 `git diff` 能通过文件补丁的格式更加具体地显示哪些行发生了改变。

假如再次修改 README 文件后暂存，然后编辑 `CONTRIBUTING.md` 文件后先不暂存。要查看尚未暂存的文件更新了哪些部分，不加参数直接输入 `git diff` ：

```shell
diff --git a/CONTRIBUTING.md b/CONTRIBUTING.md
index 51d69df..ac80afe 100644
--- a/CONTRIBUTING.md
+++ b/CONTRIBUTING.md
@@ -1,2 +1,3 @@
 Please include a nice description ...
-merged in.
+merged in. Also, split your changes into comprehensive chunks if your patch is
+longer than a dozen lines.

 If you are starting to work on a particular area, feel free to submit a PR
 that highlights your work in progress (and note in the PR title that it's
```

此命令比较的是工作目录中当前文件和暂存区域快照之间的差异。也就是修改之后还没有暂存起来的变化内容。

若要查看已暂存的将要添加到下次提交里的内容，可以用 `git diff --[staged | cached]` 命令。这将比对已暂存文件与最后一次提交的文件差异：

```shell
diff --git a/README.md b/README.md
new file mode 100644
index 0000000..56438c8
--- /dev/null
+++ b/README.md
@@ -0,0 +1,3 @@
+My Project
```

请注意，git diff 本身只显示尚未暂存的改动，而不是自上次提交以来所做的所有改动。所以有时候你一下子暂存了所有更新过的文件，运行 `git diff` 后却什么也没有，就是这个原因。

> **注意**
>
> 也可以使用 `git difftool` 命令来调用 emerge 或 vimdiff 等软件 ( 包括商业软件 ) 输出 diff 的分析结果。 使用 `git difftool --tool-help` 命令来看你的系统支持哪些 Git Diff 插件。

### 提交更新

运行提交命令 `git commit` 。这样会启动你选择的文本编辑器来输入提交说明。

> **注意**：启动的编辑器是通过 Shell 的环境 ( 或按照 [起步](https://git-scm.com/book/zh/v2/ch00/ch01-getting-started) 介绍的方式 ) 变量 `EDITOR` 指定的。

编辑器会显示类似下面的文本信息：

```shell
_
# Please enter the commit message for your changes. Lines starting
# with '#' will be ignored, and an empty message aborts the commit.
#
# On branch main
# Your branch is up to date with 'origin/main'.
#
# Changes to be committed:
#       modified:   CONTRIBUTING.md
#       new file:   README.md
#
# Changes not staged for commit:
#       modified:   CONTRIBUTING.md
#       modified:   README.md
#
~
~
.git/COMMIT_EDITMSG[+] [unix] (07:27 13/12/2020)           1,1 All
-- INSERT --
```

> **注意**：更详细的内容修改提示可以用 `-v` 选项查看。

也可以在 `commit` 命令后添加 `-m` 选项，将提交信息与命令放在同一行，如下所示：

```shell
git commit -m "Story 182: Fix benchmarks for speed"
[main 2958162] Story 182: Fix benchmarks for speed
 2 files changed, 4 insertions(+), 1 deletion(-)
 create mode 100644 README.md
```

提交后它会告诉你，当前是在哪个分支 ( `main` ) 提交的，本次提交的完整 SHA-1 校验和是什么 ( `2958162` )，以及在本次提交中，有多少文件修订过，多少行添加和删改过。

### 跳过使用暂存区域

在提交的时候给 `git commit` 加上 `-a` 选项，Git 就会自动把所有已经跟踪过的文件暂存起来一并提交，从而跳过 `git add` 步骤：

```shell
$ git commit -a -m 'added new benchmarks'
```

### 移除文件

要从暂存区域移除某个文件 ( 并连带从工作目录中删除 ) ，可以用 `git rm` 命令，这样以后就不会出现在未跟踪清单中了。

如果只是简单的从工作目录中手工删除文件，运行 `git status` 时就会在 `Changes not staged for commit` 部分 ( 也就是 **未暂存清单** ) 看到，需再次运行 `git rm` 记录此次移除文件的操作：

```shell
git rm PROJECTS.md
```

如果要删除之前修改过或已经放到暂存区的文件，则必须使用强制删除选项 `-f ` ，这样的数据不能被 Git 恢复。

如果只是想把文件从暂存区域移除，但仍然希望保留在当前工作目录中。则可以使用 `--cached` 选项：

```shell
$ git rm --cached README
```

`git rm` 命令后面可以列出文件或者目录的名字，也可以使用 `glob` 模式。比如：

```shell
$ git rm log/\*.log 
```

注意到星号 `*` 之前的反斜杠 `\` ，因为 Git 有它自己的文件模式扩展匹配方式，所以我们不用 shell 来帮忙展开。此命令删除 `log/` 目录下扩展名为 `.log` 的所有文件。类似的比如：

```shell
$ git rm \*~
```

该命令会删除所有名字以 `~` 结尾的文件。

### 移动文件

```shell
$ git mv file_from file_to
```

运行 `git mv` 就相当于运行了下面三条命令：

```shell
$ mv README.md README
$ git rm README.md
$ git add README
```

## [查看提交历史](https://git-scm.com/book/zh/v2/Git-基础-查看提交历史)

不传入任何参数的默认情况下，`git log` 会按时间先后顺序列出所有的提交，最近的更新排在最上面。

一个比较有用的选项是 `-p` 或 `--patch` ，它会显示每次提交所引入的差异 ( 按 **补丁** 的格式输出 )。你也可以限制显示的日志条目数量，例如使用 `-2` 选项来只显示最近的两次提交：

```shell
$ git log -p -2
```

想看到每次提交的简略统计信息，可以使用 `--stat` 选项：

```shell
$ git log --stat
```

另一个非常有用的选项是 `--pretty` 。这个选项可以使用不同于默认格式的方式展示提交历史。这个选项有一些内建的子选项供你使用。比如 `oneline` 会将每个提交放在一行显示，在浏览大量的提交时非常有用。另外还有 `short`，`full` 和 `fuller` 选项，它们展示信息的格式基本一致，但是详尽程度不一：

```shell
$ git log --pretty=oneline
```

最有意思的是 `format` ，可以定制记录的 [显示格式](https://git-scm.com/book/zh/v2/Git-基础-查看提交历史#pretty_format) 。

```shell
$ git log --pretty=format:"%h - %an, %ar : %s"
ca82a6d - Scott Chacon, 6 years ago : changed the version number
085bb3b - Scott Chacon, 6 years ago : removed unnecessary test
a11bef0 - Scott Chacon, 6 years ago : first commit
```

你一定奇怪 **作者** 和 **提交者** 之间究竟有何差别， 其实作者指的是实际作出修改的人，提交者指的是最后将此工作成果提交到仓库的人。所以，当你为某个项目发布补丁，然后某个核心成员将你的补丁并入项目时，你就是作者，而那个核心成员就是提交者。

当 `oneline` 或 `format` 与另一个 `log` 选项 `--graph` 结合使用时尤其有用。这个选项添加了一些 ASCII 字符串来形象地展示你的分支、合并历史：

```shell
$ git log --pretty=format:"%h %s" --graph
```

 [`git log` 的常用选项](https://git-scm.com/book/zh/v2/ch00/log_options) 列出了我们目前涉及到的和没涉及到的选项，以及它们是如何影响 log 命令的输出的。

### 限制输出长度

类似 `--since` 和 `--until` 这种按照时间作限制的选项很有用。例如，下面的命令会列出最近两周的所有提交：

```shell
$ git log --since=2.weeks
```

该命令可用的格式十分丰富——可以是类似 `"2008-01-15"` 的具体的某一天，也可以是类似 `"2 years 1 day 3 minutes ago"` 的相对日期。

还可以过滤出匹配指定条件的提交。 用 `--author` 选项显示指定作者的提交，用 `--grep` 选项搜索提交说明中的关键字。

> **注意**
>
> 你可以指定多个 `--author` 和 `--grep` 搜索条件，这样会只输出 **任意** 匹配 `--author` 模式和 `--grep` 模式的提交。然而，如果你添加了 `--all-match` 选项，则只会输出 **所有** 匹配 `--grep` 模式的提交。

另一个非常有用的过滤器是 `-S` ，它接受一个字符串参数，并且只会显示那些添加或删除了该字符串的提交。假设你想找出添加或删除了对某一个特定函数的引用的提交，可以调用：

```shell
$ git log -S function_name
```

最后一个很实用的 `git log` 选项是路径 ( path ) ，如果只关心某些文件或者目录的历史提交，可以在 git log 选项的最后指定它们的路径。因为是放在最后位置上的选项，所以用两个短划线 ( `--` ) 隔开之前的选项和后面限定的路径名。

```shell
$ git log -- "./README.md"
```

在 [限制 `git log` 输出的选项](https://git-scm.com/book/zh/v2/ch00/limit_options) 中列出了常用的选项。

> **隐藏合并提交**
>
> 按照你代码仓库的工作流程，记录中可能有为数不少的合并提交，它们所包含的信息通常并不多。为了避免显示的合并提交弄乱历史记录，可以为 `log` 加上 `--no-merges` 选项。

## 撤消操作

注意，有些撤消操作是不可逆的。这是在使用 Git 的过程中，会因为操作失误而导致之前的工作丢失的少有的几个地方之一。

有时候我们提交完了才发现漏掉了几个文件没有添加，或者提交信息写错了。此时，可以运行带有 `--amend` 选项的提交命令来重新提交：

```shell
$ git commit --amend
```

这个命令会将暂存区中的文件提交。如果自上次提交以来你还未做任何修改 ( 例如，在上次提交后马上执行了此命令 ) ，那么快照会保持不变，而你所修改的只是提交信息。

文本编辑器启动后，可以看到之前的提交信息。 编辑后保存会覆盖原来的提交信息。

例如，你提交后发现忘记了暂存某些需要的修改，可以像下面这样操作：

```shell
$ git commit -m 'initial commit'
$ git add forgotten_file
$ git commit --amend
```

最终你只会有一个提交，第二次提交将代替第一次提交的结果。

### 取消暂存的文件

如果你已经修改了两个文件并且想要将它们作为两次独立的修改提交，但是却意外地输入 `git add *` 暂存了它们两个。我们可以这样来取消暂存 `CONTRIBUTING.md` 文件：

```shell
$ git reset HEAD CONTRIBUTING.md
```

> **注意**
>
> `git reset` 确实是个危险的命令，如果加上了 `--hard` 选项则更是如此。然而在上述场景中，工作目录中的文件尚未修改，因此相对安全一些。

### 撤消对文件的修改

如果你并不想保留对 `CONTRIBUTING.md` 文件的修改怎么办？你该如何方便地撤消修改，将它还原成上次提交时的样子 ( 或者刚克隆完、刚把它放入工作目录时的样子) ？使用：

```shell
$ git checkout -- CONTRIBUTING.md
```

> <font color=red>**重要**</font>
>
> 请务必记得 `git checkout -- <file>` 是一个危险的命令。你对那个文件在本地的任何修改都会消失，Git 会用最近提交的版本覆盖掉它。除非你确实清楚不想要对那个文件的本地修改了，否则请不要使用这个命令。

## [远程仓库的使用](https://git-scm.com/book/zh/v2/Git-基础-远程仓库的使用)

> <font color=red>**远程仓库可以在你的本地主机上**</font>
>
> 你完全可以在一个 “远程” 仓库上工作，而实际上它在你本地的主机上。词语 “远程” 未必表示仓库在网络或互联网上的其它位置，而只是表示它在别处。在这样的远程仓库上工作，仍然需要和其它远程仓库上一样的标准推送、拉取和抓取操作。

### 查看远程仓库

如果想查看你已经配置的远程仓库服务器，可以运行 `git remote` 命令。它会列出你指定的每一个远程服务器的简写。如果你已经克隆了自己的仓库，那么至少应该能看到 origin，这是 Git 给你克隆的仓库服务器默认名：

```shell
$ git remote
origin
```

你也可以指定 `-v` 选项，会显示需要读写远程仓库使用的 Git 保存简写与其对应的 URL。

```shell
$ git remote -v
origin	https://github.com/schacon/ticgit (fetch)
origin	https://github.com/schacon/ticgit (push)
```

如果你的远程仓库不止一个，该命令会将它们全部列出。例如，与几个协作者合作的，拥有多个远程仓库的仓库看起来像下面这样：

```shell
$ cd grit
$ git remote -v
bakkdoor  https://github.com/bakkdoor/grit (fetch)
bakkdoor  https://github.com/bakkdoor/grit (push)
cho45     https://github.com/cho45/grit (fetch)
cho45     https://github.com/cho45/grit (push)
defunkt   https://github.com/defunkt/grit (fetch)
defunkt   https://github.com/defunkt/grit (push)
koke      git://github.com/koke/grit.git (fetch)
koke      git://github.com/koke/grit.git (push)
origin    git@github.com:mojombo/grit.git (fetch)
origin    git@github.com:mojombo/grit.git (push)
```

### 添加远程仓库

之前的章节中已经提到并展示了 `git clone` 命令是如何自行添加远程仓库的，不过这里将告诉你如何自己来添加它。运行 `git remote add <shortname> <url>` 添加一个新的远程 Git 仓库，同时指定一个方便使用的简写：

```shell
$ git remote
origin
$ git remote add pb https://github.com/paulboone/ticgit
$ git remote -v
origin	https://github.com/schacon/ticgit (fetch)
origin	https://github.com/schacon/ticgit (push)
pb	https://github.com/paulboone/ticgit (fetch)
pb	https://github.com/paulboone/ticgit (push)
```

现在你可以在命令行中使用字符串 `pb` 来代替整个 URL。例如，如果你想拉取 Paul 的仓库中有但你没有的信息，可以运行 `git fetch pb` ：

```shell
$ git fetch pb
```

现在 Paul 的 main 分支可以在本地通过 `pb/main` 访问到。

### 从远程仓库中抓取与拉取

`git fetch <remote>` 这个命令执行完成后，你将会拥有那个远程仓库中所有分支的引用，可以随时合并或查看。

如果你使用 `clone` 命令克隆了一个仓库，命令会自动将其添加为远程仓库并默认以 “origin” 为简写。 所以，`git fetch origin` 会抓取克隆 ( 或上一次抓取 ) 后新推送的所有工作。必须注意 `git fetch` 命令只会将数据下载到你的本地仓库，它并不会自动合并或修改你当前的工作。当准备好时你必须手动将其合并入你的工作。

如果你的当前分支设置了跟踪远程分支，那么可以用 `git pull` 命令来自动抓取后合并该远程分支到当前分支。默认情况下，`git clone` 命令会自动设置本地 main 分支跟踪克隆的远程仓库的 `main` 分支 ( 或其它名字的默认分支 ) 。

### 推送到远程仓库

当你想分享你的项目时，必须将其推送到上游。这个命令很简单：`git push <remote> <branch>` 。当你想要将 `main` 分支推送到 `origin` 服务器时 ( 再次说明，克隆时通常会自动帮你设置好那两个名字 ) ，那么运行这个命令就可以将你所做的备份到服务器：

```shell
$ git push origin master
```

只有当你有所克隆服务器的写入权限，并且之前没有人推送过时，这条命令才能生效。当你和其他人在同一时间克隆，他们先推送到上游然后你再推送到上游，你的推送就会毫无疑问地被拒绝。你必须先抓取他们的工作并将其合并进你的工作后才能推送。