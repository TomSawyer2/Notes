# git学习笔记

1. git是用C语言写的。

2. git的初始化：

   ```c
   $ git config --global user.name "Name"
   $ git config --global user.email "email@email.com"
   ```

   注：`global`参数在电脑上所有仓库默认使用同一个名称与邮箱。

3. git创建目录：`$ mkdir XXX`

4. git指定目录：`$ cd XXX`

5. git显示当前目录：`$ pwd`

6. git将目录变为可以管理的仓库：`$ git init`

   注：此时在文件夹中会生成一个`.git`目录，可以用`ls -ah`看见。

7. 将文件添加到仓库：`$ git add 1.txt`

   注：文件一定要放在之前创建的文件夹内。

8. 将文件提交到仓库：`$ git commit -m "your commit"`

   注：一次`commit`可以提交多个文件，所以可以先多次add再一次提交。

9. 查看仓库目前的状态：`$ git status`

   注：可以查看仓库内文件是否有修改，修改是否添加、提交。

10. 查看文件具体的变化：`$ git diff`

11. 查看文件夹内具体的文件：`$ ls`或者 `$ dir`

12. 查看提交日志：`$ git log`

    注①：显示的log是从最近到最远排列的。

    注②：可以加上`--pretty=oneline`来简化输出。

    注③：一大串黄色的字符为`commit id`（版本号）。

    **注④：这条命令用于确定回到的旧版本。**

13. git的版本：`HEAD`表示当前版本，`HEAD^`表示上一个版本，`HEAD^^`表示上上个版本。当然也可以用`HEAD~100`来表示100个之前的版本。

14. 版本回退：`$ git reset --hard HEAD^`

15. 可以用`$ cat 1.txt`看文件的具体内容。

16. 版本前进：`$ git reset --hard da4623 `

    注：`da4623`为新版本的`commit id`的前几位，只要不跟其它id混淆，git就可以自己去找。

17. 查看对版本变换的每一次记录：`$ git reflog`

    **注：这条命令用来找新版本的`commit id`。**

18. 工作区：用于更改、保存文件的文件夹。

    版本库：包含暂存区`stage`（或者叫`index`）和自动创建的第一个分支`master`。当然还有指向`master`的指针`HEAD`。

    因此`add`的时候是把文件从工作区移至暂存区，`commit`的时候是把文件从暂存区移至`master`。

    当提交之后没有对工作区进行任何修改，那么工作区就是干净的，暂存区里没东西。

    因此每次修改工作区里的文件时都要重新`add`一遍，不然提交的就是暂存区里的旧版本或者压根啥都没提交。

19. 撤销工作区的修改：`git checkout -- 1.txt`

    注：这里会回退到最新一次的`add`或者`commit`。

20. 注意：`--`非常重要，否则就变成了切换到另一个分支。

21. 如果已经add了但还没有commit：`git reset HEAD 1.txt`就可以撤销暂存区的修改，回退到工作区去。

    然后再用一遍19.就可以完全撤销修改了。

22. 从工作区删文件：`$ rm 1.txt`或者直接删掉。

    从版本库中删掉：删掉工作区的文件之后再`$ git rm 1.txt`，并且再次`commit`。

    当然从工作区删掉文件之后`rm`和`add`的作用已经一样了。

    注：如果工作区中的文件被删掉了，但版本库里的文件还没被删掉，仍然可以去回档，但只能回到最新版本，最新一次提交修改的内容会消失。

23. 创建SSH密钥：`$ ssh-keygen -t rsa -C "email@email.com"`

    创建了之后目录内会有一个`.ssh`目录并且里面会有`id_rsa`和`id_rsa.pub`两个文件。

    要把`id_rsa.pub`内的内容添加至`github`上，当然可以有多个`ssh`。

24. 在`github`上创建仓库并与本地仓库相关联：`$ git remote add origin git@github.com:TomSawyer2/learn-to-use-git.git`

    注①：`origin`是远程库的名字。

    注②：`learn-to-use-git`是`github`上仓库的名字。

25. 向`github`上推送本地库：`$ git push -u origin master`

    注①：实际上推送的是本地的`master`分支。

    注②：第一次向远程的空仓库推送时加上`-u`会将本地的`master`分支的内容推送至线上的`master`分支，还会把本地的`master`分支和远程的`master`分支关联起来，方便以后的操作。

    注③：之后向远程的仓库推送时只需要写：`$ git push origin master`即可。

26. 第一次对`github`使用`clone`或者`push`命令时会得到检验`ssh`是否来自`github`服务器的警告。输入`yes`之后就会把这个`key`添加到本机的信任列表内，以后就不用再写了。

27. 若在没有本地库的时候从远程库上克隆内容，则需在`github`上新建一个自带`readme`的工程，再用`$ git clone git@github.com:TomSawyer2/learn-to-use-remote-git.git`克隆一个本地库。

28. `git`支持多种协议，包括`https`，但`ssh`协议速度最快。

29. 创建分支`dev`并切换到该分支：`$ git checkout -b dev`

    `git checkout`命令加上`-b`参数表示创建并切换，相当于以下两条命令：

    ```c
    $ git branch dev
    $ git checkout dev
    ```

    查看分支：`$ git branch`标`*`号的是当前分支。

    然后就可以在`dev`分支上进行提交了。

    最后切换回`master`：`$ git checkout master`

    注：在不同分支上提交的文件是相互独立的。

30. 把`dev`分支合并到`master`分支：`$ git merge dev`

    `git merge`命令用于合并指定分支到当前分支。

    **`Fast-forward`是“快进模式”，不会保留分支记录。**

31. 删除分支：`$ git branch -d dev`

32. 在分支上工作会更加安全。

33. 新版的`git`用`switch`切换分支：

    创建并切换到新的`dev`分支：`$ git switch -c dev`

    直接切换到已有的`master`分支：`$ git switch master`

34. 用`switch`比用`checkout`容易理解，并且不容易弄混淆。

35. 当多个分支之间的文件各自有更新时直接合并会出现冲突，用`$ git status`可以查看冲突的文件。此时直接`cat`文件会看到`git`用`<<<<<<<`，`=======`，`>>>>>>>`标记出不同分支的内容。

36. 查看合并分支图：`$ git log --graph`

37. 禁用`Fast foward`模式为普通模式并合并两个分支同时创建一个新的`commit`：`$ git merge --no-ff -m "commit" dev`

    **这样可以从`$ git log`分支历史上看分支信息。**

38. 实际开发的原则：`master`用于发布正式版本，`dev`用于提交临时版本，每个人都有自己的分支。

39. 当工作分支上有文件未提交时又要创建新的分支时要用`$ git stash`储藏工作现场。此时再`$ git status`会发现工作区是干净的。

    用`$ git stash list`查看被储藏的文件。

    恢复的方法①：`$ git stash apply`先恢复`stash`的内容，但此时`stash`的内容并不会被删除，需要再`$ git stash drop`来删除`stash`的内容。 

    恢复的方法②：`$ git stash pop`恢复的同时把`stash`的内容也删了。

40. 可以多次`stash`，并且在恢复的时候可以恢复指定的`stash`：`$ git stash apply stash@{0}`其中`stash@{0}`为指定的`stash`的名字，可以通过`$ git stash list`查看。

41. 复制一次特定的提交到当前分支：`$ git cherry-pick 4c805e2`，其中`4c805e2`为这次提交的名字。复制提交到当前分支后`dev`会自动提交当前分支。

    注：虽然两次提交的内容一样，但提交的名字是不一样的。

42. 如果要删除一个没有被合并过的分支`feature1`，需要写：`$ git branch -D feature1`

    注：`-D`要大写。

43. 当从远程仓库克隆时，实际上`git`自动把本地的`master`分支和远程的`master`分支对应起来了，并且远程仓库的默认名称是`origin`。

44. 查看远程库的信息：`$ git remote`

    显示更加详细的信息：`$ git remote -v`

    注：显示的`fetch`是可以抓取`origin`的地址；显示的`push`是可以推送的`origin`的地址。如果没有推送权限，就看不到`push`的地址。

45. `$ git push origin dev`推送的是`dev`分支，因此可以向线上库提交任意本地分支。

    注：一般提交`master`分支和`dev`分支，当然`feature`分支也可以选择提交，其它分支是否提交看心情。

46. 在多人协作时如果线上库与本地库有分支文件冲突，应该先`$ git pull`把线上分支的最新提交抓取下来，然后在本地合并后再推送。

47. 若`git pull`失败，则原因是没有指定本地`dev`分支与远程`origin/dev`分支的链接，根据提示，设置`dev`和`origin/dev`的链接：

    ```c
    $ git branch --set-upstream-to=origin/dev dev
    ```

48. 多人协作的工作模式：

    ①首先，可以试图用`git push origin <branch-name>`推送自己的修改；

    ②如果推送失败，则因为远程分支比你的本地更新，需要先用`git pull`试图合并；

    ③如果合并有冲突，则解决冲突，并在本地提交；

    ④没有冲突或者解决掉冲突后，再用`git push origin <branch-name>`推送就能成功！

    如果`git pull`提示`no tracking information`，则说明本地分支和远程分支的链接关系没有创建，用命令`git branch --set-upstream-to <branch-name> origin/<branch-name>`。

49. 在本地创建和远程分支对应的分支：`git checkout -b branch-name origin/branch-name`，本地和远程分支的名称最好一致。

50. `$ git rebase`用于合并修改并把分叉的修改变为一条直线。

    注：此时本地的分叉提交已经被修改过了。

    ①`rebase`操作可以把本地未`push`的分叉提交历史整理成直线；

    ②`rebase`的目的是使得我们在查看历史提交的变化时更容易，因为分叉的提交需要三方对比。

51. `tag`是跟`commit`绑定在一起的容易记的名字，实质是指向某个`commit`的指针，但不同于分支，`tag`不能移动。

52. 打`tag`的方式：

    ①移动到要打`tag`的分支；

    ②`$ git tag v1.0`；

    注：默认`tag`打在最新的`commit`上。

    *如果要对之前的某一个`commit`打`tag`：`$ git tag v0.9 f52c633`

    注：`f52c633`为`commit id`。

53. 查看所有tag：`$ git tag`

    注：`tag`不是按时间顺序列出的，而是按字母排序的。

    查看tag的具体信息：`$ git show v1.0`

54. 创建带有说明的`tag`：`$ git tag -a v1.0 -m "v1.0 released" 1094adb`，`-a`指定标签名，`-m`指定说明文字，`1094adb`为`commit id`。

55. 注意：`tag`总是和某个`commit`挂钩。如果这个`commit`既出现在`master`分支，又出现在`dev`分支，那么在这两个分支上都可以看到这个`tag`。

56. 删除本地的tag：`$ git tag -d v1.0`

    删除远程库的tag：

    ①删除本地的tag：`$ git -d v1.1 `

    ②删除远程库的tag：` $ git push origin :refs/tags/v1.1`

57. 向远程库推送单个标签：`$ git push origin v1.0`

    向远程库推送所有标签：` $ git push origin --tags`

    注：标签是不会自动推送到远程库的。

58. 在`GitHub`上，可以任意`Fork`开源仓库到自己的远程库；

    自己拥有`Fork`后的远程库的读写权限；

    可以推送`pull request`给官方仓库来贡献代码。

59. 删除已有的远程库：`$ git remote rm origin`

60. 若同时想关联多个远程库则需要把`origin`改为其它不重复的名字。

    例①：`github`：`$ git remote add github git@github.com:TomSawyer2/learn-to-use-git.git`

    例②：`gitee`：`$ git remote add gitee git@gitee.com:TomSawyer2/leatn-to-use-git.git`

    此时：如果要推送到`GitHub`，使用命令：`$ git push github master`

    ​			如果要推送到`Gitee`，使用命令：`$ git push gitee master`

61. 让`git`显示颜色：`$ git config --global color.ui true`

62. 忽略特殊文件：在`git`工作区的根目录下创建一个特殊的`.gitignore`文件，然后把要忽略的文件名填进去即可。

    *忽略文件的原则：

    ①忽略操作系统自动生成的文件，比如缩略图等；

    ②忽略编译生成的中间文件、可执行文件等，也就是如果一个文件是通过另一个文件自动生成的，那自动生成的文件就没必要放进版本库，比如`Java`编译产生的`.class`文件；

    ③忽略你自己的带有敏感信息的配置文件，比如存放口令的配置文件。

    注：所有配置文件可以直接在线浏览：https://github.com/github/gitignore

    检验`.gitignore`的标准是`$ git status`命令是不是说`working directory clean`。

63. 如果要强制加入已经被忽略的文件，应`$ git add -f 1.class`

    或者在`.gitignore`里面直接写`!.gitignore`或者`!1.class`来排除这两个文件。

64. 检查`.gitignore`文件的问题：`$ git check-ignore -v 1.class`

    会返回是`.gitignore`文件的第几行规则忽略了此文件。

65. `.gitignore`文件本身要放到版本库里，并且可以对`.gitignore`做版本管理。

66. 设置别名：`$ git config --global alias.st status`可以用`st`来代表`status`。

    `--global`参数是全局参数，也就是这些命令在这台电脑的所有`git`仓库下都有用，否则只对当前仓库起作用。

    注：本机的别名：`st->status`、`unstage->reset HEAD` 、`last->log -1`、`lg->log --color --graph --pretty=format:'%Cred%h%Creset -%C(yellow)%d%Creset %s %Cgreen(%cr) %C(bold blue)<%an>%Creset' --abbrev-commit`

    其中`last`可以用于看上一次`commit`，`lg`可以看所有`commit`。

67. 每个仓库的`git`配置文件都放在`.git/config`文件中。

    别名就在`[alias]`后面，要删除别名，直接把对应的行删掉即可。

    而当前用户的`git`配置文件放在用户主目录下的一个隐藏文件`.gitconfig`中。

    配置别名也可以直接修改这个文件，如果改错了，可以删掉文件重新通过命令配置。

68. `sourcetree yyds！`

    

    