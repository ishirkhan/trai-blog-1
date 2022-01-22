# Git 分支

## [分支简介](https://git-scm.com/book/zh/v2/Git-分支-分支简介)

### 创建分支

```shell
$ git branch testing
```

这会在当前所在的提交对象上创建一个指针。

那么，Git 又是怎么知道当前在哪一个分支上呢？很简单，它有一个名为 `HEAD` 的特殊指针，指向当前所在的本地分支 ( 译注：将 `HEAD` 想象为当前分支的别名 ) 。

你可以简单地使用 `git log` 命令查看各个分支当前所指的对象。提供这一功能的参数是 `--decorate` 。

```shell
$ git log --oneline --decorate
f30ab (HEAD -> main, testing) add feature
34ac2 Fixed bug
98ca9 The initial commit of my project
```

正如你所见，当前 `main` 和 `testing` 分支均指向校验和以 `f30ab` 开头的提交对象。

### 切换分支

```shell
$ git checkout testing
```

这样 `HEAD` 就指向 `testing` 分支了。

> **创建新分支的同时切换过去**
>
> 这可以用 `git checkout -b <newbranchname>` 一条命令搞定。

## [分支的新建与合并 ](https://git-scm.com/book/zh/v2/Git-分支-分支的新建与合并)

### 新建分支

想要新建一个分支并同时切换到那个分支上，你可以运行一个带有 `-b` 参数的 `git checkout` 命令：

```shell
$ git checkout -b iss53
Switched to a new branch "iss53"
```

它是下面两条命令的简写：

```shell
$ git branch iss53
$ git checkout iss53
```

但是，在你这么做之前，要留意你的工作目录和暂存区里那些还没有被提交的修改，它可能会和你即将检出的分支产生冲突从而阻止 Git 切换到该分支。最好的方法是，在你切换分支之前，保持好一个干净的状态。

你可以运行你的测试，确保你的修改是正确的，然后将分支 `hotfix` 合并回你的 `main` 分支：

```shell
$ git checkout main
$ git merge hotfix
```

你可以使用带 `-d` 选项的 `git branch` 命令来删除分支：

```shell
$ git branch -d hotfix
```

### 遇到冲突时的分支合并

如果你在两个不同的分支中，对同一个文件的同一个部分进行了不同的修改，Git 就没法干净的合并它们。在合并它们的时候就会产生合并冲突。

你可以在合并冲突后的任意时刻使用 `git status` 命令来查看那些因包含合并冲突而处于未合并 ( unmerged ) 状态的文件。

Git 会在有冲突的文件中加入标准的冲突解决标记，这样你可以打开这些包含冲突的文件然后手动解决冲突。出现冲突的文件会包含一些特殊区段，看起来像下面这个样子：

```shell
<<<<<<< HEAD:index.html
<div id="footer">contact : email.support@github.com</div>
=======
<div id="footer">
 please contact us at support@github.com
</div>
>>>>>>> iss53:index.html
```

这表示 `HEAD` 所指示的版本在这个区段的上半部分 ( `=======` 的上半部分 ) ，而 `iss53` 分支所指示的版本在 `=======` 的下半部分。为了解决冲突，你必须选择使用由 `=======` 分割的两部分中的一个，或者你也可以自行合并这些内容。例如，你可以通过把这段内容换成下面的样子来解决冲突：

```html
<div id="footer">
please contact us at email.support@github.com
</div>
```

上述的冲突解决方案仅保留了其中一个分支的修改，并且 `<<<<<<<` 、`=======` 和 `>>>>>>>` 这些行被完全删除了。在你解决了所有文件里的冲突之后，对每个文件使用 `git add` 命令来将其标记为冲突已解决。

如果你想使用图形化工具来解决冲突，你可以运行 `git mergetool` ，该命令会为你启动一个合适的可视化合并工具，并带领你一步一步解决这些冲突：

```shell
$ git mergetool

This message is displayed because 'merge.tool' is not configured.
See 'git mergetool --tool-help' or 'git help config' for more details.
'git mergetool' will now attempt to use one of the following tools:
opendiff kdiff3 tkdiff xxdiff meld tortoisemerge gvimdiff diffuse diffmerge ecmerge p4merge araxis bc3 codecompare vimdiff emerge
Merging:
index.html

Normal merge conflict for 'index.html':
  {local}: modified file
  {remote}: modified file
Hit return to start merge resolution tool (opendiff):
```

如果你想使用除默认工具外的其他合并工具，你可以在 “下列工具中 ( one of the following tools ) ” 这句后面看到所有支持的合并工具。

## [分支管理](https://git-scm.com/book/zh/v2/Git-分支-分支管理)

`git branch` 命令不只是可以创建与删除分支。如果不加任何参数运行它，会得到当前所有分支的一个列表：

```shell
$ git branch
  iss53
* main
  testing
```

如果需要查看每一个分支的最后一次提交，可以运行 `git branch -v` 命令：

```shell
$ git branch -v
  iss53   93b412c fix javascript issue
* master  7a98805 Merge branch 'iss53'
  testing 782fd34 add scott to the author list in the readmes
```

`--merged` 与 `--no-merged` 选项可以过滤这个列表中已经合并或尚未合并到当前分支的分支。如果要查看哪些分支已经合并到当前分支，可以运行 `git branch --merged` ：

```shell
$ git branch --merged
  iss53
* master
```

> 你总是可以提供一个附加的参数来查看其它分支的合并状态而不必检出它们。例如，尚未合并到 `master` 分支的有哪些？
>
> ```shell
> $ git checkout testing
> $ git branch --no-merged master
>   topicA
>   featureB
> ```

