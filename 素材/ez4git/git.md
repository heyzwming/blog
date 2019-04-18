# 远程仓库的使用

## 从远程仓库抓取与拉取




## 推送到远程仓库


## 查看远程仓库


## 远程仓库的移除和命名



# Git分支

几乎所有的版本控制系统都以某种形式支持分支。 使用分支意味着你可以把你的工作从开发主线上分离开来，以免影响开发主线。

## 分支简介 

为了真正理解 Git 处理分支的方式，我们需要回顾一下 Git 是如何保存数据的。

Git 保存的不是文件的变化或者差异，而是一系列不同时刻的文件快照。它是“全量存储”而不是“差量存储”。

在进行提交操作时，Git 会保存一个提交对象（commit object），该提交对象会包含一个指向暂存内容快照的指针。 （该提交对象还包含了作者的姓名和邮箱、提交时输入的信息以及指向它的父对象的指针。首次提交产生的提交对象没有父对象，普通提交操作产生的提交对象有一个父对象，而由多个分支合并产生的提交对象有多个父对象）

为了更加形象地说明，我们假设现在有一个工作目录，里面包含了三个将要被暂存和提交的文件。 暂存操作会为每一个文件计算校验和（使用我们在 起步 中提到的 SHA-1 哈希算法），然后会把当前版本的文件快照保存到 Git 仓库中（Git 使用 blob 对象来保存它们），最终将校验和加入到暂存区域等待提交：

$ git add README test.rb LICENSE
$ git commit -m 'The initial commit of my project'
当使用 git commit 进行提交操作时，Git 会先计算每一个子目录（本例中只有项目根目录）的校验和，然后在 Git 仓库中这些校验和保存为树对象。 随后，Git 便会创建一个提交对象，它除了包含上面提到的那些信息外，还包含指向这个树对象（项目根目录）的指针。如此一来，Git 就可以在需要的时候重现此次保存的快照。

现在，Git 仓库中有五个对象：三个 blob 对象（保存着文件快照）、一个树对象（记录着目录结构和 blob 对象索引）以及一个提交对象（包含着指向前述树对象的指针和所有提交信息）。

首次提交对象及其树结构。
Figure 9. 首次提交对象及其树结构
做些修改后再次提交，那么这次产生的提交对象会包含一个指向上次提交对象（父对象）的指针。

提交对象及其父对象。
Figure 10. 提交对象及其父对象
Git 的分支，其实本质上仅仅是指向提交对象的可变指针。 Git 的默认分支名字是 master。 在多次提交操作之后，你其实已经有一个指向最后那个提交对象的 master 分支。 它会在每次的提交操作中自动向前移动。

> Note： Git 的 “master” 分支并不是一个特殊分支。 它就跟其它分支完全没有区别。 之所以几乎每一个仓库都有 master 分支，是因为 git init 命令默认创建它，并且大多数人都懒得去改动它。

分支及其提交历史。
Figure 11. 分支及其提交历史
分支创建
Git 是怎么创建新分支的呢？ 很简单，它只是为你创建了一个可以移动的新的指针。 比如，创建一个 testing 分支， 你需要使用 git branch 命令：

$ git branch testing
这会在当前所在的提交对象上创建一个指针。

两个指向相同提交历史的分支。
Figure 12. 两个指向相同提交历史的分支
那么，Git 又是怎么知道当前在哪一个分支上呢？ 也很简单，它有一个名为 HEAD 的特殊指针。 请注意它和许多其它版本控制系统（如 Subversion 或 CVS）里的 HEAD 概念完全不同。 在 Git 中，它是一个指针，指向当前所在的本地分支（译注：将 HEAD 想象为当前分支的别名）。 在本例中，你仍然在 master 分支上。 因为 git branch 命令仅仅 创建 一个新分支，并不会自动切换到新分支中去。

HEAD 指向当前所在的分支。
Figure 13. HEAD 指向当前所在的分支
你可以简单地使用 git log 命令查看各个分支当前所指的对象。 提供这一功能的参数是 --decorate。

$ git log --oneline --decorate
f30ab (HEAD, master, testing) add feature #32 - ability to add new
34ac2 fixed bug #1328 - stack overflow under certain conditions
98ca9 initial commit of my project
正如你所见，当前 “master” 和 “testing” 分支均指向校验和以 f30ab 开头的提交对象。

分支切换
要切换到一个已存在的分支，你需要使用 git checkout 命令。 我们现在切换到新创建的 testing 分支去：

$ git checkout testing
这样 HEAD 就指向 testing 分支了。

HEAD 指向当前所在的分支。
Figure 14. HEAD 指向当前所在的分支
那么，这样的实现方式会给我们带来什么好处呢？ 现在不妨再提交一次：

$ vim test.rb
$ git commit -a -m 'made a change'
HEAD 分支随着提交操作自动向前移动。
Figure 15. HEAD 分支随着提交操作自动向前移动
如图所示，你的 testing 分支向前移动了，但是 master 分支却没有，它仍然指向运行 git checkout 时所指的对象。 这就有意思了，现在我们切换回 master 分支看看：

$ git checkout master
检出时 HEAD 随之移动。
Figure 16. 检出时 HEAD 随之移动
这条命令做了两件事。 一是使 HEAD 指回 master 分支，二是将工作目录恢复成 master 分支所指向的快照内容。 也就是说，你现在做修改的话，项目将始于一个较旧的版本。 本质上来讲，这就是忽略 testing 分支所做的修改，以便于向另一个方向进行开发。

> Note:分支切换会改变你工作目录中的文件
在切换分支时，一定要注意你工作目录里的文件会被改变。 如果是切换到一个较旧的分支，你的工作目录会恢复到该分支最后一次提交时的样子。 如果 Git 不能干净利落地完成这个任务，它将禁止切换分支。

我们不妨再稍微做些修改并提交：

$ vim test.rb
$ git commit -a -m 'made other changes'
现在，这个项目的提交历史已经产生了分叉（参见 项目分叉历史）。 因为刚才你创建了一个新分支，并切换过去进行了一些工作，随后又切换回 master 分支进行了另外一些工作。 上述两次改动针对的是不同分支：你可以在不同分支间不断地来回切换和工作，并在时机成熟时将它们合并起来。 而所有这些工作，你需要的命令只有 branch、checkout 和 commit。






## 分支的新建与合并


### 分支的新建与合并
让我们来看一个简单的分支新建与分支合并的例子，实际工作中你可能会用到类似的工作流。 你将经历如下步骤：

1) 开发某个网站。

2) 为实现某个新的需求，创建一个分支。

3) 在这个分支上开展工作。

正在此时，你突然接到一个电话说有个很严重的问题需要紧急修补。 你将按照如下方式来处理：

1) 切换到你的线上分支（production branch）/master branch。

2) 为这个紧急任务新建一个分支，并在其中修复它。

3) 在测试通过之后，切换回线上分支，然后合并这个修补分支，最后将改动推送到线上分支。

4) 切换回你最初工作的分支上，继续工作。

### 新建分支
首先，我们假设你正在你的项目上工作，并且已经有一些提交。

![Figure 18. 一个简单提交历史]()

现在，你已经决定要解决你的公司使用的问题追踪系统中的 #53 问题。 想要新建一个分支并同时切换到那个分支上，你可以运行一个带有 -b 参数的 git checkout 命令：

```
$ git checkout -b iss53
Switched to a new branch "iss53"
```

它是下面两条命令的简写：

```
$ git branch iss53
$ git checkout iss53
```


![Figure 19. 创建一个新分支指针]()

你继续在 #53 问题上工作，并且做了一些提交。 在此过程中，iss53 分支在不断的向前推进，因为你已经检出(checkout)到该分支（也就是说，你的 HEAD 指针指向了 iss53 分支）

```
$ vim index.html
$ git commit -a -m 'added a new footer [issue 53]'
```

~[Figure 20. iss53 分支随着工作的进展向前推进]()

现在你接到那个电话，有个紧急问题等待你来解决。 有了 Git 的帮助，你不必把这个紧急问题和 iss53 的修改混在一起。

你所要做的仅仅是切换回 master 分支。

但是，在你这么做之前，要留意你的工作目录和暂存区里那些还没有被提交的修改，它可能会和你即将检出的分支产生冲突从而阻止 Git 切换到该分支。 

最好的方法是，在你切换分支之前，保存(stashing)并提交（commit amending）你对于iss53分支的进度。
```
$ git checkout master
Switched to branch 'master'
```

这个时候(切换回到master分支)，你的工作目录和你在开始 #53 问题之前一模一样，现在你可以专心修复紧急问题了。 

请牢记：当你切换分支的时候，Git 会重置你的工作目录，使其看起来像回到了你在那个分支上最后一次提交的样子。 

Git 会自动添加、删除、修改文件以确保此时你的工作目录和这个分支最后一次提交时的样子一模一样。

接下来，你要修复这个紧急问题。 让我们建立一个针对该紧急问题的分支（hotfix branch），在该分支上工作直到问题解决：

```
$ git checkout -b hotfix
Switched to a new branch 'hotfix'
$ vim index.html
$ git commit -a -m 'fixed the broken email address'
[hotfix 1fb7853] fixed the broken email address
 1 file changed, 2 insertions(+)
```

![Figure 21. 基于 master 分支的紧急问题分支 hotfix branch]()

你可以运行你的测试，确保你的修改是正确的，然后将其合并回你的 master 分支来部署到线上。 

你可以使用 `git merge` 命令来达到上述目的：

```
$ git checkout master
$ git merge hotfix
Updating f42c576..3a0874c
Fast-forward
 index.html | 2 ++
 1 file changed, 2 insertions(+)
```

现在，最新的修改已经在 master 分支所指向的提交快照中，你可以着手发布该修复了。


![Figure 22. `master` 被快进到 `hotfix`]()

关于这个紧急问题的解决方案发布之后，你准备回到被打断之前时的工作中。 然而，你应该先删除 hotfix 分支，因为你已经不再需要它了 —— master 分支已经指向了同一个位置。 

你可以使用带 -d 选项的 `git branch` 命令来删除分支：

```
$ git branch -d hotfix
Deleted branch hotfix (3a0874c).
```

现在你可以切换回你正在工作的分支继续你的工作，也就是针对 #53 问题的那个分支（iss53 分支）。

```
$ git checkout iss53
Switched to branch "iss53"
$ vim index.html
$ git commit -a -m 'finished the new footer [issue 53]'
[iss53 ad82d7a] finished the new footer [issue 53]
1 file changed, 1 insertion(+)
```

![Figure 23. 继续在 `iss53` 分支上的工作]()

你在 hotfix 分支上所做的工作并没有包含到 iss53 分支中。 如果你需要拉取 hotfix 所做的修改，你可以使用 `git merge master` 命令将 master 分支合并入 iss53 分支，或者你也可以等到 iss53 分支完成其使命，再将其合并回 master 分支。

### 分支的合并

假设你已经修正了 #53 问题，并且打算将你的工作合并入 master 分支。 为此，你需要合并 iss53 分支到 master 分支，这和之前你合并 hotfix 分支所做的工作差不多。 你只需要检出到你想合并入的分支，然后运行 `git merge` 命令：

```
$ git checkout master
Switched to branch 'master'
$ git merge iss53
Merge made by the 'recursive' strategy.
index.html |    1 +
1 file changed, 1 insertion(+)
```

这和你之前合并 hotfix 分支的时候看起来有一点不一样。 在这种情况下，你的开发历史从一个更早的地方开始分叉开来（diverged）。 因为，master 分支所在提交并不是 iss53 分支所在提交的直接祖先，Git 不得不做一些额外的工作。 出现这种情况的时候，Git 会使用两个分支的末端所指的快照（C4 和 C5）以及这两个分支的工作祖先（C2），做一个简单的三方合并。

![Figure 24. 一次典型合并中所用到的三个快照]()

和之前将分支指针向前推进所不同的是，Git 将此次三方合并的结果做了一个新的快照并且自动创建一个新的提交指向它。 这个被称作一次合并提交，它的特别之处在于他有不止一个父提交。

![Figure 25. 一个合并提交]()

需要指出的是，Git 会自行决定选取哪一个提交作为最优的共同祖先，并以此作为合并的基础。

既然你的修改已经合并进来了，你已经不再需要 iss53 分支了。 现在你可以在任务追踪系统中关闭此项任务，并删除这个分支。

```
$ git branch -d iss53
```

### 遇到冲突时的分支合并

有时候合并操作不会如此顺利。 如果你在两个不同的分支中，对同一个文件的同一个部分进行了不同的修改，Git 就没法干净的合并它们。 如果你对 #53 问题的修改和有关 hotfix 的修改都涉及到同一个文件的同一处，在合并它们的时候就会产生合并冲突：

```
$ git merge iss53
Auto-merging index.html
CONFLICT (content): Merge conflict in index.html
Automatic merge failed; fix conflicts and then commit the result.
```

此时 Git 做了合并，但是没有自动地创建一个新的合并提交。 Git 会暂停下来，等待你去解决合并产生的冲突。 你可以在合并冲突后的任意时刻使用 `git status` 命令来查看那些因包含合并冲突而处于未合并（unmerged）状态的文件：

```
$ git status
On branch master
You have unmerged paths.
  (fix conflicts and run "git commit")

Unmerged paths:
  (use "git add <file>..." to mark resolution)

    both modified:      index.html

no changes added to commit (use "git add" and/or "git commit -a")
```

任何因包含合并冲突而有待解决的文件，都会以未合并状态标识出来。 Git 会在有冲突的文件中加入标准的冲突解决标记，这样你可以打开这些包含冲突的文件然后手动解决冲突。 出现冲突的文件会包含一些特殊区段，看起来像下面这个样子：

```
<<<<<<< HEAD:index.html
<div id="footer">contact : email.support@github.com</div>
=======
<div id="footer">
 please contact us at support@github.com
</div>
>>>>>>> iss53:index.html
``` 

这表示 HEAD 所指示的版本（也就是你的 master 分支所在的位置，因为你在运行 merge 命令的时候已经检出到了这个分支）在这个区段的上半部分（======= 的上半部分），而 iss53 分支所指示的版本在 ======= 的下半部分。 为了解决冲突，你必须选择使用由 ======= 分割的两部分中的一个，或者你也可以自行合并这些内容。 例如，你可以通过把这段内容换成下面的样子来解决冲突：

```
<div id="footer">
please contact us at email.support@github.com
</div>
```

上述的冲突解决方案仅保留了其中一个分支的修改，并且 <<<<<<< , ======= , 和 >>>>>>> 这些行被完全删除了。 在你解决了所有文件里的冲突之后，对每个文件使用 git add 命令来将其标记为冲突已解决。 一旦暂存这些原本有冲突的文件，Git 就会将它们标记为冲突已解决。

如果你对结果感到满意，并且确定之前有冲突的的文件都已经暂存了，这时你可以输入 git commit 来完成合并提交。 默认情况下提交信息看起来像下面这个样子：

```
Merge branch 'iss53'

Conflicts:
    index.html
#
# It looks like you may be committing a merge.
# If this is not correct, please remove the file
#	.git/MERGE_HEAD
# and try again.


# Please enter the commit message for your changes. Lines starting
# with '#' will be ignored, and an empty message aborts the commit.
# On branch master
# All conflicts fixed but you are still merging.
#
# Changes to be committed:
#	modified:   index.html
#
```

如果你觉得上述的信息不够充分，不能完全体现分支合并的过程，你可以修改上述信息，添加一些细节给未来检视这个合并的读者一些帮助，告诉他们你是如何解决合并冲突的，以及理由是什么。




## 分支管理

### 分支管理

现在已经创建、合并、删除了一些分支，让我们看看一些常用的分支管理工具。

`git branch` 命令不只是可以创建与删除分支。 如果不加任何参数运行它，会得到当前所有分支的一个列表：

```
$ git branch
  iss53
* master
  testing
```

注意 master 分支前的 * 字符：它代表现在检出(chectout)的那一个分支（也就是说，当前 HEAD 指针所指向的分支）。 这意味着如果在这时候提交，master 分支将会随着新的工作向前移动。 

如果需要查看每一个分支的最后一次提交，可以运行 `git branch -v` 命令：

```
$ git branch -v
  iss53   93b412c fix javascript issue
* master  7a98805 Merge branch 'iss53'
  testing 782fd34 add scott to the author list in the readmes
```

`--merged` 与 `--no-merged` 这两个有用的选项可以过滤这个列表中已经合并或尚未合并到当前分支的分支。 

如果要查看哪些分支已经合并到当前分支，可以运行 `git branch --merged`:

```
$ git branch --merged
  iss53
* master
```

因为之前已经合并了 iss53 分支，所以现在看到它在列表中。在这个列表中分支名字前没有 * 号的分支通常可以使用 `git branch -d` 删除掉；你已经将它们的工作整合到了另一个分支，所以并不会失去任何东西。

查看所有包含未合并工作的分支，可以运行 `git branch --no-merged`:

```
$ git branch --no-merged
  testing
```

这里显示了其他分支。 因为它包含了还未合并的工作，尝试使用 `git branch -d` 命令删除它时会失败：

```
$ git branch -d testing
error: The branch 'testing' is not fully merged.
If you are sure you want to delete it, run 'git branch -D testing'.
```

如果真的想要删除分支并丢掉那些工作，如同帮助信息里所指出的，可以使用 -D 选项强制删除它。





## 分支开发工作流

### 分支开发工作流

在本节，我们会介绍一些常见的利用分支进行开发的工作流程。而正是由于分支管理的便捷，才衍生出这些典型的工作模式。

### 长期分支

因为 Git 使用简单的三方合并，所以就算在一段较长的时间内，反复把一个分支合并入另一个分支，也不是什么难事。 也就是说，在整个项目开发周期的不同阶段，你可以同时拥有多个开放的分支；你可以定期地把某些特性分支合并入其他分支中。

许多使用 Git 的开发者都喜欢使用这种方式来工作，比如只在 master 分支上保留完全稳定的代码——有可能仅仅是已经发布或即将发布的代码。 他们还有一些名为 develop 或者 next 的平行分支，被用来做后续开发或者测试稳定性——这些分支不必保持绝对稳定，但是一旦达到稳定状态，它们就可以被合并入 master 分支了。 

这样，在确保这些已完成的特性分支（短期分支，比如之前的 iss53 分支）能够通过所有测试，并且不会引入更多 bug 之后，就可以合并入主干分支中，等待下一次的发布。

事实上我们刚才讨论的，是随着你的提交而不断右移的指针。 稳定分支的指针总是在提交历史中落后一大截，而前沿分支的指针往往比较靠前。

![Figure 26. 渐进稳定分支的线性图]()

通常把他们想象成流水线（work silos）可能更好理解一点，那些经过测试考验的提交会被遴选到更加稳定的流水线上去。

![Figure 27. 渐进稳定分支的流水线（“silo”）视图]()

你可以用这种方法维护不同层次的稳定性。 一些大型项目还有一个 proposed（建议） 或 pu: proposed updates（建议更新）分支，它可能因包含一些不成熟的内容而不能进入 next 或者 master 分支。 

这么做的目的是使你的分支具有不同级别的稳定性；当它们具有一定程度的稳定性后，再把它们合并入具有更高级别稳定性的分支中。 

再次强调一下，使用多个长期分支的方法并非必要，但是这么做通常很有帮助，尤其是当你在一个非常庞大或者复杂的项目中工作时。

### 特性分支

特性分支(feature branch)对任何规模的项目都适用。 特性分支是一种**短期分支**，它被用来**实现单一特性或其相关工作**。 

例如在之前用到的特性分支（iss53 和 hotfix 分支）中提交了一些更新，并且在它们合并入主干分支之后，你又删除了它们。 

这项技术能使你快速并且完整地进行上下文切换（context-switch）——因为你的工作被分散到不同的流水线中，在不同的流水线中每个分支都仅与其目标特性相关。

因此，在做代码审查之类的工作的时候就能更加容易地看出你做了哪些改动。 你可以把做出的改动在特性分支中保留几分钟、几天甚至几个月，等它们成熟之后再合并，而不用在乎它们建立的顺序或工作进度。

考虑这样一个例子，你在 master 分支上工作到 C1，这时为了解决一个问题而新建 iss91 分支，在 iss91 分支上工作到 C4，然而对于那个问题你又有了新的想法，于是你再新建一个 iss91v2 分支试图用另一种方法解决那个问题，接着你回到 master 分支工作了一会儿，你又冒出了一个不太确定的想法，你便在 C10 的时候新建一个 dumbidea 分支，并在上面做些实验。 你的提交历史看起来像下面这个样子：

![Figure 28. 拥有多个特性分支的提交历史]()

现在，我们假设两件事情：你决定使用第二个方案来解决那个问题，即使用在 iss91v2 分支中方案；另外，你将 dumbidea 分支拿给你的同事看过之后，结果发现这是个惊人之举。 这时你可以抛弃 iss91 分支（即丢弃 C5 和 C6 提交），然后把另外两个分支合并入主干分支。 最终你的提交历史看起来像下面这个样子：

![Figure 29. 合并了 `dumbidea` 和 `iss91v2` 分支之后的提交历史]()


## 远程分支

### 远程分支

在上一节的分支操作中的操作结果都被保存在本地的存储库中而非远程库中。

远程引用是对远程仓库的引用（指针），包括分支、标签等等。 你可以通过 `git ls-remote (remote)` 来显式地获得远程引用的完整列表，或者通过 `git remote show (remote)` 获得远程分支的更多信息。 然而，一个更常见的做法是利用远程跟踪分支。

远程跟踪分支是远程分支状态的引用。 它们是你不能移动的本地引用，当你做任何网络通信操作时，它们会自动移动。 远程跟踪分支像是你上次连接到远程仓库时，那些分支所处状态的书签。(?)

它们以 (remote)/(branch) 形式命名。 例如，如果你想要看你最后一次与远程仓库 origin 通信时 master 分支的状态，你可以查看 origin/master 分支。 你与同事合作解决一个问题并且他们推送了一个 iss53 分支，你可能有自己的本地 iss53 分支；但是在服务器上的分支会指向 origin/iss53 的提交。

这可能有一点儿难以理解，让我们来看一个例子。 假设你的网络里有一个在 git.ourcompany.com 的 Git 服务器。 如果你从这里克隆，Git 的 clone 命令会为你自动将其命名为 origin，拉取它的所有数据，创建一个指向它的 master 分支的指针，并且在本地将其命名为 origin/master。 Git 也会给你一个与 origin 的 master 分支在指向同一个地方的本地 master 分支，这样你就有工作的基础。

> Note:“origin” 并无特殊含义。远程仓库名字 “origin” 与分支名字 “master” 一样，在 Git 中并没有任何特别的含义一样。 同时 “master” 是当你运行 git init 时默认的起始分支名字，原因仅仅是它的广泛使用，“origin” 是当你运行 git clone 时默认的远程仓库名字。 如果你运行 git clone -o booyah，那么你默认的远程分支名字将会是 booyah/master。

![Figure 30. 克隆之后的服务器与本地仓库]()

如果你在本地的 master 分支做了一些工作，然而在同一时间，其他人推送提交到 git.ourcompany.com 并更新了它的 master 分支，那么你的提交历史将向不同的方向前进。 也许，只要你不与 origin 服务器连接，你的 origin/master 指针就不会移动。

![Figure 31. 本地与远程的工作可以分叉]()

如果要同步你的工作，运行 `git fetch origin` 命令。 这个命令查找 “origin” 是哪一个服务器（在本例中，它是 git.ourcompany.com），从中抓取本地没有的数据，并且更新本地数据库，移动 origin/master 指针指向新的、更新后的位置。

![Figure 32. git fetch 更新你的远程仓库引用]()

为了演示有多个远程仓库与远程分支的情况，我们假定你有另一个内部 Git 服务器，仅用于你的 sprint 小组的开发工作。 这个服务器位于 git.team1.ourcompany.com。 你可以运行 git remote add 命令添加一个新的远程仓库引用到当前的项目。 将这个远程仓库命名为 teamone，将其作为整个 URL 的缩写。

![Figure 33. 添加另一个远程仓库]()

现在，可以运行 git fetch teamone 来抓取远程仓库 teamone 有而本地没有的数据。 因为那台服务器上现有的数据是 origin 服务器上的一个子集，所以 Git 并不会抓取数据而是会设置远程跟踪分支 teamone/master 指向 teamone 的 master 分支。

![Figure 34. 远程跟踪分支 teamone/master]()

### 推送

当你想要公开分享一个分支时，需要将其推送到有写入权限的远程仓库上。 本地的分支并不会自动与远程仓库同步 - 你必须显式地推送想要分享的分支。 这样，你就可以把不愿意分享的内容放到私人分支上，而将需要和别人协作的内容推送到公开分支。

如果希望和别人一起在名为 serverfix 的分支上工作，你可以像推送第一个分支那样推送它。 运行 git push (remote) (branch):

```
$ git push origin serverfix
Counting objects: 24, done.
Delta compression using up to 8 threads.
Compressing objects: 100% (15/15), done.
Writing objects: 100% (24/24), 1.91 KiB | 0 bytes/s, done.
Total 24 (delta 2), reused 0 (delta 0)
To https://github.com/schacon/simplegit
 * [new branch]      serverfix -> serverfix
```

下一次其他协作者从服务器上抓取数据时，他们会在本地生成一个远程分支 origin/serverfix，指向服务器的 serverfix 分支的引用：

```
$ git fetch origin
remote: Counting objects: 7, done.
remote: Compressing objects: 100% (2/2), done.
remote: Total 3 (delta 0), reused 3 (delta 0)
Unpacking objects: 100% (3/3), done.
From https://github.com/schacon/simplegit
 * [new branch]      serverfix    -> origin/serverfix
```

要特别注意的一点是当抓取到新的远程跟踪分支时，本地不会自动生成一份可编辑的副本（拷贝）。 换一句话说，这种情况下，不会有一个新的 serverfix 分支 - 只有一个不可以修改的 origin/serverfix 指针。

可以运行 git merge origin/serverfix 将这些工作合并到当前所在的分支。 如果想要在自己的 serverfix 分支上工作，可以将其建立在远程跟踪分支之上：

```
$ git checkout -b serverfix origin/serverfix
Branch serverfix set up to track remote branch serverfix from origin.
Switched to a new branch 'serverfix'
```

这会给你一个用于工作的本地分支，并且起点位于 origin/serverfix。

### 跟踪分支

从一个远程跟踪分支检出一个本地分支会自动创建一个叫做 “跟踪分支”（有时候也叫做 “上游分支”）。 跟踪分支是与远程分支有直接关系的本地分支。 如果在一个跟踪分支上输入 git pull，Git 能自动地识别去哪个服务器上抓取、合并到哪个分支。

当克隆一个仓库时，它通常会自动地创建一个跟踪 origin/master 的 master 分支。 然而，如果你愿意的话可以设置其他的跟踪分支 - 其他远程仓库上的跟踪分支，或者不跟踪 master 分支。 最简单的就是之前看到的例子，运行 git checkout -b [branch] [remotename]/[branch]。 这是一个十分常用的操作所以 Git 提供了 --track 快捷方式：

```
$ git checkout --track origin/serverfix
Branch serverfix set up to track remote branch serverfix from origin.
Switched to a new branch 'serverfix'
```

如果想要将本地分支与远程分支设置为不同名字，你可以轻松地使用上一个命令增加一个不同名字的本地分支：

```
$ git checkout -b sf origin/serverfix
Branch sf set up to track remote branch serverfix from origin.
Switched to a new branch 'sf'
```

现在，本地分支 sf 会自动从 origin/serverfix 拉取。

设置已有的本地分支跟踪一个刚刚拉取下来的远程分支，或者想要修改正在跟踪的上游分支，你可以在任意时间使用 -u 或 --set-upstream-to 选项运行 git branch 来显式地设置。

```
$ git branch -u origin/serverfix
Branch serverfix set up to track remote branch serverfix from origin.
```

> Note: 上游快捷方式
当设置好跟踪分支后，可以通过 @{upstream} 或 @{u} 快捷方式来引用它。 所以在 master 分支时并且它正在跟踪 origin/master 时，如果愿意的话可以使用 git merge @{u} 来取代 git merge origin/master。

如果想要查看设置的所有跟踪分支，可以使用 git branch 的 -vv 选项。 这会将所有的本地分支列出来并且包含更多的信息，如每一个分支正在跟踪哪个远程分支与本地分支是否是领先、落后或是都有。

$ git branch -vv
  iss53     7e424c3 [origin/iss53: ahead 2] forgot the brackets
  master    1ae2a45 [origin/master] deploying index fix
* serverfix f8674d9 [teamone/server-fix-good: ahead 3, behind 1] this should do it
  testing   5ea463a trying something new
这里可以看到 iss53 分支正在跟踪 origin/iss53 并且 “ahead” 是 2，意味着本地有两个提交还没有推送到服务器上。 也能看到 master 分支正在跟踪 origin/master 分支并且是最新的。 接下来可以看到 serverfix 分支正在跟踪 teamone 服务器上的 server-fix-good 分支并且领先 3 落后 1，意味着服务器上有一次提交还没有合并入同时本地有三次提交还没有推送。 最后看到 testing 分支并没有跟踪任何远程分支。

需要重点注意的一点是这些数字的值来自于你从每个服务器上最后一次抓取的数据。 这个命令并没有连接服务器，它只会告诉你关于本地缓存的服务器数据。 如果想要统计最新的领先与落后数字，需要在运行此命令前抓取所有的远程仓库。 可以像这样做：$ git fetch --all; git branch -vv

### 拉取

当 `git fetch` 命令从服务器上抓取本地没有的数据时，它并不会修改工作目录中的内容。 它只会获取数据然后让你自己合并。 

然而，有一个命令叫作 `git pull` 在大多数情况下它的含义是一个 git fetch 紧接着一个 git merge 命令。 

如果有一个像之前章节中演示的设置好的跟踪分支，不管它是显式地设置还是通过 clone 或 checkout 命令为你创建的，`git pull` 都会查找当前分支所跟踪的服务器与分支，从服务器上抓取数据然后尝试合并入那个远程分支。

由于 git pull 的魔法经常令人困惑所以通常单独显式地使用 fetch 与 merge 命令会更好一些。

### 删除远程分支

假设你已经通过远程分支做完所有的工作了 - 也就是说你和你的协作者已经完成了一个特性并且将其合并到了远程仓库的 master 分支（或任何其他稳定代码分支）。 可以运行带有 --delete 选项的 git push 命令来删除一个远程分支。 如果想要从服务器上删除 serverfix 分支，运行下面的命令：

```
$ git push origin --delete serverfix
To https://github.com/schacon/simplegit
 - [deleted]         serverfix
```

基本上这个命令做的只是从服务器上移除这个指针。 Git 服务器通常会保留数据一段时间直到垃圾回收运行，所以如果不小心删除掉了，通常是很容易恢复的。


## 变基



# 分布式git


## 分布式工作流程

你现在拥有了一个远程 Git 版本库，能为所有开发者共享代码提供服务，在一个本地工作流程下，你也已经熟悉了基本 Git 命令。你现在可以学习如何利用 Git 提供的一些分布式工作流程了。

这一章中，你将会学习如何作为贡献者或整合者，在一个分布式协作的环境中使用 Git。 你会学习为一个项目成功地贡献代码，并接触一些最佳实践方式，让你和项目的维护者能轻松地完成这个过程。另外，你也会学到如何管理有很多开发者提交贡献的项目。

分布式工作流程
同传统的集中式版本控制系统（CVCS）不同，Git 的分布式特性使得开发者间的协作变得更加灵活多样。 在集中式系统中，每个开发者就像是连接在集线器上的节点，彼此的工作方式大体相像。 而在 Git 中，每个开发者同时扮演着节点和集线器的角色——也就是说，每个开发者既可以将自己的代码贡献到其他的仓库中，同时也能维护自己的公开仓库，让其他人可以在其基础上工作并贡献代码。 由此，Git 的分布式协作可以为你的项目和团队衍生出种种不同的工作流程，接下来的章节会介绍几种利用了 Git 的这种灵活性的常见应用方式。 我们将讨论每种方式的优点以及可能的缺点；你可以选择使用其中的某一种，或者将它们的特性混合搭配使用。

集中式工作流
集中式系统中通常使用的是单点协作模型——集中式工作流。 一个中心集线器，或者说仓库，可以接受代码，所有人将自己的工作与之同步。 若干个开发者则作为节点——也就是中心仓库的消费者——并且与其进行同步。

集中式工作流。
Figure 54. 集中式工作流。
这意味着如果两个开发者从中心仓库克隆代码下来，同时作了一些修改，那么只有第一个开发者可以顺利地把数据推送回共享服务器。 第二个开发者在推送修改之前，必须先将第一个人的工作合并进来，这样才不会覆盖第一个人的修改。 这和 Subversion （或任何 CVCS）中的概念一样，而且这个模式也可以很好地运用到 Git 中。

如果在公司或者团队中，你已经习惯了使用这种集中式工作流程，完全可以继续采用这种简单的模式。 只需要搭建好一个中心仓库，并给开发团队中的每个人推送数据的权限，就可以开展工作了。Git 不会让用户覆盖彼此的修改。 例如 John 和 Jessica 同时开始工作。 John 完成了他的修改并推送到服务器。 接着 Jessica 尝试提交她自己的修改，却遭到服务器拒绝。 她被告知她的修改正通过非快进式（non-fast-forward）的方式推送，只有将数据抓取下来并且合并后方能推送。 这种模式的工作流程的使用非常广泛，因为大多数人对其很熟悉也很习惯。

当然这并不局限于小团队。 利用 Git 的分支模型，通过同时在多个分支上工作的方式，即使是上百人的开发团队也可以很好地在单个项目上协作。

集成管理者工作流
Git 允许多个远程仓库存在，使得这样一种工作流成为可能：每个开发者拥有自己仓库的写权限和其他所有人仓库的读权限。 这种情形下通常会有个代表`‘官方’'项目的权威的仓库。 要为这个项目做贡献，你需要从该项目克隆出一个自己的公开仓库，然后将自己的修改推送上去。 接着你可以请求官方仓库的维护者拉取更新合并到主项目。 维护者可以将你的仓库作为远程仓库添加进来，在本地测试你的变更，将其合并入他们的分支并推送回官方仓库。 这一流程的工作方式如下所示（见 集成管理者工作流。）：

项目维护者推送到主仓库。

贡献者克隆此仓库，做出修改。

贡献者将数据推送到自己的公开仓库。

贡献者给维护者发送邮件，请求拉取自己的更新。

维护者在自己本地的仓库中，将贡献者的仓库加为远程仓库并合并修改。

维护者将合并后的修改推送到主仓库。

集成管理者工作流。
Figure 55. 集成管理者工作流。
这是 GitHub 和 GitLab 等集线器式（hub-based）工具最常用的工作流程。人们可以容易地将某个项目派生成为自己的公开仓库，向这个仓库推送自己的修改，并为每个人所见。 这么做最主要的优点之一是你可以持续地工作，而主仓库的维护者可以随时拉取你的修改。 贡献者不必等待维护者处理完提交的更新——每一方都可以按照自己节奏工作。

司令官与副官工作流
这其实是多仓库工作流程的变种。 一般拥有数百位协作开发者的超大型项目才会用到这样的工作方式，例如著名的 Linux 内核项目。 被称为副官（lieutenant）的各个集成管理者分别负责集成项目中的特定部分。 所有这些副官头上还有一位称为司令官（dictator）的总集成管理者负责统筹。 司令官维护的仓库作为参考仓库，为所有协作者提供他们需要拉取的项目代码。 整个流程看起来是这样的(见 司令官与副官工作流。):

普通开发者在自己的特性分支上工作，并根据 master 分支进行变基。 这里是司令官的`master`分支。

副官将普通开发者的特性分支合并到自己的 master 分支中。

司令官将所有副官的 master 分支并入自己的 master 分支中。

司令官将集成后的 master 分支推送到参考仓库中，以便所有其他开发者以此为基础进行变基。

司令官与副官工作流。
Figure 56. 司令官与副官工作流。
这种工作流程并不常用，只有当项目极为庞杂，或者需要多级别管理时，才会体现出优势。 利用这种方式，项目总负责人（即司令官）可以把大量分散的集成工作委托给不同的小组负责人分别处理，然后在不同时刻将大块的代码子集统筹起来，用于之后的整合。

工作流程总结
上面介绍了在 Git 等分布式系统中经常使用的工作流程，但是在实际的开发中，你会遇到许多可能适合你的特定工作流程的变种。 现在你应该已经清楚哪种工作流程组合可能比较适合你了，我们会给出一些如何扮演不同工作流程中主要角色的更具体的例子。 下一节我们将会学习为项目做贡献的一些常用模式。

## 向一个项目贡献

### 向一个项目贡献

描述如何向一个项目贡献的主要困难在于完成贡献有很多不同的方式。 因为 Git 非常灵活，人们可以通过不同的方式来一起工作。

第一个影响因素是**活跃贡献者的数量** - 积极地向这个项目贡献代码的用户数量以及他们的贡献频率。 在许多情况下，你可能会有两三个开发者一天提交几次，对于不活跃的项目可能更少。 对于大一些的公司或项目，开发者的数量可能会是上千，每天都有成百上千次提交。 这很重要，因为随着开发者越来越多，在确保你的代码能干净地应用或轻松地合并时会遇到更多问题。 提交的改动可能表现为过时的，也可能在你正在做改动或者等待改动被批准应用时被合并入的工作严重损坏。 

如何保证代码始终是最新的，并且提交始终是有效的？

下一个影响因素是**项目使用的工作流程**。 它是中心化的吗，即每一个开发者都对主线代码有相同的写入权限？ 项目是否有一个检查所有补丁的维护者或整合者？ 是否所有的补丁是同行评审后批准的？ 你是否参与了那个过程？ 是否存在副官系统，你必须先将你的工作提交到上面？

下一个问题是**提交权限**。 是否有项目的写权限会使向项目贡献所需的流程有极大的不同。 如果没有写权限，项目会选择何种方式接受贡献的工作？ 是否甚至有一个如何贡献的规范？ 你一次贡献多少工作？ 你多久贡献一次？

所有这些问题都会影响实际如何向一个项目贡献，以及对你来说哪些工作流程更适合或者可用。 

### 提交准则

在我们开始查看特定的用例前，这里有一个关于提交信息的快速说明。 有一个好的创建提交的准则并且坚持使用会让与 Git 工作和与其他人协作更容易。 

首先，你不会想要把空白错误（根据 git help diff 的描述，结合下面给出的图片，空白错误是指行尾的空格、Tab 制表符，和行首空格后跟 Tab 制表符的行为）提交上去。 Git 提供了一个简单的方式来检查这点 - 在提交前，运行 git diff --check，它将会找到可能的空白错误并将它们为你列出来。

![Figure 57. `git diff --check` 的输出]()

如果在提交前运行那个命令，可以知道提交中是否包含可能会使其他开发者恼怒的空白问题。

接下来，**尝试让每一个提交成为一个逻辑上的独立变更集。** 

如果可以，尝试让改动可以理解 - 不要在整个周末编码解决五个问题，然后在周一时将它们提交为一个巨大的提交。 即使在周末期间你无法提交，在周一时使用暂存区域**将你的工作最少拆分为每个问题一个提交**，并且**为每一个提交附带一个有用的信息**。 如果其中一些改动修改了同一个文件，尝试使用 git add --patch 来**部分暂存文件**（在 交互式暂存 中有详细介绍）。 不管你做一个或五个提交，只要所有的改动是在同一时刻添加的，项目分支末端的快照就是独立的，使同事开发者必须审查你的改动时尽量让事情容易些。 当你之后需要时这个方法也会使拉出或还原一个变更集更容易些。

最后一件要牢记的事是**提交信息**。 有一个创建优质提交信息的习惯会使 Git 的使用与协作容易的多。  Git 项目要求一个更详细的解释，包括**做改动的动机**和**它的实现与之前行为的对比** - 这是一个值得遵循的好规则。 在这些信息中使用现在时态祈使语气也是一个好想法。 换句话说，使用命令。 使用 “Add tests for.” 而不是 “I added tests for” 或 “Adding tests for,”。 

在接下来的例子中，以及贯穿本书大部分，出于简洁性的原因本书不会有像这样漂亮格式的信息；相反，我们使用 -m 选项的 git commit。

### 私有小型团队

你可能会遇到的最简单的配置是有一两个其他开发者的私有项目。 “私有” 在这个上下文中，意味着闭源 - 不可以从外面的世界中访问到。 你和其他的开发者都有仓库的推送权限。

在这个环境下，可以采用一个类似使用 Subversion 或其他集中式的系统时会使用的工作流程。 依然可以得到像离线提交、非常容易地新建分支与合并分支等高级功能，但是工作流程可以是很简单的；主要的区别是合并发生在客户端这边而不是在提交时发生在服务器那边。 让我们看看当两个开发者在一个共享仓库中一起工作时会是什么样子。 第一个开发者，John，克隆了仓库，做了改动，然后本地提交。 （为了缩短这些例子长度，协议信息已被替换为 ...。）

```
# John's Machine
$ git clone john@githost:simplegit.git
Initialized empty Git repository in /home/john/simplegit/.git/
...
$ cd simplegit/
$ vim lib/simplegit.rb
$ git commit -am 'removed invalid default value'
[master 738ee87] removed invalid default value
 1 files changed, 1 insertions(+), 1 deletions(-)
```

第二个开发者，Jessica，做了同样的事情 - 克隆仓库并提交了一个改动：

```
# Jessica's Machine
$ git clone jessica@githost:simplegit.git
Initialized empty Git repository in /home/jessica/simplegit/.git/
...
$ cd simplegit/
$ vim TODO
$ git commit -am 'add reset task'
[master fbff5bc] add reset task
 1 files changed, 1 insertions(+), 0 deletions(-)
```

现在，Jessica 把她的工作推送到服务器上：

```
# Jessica's Machine
$ git push origin master
...
To jessica@githost:simplegit.git
   1edee6b..fbff5bc  master -> master
```

John 也尝试推送他的改动：

```
# John's Machine
$ git push origin master
To john@githost:simplegit.git
 ! [rejected]        master -> master (non-fast forward)
error: failed to push some refs to 'john@githost:simplegit.git'
```

不允许 John 推送是因为在同一时间 Jessica 已经推送了。你会注意到两个开发者并没有编辑同一个文件。但 Git 要求你在本地合并提交。 **John 必须抓取 Jessica 的改动并合并它们，才能被允许推送**。

```
$ git fetch origin
...
From john@githost:simplegit
 + 049d078...fbff5bc master     -> origin/master
```

在这个时候，John 的本地仓库看起来像这样：

![Figure 58. John 的分叉历史]()

John 有一个引用指向 Jessica 推送上去的改动，但是他必须将它们合并入自己的工作中之后才能被允许推送。

```
$ git merge origin/master
Merge made by recursive.
 TODO |    1 +
 1 files changed, 1 insertions(+), 0 deletions(-)
```

合并进行地很顺利 - John 的提交历史现在看起来像这样：


![Figure 59. 合并了 `origin/master` 之后 John 的仓库]()

现在，John 可以测试代码，确保它依然正常工作，然后他可以把合并的新工作推送到服务器上：

$ git push origin master
...
To john@githost:simplegit.git
   fbff5bc..72bbc59  master -> master
最终，John 的提交历史看起来像这样：

推送到 `origin` 服务器后 John 的历史。
Figure 60. 推送到 origin 服务器后 John 的历史
在此期间，Jessica 在一个特性分支上工作。 她创建了一个称作 issue54 的特性分支并且在那个分支上做了三次提交。 她还没有抓取 John 的改动，所以她的提交历史看起来像这样：

![Figure 61. Jessica 的特性分支]()
Jessica 想要与 John 同步，所以她进行了抓取操作：

```
# Jessica's Machine
$ git fetch origin
...
From jessica@githost:simplegit
   fbff5bc..72bbc59  master     -> origin/master
```

那会同时拉取 John 推送的工作。 Jessica 的历史现在看起来像这样：

抓取 John 的改动后 Jessica 的历史。
Figure 62. 抓取 John 的改动后 Jessica 的历史
Jessica 认为她的特性分支已经准备好了，但是她想要知道必须合并什么进入她的工作才能推送。 她运行 git log 来找出：

```
$ git log --no-merges issue54..origin/master
commit 738ee872852dfaa9d6634e0dea7a324040193016
Author: John Smith <jsmith@example.com>
Date:   Fri May 29 16:01:27 2009 -0700

   removed invalid default value
```

issue54..origin/master 语法是一个日志过滤器，要求 Git 只显示所有在后面分支（在本例中是 origin/master）但不在前面分支（在本例中是 issue54）的提交的列表。 我们将会在 提交区间 中详细介绍这个语法。

目前，我们可以从输出中看到有一个 John 生成的但是 Jessica 还没有合并入的提交。 如果她合并 origin/master，也就是说将会修改她的本地工作的那个单个提交。

现在，Jessica 可以合并她的特性工作到她的 master 分支，合并 John 的工作（origin/master）进入她的 master 分支，然后再次推送回服务器。 首先，为了整合所有这些工作她切换回她的 master 分支。

$ git checkout master
Switched to branch 'master'
Your branch is behind 'origin/master' by 2 commits, and can be fast-forwarded.
她既可以先合并 origin/master 也可以先合并 issue54 - 它们都是上游，所以顺序并没有关系。 不论她选择的顺序是什么最终的结果快照是完全一样的；只是历史会有一点轻微的区别。 她选择先合并入 issue54：

$ git merge issue54
Updating fbff5bc..4af4298
Fast forward
 README           |    1 +
 lib/simplegit.rb |    6 +++++-
 2 files changed, 6 insertions(+), 1 deletions(-)
没有发生问题；如你所见它是一次简单的快进。 现在 Jessica 合并入 John 的工作（origin/master）：

$ git merge origin/master
Auto-merging lib/simplegit.rb
Merge made by recursive.
 lib/simplegit.rb |    2 +-
 1 files changed, 1 insertions(+), 1 deletions(-)
每一个文件都干净地合并了，Jessica 的历史看起来像这样：

合并了 John 的改动后 Jessica 的历史。
Figure 63. 合并了 John 的改动后 Jessica 的历史
现在 origin/master 是可以从 Jessica 的 master 分支到达的，所以她应该可以成功地推送（假设同一时间 John 并没有再次推送）：

$ git push origin master
...
To jessica@githost:simplegit.git
   72bbc59..8059c15  master -> master
每一个开发者都提交了几次并成功地合并了其他人的工作。

推送所有的改动回服务器后 Jessica 的历史。
Figure 64. 推送所有的改动回服务器后 Jessica 的历史
这是一个最简单的工作流程。 你通常在一个特性分支工作一会儿，当它准备好整合时合并回你的 master 分支。 当想要共享工作时，将其合并回你自己的 master 分支，如果有改动的话然后抓取并合并 origin/master，最终推送到服务器上的 master 分支。 通常顺序像这样：


![Figure 65. 一个简单的多人 Git 工作流程的通常事件顺序]()


### 私有管理团队

在接下来的情形中，你会看到大型私有团队中贡献者的角色。 在你将学习到的这种工作环境中，小组**基于特性(feature)**进行协作，这些团队的贡献将会由其他人整合。

让我们假设 John 与 Jessica 在一个特性上工作，同时 Jessica 与 Josie 在第二个特性上工作。 

在本例中，公司使用了一种**整合-管理者**工作流程，独立小组的工作只能被特定的工程师整合，主仓库的 master 分支只能被那些工程师更新。 在这种情况下，所有的工作都是在基于团队的分支上完成的并且稍后会被整合者拉到一起。

因为 Jessica 在两个特性上工作，并且平行地与两个不同的开发者协作，让我们跟随她的工作流程。 

假设她已经克隆了仓库，首先决定在 `featureA` 上工作。 她为那个特性创建了一个新分支然后在那做了一些工作：

```
# Jessica's Machine
$ git checkout -b featureA
Switched to a new branch 'featureA'
$ vim lib/simplegit.rb
$ git commit -am 'add limit to log function'
[featureA 3300904] add limit to log function
 1 files changed, 1 insertions(+), 1 deletions(-)
```

在这个时候，她需要将工作共享给 John，所以她推送了 featureA 分支的提交到服务器上。 Jessica 没有 master 分支的推送权限 - 只有整合者有 - 所以为了与 John 协作必须推送另一个分支。

$ git push -u origin featureA
...
To jessica@githost:simplegit.git
 * [new branch]      featureA -> featureA
Jessica 向 John 发邮件告诉他已经推送了一些工作到 featureA 分支现在可以看一看。 当她等待 John 的反馈时，Jessica 决定与 Josie 开始在 featureB 上工作。 为了开始工作，她基于服务器的 master 分支开始了一个新分支。

# Jessica's Machine
$ git fetch origin
$ git checkout -b featureB origin/master
Switched to a new branch 'featureB'
现在，Jessica 在 featureB 分支上创建了几次提交：

$ vim lib/simplegit.rb
$ git commit -am 'made the ls-tree function recursive'
[featureB e5b0fdc] made the ls-tree function recursive
 1 files changed, 1 insertions(+), 1 deletions(-)
$ vim lib/simplegit.rb
$ git commit -am 'add ls-files'
[featureB 8512791] add ls-files
 1 files changed, 5 insertions(+), 0 deletions(-)
Jessica 的仓库看起来像这样：

Jessica 的初始提交历史。
Figure 66. Jessica 的初始提交历史
她准备好推送工作了，但是一封来自 Josie 的邮件告知一些初始工作已经被推送到服务器上的 featureBee 上了。 Jessica 在能推送到服务器前首先需要将那些改动与她自己的合并。 然后她可以通过 git fetch 抓取 Josie 的改动：

$ git fetch origin
...
From jessica@githost:simplegit
 * [new branch]      featureBee -> origin/featureBee
Jessica 现在可以通过 git merge 将其合并到她做的工作中：

$ git merge origin/featureBee
Auto-merging lib/simplegit.rb
Merge made by recursive.
 lib/simplegit.rb |    4 ++++
 1 files changed, 4 insertions(+), 0 deletions(-)
有点儿问题 - 她需要将在 featureB 分支上合并的工作推送到服务器上的 featureBee 分支。 她可以通过指定本地分支加上冒号（:）加上远程分支给 git push 命令来这样做：

$ git push -u origin featureB:featureBee
...
To jessica@githost:simplegit.git
   fba9af8..cd685d1  featureB -> featureBee
这称作一个 引用规格。 查看 引用规格 了解关于 Git 引用规格与通过它们可以做的不同的事情的详细讨论。 也要注意 -u 标记；这是 --set-upstream 的简写，该标记会为之后轻松地推送与拉取配置分支。

紧接着，John 发邮件给 Jessica 说他已经推送了一些改动到 featureA 分支并要求她去验证它们。 她运行一个 git fetch 来拉取下那些改动：

$ git fetch origin
...
From jessica@githost:simplegit
   3300904..aad881d  featureA   -> origin/featureA
然后，通过 git log 她可以看到哪些发生了改变：

$ git log featureA..origin/featureA
commit aad881d154acdaeb2b6b18ea0e827ed8a6d671e6
Author: John Smith <jsmith@example.com>
Date:   Fri May 29 19:57:33 2009 -0700

    changed log output to 30 from 25
最终，她合并 John 的工作到她自己的 featureA 分支：

$ git checkout featureA
Switched to branch 'featureA'
$ git merge origin/featureA
Updating 3300904..aad881d
Fast forward
 lib/simplegit.rb |   10 +++++++++-
1 files changed, 9 insertions(+), 1 deletions(-)
Jessica 想要轻微调整一些东西，所以她再次提交然后将其推送回服务器：

$ git commit -am 'small tweak'
[featureA 774b3ed] small tweak
 1 files changed, 1 insertions(+), 1 deletions(-)
$ git push
...
To jessica@githost:simplegit.git
   3300904..774b3ed  featureA -> featureA
Jessica 的提交历史现在看起来像这样：

在一个特性分支提交后 Jessica 的历史。
Figure 67. 在一个特性分支提交后 Jessica 的历史
Jessica、Josie 与 John 通知整合者在服务器上的 featureA 与 featureBee 分支准备好整合到主线中了。 在整合者合并这些分支到主线后，一次抓取会拿下来一个新的合并提交，使历史看起来像这样：

合并了 Jessica 的两个特性分支后她的历史。
Figure 68. 合并了 Jessica 的两个特性分支后她的历史
许多团队切换到 Git 是因为这一允许多个团队并行工作、并在之后合并不同工作的能力。 团队中更小一些的子小组可以通过远程分支协作而不必影响或妨碍整个团队的能力是 Git 的一个巨大优势。 在这儿看到的工作流程顺序类似这样：

这种管理团队工作流程的基本顺序。
Figure 69. 这种管理团队工作流程的基本顺序
派生的公开项目
向公开项目做贡献有一点儿不同。 因为没有权限直接更新项目的分支，你必须用其他办法将工作给维护者。 第一个例子描述在支持简单派生的 Git 托管上使用派生来做贡献。 许多托管站点支持这个功能（包括 GitHub、BitBucket、Google Code、repo.or.cz 等等），许多项目维护者期望这种风格的贡献。 下一节会讨论偏好通过邮件接受贡献补丁的项目。

首先，你可能想要克隆主仓库，为计划贡献的补丁或补丁序列创建一个特性分支，然后在那儿做工作。 顺序看起来基本像这样：

$ git clone (url)
$ cd project
$ git checkout -b featureA
# (work)
$ git commit
# (work)
$ git commit
Note
你可能会想要使用 rebase -i 来将工作压缩成一个单独的提交，或者重排提交中的工作使补丁更容易被维护者审核 - 查看 重写历史 了解关于交互式变基的更多信息。

当你的分支工作完成后准备将其贡献回维护者，去原始项目中然后点击 “Fork” 按钮，创建一份自己的可写的项目派生仓库。 然后需要添加这个新仓库 URL 为第二个远程仓库，在本例中称作 myfork：

$ git remote add myfork (url)
然后需要推送工作到上面。 相对于合并到主分支再推送上去，推送你正在工作的特性分支到仓库上更简单。 原因是工作如果不被接受或者是被拣选的，就不必回退你的 master 分支。 如果维护者合并、变基或拣选你的工作，不管怎样你最终会通过拉取他们的仓库找回来你的工作。

$ git push -u myfork featureA
当工作已经被推送到你的派生后，你需要通知维护者。 这通常被称作一个拉取请求（pull request），你既可以通过网站生成它 - GitHub 有它自己的 Pull Request 机制，我们将会在 GitHub 介绍 - 也可以运行 git request-pull 命令然后手动地将输出发送电子邮件给项目的维护者。

request-pull 命令接受特性分支拉入的基础分支，以及它们拉入的 Git 仓库 URL，输出请求拉入的所有修改的总结。 例如，Jessica 想要发送给 John 一个拉取请求，她已经在刚刚推送的分支上做了两次提交。她可以运行这个：

$ git request-pull origin/master myfork
The following changes since commit 1edee6b1d61823a2de3b09c160d7080b8d1b3a40:
  John Smith (1):
        added a new function

are available in the git repository at:

  git://githost/simplegit.git featureA

Jessica Smith (2):
      add limit to log function
      change log output to 30 from 25

 lib/simplegit.rb |   10 +++++++++-
 1 files changed, 9 insertions(+), 1 deletions(-)
这个输出可以被发送给维护者 - 它告诉他们工作是从哪个分支开始、归纳的提交与从哪里拉入这些工作。

在一个你不是维护者的项目上，通常有一个总是跟踪 origin/master 的 master 分支会很方便，在特性分支上做工作是因为如果它们被拒绝时你可以轻松地丢弃。 如果同一时间主仓库移动了然后你的提交不再能干净地应用，那么使工作主题独立于特性分支也会使你变基（rebase）工作时更容易。 例如，你想要提供第二个特性工作到项目，不要继续在刚刚推送的特性分支上工作 - 从主仓库的 master 分支重新开始：

$ git checkout -b featureB origin/master
# (work)
$ git commit
$ git push myfork featureB
# (email maintainer)
$ git fetch origin
现在，每一个特性都保存在一个贮藏库中 - 类似于补丁队列 - 可以重写、变基与修改而不会让特性互相干涉或互相依赖，像这样：

`featureB` 的初始提交历史。
Figure 70. featureB 的初始提交历史
假设项目维护者已经拉取了一串其他补丁，然后尝试拉取你的第一个分支，但是没有干净地合并。 在这种情况下，可以尝试变基那个分支到 origin/master 的顶部，为维护者解决冲突，然后重新提交你的改动：

$ git checkout featureA
$ git rebase origin/master
$ git push -f myfork featureA
这样会重写你的历史，现在看起来像是 featureA 工作之后的提交历史

`featureA` 工作之后的提交历史。
Figure 71. featureA 工作之后的提交历史
因为你将分支变基了，所以必须为推送命令指定 -f 选项，这样才能将服务器上有一个不是它的后代的提交的 featureA 分支替换掉。 一个替代的选项是推送这个新工作到服务器上的一个不同分支（可能称作 featureAv2）。

让我们看一个更有可能的情况：维护者看到了你的第二个分支上的工作并且很喜欢其中的概念，但是想要你修改一下实现的细节。 你也可以利用这次机会将工作基于项目现在的 master 分支。 你从现在的 origin/master 分支开始一个新分支，在那儿压缩 featureB 的改动，解决任何冲突，改变实现，然后推送它为一个新分支。

$ git checkout -b featureBv2 origin/master
$ git merge --squash featureB
# (change implementation)
$ git commit
$ git push myfork featureBv2
--squash 选项接受被合并的分支上的所有工作，并将其压缩至一个变更集，使仓库变成一个真正的合并发生的状态，而不会真的生成一个合并提交。 这意味着你的未来的提交将会只有一个父提交，并允许你引入另一个分支的所有改动，然后在记录一个新提交前做更多的改动。 同样 --no-commit 选项在默认合并过程中可以用来延迟生成合并提交。

现在你可以给维护者发送一条消息，表示你已经做了要求的修改然后他们可以在你的 featureBv2 分支上找到那些改动。

`featureBv2` 工作之后的提交历史。
Figure 72. featureBv2 工作之后的提交历史
通过邮件的公开项目
许多项目建立了接受补丁的流程 - 需要检查每一个项目的特定规则，因为它们之间有区别。 因为有几个历史悠久的、大型的项目会通过一个开发者的邮件列表接受补丁，现在我们将会通过一个例子来演示。

工作流程与之前的用例是类似的 - 你为工作的每一个补丁序列创建特性分支。 区别是如何提交它们到项目中。 生成每一个提交序列的电子邮件版本然后邮寄它们到开发者邮件列表，而不是派生项目然后推送到你自己的可写版本。

$ git checkout -b topicA
# (work)
$ git commit
# (work)
$ git commit
现在有两个提交要发送到邮件列表。 使用 git format-patch 来生成可以邮寄到列表的 mbox 格式的文件 - 它将每一个提交转换为一封电子邮件，提交信息的第一行作为主题，剩余信息与提交引入的补丁作为正文。 它有一个好处是是使用 format-patch 生成的一封电子邮件应用的提交正确地保留了所有的提交信息。

$ git format-patch -M origin/master
0001-add-limit-to-log-function.patch
0002-changed-log-output-to-30-from-25.patch
format-patch 命令打印出它创建的补丁文件名字。 -M 开关告诉 Git 查找重命名。 文件最后看起来像这样：

$ cat 0001-add-limit-to-log-function.patch
From 330090432754092d704da8e76ca5c05c198e71a8 Mon Sep 17 00:00:00 2001
From: Jessica Smith <jessica@example.com>
Date: Sun, 6 Apr 2008 10:17:23 -0700
Subject: [PATCH 1/2] add limit to log function

Limit log functionality to the first 20

---
 lib/simplegit.rb |    2 +-
 1 files changed, 1 insertions(+), 1 deletions(-)

diff --git a/lib/simplegit.rb b/lib/simplegit.rb
index 76f47bc..f9815f1 100644
--- a/lib/simplegit.rb
+++ b/lib/simplegit.rb
@@ -14,7 +14,7 @@ class SimpleGit
   end

   def log(treeish = 'master')
-    command("git log #{treeish}")
+    command("git log -n 20 #{treeish}")
   end

   def ls_tree(treeish = 'master')
--
2.1.0
也可以编辑这些补丁文件为邮件列表添加更多不想要在提交信息中显示出来的信息。 如果在 --- 行与补丁开头（diff --git 行）之间添加文本，那么开发者就可以阅读它；但是应用补丁时会排除它。

为了将其邮寄到邮件列表，你既可以将文件粘贴进电子邮件客户端，也可以通过命令行程序发送它。 粘贴文本经常会发生格式化问题，特别是那些不会合适地保留换行符与其他空白的 “更聪明的” 客户端。 幸运的是，Git 提供了一个工具帮助你通过 IMAP 发送正确格式化的补丁，这可能对你更容易些。 我们将会演示如何通过 Gmail 发送一个补丁，它正好是我们所知最好的邮件代理；可以在之前提到的 Git 源代码中的 Documentation/SubmittingPatches 文件的最下面了解一系列邮件程序的详细指令。

首先，需要在 ~/.gitconfig 文件中设置 imap 区块。 可以通过一系列的 git config 命令来分别设置每一个值，或者手动添加它们，不管怎样最后配置文件应该看起来像这样：

[imap]
  folder = "[Gmail]/Drafts"
  host = imaps://imap.gmail.com
  user = user@gmail.com
  pass = p4ssw0rd
  port = 993
  sslverify = false
如果 IMAP 服务器不使用 SSL，最后两行可能没有必要，host 的值会是 imap:// 而不是 imaps://。 当那些设置完成后，可以使用 git imap-send 将补丁序列放在特定 IMAP 服务器的 Drafts 文件夹中：

$ cat *.patch |git imap-send
Resolving imap.gmail.com... ok
Connecting to [74.125.142.109]:993... ok
Logging in...
sending 2 messages
100% (2/2) done
在这个时候，你应该能够到 Drafts 文件夹中，修改收件人字段为想要发送补丁的邮件列表，可能需要抄送给维护者或负责那个部分的人，然后发送。

你也可以通过一个 SMTP 服务器发送补丁。 同之前一样，你可以通过一系列的 git config 命令来分别设置选项，或者你可以手动地将它们添加到你的 ~/.gitconfig 文件的 sendmail 区块：

[sendemail]
  smtpencryption = tls
  smtpserver = smtp.gmail.com
  smtpuser = user@gmail.com
  smtpserverport = 587
当这完成后，你可以使用 git send-email 发送你的补丁：

$ git send-email *.patch
0001-added-limit-to-log-function.patch
0002-changed-log-output-to-30-from-25.patch
Who should the emails appear to be from? [Jessica Smith <jessica@example.com>]
Emails will be sent from: Jessica Smith <jessica@example.com>
Who should the emails be sent to? jessica@example.com
Message-ID to be used as In-Reply-To for the first email? y
然后，对于正在发送的每一个补丁，Git 会吐出这样的一串日志信息：

(mbox) Adding cc: Jessica Smith <jessica@example.com> from
  \line 'From: Jessica Smith <jessica@example.com>'
OK. Log says:
Sendmail: /usr/sbin/sendmail -i jessica@example.com
From: Jessica Smith <jessica@example.com>
To: jessica@example.com
Subject: [PATCH 1/2] added limit to log function
Date: Sat, 30 May 2009 13:29:15 -0700
Message-Id: <1243715356-61726-1-git-send-email-jessica@example.com>
X-Mailer: git-send-email 1.6.2.rc1.20.g8c5b.dirty
In-Reply-To: <y>
References: <y>

Result: OK
总结
这个部分介绍了处理可能会遇到的几个迥然不同类型的 Git 项目的一些常见的工作流程，介绍了帮助管理这个过程的一些新工具。 接下来，你会了解到如何在贡献的另一面工作：维护一个 Git 项目。 你将会学习如何成为一个仁慈的独裁者或整合管理者。


## 维护项目



## 总结





# GitHub


## 账户创建和配置

GitHub 是最大的 Git 版本库托管商，是成千上万的开发者和项目能够合作进行的中心。 

本章将讨论如何高效地使用 GitHub。 我们将学习如何注册和管理账户、创建和使用 Git 版本库、向已有项目贡献的通用流程以及如何接受别人向你自己项目的贡献、GitHub 的编程接口和很多能够让这些操作更简单的小提示。

### 账户的创建和配置
你所需要做的第一件事是创建一个免费账户。 直接访问 https://github.com，选择一个未被占用的用户名，提供一个电子邮件地址和密码，点击写着`‘Sign up for GitHub’'的绿色大按钮即可。

![Figure 82. GitHub 注册表单]()

 GitHub 会给你提供的邮件地址发送一封验证邮件。

> Note: GitHub 为免费账户提供了完整功能，限制是你的项目都将被完全公开（每个人都具有读权限）。 GitHub 的付费计划可以让你拥有一定数目的私有项目,但是前段时间被GitHub被微软收购后好像开放了私有仓库，每个人都可以创建自己的私有仓库。

点击屏幕左上角的 Octocat 图标（GitHub的吉祥物），你将来到控制面板页面。

### SSH 访问

现在，你完全可以使用 https:// 协议，通过你刚刚创建的用户名和密码访问 Git 版本库。 但是，如果仅仅克隆公有项目，你甚至不需要注册——刚刚我们创建的账户是为了以后 fork 其它项目，以及推送我们自己的修改。

如果你习惯使用 SSH 远程，你需要配置一个公钥。使用窗口右上角的链接打开你的账户设置：

![Figure 83. ‘`Account settings`’链接。]()

然后选择`‘SSH keys’'部分。

![Figure 84. ‘`SSH keys’'链接。]()

在这个页面点击“Add an SSH key”按钮，给你的公钥起一个名字，将你的`~/.ssh/id_rsa.pub`公钥文件的内容粘贴到文本区，然后点击`‘Add key’'。

> Note: 确保给你的 SSH 密钥起一个能够记得住的名字。 你可以为每一个密钥起名字（例如，“我的笔记本电脑”或者“工作账户”等），以便以后需要吊销密钥时能够方便地区分。


## 对项目做出贡献

### 对项目做出贡献

账户已经建立好了，现在我们来了解一些能帮助你对现有的项目做出贡献的知识。

### 派生（Fork）项目

如果你想要参与某个项目，但是并没有推送权限，这时可以对这个项目进行“派生”。 

派生的意思是指，GitHub 将在你的空间中创建一个完全属于你的项目副本，且你对其具有推送权限。

> Note: 在以前，“fork”是一个贬义词，指的是某个人使开源项目向不同的方向发展，或者创建一个竞争项目，使得原项目的贡献者分裂。 在 GitHub，“fork”指的是你自己的空间中创建的项目副本，这个副本允许你以一种更开放的方式对其进行修改。

通过这种方式，项目的管理者不再需要忙着把用户添加到贡献者列表并给予他们推送权限。 人们可以派生这个项目，将修改推送到派生出的项目副本中，并通过创建合并请求（Pull Request）来让他们的改动进入源版本库。 

创建了合并请求后，就会开启一个可供审查代码的板块，项目的拥有者和贡献者可以在此讨论相关修改，直到项目拥有者对其感到满意，并且认为这些修改可以被合并到版本库。

你可以通过点击项目页面右上角的“Fork”按钮，来派生这个项目。

![Figure 89. “Fork”按钮]()

稍等片刻，你将被转到新项目页面，该项目包含可写的代码副本。

### GitHub 流程

GitHub 设计了一个以合并请求为中心的特殊合作流程。 它基于我们在 Git 分支 的 特性分支 中提到的工作流程。 不管你是在一个紧密的团队中使用单独的版本库，或者使用许多的“Fork”来为一个由陌生人组成的国际企业或网络做出贡献，这种合作流程都能应付。

流程通常如下：

1) 从 master 分支中创建一个新分支

2) 提交一些修改来改进项目

3) 将这个分支推送到 GitHub 上

4) 创建一个合并请求

5) 讨论，根据实际情况继续修改

6) 项目的拥有者合并或关闭你的合并请求

这基本和 集成管理者工作流 中的一体化管理流程差不多，但是团队可以使用 GitHub 提供的网页工具替代电子邮件来交流和审查修改。

现在我们来看一个使用这个流程的例子。

### 创建合并请求

Tony 在找一些能在他的 Arduino 微控制器上运行的代码，他觉得 https://github.com/schacon/blink 中的代码不错。

但是有个问题，这个代码中的的闪烁频率太高，我们觉得 3 秒一次比 1 秒一次更好一些。 所以让我们来改进这个程序，并将修改后的代码提交给这个项目。

首先，单击“Fork”按钮来获得这个项目的副本。 我们使用的用户名是“tonychacon”，所以这个项目副本的访问地址是： https://github.com/tonychacon/blink 。 我们将它克隆到本地，创建一个分支，修改代码，最后再将改动推送到 GitHub。

```
$ git clone https://github.com/tonychacon/blink (1)
Cloning into 'blink'...

$ cd blink
$ git checkout -b slow-blink (2)
Switched to a new branch 'slow-blink'

$ sed -i '' 's/1000/3000/' blink.ino (3)

$ git diff --word-diff (4)
diff --git a/blink.ino b/blink.ino
index 15b9911..a6cc5a5 100644
--- a/blink.ino
+++ b/blink.ino
@@ -18,7 +18,7 @@ void setup() {
// the loop routine runs over and over again forever:
void loop() {
  digitalWrite(led, HIGH);   // turn the LED on (HIGH is the voltage level)
  [-delay(1000);-]{+delay(3000);+}               // wait for a second
  digitalWrite(led, LOW);    // turn the LED off by making the voltage LOW
  [-delay(1000);-]{+delay(3000);+}               // wait for a second
}

$ git commit -a -m 'three seconds is better' (5)
[slow-blink 5ca509d] three seconds is better
 1 file changed, 2 insertions(+), 2 deletions(-)

$ git push origin slow-blink (6)
Username for 'https://github.com': tonychacon
Password for 'https://tonychacon@github.com':
Counting objects: 5, done.
Delta compression using up to 8 threads.
Compressing objects: 100% (3/3), done.
Writing objects: 100% (3/3), 340 bytes | 0 bytes/s, done.
Total 3 (delta 1), reused 0 (delta 0)
To https://github.com/tonychacon/blink
 * [new branch]      slow-blink -> slow-blink
```

1) 将派生出的副本克隆到本地

2) 创建出名称有意义的分支

3) 修改代码

4) 检查改动

5) 将改动提交到分支中

6) 将新分支推送到 GitHub 的副本中

现在到 GitHub 上查看之前的项目副本，可以看到 GitHub 提示我们有新的分支，并且显示了一个大大的绿色按钮让我们可以检查我们的改动，并给源项目创建合并请求。

你也可以到“Branches”（分支）页面查看分支并创建合并请求： https://github.com/<用户名>/<项目名>/branches

![Figure 91. 合并请求按钮]()

如果你点击了那个绿色按钮，就会看到一个新页面，在这里我们可以对改动填写标题和描述，让项目的拥有者考虑一下我们的改动。

同时我们也能看到比主分支中所“领先”（ahead）的提交（在这个例子中只有一个）以及所有将会被合并的改动与之前代码的对比。

![Figure 92. 合并请求创建页面]()

当你单击了“Create pull request”（创建合并请求）的按钮后，这个项目的拥有者将会收到一条包含关改动和合并请求页面的链接的提醒。

> Note: 虽然合并请求通常是在贡献者准备好在公开项目中提交改动的时候提交，但是也常被用在仍处于开发阶段的内部项目中。因为合并请求在提交后 依然可以加入新的改动 ，它也经常被用来建立团队合作的环境，而不只是在最终阶段使用。

### 利用合并请求

现在，项目的拥有者可以看到你的改动并合并它，拒绝它或是发表评论。在这里我们就当作他喜欢这个点子，但是他想要让灯熄灭的时间比点亮的时间稍长一些。

接下来可能会通过电子邮件进行互动，就像我们在 分布式 Git 中提到的工作流程那样，但是在 GitHub，这些都在线上完成。项目的拥有者可以审查修改，只需要单击某一行，就可以对其发表评论。

![Figure 93. 对合并请求内的特定一行发表评论]()

当维护者发表评论后，提交合并请求的人，以及所有正在关注（Watching）这个版本库的用户都会收到通知。我们待会儿将会告诉你如何修改这项设置。现在，如果 Tony 有开启电子邮件提醒，他将会收到这样的一封邮件：

![Figure 94. 通过电子邮件发送的评论提醒]()

每个人都能在合并请求中发表评论。在 合并请求讨论页面 里我们可以看到项目拥有者对某行代码发表评论，并在讨论区留下了一个普通评论。你可以看到被评论的代码也会在互动中显示出来。

![Figure 95. 合并请求讨论页面]()

现在贡献者可以看到如何做才能让他们的改动被接受。幸运的是，这也是一件轻松的事情。如果你使用的是电子邮件进行交流，你需要再次对代码进行修改并重新提交至邮件列表，在 GitHub 上，你只需要再次提交到你的分支中并推送即可。

如果贡献者完成了以上的操作，项目的拥有者会再次收到提醒，当他们查看页面时，将会看到最新的改动。事实上，只要提交中有一行代码改动，GitHub 都会注意到并处理掉旧的变更集。

![Figure 96. 最终的合并请求]()

如果你点开合并请求的“Files Changed”（更改的文件）选项卡，你将会看到“整理过的”差异表 —— 也就是这个分支被合并到主分支之后将会产生的所有改动，其实就是 git diff master...<分支名> 命令的执行结果。你可以浏览 确定引入了哪些东西 来了解更多关于差异表的知识。

你还会注意到，GitHub 会检查你的合并请求是否能直接合并，如果可以，将会提供一个按钮来进行合并操作。这个按钮只在你对版本库有写入权限并且可以进行简洁合并时才会显示。你点击后 GitHub 将做出一个“非快进式”（non-fast-forward）合并，即使这个合并 能够 快进式（fast-forward）合并，GitHub 依然会创建一个合并提交。

如果你需要，你还可以将分支拉取并在本地合并。如果你将这个分支合并到 master 分支中并推送到 GitHub，这个合并请求会被自动关闭。

这就是大部分 GitHub 项目使用的工作流程。创建分支，基于分支创建合并请求，进行讨论，根据需要继续在分支上进行修改，最终关闭或合并合并请求。

> Note: 不必总是 Fork,有件很重要的事情：你可以在同一个版本库中不同的分支提交合并请求。如果你正在和某人实现某个功能，而且你对项目有写权限，你可以推送分支到版本库，并在 master 分支提交一个合并请求并在此进行代码审查和讨论的操作。不需要进行“Fork”。

### 合并请求的进阶用法

目前，我们学到了如何在 GitHub 平台对一个项目进行最基础的贡献。现在我们会教给你一些小技巧，让你可以更加有效率地使用合并请求。

### 将合并请求制作成补丁

有一件重要的事情：许多项目并不认为合并请求可以作为补丁，就和通过邮件列表工作的的项目对补丁贡献的看法一样。大多数的 GitHub 项目将合并请求的分支当作对改动的交流方式，并将变更集合起来统一进行合并。

这是个重要的差异，因为一般来说改动会在代码完成前提出，这和基于邮件列表的补丁贡献有着天差地别。这使得维护者们可以更早的沟通，由社区中的力量能提出更好的方案。当有人从合并请求提交了一些代码，并且维护者和社区提出了一些意见，这个补丁系列并不需要从头来过，只需要将改动重新提交并推送到分支中，这使得讨论的背景和过程可以齐头并进。

举个例子，你可以回去看看 最终的合并请求，你会注意到贡献者没有变基他的提交再提交一个新的合并请求，而是直接增加了新的提交并推送到已有的分支中。如果你之后再回去查看这个合并请求，你可以轻松地找到这个修改的原因。点击网页上的“Merge”（合并）按钮后，会建立一个合并提交并指向这个合并请求，你就可以很轻松的研究原来的讨论内容。

### 与上游保持同步

如果你的合并请求由于过时或其他原因不能干净地合并，你需要进行修复才能让维护者对其进行合并。GitHub 会对每个提交进行测试，让你知道你的合并请求能否简洁的合并。

![Figure 97. 不能进行干净合并]()

如果你看到了 不能进行干净合并 的画面，你就需要修复你的分支让这个提示变成绿色，这样维护者就不需要再做额外的工作。

你有两种方法来解决这个问题。你可以把你的分支变基到目标分支中去（通常是你派生出的版本库中的 master 分支），或者你可以合并目标分支到你的分支中去。

GitHub 上的大多数的开发者会使用后一种方法，基于我们在上一节提到的理由：我们最看重的是历史记录和最后的合并，变基除了给你带来看上去简洁的历史记录，只会让你的工作变得更加困难且更容易犯错。

如果你想要合并目标分支来让你的合并请求变得可合并，你需要将源版本库添加为一个新的远端，并从远端抓取内容，合并主分支的内容到你的分支中去，修复所有的问题并最终重新推送回你提交合并请求使用的分支。

在这个例子中，我们再次使用之前的“tonychacon”用户来进行示范，源作者提交了一个改动，使得合并请求和它产生了冲突。现在来看我们解决这个问题的步骤。

```
$ git remote add upstream https://github.com/schacon/blink (1)

$ git fetch upstream (2)
remote: Counting objects: 3, done.
remote: Compressing objects: 100% (3/3), done.
Unpacking objects: 100% (3/3), done.
remote: Total 3 (delta 0), reused 0 (delta 0)
From https://github.com/schacon/blink
 * [new branch]      master     -> upstream/master

$ git merge upstream/master (3)
Auto-merging blink.ino
CONFLICT (content): Merge conflict in blink.ino
Automatic merge failed; fix conflicts and then commit the result.

$ vim blink.ino (4)
$ git add blink.ino
$ git commit
[slow-blink 3c8d735] Merge remote-tracking branch 'upstream/master' \
    into slower-blink

$ git push origin slow-blink (5)
Counting objects: 6, done.
Delta compression using up to 8 threads.
Compressing objects: 100% (6/6), done.
Writing objects: 100% (6/6), 682 bytes | 0 bytes/s, done.
Total 6 (delta 2), reused 0 (delta 0)
To https://github.com/tonychacon/blink
   ef4725c..3c8d735  slower-blink -> slow-blink
```

1) 将源版本库添加为一个远端，并命名为“upstream”（上游）

2) 从远端抓取最新的内容

3) 将主分支的内容合并到你的分支中

4) 修复产生的冲突

5) 再推送回同一个分支

6) 你完成了上面的步骤后，合并请求将会自动更新并重新检查是否能干净的合并。

![Figure 98. 合并请求现在可以干净地合并了]()

Git 的伟大之处就是你可以一直重复以上操作。如果你有一个运行了十分久的项目，你可以轻松地合并目标分支且只需要处理最近的一次冲突，这使得管理流程更加容易。

如果你一定想对分支做变基并进行清理，你可以这么做，但是强烈建议你不要强行的提交到已经提交了合并请求的分支。

### Markdown

对于在 GitHub 中绝大多数文本框中能够做到的事，引用其他议题只是个开始。在议题和合并请求的描述，评论和代码评论还有其他地方，都可以使用“GitHub 风格的 Markdown”。Markdown 可以让你输入纯文本，但是渲染出丰富的内容。

查看 一个 Markdown 的例子和渲染效果 里的例子来了解如何书写评论或文本，并通过 Markdown 进行渲染。

![Figure 102. 一个 Markdown 的例子和渲染效果]()

### GitHub 风格的 Markdown

GitHub 风格的 Markdown 增加了一些基础的 Markdown 中做不到的东西。它在创建合并请求和议题中的评论和描述时十分有用。

### 任务列表
第一个 GitHub 专属的 Markdown 功能，特别是用在合并请求中，就是任务列表。一个任务列表可以展示出一系列你想要完成的事情，并带有复选框。把它们放在议题或合并请求中时，通常可以展示你想要完成的事情。

你可以这样创建一个任务列表：

- [X] 编写代码
- [ ] 编写所有测试程序
- [ ] 为代码编写文档
如果我们将这个列表加入合并请求或议题的描述中，它将会被渲染 Markdown 评论中渲染后的任务列表 这样。

任务列表示例
Figure 103. Markdown 评论中渲染后的任务列表
在合并请求中，任务列表经常被用来在合并之前展示这个分支将要完成的事情。最酷的地方就是，你只需要点击复选框，就能更新评论 —— 你不需要直接修改 Markdown。

不仅如此，GitHub 还会将你在议题和合并请求中的任务列表整理起来集中展示。举个例子，如果你在一个合并请求中有任务清单，你将会在所有合并请求的总览页面上看到它的进度。这使得人们可以把一个合并请求分解成不同的小任务，同时便于其他人了解分支的进度。你可以在 在合并请求列表中的任务列表总结 看到一个例子。

任务列表示例
Figure 104. 在合并请求列表中的任务列表总结
当你在实现一个任务的早期就提交合并请求，并使用任务清单追踪你的进度，这个功能会十分的有用。

摘录代码
你也可以在评论中摘录代码。这在你想要展示尚未提交到分支中的代码时会十分有用。它也经常被用在展示无法正常工作的代码或这个合并请求需要的代码。

你需要用“反引号”将需要添加的摘录代码包起来。

```java
for(int i=0 ; i < 5 ; i++)
{
   System.out.println("i is : " + i);
}
```
如果加入语言的名称，就像我们这里加入的“java”一样，GitHub 会自动尝试对摘录的片段进行语法高亮。在下面的例子中，它最终会渲染成这个样子： 渲染后的摘录代码示例 。

渲染后的摘录代码
Figure 105. 渲染后的摘录代码示例
引用
如果你在回复一个很长的评论之中的一小段，你只需要复制你需要的片段，并在每行前添加 > 符号即可。事实上，因为这个功能会被经常用到，它也有一个快捷键。只要你把你要回应的文字选中，并按下 r 键，选中的问题会自动引用并填入评论框。

引用的部分就像这样:

> Whether 'tis Nobler in the mind to suffer
> The Slings and Arrows of outrageous Fortune,

How big are these slings and in particular, these arrows?
经过渲染后，就会变成这样： 渲染后的引用示例

渲染后的引用
Figure 106. 渲染后的引用示例
表情符号（EMOJI）
最后，我们可以在评论中使用表情符号。这经常出现在 GitHub 的议题和合并请求的评论中。GitHub 上甚至有表情助手。如果你在输入评论时以 : 开头，自动完成器会帮助你找到你需要的表情。

表情符号自动完成器
Figure 107. 表情符号自动完成器
你也可以在评论的任何地方使用 :<表情名称>: 来添加表情符号。举个例子，你可以输入以下文字：

I :eyes: that :bug: and I :cold_sweat:.

:trophy: for :microscope: it.

:+1: and :sparkles: on this :ship:, it's :fire::poop:!

:clap::tada::panda_face:
渲染之后，就会变成这样： 使用了大量表情符号的评论

Emoji
Figure 108. 使用了大量表情符号的评论
虽然这个功能并不是非常实用，但是它在这种不方便表达感情的媒体里，加入了趣味的元素。

Note
事实上现在已经有大量的在线服务可以使用表情符号，这里有个列表可以让你快速的找到能表达你的情绪的表情符号：

http://www.emoji-cheat-sheet.com

图片
从技术层面来说，这并不是 GitHub 风格 Markdown 的功能，但是也很有用。如果不想使用 Markdown 语法来插入图片，GitHub 允许你通过拖拽图片到文本区来插入图片。

拖拽插入图片
Figure 109. 通过拖拽的方式自动插入图片
如果你回去查看 在合并请求中的交叉引用 ，你会发现文本区上有个“Parsed as Markdown”的提示。点击它你可以了解所有能在 GitHub 上使用的 Markdown 功能。





## 维护项目

### 维护项目

现在我们可以很方便地向一个项目贡献内容，来看一下另一个方面的内容：创建、维护和管理你自己的项目。

### 创建新的版本库

让我们创建一个版本库来分享我们的项目。 通过点击面板右侧的“New repository”按钮，或者顶部工具条你用户名旁边的 + 按钮来开始我们的旅程。 参见 这是 “New repository” 下拉列表.。


![Figure 110. 这是 ‘`Your repositories’'区域.]()

![Figure 111. 这是 “New repository” 下拉列表.]()

这会带你到 “new repository” 表单:


![Figure 112. 这是 “new repository” 表单.]()

这里除了一个你必须要填的项目名，其他字段都是可选的。 现在只需要点击 “Create Repository” 按钮，Duang!!! – 你就在 GitHub 上拥有了一个以 <user>/<project_name> 命名的新仓库了。

因为目前暂无代码，GitHub 会显示有关创建新版本库或者关联到一个已有的 Git 版本库的一些说明。

现在你的项目就托管在 GitHub 上了，你可以把 URL 给任何你想分享的人。 GitHub 上的项目可通过 HTTP 或 SSH 访问，格式是：HTTP ： https://github.com/<user>/<project_name> ， SSH ： git@github.com:<user>/<project_name> 。 Git 可以通过以上两种 URL 进行抓取和推送，但是用户的访问权限又因连接时使用的证书不同而异。

> Note: 通常对于公开项目可以优先分享基于 HTTP 的 URL，因为用户克隆项目不需要有一个 GitHub 帐号。 如果你分享 SSH URL，用户必须有一个帐号并且上传 SSH 密钥才能访问你的项目。

### 添加合作者

如果你想与他人合作，并想给他们提交的权限，你需要把他们添加为 “Collaborators”。 如果 Ben，Jeff，Louise 都在 GitHub 上注册了，你想给他们推送的权限，你可以将他们添加到你的项目。 这样做会给他们 “推送” 权限，就是说他们对项目和 Git 版本库都有读写的权限。

点击边栏底部的 “Settings” 链接。


![Figure 113. 版本库设置链接.]()

然后从左侧菜单中选择 “Collaborators” 。 然后，在输入框中填写用户名，点击 “Add collaborator.” 如果你想授权给多个人，你可以多次重复这个步骤。 如果你想收回权限，点击他们同一行右侧的 “X”


![Figure 114. 版本库合作者.]()

### 管理合并请求

现在你有一个包含一些代码的项目，可能还有几个有推送权限的合作者，下面来看当你收到合并请求时该做什么。

合并请求可以来自仓库副本的一个分支，或者同一仓库的另一个分支。 唯一的区别是 fork 过来的通常是和你不能互相推送的人，而内部的推送通常都可以互相访问。

作为例子，假设你是 “tonychacon” ，你创建了一个名为 “fade” 的 Arduino 项目.

### 邮件通知

有人来修改了你的代码，给你发了一个合并请求。 你会收一封关于合并请求的提醒邮件，它看起来像 新的合并请求的邮件通知.。

![Figure 115. 新的合并请求的邮件通知.]()

关于这个邮件有几个要注意的地方。 它会给你一个小的变动统计结果 — 一个包含合并请求中改变的文件和改变了多少的列表。 它还给你一个 GitHub 上进行合并请求操作的链接。 还有几个可以在命令行使用的 URL。


### 在合并请求上进行合作
就像我们在 GitHub 流程，_ 说过的，现在你可以跟开启合并请求的人进行会话。 你既可以对某些代码添加注释，也可以对整个提交添加注释或对整个合并请求添加注释，在任何地方都可以用 GitHub Flavored Markdown。

每次有人在合并请求上进行注释你都会收到通知邮件，通知你哪里发生改变。 他们都会包含一个到改变位置的链接，你可以直接在邮件中对合并请求进行注释。

Figure 116. Responses to emails are included in the thread.
一旦代码符合了你的要求，你想把它合并进来，你可以把代码拉取下来在本地进行合并，也可以用我们之前提到过的 git pull <url> <branch> 语法，或者把 fork 添加为一个 remote，然后进行抓取和合并。

对于很琐碎的合并，你也可以用 GitHub 网站上的 “Merge” 按钮。 它会做一个 “non-fast-forward” 合并，即使可以快进（fast-forward）合并也会产生一个合并提交记录。 就是说无论如何，只要你点击 merge 按钮，就会产生一个合并提交记录。 你可以在 合并按钮和手工合并一个合并请求的指令. 看到，如果你点击提示链接，GitHub 会给你所有的这些信息。


![Figure 117. 合并按钮和手工合并一个合并请求的指令.]()

如果你决定不合并它，你可以把合并请求关掉，开启合并请求的人会收到通知。



### 合并请求之上的合并请求

你不仅可以在主分支或者说 master 分支上开启合并请求，实际上你可以在网络上的任何一个分支上开启合并请求。 其实，你甚至可以在另一个合并请求上开启一个合并请求。

如果你看到一个合并请求在向正确的方向发展，然后你想在这个合并请求上做一些修改或者你不太确定这是个好主意，或者你没有目标分支的推送权限，你可以直接在合并请求上开启一个合并请求。

当你开启一个合并请求时，在页面的顶端有一个框框显示你要合并到哪个分支和你从哪个分支合并过来的。 如果你点击那个框框右边的 “Edit” 按钮，你不仅可以改变分支，还可以选择哪个 fork。

![Figure 118. 手工修改合并请求的目标.]()
这里你可以很简单地指明合并你的分支到哪一个合并请求或 fork。

提醒和通知
GitHub 内置了一个很好的通知系统，当你需要与别人或别的团队交流时用起来很方便。

在任何评论中你可以先输入一个`@`，系统会自动补全项目中合作者或贡献者的名字和用户名。

提醒
Figure 119. 输入 @ 来提醒某人.
你也可以提醒不在列表中的用户，但是通常自动补全用起更快。

当你发布了一个带用户提醒的评论，那个用户会收到通知。 这意味着把人们拉进会话中要比让他们投票有效率得多。 对于 GitHub 上的合并请求，人们经常把他们团队或公司中的其它人拉来审查问题或合并请求。

如果有人收到了合并请求或问题的提醒，他们会＂订阅＂它，后面有新的活动发生他们都会持续收到提醒。 如果你是合并请求或者问题的发起方你也会被订阅上，比如你在关注一个版本库或者你评论了什么东西。 如果你不想再收到提醒，在页面上有个 “Unsubscribe” 按钮，点一下就不会再收到更新了。

取消订阅
Figure 120. 取消订阅一个问题或合并请求.
通知页面
当我们在这提到特指 GitHub 的 “notifications” ，指的是当 GitHub 上有事件发生时，它通知你的方式，这里有几种不同的方式来配置它们。 如果你打开配置页面的 “Notification center” 标签，你可以看到一些选项。


![Figure 121. 通知中心选项.](s)

有两个选项，通过＂邮件(Email)＂和通过＂网页(Web)＂，你可以选用一个或者都不选或者都选。

### 网页通知

网页通知只在 GitHub 上存在，你也只能在 GitHub 上查看。 如果你打开了这个选项并且有一个你的通知，你会在你屏幕上方的通知图标上看到一个小蓝点。参见 通知中心.。


![Figure 122. 通知中心.]()

如果你点击那个玩意儿，你会看到你被通知到的所有条目，按照项目分好了组。 你可以点击左边栏的项目名字来过滤项目相关的通知。 你可以点击通知旁边的对号图标把通知标为已读，或者点击组上面的图标把项目中 所有的 通知标为已读。 在每个对号图标旁边都有一个静音按钮，你可以点一下，以后就不会收到它相关的通知。

所有这些工具对于处理大量通知非常有用。 很多 GitHub 资深用户都关闭邮件通知，在这个页面上处理他们所有的通知。

### 邮件通知
邮件通知是你处理 GitHub 通知的另一种方式。 如果你打开这个选项，每当有通知时，你会收到一封邮件。 我们在 通过电子邮件发送的评论提醒 和 新的合并请求的邮件通知. 看到了一些例子。 邮件也会被合适地按话题组织在一起，如果你使用一个具有会话功能的邮件客户端那会很方便。

GitHub 在发送给你的邮件头中附带了很多元数据，这对于设置过滤器和邮件规则非常有帮助。

举个例子，我们来看一看在 新的合并请求的邮件通知. 中发给 Tony 的一封真实邮件的头部，我们会看到下面这些：

To: tonychacon/fade <fade@noreply.github.com>
Message-ID: <tonychacon/fade/pull/1@github.com>
Subject: [fade] Wait longer to see the dimming effect better (#1)
X-GitHub-Recipient: tonychacon
List-ID: tonychacon/fade <fade.tonychacon.github.com>
List-Archive: https://github.com/tonychacon/fade
List-Post: <mailto:reply+i-4XXX@reply.github.com>
List-Unsubscribe: <mailto:unsub+i-XXX@reply.github.com>，...
X-GitHub-Recipient-Address: tchacon@example.com
这里有一些有趣的东西。 如果你想高亮或者转发这个项目甚至这个合并请求相关的邮件，Message-ID 中的信息会以 <user>/<project>/<type>/<id> 的格式展现所有的数据。 例如，如果这是一个问题(issue)，那么 <type> 字段就会是 “issues” 而不是 “pull” 。

List-Post 和 List-Unsubscribe 字段表示如果你的邮件客户端能够处理这些，那么你可以很容易地在列表中发贴或取消对这个相关帖子的订阅。 那会很有效率，就像在页面中点击静音按钮或在问题／合并请求页面点击 “Unsubscribe” 一样。

值得注意的是，如果你同时打开了邮件和网页通知，那么当你在邮件客户端允许加载图片的情况下阅读邮件通知时，对应的网页通知也将会同时被标记为已读。

特殊文件
如果你的版本库中有一些特殊文件，GitHub 会提醒你。

README
第一个就是 README 文件，可以是几乎任何 GitHub 可以识别的格式。 例如，它可以是 README ，README.md ， README.asciidoc 。 如果 GitHub 在你的版本库中找到 README 文件，会把它在项目的首页渲染出来。

很多团队在这个文件里放版本库或项目新人需要了解的所有相关的信息。 它一般包含这些内容：

该项目的作用

如何配置与安装

有关如何使用和运行的例子

项目的许可证

如何向项目贡献力量

因为 GitHub 会渲染这个文件，你可以在文件里植入图片或链接让它更容易理解。

贡献 CONTRIBUTING
另一个 GitHub 可以识别的特殊文件是 CONTRIBUTING 。 如果你有一个任意扩展名的 CONTRIBUTING 文件，当有人开启一个合并请求时 GitHub 会显示 开启合并请求时有 CONTRIBUTING 文件存在.。

贡献注意事项
Figure 123. 开启合并请求时有 CONTRIBUTING 文件存在.
这个的作用就是你可以在这里指出对于你的项目开启的合并请求你想要的／不想要的各种事情。 这样别人在开启合并请求之前可以读到这些指导方针。

项目管理
对于一个单个项目其实没有很多管理事务要做，但也有几点有趣的。

改变默认分支
如果你想用 “master” 之外的分支作为你的默认分支，其他人将默认会在这个分支上开启合并请求或进行浏览，你可以在你版本库的设置页面的 "options" 标签下修改。

默认分支
Figure 124. 改变项目的默认分支.
简单地改变默认分支下拉列表中的选项，它就会作为所有主要操作的默认分支，他人进行克隆时该分支也将被默认检出。

移交项目
如果你想把一个项目移交给 GitHub 中的另一个人或另一个组织，还是设置页面的这个 ＂options＂标签下有一个 “Transfer ownership” 选项可以用来干这个。

移交
Figure 125. 把项目移交给另一个 GitHub 用户或组织。
当你正准备放弃一个项目且正好有别人想要接手时，或者你的项目壮大了想把它移到一个组织里时，这就管用了。

这么做不仅会把版本库连带它所有的观察和星标数都移到另一个地方，它还会将你的 URL 重定向到新的位置。 它也重定向了来自 Git 的克隆和抓取，而不仅仅是网页端请求。



## 管理组织




## 脚本Github

