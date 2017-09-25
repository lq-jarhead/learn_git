##   Git教程 ##
本文为本人阅读廖雪峰教程的总结和笔记，教程链接[https://www.liaoxuefeng.com/wiki/0013739516305929606dd18361248578c67b8067c8c017b000/001374027586935cf69c53637d8458c9aec27dd546a6cd6000](https://www.liaoxuefeng.com/wiki/0013739516305929606dd18361248578c67b8067c8c017b000/001374027586935cf69c53637d8458c9aec27dd546a6cd6000 "Git教程-廖雪峰")
### 1分布式vs集中式 ###
Git是分布式的版本管理系统，和其他集中式的版本管理系统相比，如SVN，最大的优点是本地管理、多人同时开工，
**Git与SVN的主要差别**：

1. 这两个工具主要的区别在于历史版本维护的位置，Git本地仓库包含代码库还有历史库，在本地的环境开发就可以记录历史，而SVN的历史库存在于中央仓库，每次对比与提交代码都必须连接到中央仓库才能进行。
1. Git中，自己可以在脱机环境查看开发的版本历史
1. 多人开发时如果充当中央仓库的Git仓库挂了，任何一个开发者的仓库都可以作为中央仓库进行服务，不过开发者仓库一般不直接充当中央库，但你可以随时创建一个新的中央库然后同步就立刻恢复了中央库

### 2基本操作 ###
1. 在Ubuntu下安装：`sudo apt-get install git`
2. **创建仓库**：在某个文件夹里面右击打开git bash here
	1. mkdir learngit
	2. cd learngit
	3. git init # 初始化
	4. git config --global user.email "xx"
	5. git config --global user.name "yy"#注意yy和name之间的空格
4. **提交文件**到仓库及修改、查看更改：#在.git同个目录下，新建readme.txt
	5. git add readme.txt#添加文件到仓库
	6. git commit -m "this is the first version"#提交文件到仓库，可以多个一起提交，message用来保存提交的描述信息
	7. #手动改动readme.txt中的内容
	8. git status #查看仓库的当前状态，提交修改和新文件一样需要两步
	9. git diff   #查看最近一次的更改
	10. git add
	11. git commit -m"the second"
12. **版本回退**:
	13. git  log #查看版本操作记录情况 git log --pretty=online
	14. git reset --hard Head^^ #HEAD是当前版本，^^往前两个版本HEAD~4,回退4个
	15. git reflog #查看所有的版本操作
	16. git reset --hard xxx  #回退到未来版本，xxx为版本号，前面几个即可，系统自动搜索



操作解释说明：


- git status #用来查询当前版本位置，缓存区有无文件需要commit
- git diff #比较工作区和缓存区的区别，若commit了，则无返回。**但是**，改变了readme.txt文件保存后，不用add，用git diff即可返回出哪里更改了
- git add readme.txt，git commit -m"xxx"两个命令常一起使用。git add xxx可以使用多次，就是将多个文件添加到缓存区，git commit就是将缓存区内容全部提交到repository版本仓库。

----------

### 3 工作区和版本库、缓存区 ###
- 工作区：working directory工作目录，就是我们放置文件的目录，在这里就是readme.txt所在的目录，即是我们的工作目录。
- 版本库：repository，版本仓库，就是.git文件夹，用来储存各个版本信息
- 缓存区：版本库中有一个stage文件夹，就是我们的缓存区

用实际操作说明他们的关系：在**工作区**我们编辑readme.txt文件并保存，通过`git add readme.txt`添加到**缓存区**，再通过`git commit -m"xxx"`将缓存区所有东西提交到**版本库**，就成了一个明确的版本了。

### 4 管理、撤销修改和删除文件等 ###
- git rm readme.txt#删除工作区的文件
- git checkout --readme.txt #"在commit之前，该命令为撤销删除"，我这里没有实现，总是报错。checkout 就用来回退版本了


### 5 本地仓库和远程仓库 ###
本地Git仓库和GitHub仓库之间的传输是通过SSH加密的，所以，需要一点设置：

**创建本地仓库：**

- 创建SSH key：`$ ssh-keygen -t rsa -C"1622571327@qq.com"` (在Ubuntu中，打开shell)。可以在用户主目录里找到.ssh目录，里面有id\_rsa和id\_rsa.pub两个文件，这两个就是SSH Key的秘钥对，id_rsa是私钥，不能泄露出去，id\_rsa.pub是公钥，可以放心地告诉任何人.
- 登录GitHub，create ssh key 并在其中填入我们的公共钥匙

**创建远程仓库：**

- 登录GitHub，create a new repository ,创建一个新的仓库，命名最好与本地一致
- 按照提示，在本地仓库命令栏中输入`git remote add origin git@github.com:lq-jarhead/learngit.git`完成添加。

**克隆仓库：**

- 在某个目录处，打开git shell,此处就是根目录
- `git clone https://github.com/lq-jarhead/learngit` 就是将仓库复制到此处

### 6 分支管理： ###
涉及到分支的创建、合并和删除，还有解决冲突。比如，我们在同一个readme.txt文件编辑了，最后两个分支和master合并，就会产生冲突，需要人工解决了。不用纠结太多，分支管理更多用在不同模块更改吧。

- `git checkout -b xxx`#创建并切换到xxx分支 `git checkout xxx`#就是切换到xxx分支
- #在Dev分支下，对文件readme.txt进行更改等
- `git add readme.txt`
- `git commit -m"lengqian"`
- `git checkout master`#切换到master
- `git merge --no-ff -m"merge" xxx`#将dev分支和当面master合并,no fastforward和-m"共存"，为了后期查看具体merge信息。
- `git branch -d xxx`#删除xxx分支 git branch -D xxx#确认删除（强行删除）
- git branch#查看当前目前所有分支，星号标记的为当前分支
- git log #就可以查看合并记录了 

注意：目前是在master分支，我们创建一个分支test，在其中更改并保存readme.txt，再提交给版本库。返回到master分支，在其中更改并保存readme.txt，再提交给版本库。这时候，二者就有冲突的地方了，我们用**合并**命令就知道冲突在那里，手动消除冲突后。直接提交给版本库即可。（这里不是**再次合并**，应为第一次已经合并了，尽管有冲突，我们手动消除后就没有了冲突，直接提交即可！）

### 7 Bug分支管理 ###
1.  目前，lengqian正在leng分区上修改
1.  老大说master有一个bug,需要马上更改
2. `git stash`#将leng分区临时保存起来
3. `git checkout master`#切换到master分区
4. `git checkout -b bug` #创建bug分区，用来修改
5. 修改好了，直接将bug合并到master中
6. `git checkout leng`#切换到leng分区
7. `git stash list`#可以看到我们的保存的文件
8. `git stash pop`#切换为最近一次临时保存的界面，并删除stash
9. #用stash list可以查看stash列表，`git stash apply stash@{1}`#用来恢复那个版本，

### 8 多人协作 ###
1. 大家公用一个远程仓库，开发之前`git remote -v` 查看远程仓库的信息。
2. `git clone ...` 拷贝至本地，用git branch可以看出只有master分区
3. `git checkout -b dev` 创建自己本地的开发分区，并用`git push origin dev` 推送至仓库，也是同伴的开发分区基础
4. 同伴`git pull origin dev`#即可将dev分区复制到本地
5. 在本地dev中修改完成后，用`git push origin dev` 将本地的dev分区push到远程仓库（这个推送之后，也就是合并到远端的dev中去了）

作者小总结：

1. 查看远程库信息，使用git remote -v，前提是拷贝下来，并且在.git目录处打开Bash；
1. 本地新建的分支如果不推送到远程，对其他人就是不可见的；
1. 从本地推送分支，使用git push origin branch-name，如果推送失败，先用git pull抓取远程的新提交；
1. 在本地创建和远程分支对应的分支，使用git checkout -b branch-name origin/branch-name，本地和远程分支的名称最好一致；
1. 建立本地分支和远程分支的关联，使用git branch --set-upstream branch-name origin/branch-name；
1. 从远程抓取分支，使用git pull，如果有冲突，要先处理冲突。

**注意：**多人协作的情况比较复杂，问题关键在于远端仓库、本人和他，这三个不同的仓库临时保存了不同的更改和文件。本人、他，这两方要分主次，有个为主要的，决定了最终master主分区的操作。9/25/2017 3:44:33 PM 
### 9 标签tag管理 ###
**创建tag标签**，标签有点类似HEAD，目的是为了用自己语言去给某个commit操作贴上标签，方便以后查看。有以下几种方法：

- 给某个分支贴上标签，`git checkout dev ` 紧接着`git tag v1.0`就可以了 `git show v1.0`就可以查看
- 给历史操作贴上标签，`git log`找到合适的commit id，`git tag v0.9 34mlj` 即可
- 标签加上说明,`git tag -a v1.2 -m"this is v1.2"` -a指标签， - m指说明
-  #`git tag`#查看所有标签，`git show vxx`查看xvv标签

 **标签操作** 