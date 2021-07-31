# 学习笔记

## git部分

### ①gitflow：

1.在gitflow中，`master`分支用于保存官方的发布历史，而`develop`分支用于集成各种功能开发。

2.在进行功能开发时，应该从`develop`分支创建新的开发分支并在开发结束后`merge`回`develop`分支。

3.可以创建一个`release`分支用于发布产品，在这个分支上不增加新功能，只修改bug，并最终`merge`到`master`分支上并打上版本号`tag`。

4.创建用于发布后的维护工作或者紧急问题的快速修复的分支应直接基于`master`创建，一旦问题被修复就应该被合并入`master`和`develop`分支（或者用于当前发布的分支）。在这之后`master`上还要使用更新的版本号打好标签。

#### 一次协作开发的完整流程：

1.为`master`分支创建一个`develop`分支，并让所有协作者`clone`这个中心仓库，同时创建`develop`的追踪分支。

2.开发者分别从`develop`分支上创建自己的分支开始工作。

3.开发者完成开发后的工作：

​	方式①：提出`pull request`请求并入`develop`分支。

​	方式②：自行并入`develop`仓库并提交。

```bash
$ git pull origin develop
$ git checkout develop
$ git merge some-feature
$ git push
$ git branch -d some-feature
```

​					注意：应先检查中心仓库是否有改动，并且不能直接并入`master`分支。

4.开发者准备发布：`$ git checkout -b release-0.1 develop`

​	新建一个`release`分支用于发布前的准备，包括一些清理工作、全面的测试、文档的更新以及任何其他的准备工作。一旦有一位发布者创建了这个分支并把它推向中央仓库，这次产品发布包含的功能也就固定下来了。任何还处于开发状态的功能只能等待下一个发布周期。

5.发布结束后：把发布分支合并入`master`和`develop`分支，然后再将发布分支删除。

```bash
$ git checkout master
$ git merge release-0.1
$ git push
$ git checkout develop
$ git merge release-0.1
$ git push
$ git branch -d release-0.1
```

​	注①：此时适合code review。

​	注②：发布分支扮演的角色是功能开发（`develop`）与官方发布（`master`）之间的一个缓冲。无论什么时候把东西合并入`master`，都应该随即打上合适的标签。

```bash
$ git tag -a 0.1 -m"Initial public release" master
$ git push --tags
```

​	注③：可以在服务端配置`hook`来提高推送完`master`分支后产品发布构建的效率。

6.用户发现了bug：

​	基于`master`创建一个用于维护的分支，并在此分支上修复了bug，然后把改动的代码直接合并入`master`分支。

```bash
$ git checkout -b issue-#001 master
\# Fix the bug
$ git checkout master
$ git merge issue-#001
$ git push
```

​	同时也要将这些改动合并到`develop`分支，并删除维护分支。

```bash
$ git checkout develop
$ git merge issue-#001
$ git push
$ git branch -d issue-#001
```

### ②github flow：

**工作流程：**1.根据需求，从`master`拉出新分支，不区分功能分支或补丁分支。

​					2.新分支开发完成后，或者需要讨论的时候，就向`master`发起一个`pull request`。

​					3.PR让别人注意到请求，也让大家一起评审和讨论你的代码。同时你还可以不断提交代码。

​					4.你的PR被合并进`master`，重新部署后，原来的那个分支就被删除。（或先部署再合并）

### ③gitlab flow：

1.最大原则：上游优先"（upstream first），即只存在一个主分支`master`，是所有其他分支的"上游"。只有上游分支采纳的代码变化，才能应用到其他分支。

2.持续发布的项目：

​	应在`master`分支以外，再建立不同的环境分支。

​	例："开发环境"的分支是`master`，"预发环境"的分支是`pre-production`，"生产环境"的分支是`production`。

​	开发分支是预发分支的"上游"，预发分支又是生产分支的"上游"。代码的变化，必须由"上游"向"下游"发展。

​	例：生产环境出现了bug，这时就要新建一个功能分支，先把它合并到`master`，确认没有问题，再`cherry-pick`到`pre-production`，这一步也没有问题，才进入`production`。

​	注：只有紧急情况，才允许跳过上游，直接合并到下游分支。

3.版本发布：每一个稳定版本，都要从`master`分支拉出一个分支。

​	例：`2-3-stable`、`2-4-stable`。

​	以后，只有修补bug，才允许将代码合并到这些分支，并且此时要更新小版本号。

### ④一些技巧：

#### 一、issue相关：

Issue 用于 Bug追踪和需求管理。建议先新建 Issue，再新建对应的功能分支。功能分支总是为了解决一个或多个 Issue。功能分支的名称，可以与issue的名字保持一致，并且以issue的编号起首，比如"15-require-a-password-to-change-it"。开发完成后，在提交说明里面，可以写上"fixes #14"或者"closes #67"。Github规定，只要commit message里面有下面这些动词+ 编号，就会关闭对应的issue。

<u>close、closes、closed、fix、fixes、fixed、resolve、resolves、resolved</u>

这种方式还可以一次关闭多个issue，或者关闭其他代码库的issue。

格式：`username/repository#issue_number`

Pull Request被接受以后，issue关闭，原始分支就应该删除。如果以后该issue重新打开，新分支可以复用原来的名字。

#### 二、节点commit相关：

fast forward模式下的merge不会单独生成节点，而普通模式会。

前者不利于保持commit信息的清晰，也不利于以后的回滚，建议总是采用后者（使用`--no-ff`参数）。只要发生合并，就要有一个单独的合并节点。

#### 三、合并commit相关：

为了便于他人阅读你的提交，也便于`cherry-pick`或撤销代码变化，在发起PR之前，应该把多个commit合并成一个。（前提是该分支只有一个人开发且没有跟`master`合并过）

可采用`rebase`命令附带的`squash`操作，并且可以把squash和fixup当命令行参数用，自动合并。

**具体流程：**①`$ git rebase -i origin/master`i其中的参数表示互动（interactive）。

​					②列出当前分支最新的commit（越下面越新）。每个commit前面有一个操作命令，默认是pick，表示该行commit被选中，要进行rebase操作。

可以使用的命令：

   1. pick：正常选中；

   2. reword：选中，并且修改提交信息；

   3. edit：选中，rebase时会暂停，允许你修改这个commit；

   4. squash：选中，会将当前commit与上一个commit合并；

   5. fixup：与squash相同，但不会保存当前commit的提交信息；

   6. exec：执行其他shell命令。

      ​			squash和fixup可以用来合并commit。把需要合并的commit前面改成squash（或s）。

      ​			注①：用squash合并之后的commit仍然包含多条commit，不过捆绑在一起，其他不变。

      ​			注②：用fixup合并之后的commit仍然包含多条commit，也捆绑在一起，不过会被注释掉。

      ​			注③：合并commit后，就可以推送当前分支到远程仓库了，git push命令要加上force参数，因为rebase以后，分支历史改变了，跟远程分支不一定兼容，有可能要强行推送。

      `$ git push --force origin myfeature`

      *还有别的合并的方法，但太过麻烦。
