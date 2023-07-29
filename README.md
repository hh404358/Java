# Java
learning Java

一.git&github
(1)gitignore
git status，检查下是否还有我们不需要的文件会被添加到git中去：

$ git status
On branch master

Initial commit

Changes to be committed:
  (use "git rm --cached <file>..." to unstage)

        new file:   .gitignore
        new file:   HelloWorld.java

Untracked files:
  (use "git add <file>..." to include in what will be committed)

        Config.ini
比如我的项目目录下有一个Config.ini文件，这个是个本地配置文件我不希望上传到git中去，我们可以在gitignore文件中添加这样的配置：

Config.ini
或者你想忽略所有的.ini文件你可以这样写：

*.ini
如果有些文件已经被你忽略了，当你使用git add时是无法添加的，比如我忽略了*.class，现在我想把HelloWorld.class添加到git中去：

$ git add HelloWorld.class
The following paths are ignored by one of your .gitignore files:
HelloWorld.class
Use -f if you really want to add them.
git会提示我们这个文件已经被我们忽略了，需要加上-f参数才能强制添加到git中去：

$ git status
On branch master

Initial commit

Changes to be committed:
  (use "git rm --cached <file>..." to unstage)

        new file:   .gitignore
        new file:   HelloWorld.class
        new file:   HelloWorld.java
这样就能强制添加到缓存中去了。
如果我们意外的将想要忽略的文件添加到缓存中去了，我们可以使用rm命令将其从中移除：

$ git rm HelloWorld.class --cached
rm 'HelloWorld.class'
如果你已经把不想上传的文件上传到了git仓库，那么你必须先从远程仓库删了它，我们可以从远程仓库直接删除然后pull代码到本地仓库这些文件就会本删除，或者从本地删除这些文件并且在.gitignore文件中添加这些你想忽略的文件，然后再push到远程仓库。

5.查看gitignore规则
如果你发下.gitignore写得有问题，需要找出来到底哪个规则写错了，可以用git check-ignore命令检查：

$ git check-ignore -v HelloWorld.class
.gitignore:1:*.class    HelloWorld.class
可以看到HelloWorld.class匹配到了我们的第一条*.class的忽略规则所以文件被忽略了。

6.忽略规则文件语法
a.忽略指定文件/目录
# 忽略指定文件
HelloWrold.class

# 忽略指定文件夹
bin/
bin/gen/
b.通配符忽略规则
通配符规则如下：


# 忽略.class的所有文件
*.class

# 忽略名称中末尾为ignore的文件夹
*ignore/

# 忽略名称中间包含ignore的文件夹
*ignore*/

链接：https://www.jianshu.com/p/a09a9b40ad20
(2)license


总结一下，MIT 最自由，简直就是没有任何限制，任何人都可以售卖我的软件，甚至可以用我的名字促销。BSD 和 Apache 协议也很自由，跟 MIT 的区别分别是不允许用作者本人名义促销和保护作者版权。GPL 可以说最霸道，对代码的修改部分也必须是 GPL 的，同时基于 GPL 代码而开发的代码也必须按照 GPL 发布，而 MPL ，也就是 Mozilla Public License 就温和一些，如果后续开发的代码中添加了新文件，同时新文件中也没有用到原来的代码，那么新文件可以不必继续沿用 MPL 。

