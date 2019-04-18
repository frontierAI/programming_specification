# 1如何使用git
## 1.1使用git进行版本控制

### 1.1.1 安装和初始化
在git的官网上下载对应系统的git系统并安装：https://git-scm.com/downloads

安装完成后，在开始菜单里找到“Git”->“Git Bash”，蹦出一个类似命令行窗口的东西，就说明Git安装成功了。

还需要最后一步设置，在命令行输入：
~~~
git config --global user.name "Your Name"
git config --global user.email "email@example.com"
~~~

~~~
git --help
git --version        #查看git的版本
~~~
### 1.1.2 新建一个仓库和文件
~~~
mkdir learngit        #创建一个叫做‘learngit’的仓库（文件夹）
cd learngit           #进入‘learngit’
git init              #初始化仓库
touch readme.txt      #新建名为‘readme’的txt格式文件
~~~

### 1.1.3 提交文件
~~~
git add .                           #add所有内容至暂存区
git commit –m “add readme.txt”      #提交更改记录至存储库
~~~

### 1.1.4 查看文件的变化
~~~
git status            #查看仓库当前的状态(**要随时掌握工作区的状态，可以经常使用这个命令**)
git diff readme.txt   #查看‘readme’的difference
~~~

### 1.1.5 版本穿梭
  当你不断对文件进行修改，不断commit后，你随时可以退回到某个特定的版本。
~~~
git log                      #查看从最近到最远commit的日志
git reset --hard head^       #退回到上一个版本
git reset --hard head^^      #退回到上上一个版本
git reflog                   #查看命令历史，可以看到各个版本
git reset --hard 36857456    #退回到某个版本（版本号为：36847456），版本号是log/reflog的时候看到的commit id（不用写全，前几位就可以）
~~~

### 1.1.6 撤销修改
~~~
git checkout -- readme.txt   #若只是修改了文件，还未add，可以用git checkout -- file进行撤销
git reset head readme.txt    #若add了文件，还未commit，可以用git reset head file进行撤销
git reset --hard head^       #若commit了文件，还未push到远程仓库，可以用1.1.5中的版本穿梭进行撤销
~~~

## 1.2使用git进行分支管理
Git鼓励我们使用分支去完成某个任务，我们可以在原来的分支上创建一个属于你自己的分支，别人看不到，还继续在原来的分支上正常工作，而你在自己的分支上干活，想提交就提交，直到开发完毕后，再一次性合并到原来的分支上，这样，既安全，又不会影响别人工作。
在git里master是主分支，dev分支一般用于开发。

### 1.2.1 创建分支
~~~
git branch               #查看分支，当前分支会有一个*在前面
git branch dev           #在当前分支上建立一个dev分支
git checkout dev         #切换至dev分支
git checkout -b dev      #这是上面两个命令的合并，即：在当前分支上创建名为dev的分支并切换至dev分支。
git branch –m dev dev2   #将dev分支名称修改为dev2
~~~

### 1.2.2 合并分支
~~~
git merge dev2            #将dev2分支合并到当前分支，注意切换分支
git branch -d dev2        #将dev2分支删除
~~~

### 1.2.3 解决冲突
当两条分支对同一个文件的同一个文本块进行了不同的修改，并试图合并时，Git不能自动合并的，称之为冲突(conflict)。解决冲突需要人工处理。


# 2 github collaboration workflow
## 2.1 简介
Github是一个开源社区和企业项目管理平台。

GitHub官方出了一个视频，展示了，从一个需求的诞生，到真正的落地实现，大家是怎么通过 GitHub 来完成这一切的。
[点击观看视频](https://www.zhihu.com/question/28976652/answer/583179243)

首先，[点击注册github账号](https://github.com/)。

接下来我们进行github协作工作流的介绍。

## 2.2 教程
step 1：manager创建项目，选择“private”。

![输入图片说明](https://note.youdao.com/yws/api/personal/file/EE903E6E64864C68A1DAB8CC63D11E33?method=download&shareKey=a88a3f76ad26b0dc3d408ea65c5ddfa3 "创建项目.png")
在项目首页进入‘setting’tab，在左边选项栏里选择项目成员管理-> 开发者，然后点击‘添加项目成员’->‘搜索添加’。
![输入图片说明](https://note.youdao.com/yws/api/personal/file/9588F861E3A146ABAAB0269B3B6A9F3C?method=download&shareKey=5c4510be86e8571f68fe3f8f2ec68370 "添加项目成员.png")


step2 : member在云端fork manager的project。然后clone自己的项目到本地，**并建立origin和upstream远程仓库**
~~~
(1) fork
(2)
git clone  https://git.oschina.net/dancemoment/project.git                 #clone自己fork的项目到本地
(3)
git remote -v                                                              #查看远程仓库及详细信息（此时只有origin）
git remote add upstream https://git.oschina.net/yoyo182487329/project.git  #添加upstream远程仓库
git remote -v                                                              #查看远程仓库及详细信息（此时有origin和upstream）
~~~

step3 : 若manager在master有更新， **member则要先把upstream的更新pull到本地**，然后再进行工作。
~~~
if manager do:
vim manager_do1.txt                             #创建manager_do1.txt文件
git add .                                       #add所有修改内容至暂存区
git commit -m 'add manager_do1.txt'             #提交更改记录至存储库
git push -u origin master:master                #将本地的master分支push到远程分支

member do:
git checkout dev                            	#进入dev分支
git pull upstream master                        #将upstream远程仓库的master分支与当前分支合并
~~~
注意：
* **pull之前一定要先commit，否则可能会出现没有commit的数据被直接覆盖**
* pull的时候可能会弹出Please enter a commit message to explain why this merge is necessary.

    如果要输入解释的话就需要:  
    1.按键盘字母 i 进入insert模式  
    2.修改最上面那行黄色合并信息,可以不修改

    如果不想输入，则可以不管：  
    1.按键盘左上角"Esc"  
    2.输入":wq",注意是冒号+wq,按回车键即可
    
step4 : member在进行每个项目开发之前，要在master的分之下建立新的feature分支，在feature分支上面进行开发，完毕后合并至master分支， 并推送到个人的远程origin仓库的master分支
~~~
git checkout -b feature_1                 	#建立feature_1分支切换至feature_1分支
vim feature_allen.txt                       #创建member的文件
git add .                                   #add所有修改内容至暂存区
git commit -m 'add feature_allen.txt'       #提交更改记录至存储库
git checkout master           			    #切换至master分支
git merge feature_1            			    #将feature_1分支与当前（master）分支合并
git push origin master:master	     		#将member自己的本地分支push到对应的远程分支
~~~

step5 : member在github上个人的master分支，向manager的project仓库的master分支上面提交pull request。

step6：manager看到pull request后进行code review，在云端逐行评论代码，并提出修改意见

step7：member根据修改意见在本地进行代码修改，修改提交后推送到个人云端，并重复step5的动作直到manager merge你的pull request。

step8：manager更新自己本地的仓库
~~~
git fetch origin                               #从远程复制origin仓库所有分支到本地
git branch -a                                  #显示本地和远程的分支
Git checkout –b release-v0.1 origin/master     #以origin仓库的dev分支为起点，创建本地release-v0.1的分支
vim release.txt                                #创建release.txt文件
git add .                                      #add所有修改内容至暂存区
git commit –m “add a release.txt”              #提交更改记录至存储库
git push origin release-v0.1:release-v0.1      #将本地分支release-v0.1 push到对应的远程分支
~~~

step9: member find a bug in release-v0.1, member do:

~~~
git fetch upstream                              #从远程复制upstream仓库所有分支到本地
git checkout –b fixbug1 upstream/release-v0.1   #以upstream仓库的release-v0.1分支为起点创建fixbug1的分支
touch fixsomething.txt                          #新建一个fixsomething的txt文件
git add .                                       #add所有修改内容至暂存区
git commit –m “fix the bug1”                    #提交更改记录至存储库
git push origin fixbug1                         #将本地分支fixbug1 push到origin远程仓库的fixbug1分支
~~~
step10：pull request: merge the fixbug1 and the upstream/release-v0.1

step11：manager response the pull request

step12：prepare to ultimate release version(manager do)
~~~
git checkout master                    #切换至master分支
git pull origin release-v0.1           #取回origin仓库的release-v0.1分支并与当前分支合并
~~~

step13：
~~~
Git tag –a v0.1 –m “this version has two function”   # 给版本打标签—标签名“v0.1”，说明“this version has two function”
git push origin master:master                        #将本地分支master push到origin远程仓库的master分支
~~~
![输入图片说明](https://git.oschina.net/uploads/images/2017/0904/163035_312dc821_1501244.png "屏幕截图.png")

上图是git分支的工作流。我们都在自己的分支上干活，可以随时提交，随时合并。

# 3 git技巧
## 3.1如何使git支持中文
step1.打开git Bash后，对窗口右键->Options->Text->Locale改为zh_CN，Character set改为UTF-8
step2.关闭git bash，再打开，此时，输出中文字符，中文路径都没有问题啦

## 3.2使用git对word进行版本控制
由于git只能对纯文本文件进行版本控制，而word的.doc或者.docx就不是一个纯文本文件，所以需要第三方转化工具，将其转化为纯文本。
### 3.2.1 使用pandoc（选装插件）
第三方工具有很多，其中一个是pandoc，具体方法如下：

step1. Install pandoc.   
去http://pandoc.org/installing.html 找到合适的pandoc下载文件，然后下载安装。

step2. 在windows系统下，编辑 git 安装目录下的 /mingw64/etc/gitconfig 文件，加上这么一段话：
~~~
[diff "pandoc"]
  textconv=pandoc --to=markdown
  prompt = false
[alias]
  wdiff = diff --word-diff=color --unified=1
~~~
step3.在你的工程目录下新建一个 .gitattributes文件（windows），然后写入：
~~~
*.docx diff=pandoc
~~~
当然上面的是docx文件，如果是doc文件，把docx换成doc应该也是一样的。

step4.在工程目录下初始化git
~~~
git init
git add .       #把所有的文件都添加进去（包括.gitattributes文件）
~~~
其他的 git commit -m     git remote add origin  git push origin master  等都是一样的。

step5.1 现在如果想要看本次修改之后与上次commit之间的差别，可以使用命令（file.docx是你的word文件名）：
~~~
git wdiff file.docx        #这个命令会将本次修改的与上次不同的地方用彩色标识出来。
~~~
step5.1 如果想查看 历次的改变（all changes），可以使用命令：
~~~
git log -p --word-diff=color file.docx
~~~
### 3.2.2 使用strings（无需安装，较为简单）
step1.在你的工程目录下新建一个 .gitattributes文件（windows），然后写入：
~~~
*.doc diff=word
~~~
step2.使用strings 程序，把Word文档转换成可读的文本文件
~~~
$ git config diff.word.textconv catdoc
~~~
step3.比较两个版本差别
~~~
git diff
~~~


参考资料：

1.廖雪峰git教程：http://www.yiibai.com/git/git_fetch.html

2.git易百教程（常用命令）：http://www.yiibai.com/git/git_fetch.html

3.git常用命令总结：http://www.cnblogs.com/mengdd/p/4153773.html

4.git对Word版本控制（pandoc）：http://www.cnblogs.com/yezuhui/p/6853271.html

5.git对word版本控制（strings）https://git-scm.com/book/zh/v1/自定义-Git-Git属性

6.git支持中文：http://blog.csdn.net/xieshanchaohai/article/details/54344570

7.解决冲突：http://www.cnblogs.com/mengdd/p/3585038.html

