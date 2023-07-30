# Java learning 

## 一.git&github
### (1)github
#### gitignore
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
 ##### 忽略指定文件
HelloWrold.class

 ##### 忽略指定文件夹
bin/
bin/gen/
b.通配符忽略规则
通配符规则如下：


##### 忽略.class的所有文件
*.class

##### 忽略名称中末尾为ignore的文件夹
*ignore/

##### 忽略名称中间包含ignore的文件夹
*ignore*/

链接：https://www.jianshu.com/p/a09a9b40ad20
### (2)license


总结一下，MIT 最自由，简直就是没有任何限制，任何人都可以售卖我的软件，甚至可以用我的名字促销。BSD 和 Apache 协议也很自由，跟 MIT 的区别分别是不允许用作者本人名义促销和保护作者版权。GPL 可以说最霸道，对代码的修改部分也必须是 GPL 的，同时基于 GPL 代码而开发的代码也必须按照 GPL 发布，而 MPL ，也就是 Mozilla Public License 就温和一些，如果后续开发的代码中添加了新文件，同时新文件中也没有用到原来的代码，那么新文件可以不必继续沿用 MPL 。

### (3)git
#### bug1:网页拒绝请求
127.0.0.1和localhost不能正确映射的问题 - rulasann - 博客园 (cnblogs.com)
#### bug2:fatal: unable to access 'https://github.com/xxx/autowrite.git/': OpenSSL SSL_read: Connection was reset, errno 10054

fatal: unable to access 'https://github.com/xxx/autowrite.git/':
Failed to connect to github.com port 443: Timed out
因为git在拉取或者提交项目时，中间会有git的http和https代理，但是我们本地环境本身就有SSL协议了，所以取消git的https代理即可，不行再取消http的代理。

后续
原因还有一个，当前代理网速过慢，所以偶尔会成功，偶尔失败。

##### 解决方案
1.在项目文件夹的命令行窗口执行下面代码，然后再git commit 或git clone
取消git本身的https代理，使用自己本机的代理，如果没有的话，其实默认还是用git的

//取消http代理
git config --global --unset http.proxy
//取消https代理 
git config --global --unset https.proxy
2.科学上网（vpn）
这样就能提高服务器连接速度，能从根本解决 time out 443问题
原文链接：https://blog.csdn.net/good_good_xiu/article/details/118567249
#### bug3:git中的SSL certificate problem: unable to get local issuer certificate错误的解决办法 
　　我们在使用git初始化一个项目时，尤其是通过git submodule update --init --remote初始化子模块时，可能会遇到下面这个错误：

fatal: unable to access 'https://myserver.com/gogs/user1/myapp/': SSL certificate problem: unable to get local issuer certificate
　　这是由于当你通过HTTPS访问Git远程仓库的时候，如果服务器上的SSL证书未经过第三方机构认证，git就会报错。原因是因为未知的没有签署过的证书意味着可能存在很大的风险。解决办法就是通过下面的命令将git中的sslverify关掉：

git config --global http.sslverify false
　　上面这行命令的影响范围是系统当前用户，如果要设置为全局所有用户，可以改成这样：

 git config --system http.sslverify false
　　如果只是想针对当前仓库进行设置，可以在需要修改的仓库目录下执行：

 git config http.sslverify false
　　如果你的仓库中存在嵌套的git子模块（就是子模块中又引用了子模块），在进行初始化时，仍然有可能遇到self signed certificate in certificate chain的错误，此时可以通过执行下面的命令来解决：

npm config set strict-ssl false
[原文链接]：(https://blog.csdn.net/qq_55125921/article/details/125220576)

#### bug4: LF will be replaced by CRLF
一、问题
windows平台进行 git add 时，控制台打印警告warning: in the working copy of ‘XXX.py’, LF will be replaced by CRLF the next time Git touches it
 

二、问题分析
Dos/Windows平台默认换行符：回车（CR）+换行（LF），即’\r\n’
Mac/Linux平台默认换行符：换行（LF），即’\n’
企业服务器一般都是Linux系统进行管理，所以会有替换换行符的需求
 

三、解决方法
设置方法一：

#提交时转换为LF，检出时转换为CRLF
git config --global core.autocrlf true
1
2
*适用于Windows系统，且一般为Windows默认设置，会在提交时对换行符进行CRLF - LF的转换，检出时又会进行LF - CRLF的转换。
 

设置方法二：

#提交时转换为LF，检出时不转换
git config --global core.autocrlf input
1
2
*适用于Linux系统，所有换行符都会进行CRLF - LF转换，但操作时不会转换回CRLF。
 

设置方法三：

#提交检出均不转换
git config --global core.autocrlf false
1
2
*适用于Windows系统，且只在Windows上开发的情况。在提交、检出时不会对CRLF/LF换行符进行转换
 

文件提交时进行safecrlf检查：

#拒绝提交包含混合换行符的文件
git config --global core.safecrlf true

#允许提交包含混合换行符的文件
git config --global core.safecrlf false

#提交包含混合换行符的文件时给出警告
git config --global core.safecrlf warn


四、问题思考
跨平台文件都有兼容性的问题，为什么只有core.autocrlf参数设置true检出时，会有LF-CRLF的转换？
有看到跨平台文件的问题：
· Linux文件在Windows下会显示成一行。
· Windows文件在Linux下结尾可能多出^M符号
 
那么就有以下可能性：
① Windows因为可视化界面较好，操作简易，且文件格式对日常操作没有较大影响，所以不做该功能。
② Git的pull等功能将文件拉取到本地时，都会基于检出配置进行操作，所以只要把core.autocrlf设置成true就好了。
原文链接：https://blog.csdn.net/Babylonxun/article/details/126598477

#### bug5:fatal: unable to access 'http://github.com/******': Fai
led to connect to github.com port 443 after 21051 ms: Couldn't connect to server
这是由于本机系统代理端口和git端口不一致导致的。

解决办法：
一、查看自己本机系统代理：

设置---网络和Internet---代理---地址:端口

 二、修改git配置：（其中的10809改为你电脑的端口号）

git config --global http.proxy http://127.0.0.1:10809
git config --global https.proxy http://127.0.0.1:10809
三、再次push就可以成功上传。


原文链接：https://blog.csdn.net/m0_64007201/article/details/129628363

Tip：
作者：元帅
链接：https://www.zhihu.com/question/492329093/answer/2171975894
来源：知乎
著作权归作者所有。商业转载请联系作者获得授权，非商业转载请注明出处。

一、查看本机IP
Win+R调出cmd，输入：ipconfig，即可查看本机IP。
或者是在某度直接输入：本机IP，也可获取到本机IP。以上2中方法均可以获取到本机IP信息。

二、查看本机监听的端口
Win+R调出cmd，输入：netstat -na，即可查看本机与外部某些地址的连接状态，可以分别看到目前所处的状态是那个阶段，以及开放的一些端口，哪些端口可供使用，哪些端口未开放。

#### Bug6:fatal: refusing to merge unrelated histories
把git在最新2.9.2，合并pull两个不同的项目，出现的问题如何去解决

如果合并了两个不同的开始提交的仓库，在新的 git 会发现这两个仓库可能不是同一个，为了防止开发者上传错误，于是就给下面的提示

如我在Github新建一个仓库，写了License，然后把本地一个写了很久仓库上传。这时会发现 github 的仓库和本地的没有一个共同的 commit 所以 git 不让提交，认为是写错了 origin ，如果开发者确定是这个 origin 就可以使用 --allow-unrelated-histories 告诉 git 自己确定

遇到无法提交的问题，一般先pull 也就是使用 git pull origin master 这里的 origin 就是仓库，而 master 就是需要上传的分支，因为两个仓库不同，发现 git 输出 refusing to merge unrelated histories 无法 pull 内容

因为他们是两个不同的项目，要把两个不同的项目合并，git需要添加一句代码，在 git pull 之后，这句代码是在git 2.9.2版本发生的，最新的版本需要添加 --allow-unrelated-histories 告诉 git 允许不相关历史合并

假如我们的源是origin，分支是master，那么我们需要这样写git pull origin master --allow-unrelated-histories 如果有设置了默认上传分支就可以用下面代码

git pull --allow-unrelated-histories
1


这个方法只解决因为两个仓库有不同的开始点，也就是两个仓库没有共同的 commit 出现的无法提交。如果使用本文的方法还无法提交，需要看一下是不是发生了冲突，解决冲突再提交

原文链接：https://blog.csdn.net/lindexi_gd/article/details/52554159
…or create a new repository on the command line
echo "# learning-github" >> README.mdgit initgit add README.mdgit commit -m "first commit"git branch -M maingit remote add origin https://github.com/hh404358/learning-github.gitgit push -u origin main
…or push an existing repository from the command line
git remote add origin https://github.com/hh404358/learning-github.gitgit branch -M maingit push -u origin main
…or import code from another repository
You can initialize this repository with code from a Subversion, Mercurial, or TFS project.

#### bug7:git remote add origin fatal: remote origin already exists.（报错远程起源已经存在。）

上网查了下，有很多小白遇到过这个问题，以下是网上摘取的解决办法，
解决办法如下：

1、先输入 git remote rm origin
2、再输入 git remote add origin**************
这样就不会报错了！

第二个问题
git remote add origin******
The authenticity of host 'github.com ' can't be established（无法建立主机“github.com”的真实性）
这是由于你的git地址采用了ssh方式，切换为https方式即可，也可能是你的仓库地址不对，可以用命令先查看一下：

 git remote -v
如果跟你的github地址不一样，那就去你的github上复制一下仓库地址
然后在终端中输入：

git remote set-url origin https://github.com/yourname/learngit.git (这个是你的复制的仓库地址)
最后再push下就可以了！

git push origin master
原文链接：https://blog.csdn.net/u014712086/article/details/107005216

#### merge&&rebase
不同点
比如现在在某个子分支执行git rebase(merge) master操作。
merge:将在子分支的所有提交记录成一次commit，保留在记录中。（下图的E即为该记录）
rebase:不会保留commit记录，直接将分支中的内容排到master的记录之后。如图：合并前版本如下：

**简单总结就是：merge就是将自己版本合并到master分支下。rebase就是覆盖的操作。**

使用场景：
场景一
一直在某个子分支开发功能。你还没开发完，然后你的同事告诉你，你引用的其他模块的内容有更新，你需要拉取一下最新的master代码更新一下该的模块。
推荐使用rebase。该模块的内容更新和你功能无关，合并代码也不会影响你的功能，无需保留该记录。如果使用merge，在开发周期长的情况下，会创造很多无用的commit记录。
场景二
你和同事两个人在开发同一个模块，你同事开发的部分功能已经合并到了master，通知你更新一下最新的模块代码。
推荐使用merge。你和同事开发的是同一模块，很可能他的某个改动会导致你的功能出问题，如果出了问题，保留记录能便于后期排查问题。（如果从逻辑上可以判断不会有影响，那么使用rebase也可以）
场景三
现在有个子分支模块已经开发完，master分支需要将这个分支的内容合并进来。
推荐使用merge。使用merge可以留有提交记录，如果该模块出了问题，方便后面排查问题。
注意点：
假如使用rebase，一定要遵守rebase黄金法则，共享的public分支不能rebase，
通俗的说，当一个分支是一个人开发处理的，才可以rebase，假如一个分支被多个人共享开发，然后rebase，那就乱套了，处理起来复杂；

来自 <https://zhuanlan.zhihu.com/p/558666061> 

## markdown语法
# markdown learning

## 1.标题

### #法：

#+空格+文字     一级标题

##+空格+文字   二级标题

以此类推

### 快捷键法：

ctrl+0          文本

ctrl+1           一级标题

ctrl+2           二级标题

以此类推

## 2.字体

粗体：** + 文字 + **

斜体：* +文字+ *

粗体加斜体：3个*  + 文字 +      3个*

（打了三个*，会变成斜体而隐藏另外两个，而上面两种可以通过加空格拜托隐藏但是3个星不可以）

叉线：~~ +文字 ~~

## 3.引用

> xxx said

大于号+文字

## 4.分割线

三个减号

---

或者3个*

***



## 5.代码段

3个~

~~~c++
class person
{
    public：
}
~~~



## 6.图片

！[图片名字] (图片路径)

~~~
注意：所有符号均为英文的符号
~~~



![R-C (1)](C:\Users\27707\Pictures\Camera Roll\R-C (1).jpg)

## 7.超链接

[点击调转至bilili](https://www.bilibili.com)

[文字]（网址）

## 8.列表

1. a

2. b
3. c
4. d

数字+.英文点+空格+文本

- 

+ 

加号/减号 +空格

## 9.表格

### 鼠标右键-》插入-》表格

|      |      |      |
| ---- | ---- | ---- |
|      |      |      |
|      |      |      |
|      |      |      |

### 快捷键 ：CTRL+T

|      |      |      |
| ---- | ---- | ---- |
|      |      |      |
|      |      |      |
|      |      |      |

### 标准法

| 张三 | 男   | 20   |
| ---- | ---- | ---- |
|      |      |      |

|（直线）+文字+直线+文字+直线

（介于加空格无法阻止他变成表格而隐藏本意，记为直线）

#  








































![image](https://github.com/hh404358/Java/assets/122706641/68c27132-c9da-4f5b-b001-a368a2f20237)
