# [为你自己学git 高见龙](https://gitbook.tw/chapters/introduction/what-is-git)
# [(99+ 封私信) GIT从入门到精通 - 知乎](https://zhuanlan.zhihu.com/p/653156299) 
# [史上最全 Git 图文教程（非常详细）零基础入门到精通，收藏这一篇就够了-CSDN博客](https://blog.csdn.net/Javachichi/article/details/140660754) 
# [Git从入门到精通（全）-阿里云开发者社区](https://developer.aliyun.com/article/1104558) 
# [【Git保姆级使用教程】Git从入门到精通超级详细的使用教程，一套教程带你搞定Git（高见龙版本）。_git从入门到精通pdf-CSDN博客](https://blog.csdn.net/syu_acm/article/details/141530578) 
# [git官方操作指南](https://git-scm.com/book/zh/v2) 


## 第1节
0. git终端gitbash如何显示中文不为乱码，如果加入windowsteminal,参考[Windows Terminal 集成 Git Bash_windows bash terminal-CSDN博客](https://blog.csdn.net/Magic_Ninja/article/details/122671663) 
	- git bash配置文件修改`vim /etc/bash.bashrc`

1. 每次对文件的修改都会生成一个git版本，形成一个变化内容的快照，可以用git非常快速自如回到任何过去的版本，而不会增加太多硬盘空间![01_git并不是记录版本的差异，而是记录档案内容的快照.png](/private/study/IT/git/01_git并不是记录版本的差异，而是记录档案内容的快照.png) 
2. git是分散式系统，不需要时时与服务器同步连线，事实上一般都是离线在本机上实现git操作，等有网络的时候再与服务器同步。
3. 虽然git无法对psd等二进制文件精确知道什么人在什么时候修改了哪些字，但整体上当不小心psd文件被覆盖或删除的时候，可以救回旧的版本
4. linux建立新文件的命令`touch`，例如`touch about.html`,但如果`about.html`文件早已存在，执行命令不会变更该文件内容，只会改变该文件的最后修改时间，在windows中是没有的

5. 第一次运行本地运行git时，要预设使用者的名称及邮箱名称，类似下面,或直接修改`~/.gitconfig`文件亦可
```
$ git config --global user.name "ciscio"
$ git config --global user.email "yeguirenf@gmail.com"
$ git config --list 指令检视配置结果
```

6. 可以用每个专案设定不同的作者![02_git每个专案设定不同的作者.png](/private/study/IT/git/02_git每个专案设定不同的作者.png) 

7. git终端下默认编辑器是vim，也可以配置成其他编辑器，如：
```
$git config --global core.editor emacs
```
8. git有些命令太长且繁琐，可配置成缩写形式，如
```
$git config --global alias.co checkout
$git config --global alias.br branch
$git config --global alias.st status
$git config --global alias.l "log --oneline --graph"
$git config --global alias.ls 'log --graph --pretty=format:"%h <%an> %ar %s"'
```

9. 一些临时的练习目录可以放在`/tmp`目录下，MacOs等os会在重启的时候自动清空`/tmp`该目录下所有内容，不需要手动整理
10. 在当前目录下执行`git init`是对当前目录进行git初始化，主要目的是要让git对这个目录进行版本控制,执行初始化，该目录下会出现一个`.git`隐藏目录，该目录下有一系列生成的文件,`git init`不仅适用新建目录，对已有的目录同样适用用来版控，如果想取消版控，将该目录下的`.git`目录直接删除即可,** 也就是说,整个目录时，什么档案或目录删除了都救得回来，除非`.git`目录也删除了，就没办法挽救了
11. `git init`后再执行`git status`发现什么变化，意思就是现在没东西可以commit提交,尝试执行`$echo "hello,git" > welcome.html`再执行`git status`，提示`welcome.html`文件只是加入了当前目录，但处于未加入git版控状态`Untracked`
12. `git add welcome.html`后再执行`git status`发现该文件从`untracked`状态变成`new file`状态了,表示该文件已经被安置在暂存区（Staging Area）,暂存区也称为索引（index）,等候稍后跟其它档案一起被存到储存库里
13. 可用通配符`git add **.html`将后缀html的所有文件加入到暂存区，如果将该目录下所有文件一次加入到暂存区，执行`git add --all`或`git add .`(在git2.x后两者同义,但`git add .`仅将当前目录及所有子目录文件加入到暂存区，不包括上级目录，而`git add --all`包括所有子目录及上溯到顶级项目工程根目录)
14. 新建abc.txt文件后，执行`git add abc.txt`添加到暂存区，再接着编辑abc.txt文件并保存，再执行`git status`发现abc.txt文件变成2个，一个是`new file: abc.txt`，另一个是`modified: abc.txt`状态，意味着修改后的文件并不会自动加入到暂存区，必须再执行一次`git add abc.txt`才会将变动加至暂存区
15. 要将通过`git add`加入到暂存区的文件永久保存至仓库，即存档执行`git commit -m "init commit"`，`- m "init commit"`意思是注释，就是简单说明一下此次commit做了什么事，方便以后自己能明白,这个非常重要，便于以后检索
16. `git commit`仅提交在暂存区的文件，如果新增一个文件，但没有`git add`加入暂存区，则`git commit`时被直接忽视
17. 暂存区为空，也可以commit，加上参数,`git commit --allow-empty -m "空的"`
---
## 第2节

1. 一般是要先add，再commit,也有例外，多加一个参数`-a`,如
```
$git commit -a -m "update content"
```
但`-a`只针对已经存在于仓库的文件有效，对还是新加入的`untracked file`的文件是无效的


2. `$ git log --oneline --author="Sherly\|Eddie"`  查询这两人的commit纪录

3. 使用`--grep`参数，查询相关commit注释符合字样的内容
```
$ git log --oneline --grep="WTF"
```
4. 使用`-S`参数，可以查找在所有的commit文件内容中指定的字符串
```
$ git log -S "Ruby"
```
5. 找出今天上午9点到12点之间所有的commit,还加上一个after,就可以查到2017年1月后每天早上9点到12点的commit
```
$ git log --oneline --since="9am" --until="12am" --after="2017-01"
```
6. 通过rm删除文件后仍需add后再commit，也可以将这两步合并为一步
```
$ rm welcome.html              # 刪除檔案 welcome.html
$ git add welcome.html
```
上两步合并为下面的一步
```
$ git rm welcome.html
```
7. `-cached`参数，`rm`或`git rm`都会删除文件，如果只是取消该文件的git版控状态，而保留该文件，让该文件成为最先刚加入目录未add加入暂存区的`Untracked`的状态，达到未版控的状态
```
$ git rm welcome.html --cached
```

8. 改文件名对git来说会被认为是两个动作，一个是删除旧文件，状态为deleted,一个是新增文件,状态为Untracked,`git add --all`后，文件状态为`renamed`
```
$mv index.html world.html
$git add --all
```
以上2步可合并为1步
```
$git mv index.html world.html
```

9. git不在乎文件的名称，在乎的是文件内容本身，根据内容算出sha-1值，改名不会生出一个新的blob物件，仍指向原来旧的Blob物件，名称改为会生成一颗新的Tree物件

10. 修改commit记录
![03_git_修改commit记录有好几种方法.png](/private/study/IT/git/03_git_修改commit记录有好几种方法.png) 

11. 用`--amend`参数，修改最后一次,也就是最新提交的commit的注释信息
```
$git commit --amend -m "Welcome To Facebook"
$git log --oneline
```

12. 虽然只是修改了commit提交的注释信息，内容并没有修改，但仍是一次新的comit，产生了新sha-1值,所以尽量不要在已经push出去之后再修改，否则可能造成其它人的困扰

13. 刚commit，但发现有一个文件忘了加到，如果不想重新commit，可先将此文件add后，执行`git commit --amend --no-edit`,实现将文件并入最后一次的commit,`--no-edit`参数的意思是不要编辑commit提交信息,另一种办法是
```
$git reset //把最后一次的commit拆掉
$git add newfile //加入新文件
$git commit -m "mark" //重新提交 
```
14. 提交时出现`Unlink of file. ’ file path and name’ failed. Should I try again? (y/n)`的原因
	- 因为其它ide程序正在使用该文件，关闭后即可
	- 也可执行`git gc`后再提交，则不会再出现此提示。已尝试，不管用。
# 第3节
1. 工作区新增空目录`images`，使用`git status`是不变有变化的，也无法提交，除非在目录下放一个名为`.keep`或`.gitkeep`的空文件，让git感应到空目录的存在。如在该空目录下执行`touch images/.keep`
2. 有些文件不想放在git里面如何实现
	- 在目录里新建一个`.gitignore`文件，此文件只对以后增加的文件有效，对之前的文件不起作用。
	- 将不想放在git的文件名称如`secret.yml`添加到`.gitignore`中即可，下次提交会忽略掉此文件中提到的文件
3. 但如果执行`git add -f secret.yml`则会忽略掉这个忽略，无视`.gitignore`文件中的设置。
4.  `.gitignore`文件，此文件只对以后增加的文件有效，对之前的文件不起作用。
5. 如果要对之前的文件起作用，必须使用`git rm --cached`移出git控管的这些文件。
6. 如果要强行清除那些已经被忽略的文件，可用`git clean -fX`命令。

# 第4节
1. `git log -p welcome.html`可以查看这个文件每次的提交做了什么修改，
2. `git blame git_note.md`或`git blame -L 5,10 git_note.md`指定行数，可以清楚看出哪一行是谁在什么时候写的。
3. 误删除重要文件，如`rm *\**.html`,可用`git checkout .`或`git checkout xx.html`救回全部或一个被删除的文件，实质是拿暂存区的文件或内容，覆盖掉已被删除的工作目录内的文件或内容，如果`git check HEAD~2 git_note.md`则会拿两个版本之前的`git_note.md`文档覆盖掉现在的工作目录里的`git_note.md`文档，且同时也同步更新暂存区的状态。
4. 刚才的commit后悔了，想撤销重做的办法是，`git reset 2558575^`,或`git reset head^`,回撤2次就直接执行`git reset head^^`,5次就执行`git reset head~5`

# reset的三种模式（reset在这里有`前往go to`或`变成 become`的意思）
1. `--mixed`预设缺省模式，会丢弃暂存区文档，但不会动工作目录文档
2. `--soft`,工作目录和暂存区文档都不会丢弃，看起来只有head的移动而已。
3. `--hard`,工作目录和暂存区文档全部丢弃。
4. `git reflog`可以查看head移动的记录，只要切换分支或是reset都会造成head移动
5. `git log -g`如果加上`-g`参数，也能看到`reflog`
6. `reflog`预设记录会保留30天。

# 查看及切换分支的方法
1. 查看`git branch`,查看本地和远程所有分支`git branch -a`
2. 切换`git checkout xxx`，如果`xxx`分支不存在，则会报错，可改为`git checkout -b xxx`,即可新建一个分支，如之前有，则不会重建。
3. 查看当前head(40个字节),`cat .git/HEAD`
4. head通常会指向目前所在的分支，不过也不一定，当head没有指向某个分支时便出出现`detached HEAD`的状态。
5. 新建分支`git branch xxx`或`git branch bird sha-1`,`bird`为新建的分支名，`sha-1`为commit名称。即在`sha-1`这个commit上开一个叫做bird的分支。用`git checkout -b bird sha-1`也有同样的效果，还会直接切换过去。
6. 分支改名`git branch -m master slave`将`master`分支改名为`slave`分支。
7. 删除分支dog，`git branch -d dog`,如果dog分支没被完全合并，git会有提示，`git branch -D dog`则可强制删除。
8. git中所有分支都能删除，包括master分支
9. git中的分支概念不是把文件先复制到另外的目录，然后改动合并，对比后放回原来的目录，实际上分支只是贴在commit上面的一张贴纸。
10. 当做了一次新的commit后，这个新的commit会指向它的前一个commit，当前的分支指的head所指的分支，会贴到刚刚新的那个commit上，同时head也会跟着前进。比如当前head指向master分支，执行`git branch cat`创建cat分支，实际上cat和master都是贴在同一个comiit的两张贴纸，再执行`git checkout cat`切换到cat分支，则head转而从指向master转至指向cat分支。
11. 接着执行一次新的commit，这个新commit依然会指向前一个commit，此时cat分支上的“贴纸”会被撕下来，转而贴到最新的commit上，当然head也跟着同步贴上去重新指向cat
12. 切换到不同的分支，会显示不同的暂存区和工作目录下的文件。
13. 切换分支时主要做了以下两件事
	1. 更新暂存区和工作目录,会用该分支指向的commit的内容来`更新`暂存区和工作目录。
	2. 变更`HEAD`的位置
# 只提交文档内容的一部分的办法
1. `git add -p index.html`的`-p`的含义会给出是否把文档内容添加到暂存区，选`y`则添加整个内容，选`e`只添加部分内容。

# 分支如何合并？
1. `git merge xxx`当前分支将xxx分支合并进来，`git merge xxx --no-ff`意思是不要使用快转模式合并，这样额外多出一个commit物件。
2. 合并分支其实是合并`分支指向的commit`，分支只是一张贴纸，它是无法合并的。

# .git目录解读
1. 如`index.html`文件加入暂存区后，会在.git目录下生成一个Blob（Binary large object）对象。这个blob对象只是存放`index.html`文件的“内容”，但`.git`目录下并不会存在`index.html`。
2. `echo "hello, 5xRuby" | git hash-object --stdin`可计算出blob对象的sha-1值。共40个字，前2个字生成目录，存放在`.git/objects/xx`,剩下38字，形成文件存放在此目录下。可通过`git cat-file -t 40个sha-1值`，返回对象的形态为`blob`,再通过`git cat-file -p 40个sha-1值`,返回此文件内容为`hello,5xRuby`
3. git只对文件处理，包括没有任何内容的空文件，但不对空目录处理，空目录无法加入到git中
4. 文件在git会以Blob对象的形式存放
5. 目录及文件的名称会以Tree对象的形式存放
6. Tree对象的内容会指向某个或某些blob对象，或者其他的Tree对象。对看起来像目录也子目录关系，其实不是，有个专有名称为DAG(directed acyclic graph)有向无环图，这些对象之韵只有指来指去的关系，没有阶层关系。
7. commit对象会指向某个tree对象
8. tree对象的内容会指向某个或某些blob对象或其它的tree对象,这种结构有点像葡萄，只要伸手把源头的commit对象拎起来，整串内容都可以被拿出来。
9. 除了第一个commit对象外，所有commit对象都会指向其前一次的commit对象。
10. 使用`git count-objects`可计算对象数。
11. 使用`git tag -a big_treasure -m "xxxx"`可增加tag对象。
12. tag对象会指向某个commit对象
13. 分支虽然不属于4种对象之一，但它会指向某个commit对象。head也不属于4种对象之一，它会指向某个分支。
14. 往git服务器上推送后,在`.git/refs`下会多出一个remote目录，里面放的是远端的分支，与本地分类相似，也会指向某个commit对象。
15. `git ls-files -s`命令可查看当前文件在git中的样子
16. `git gc`可启动`资源回收机制`，把原来在`.git/objects`目录下的对象全部打包到`.git/objects/pack`目录下，通过`find .git/objects -type f`查看现在的目录状态
17. 也可通过较底层的命令`git verify-pack -v .git/objects/pack/pack-8ce5a808268947547de475033149350d54887f32.idx`,分3栏，第1栏是对象的sha-1值，第2栏是对象的形态，第3栏则是文件大小。

 # 远程访问
1. `HTTPS`方式push远程仓库时需要验证用户名和密码， `SSH`不需要验证用户名和密码，但需要在github上添加公钥，github在2021年8月13日后已经停止用https方式推送至远程仓库。
2. 如何生成`SSH`公钥和私钥，实现本地与远程仓库的安全连接。
	- 进入`~/.ssh`目录下，第一次执行`ssh-keygen -t rsa -b 4096`后直接回车，生成一对`id_rsa`名称的密钥文件，如果之前已经生成过，不能直接回车，否则会覆盖之前的密钥，此时可输入一个新的任意的密钥文件名称，如`test`,再回车,则在该目录下生成`test`私钥文件和`test.pub`公钥文件。
	- 复制`test.pub`公钥文件全部内容，粘贴到`github→settings→SSH and GPG keys→new SSH key`,这种方式是适用于github账号下所有仓库，也可在某个具体仓库下的页面选择`settings`设置公钥，仅适用于该仓库，对其他仓库不适用。
	- 修改当前目录`~/.ssh`下的`conifg`文件，添加5行至该文件尾部
---


```
# github
Host github.com
HostName github.com
PreferredAuthentications publickey
IdentityFile ~/.ssh/test
```
---



	-  完成上述后，此时可执行`git clone git@github.com:flowerhood/remote_git.git`

3. 几种同步方式
	1. （先有远程仓库）可以先在github建立一个空仓库，然后clone至本地，在本地添加和编辑文件后，再使用`git push`不用任何参数直接推送至github端。再在github上增加一个readme文件，在本地上执行`git pull`拉取至本地。
	2. （先有本地仓库），还是要预先在github建立一个新的空仓库如`remote_git`，切换到本地仓库main分支，执行`git remote add origin git@github.com:flowerhood/remote_git.git`,此处的`origin`指的后面的`git@github.com:flowerhood/remote_git.git`服务器的位置，可以换成任意的名字，预设的github远端节点也会叫`origin`,再通过`git remote -v`查看当前本地仓库对应的远程仓库的别名和地址，再执行`git branch -M main`将本地master分支更名为`main`分支，以便和github默认的main分支名称保持一致。再执行`git push -u origin master:main`，其中u是	`upstream上游`的缩写，意思将本地仓库和别名为origin的上游远程仓库关联起来。`msaster:main`表示本地仓库的master内容推送到远程仓库的main分支，如本地master已更名为main分支，则改为`git push -u origin main:main`或进一步简写为`git push -u origin main`,简写的意思即使远程server不存在main分支，就会建立一个main的同名分支。如果存在同名分支，便会移动sever上main分支的位置，使它指到目前最新进度上。比如`git push dragonball cat`,意思就是把cat分支推上dragonball这个远端节点所代表的位置，并在上面建立一个名为cat同名分支（或更新进度），也可给远程分支另取与本地分支不同名的分支，如`git push origin main:cat`,orgin节点下面的cat分支，无则新建，有则更新。
	3. 上一步容易出现[解决Git上传文件出错：[rejected] master -> master (fetch first)错误](https://cloud.baidu.com/article/3318156) push时如果远端比较新，则需要先拉取`git pull --rebase`,再`git push`,也可加参数-f强推，不过有可能覆盖别人push的内容，`git push -f`慎用。
	4. 远程仓库添加新文件后，可以通过`git pull <远程仓库名如origin> <远程分支名如main>:<本地分支名如main>`拉取同步数据,自动进行了一次merge合并至本地仓库操作。如果不想自动合并，只是获取远程仓库数据，执行`git fetch`，以后再进行手动合并。`git pull = git fetch + git merge`,如果不想用merge合并，想用rebase合并，则拉取时执行`git pull --rebase`,好处就是多人开发的时候，大家各自在自己的分支上进行commit，拉回来用一般的方式合并会产生为了合并而产生的额外commit，如果不想要这个额外的commit，则采用此种rebase方式拉取为妥。


# [(128) 两小时Git入门教程 - YouTube](https://www.youtube.com/watch?v=PN1k1CLXtlw) 
1. ls -altr
2. `git init my-repo`通过命令直接生成一个名为my-repo的本地仓库。
3. 工作区的文件是不能直接commit提交至仓库的，只有暂存区的文件才能commit提交到仓库（即.git目录）
4. 文件四种状态
	1. untrack(新创建，还未被git管理，只存在于工作区)
	2. unmodified（已被git管理的文件，文件内容没有变化，没有被修改过）
	3. modified（已经被修改，还未添加到暂存区）
	4. staged（修改后已经添加到暂存区）
5. `git add .`已经提交至暂存区的文件可通过`git rm --cached <file>`删除撤回。
6. `git ls-files`可查看暂存区的文件。
7. `git reset --hard HEAD^`回退到上一个commit版本。再用`ls`和`git ls-files`分别查看工作区和暂存区文件的变化，如果是默认的`--mixed`参数，则会丢弃暂存区的文件但保留工作区的文件。
8. git diff查看工作区暂存区和本地仓库之间的差异以及不同版本不同分支之间的差异
	1. git可将每个文件的内容都分别生成一个40位的hash值。便于比较差异。
	2. 使用`git diff`比较工作区和暂存区的差异。
	3. 使用`git diff HEAD`比较工作区和版本库之间的差异
	4. 使用`git diff --cached|staged`比较暂存区和版本库之间的差异
	5. 使用`git diff 7xxxx 7yyyyy`比较两次commit提交版本的差异 ，或`git diff 7yyyyy HEAD`比较最新提交的节点和之前提交ID之间的差异。`git diff HEAD~ HEAD`当前版本和上一次版本的差异,`HEAD~`表示上一次提交的版本，`HEAD~2`表示上两次提交的版本，依次类推。HEAD指向分支的最新提交节点。
	6. `git diff HEAD~3 HEAD file3.txt`仅比较两个版本file3文件的差异。
	7. `git diff <branch_name> <branch_name>`比较分支之间的差异。
9.  git删除文件
	1. 分两步进行，先从工作区中通过rm删除，再通过git add更新暂存区同步删除。
	2. `git rm files.txt`,一步实现同时删除工作区和暂存区文件。
	3. 以上2步如果最后没有提交，则被删除的文件在版本库仍存在。
	4. `git rm -r 星号`递归删除某个目录下的所有子目录和文件
	5. 以上删除文件后，不能忘记提交
10. `.gitignore`文件
	1. 此文件中提到的文件名被忽略，按`git status`,也看不到该文件的任何信息，即使按`git add .`也不会添加到暂存区。更加不能添加到版本库。但如果忽略的文件名在录入此文件中之前已经在版本中存在。则无法生效。这个文件不能是已经被添加到版本库早已存在的文件。该文件后期更改，依然能执行`git add`后再提交。解决的办法就是执行`git rm --cached file.log`从暂存区删除该文件，再提交，同步删除版本方库的该文件。
	2. 也就是说仅对工作区文件提交暂存区之前形成的忽略文件才有效，
	3. `.gitignore`中也可添加目录，如`temp/`尾部的`/`表示`temp`不是文件，而是一个目录，并忽略该目录中所有文件。
		1. `*.a`,
		2. `!lib.a`取消忽略lib.a文件，即使上一行忽略了`.a`文件。
		3. `/TODO`只忽略当前目录的TODO文件，其他目录下的同名文件不忽略。
		4. `doc/*.txt`与`doc/**/*.txt`具有不同的含义。
		5. [abc][0-9][a-zA-Z]中括号【】表示匹配列表中的单个字符。两个星号表示匹配任意的中间目录，`[^abc]`因为`^`在`[]`内部，表示开头，如果在`[]`外部，表示取反

11. github类似托管平台及git客户端
	1. gitee码云（国内）
	2. gitlab（国外），可部署本地私有化仓库
	3. git客户端，gitKraken，sourcetree，TortoiseGit,vscode等
	4. 在当前本地仓库目录下，执行`code .`可调出vscode来打开当前目录。
	5. vscode中，选择`view→command patern`输入path可将code命令加入到系统path，输入`setting→选择settings.json`vscode的配置文件。
	6. vscode源代码区打开修改过的文件，可看到并列的2个窗口，左边是修改前，右边是修改后
	7. 文件的标识
		1. ？？（Untracked）
		2. M(Modified) 已修改
		3. A（Added）  已添加暂存
		4. D（Deleted）已删除
		5. R（Renamed）重命令
		6. U（Updated）已更新未合并
	8. `git checkout`除了切换分支和状态外，还可以用来恢复文件或目录到之前的某一个状态，比如意外地修改了某一个文件，可用此命令恢复到文件修改前的状态。此时如果恰好分支名称和被修改的文件名称相同的话，就会出现歧义。此命令会默认切换分支而不是恢复文件，为避免这种歧义，git2.23版本后提供了一个新命令为`git switch x`,专门用来切换到已有的分支。`git switch -c y`切换到新建的分支。
	9. 切换当前新分支后，工作区会发生变化，会自动包含旧分支工作区所有文件及本新分支新建的文件，但如果切换回旧分支后，原新分支新增的工作目录新文件会消失不见。切回原分支后，工作区会恢复原分支工作目录下所有文件，并不包括新分支新建立的文件。原因是旧分支没有合并新分支，所以新分支新文件在旧分支下不显示。
	10. `git merge 将要被合并的分支名，如dev`,当前分支如果是main，如果目标分支也是main，确保当前分支为`main`,在当前分支下执行`git merge dev`,回车后，git会自动产生一次提交commit，在gitKraken中可形象观察到分支合并前后的变化，也可以执行`git log --graph --oneline --decorate --all`观察。
	11. 上步合并分支结束后，原被合并的dev分支仍存在。并不会自动消失。除非手动删除。`git branch -d dev`表示删除已经完成合并的分支。没有合并的分支需要执行`git branch -D dev`才能删除。
	12. 如果两个分支修改了同一个文件的同一行代码，git就不知道应该保留哪个分支的修改内容了，`git diff`命令可以看出两个分支修改的内容差异。git会使用左箭头等号及右箭头分别来表示两上分支的修改内容。不想继续执行合并用`git merge --abort`来中止合并过程，两个分支未合并前，同一文件各自修改，各自分支目录下执行ls互不可见，`git commit -a -m "xxxx"`表示在add暂存操作的同时提交。一个命令完成添加暂存和提交两个动作。
	13. `git merge --no-ff -m  "xxxxx" xxbranch`可以更好地保留这个分支的历史记录，同时自动生成一个新的commit节点。
	14. 自己在自己的分支dev上修改代码后，可以push后，再建立PR（pull Request）或gitlab上MR（merge request）请求合并入master或其他分支。
	15. 文件在工作区修改后git  status显示为modified，可以执行撤销修改回到修改前的文件，`git restore xxx.file`撤销修改，也可将修改加入暂存区`git add .`,也可将文件撤销加入暂存区，通过`git restore --staged xxx.file`实现。撤销commit回到早期版本状态，使用`git reset --hard xxxxxxx.sha-1`,如果远程分支也需要同步回退，重新`git push -u origin dev`一次就可以了。可能会有`更新被拒绝，因为您当前分支的最新提交落后于其对应的远程分支`,这时需要强制推送，执行`git push --force -u orgin dev`
		```
		每次git push时，不一定都推送最新的head对应的commit，可以选择性推送部分commit历史记录中的某条commit对应的sha值，如推送倒数第二条的commit到远程origin仓库的main分支，执行git push origin sha-1值:main即可。
		```
	16. 常见处理分支冲突的办法，[(131) 你早晚都得会！最新Git & GitHub 实战教程：30分钟带你掌握开发必备技能 | 进阶技能 - YouTube](https://www.youtube.com/watch?v=a9T7dqDtRaY) 
		1. 一般push失败时，应该远程分支有其它人已经更新分支，需先pull远程至本地，也有可能pull也会失败，需要指定如何调和偏离的分支。可以选择`merge`方式，把远程分支的更改合并到本地分支，创建一个新的合并，再提交。`git merge --no-rebase`或执行`git config pull.rebase false`后，以后直接执行`git pull`就可以了。不需要另加参数了。
		2. 第二种方式是`rebase`方式，把本地提交移动到远程分支的最新提交的后面。好处是可以保持提交历史的线性，可能会破坏提交历史。
		3. 当前版本在开发中，发现上一版本有bug，这时执行`git stash`把当前开发的内容暂存起来，再新建一个分支去修复bug,修复完毕后，再回到当前正开发的分支，执行`git stash pop`恢复之前的开发进度了。不可以不使用`git stash`而直接拉一个新的分支去修复bug，因为git仓库中，一个分支未提交的修改对其它的分支是可见的。
		4. `git cherry-pick commitida commitidb`可以从多个commit中挑选一些进行合并，
		5. [有趣的开源社区 - HelloGitHub](https://hellogithub.com/) github中，查询`in:name pdf in:description pdf 翻译 stars:>1000 pushed:>2025-01-01 fork:>100 language:Python`
		6. [收费软件平替查询](https://openalternative.co/) 
		7. 更新git bash，执行`git update-git-for-windows`
		8. [Git合并冲突处理指南：解决二进制文件冲突_git 二进制文件冲突-CSDN博客](https://blog.csdn.net/PolarisRisingWar/article/details/138790764) 

12.  Rebase与merge的区别，`git switch main→git merge dev`
		1. rebase不用像merge必须回到main分支上展开合并，在任意分支上都可以执行合并，如在dev分支上操作，`git switch dev→git rebase main`,dev的两次提交记录都会延伸变基到main分支的末尾，形成一条直线。
		2. `git switch main→git rebase dev`则会将main的2次提交记录延伸到dev分支的末尾。
		3. rebase执行时会先找到需要合并的2个分支的共同祖先，也就是前面main分支的第三个提交节点，也就是分叉点，从分叉点后所有节点嫁接到目标分支最新提交记录的末尾。
		4. merge的优点是不会破坏原分支的提交历史，方便回溯和查看。缺点是会产生额外的提交节点，分支图较复杂。
		5. rebase优点是不会新增额外的提交记录，形成线性历史，比较直观和干净。缺点是会改变提交历史，改变了当前分支branch out的节点，应避免在共享分支使用。一般不会在公共的分支上执行rebase操作.


13. git中关于crlf与lf行尾转换的警告[Git中的“LF will be replaced by CRLF”警告详解_git lf will be replace by crlf-CSDN博客](https://blog.csdn.net/taiyangdao/article/details/78629107) 
	- 系统行末结束符 换行（Line Feed）和回车(Carriage Return)
	- linux中回车键对应`$`符号，行尾必须是LF，回windows行尾必须是CRLF
	- 回车的本意包含（\n + \r或者\r + \n）2个动作。机械英文打字机，字车最右边手动归位至最左边，名回车，然后滚筒上卷一行，以便开始输入下一行，名换行。不一定换行是下一行行首。

14. [为什么 Git 将此文本文件视为二进制文件？- 堆栈溢出 --- Why does Git treat this text file as a binary file? - Stack Overflow](https://stackoverflow.com/questions/6855712/why-does-git-treat-this-text-file-as-a-binary-file?__cf_chl_tk=3beZdC2mbqmprpe9h8CzkS6IDWWabqS5r_5kZSixgZo-1760171667-1.0.1.1-m8dLHWRgriZN6Yra7QYrW2rfvFWbY0GedyyZfxdKBKI) 


# 第七章 修改commit历史信息
- 修改最后一次commit信息可用`--amend`
- 修改所有commit历史范围的提交信息，分两步
	1. `git log --oneline`显示全部commit历史。
	2. `git rebase -i sha-1(init commit)`指定指令应用范围从当前至第一个init commit。
	3. 在vim窗口中，将想要修改的commit开头`pick`改为`reword`,pick的意思是保留这次的commit不做修改，而reword是重新修改的意思。
	4. 如果出现合并冲突，通过`git diff`查看冲突原因，解决后`git rebase --continue`继续中断后的进程。
	5. 如果rebase执行成功后，想取消并回到rebase之前的话，执行`git reset ORIG__HEAD --hard`即可。
	6. 如果将多个commit合并为一个，在vim窗口中，将想要修改的commit开头`pick`改为`squash`,pick的意思是保留这次的commit不做修改，而squash是与上面的一行commit合并为一个commit。
	7. 如果将一个commit拆分为多个commit，在vim窗口中，将想要修改的commit开头`pick`改为`edit`,pick的意思是保留这次的commit不做修改，rebase到`edit`会中断执行。此时再执行`git reset HEAD^`,目录中会出现多个处于untracked的文件，再依次执行`add和commit`二段式指令，执行完成后，最后继续执行`git rebase --continue`跑完。
	8. 在两个相邻commit之间插入新的其它commit时，参照上面拆分方法。在sourcetree中操作时，光标定在两个相邻commit的下面那个，不能定位上面的来操作。
	9. 改变commit历史顺序时，直接在vim中编辑所需要的行间顺序即可。如果想删除某个commit，也可在vim删除中删除commit信息即可。
	10. 调整commit顺序或删除某个commit，都有逻辑相关性问题，要慎用。
	11. 如果要取消最后一次的commit，可使用`git revert HEAD --no-edit`,表示不编辑commit信息，原commit历史依然存在不会消失，反而在上面还会增加一条原commit历史信息的反向commit记录。这个指令的意思是再做一个新的commit，来取消你不要的commit的概念，所以commit数量才会增加，相当于红冲。如果这个红冲的commit也不想要了，再来一次`git revert HEAD --no-edit`，则回到第一次revert操作前的状态。负负得正。如果要砍掉这个revert，直接使用`git reset HEAD^ --hard`也能回来revert之前的状态。
	12. revert命令适用于多人共同协作的项目，团队开发，不一定有机会使用reset指令，这时可用revert指令来做出一个取消的commit，对其它人来说不算是修改历史，而是新增一个commit，只是这个commit与某个即有的commit反向而已。
	13. reset和rebase都会改变历史记录，而revert不会，reset和rebase通常适用于尚未push出去的commit，而rebase适用于已经push出去的commit，或是不允许使用reset或rebase之修改历史记录的指令的场合。
	14. `git tag tag__name sha-1`或`git tag tag__name sha-1 -a -m "xxxxx"`给commit添加tag轻量标签和附注标签。-a参数表示请git建立有附注的标签。
	15. `git show tag__name`查看标签的信息。标签存放于`.git/refs/tags`,轻量标签指向某一个commit，而附注标签指向某个tag物件，该物件才再指向那个commit。
	16. `git tag -d tag__name`删除标签。
	17. 标签与分支都是一个指标，分支存放在`.git/refs/heads`目录下，两者都是一个40字节的sha-1值。被删除时，都不会影响到被指到的那个物件。两者的差别在于分支会随着commit而移动，而标签不会。分支是会移动的标签。
	18. 工作临时中断，需要临时先处理其它分支，可先commit本分支后再切换到其他分支工作，处理完毕后，再`git reset HEAD^`回来原分支继续完成剩下的工作。另一种办法是，可使用`git stash`先把本分支作的修改先存起来，保存场景，（untracked状态的文件是没办法被stash,需要额外使用-u参数），再观察`git status`发现干净了。那些文件去哪儿了？可用`git stash list`查看。可执行多次`git stash`命令形成多份stash。WIP（work in progress）工作进行中的意思。
	19. 已完成先前临时插入的其他工作，再回来原来中断并已stash的工作。找到`git stash list`中原中断的工作，如`stash@{2}`,执行`git stash pop stash@{2}`,将某个stash拿出来套用在目前的分支上，套用过的stash就会被删除。如果不想删除stash，可用`git stash apply stash@{2}0`代替，如果后面没有指定要pop哪一个stash，会从编号最小的，也就是`stash@{0}`开始拿（就是最后叠上去的那次）
	20. 如果那个stash确定不要，可以使用`git stash drop stash@{0}`从列表中删除。
	21. 删除多个commit中都共同存在的且已push的某个密码文件，比较直观的办法是使用rebase指令，一个一个comiit去编辑。可用`git filter-branch --tree-filter "rm -f config/database.yml"`即可。如果后悔了，而filter-branch操作执行的同时git会把状态备份一份在`.git/refs/original/refs/heads`目录里，实际只是备份进行filter-branch之前的那个HEAD的sha-1值而已。此时后悔可直接hard reset sha-1即可恢复删除前状态。或执行`git reset refs/original/refs/heads/master --hard`
	22. 老实说已经push了，和倒出去的水一样收不回来，能做的就是`git push -f`,重新强制push一份刚刚已经filter-branch过的commit上去
	23. 只想要某个其他分支的几个commit，可用`git cherry-pick sha-1 sha-2 sha-3`,如果捡过来先不合并，在后面加上`--no-edit`,始`git cherry-pick sha-1 sha-2 sha-3 --no-edit`,先放在暂存区

	24. 如果要彻底删除git中的文件，删除多个commit中都共同存在的且已push的某个密码文件，比较直观的办法是使用rebase指令，一个一个comiit去编辑。可用`git filter-branch -f --tree-filter "rm -f config/database.yml"`即可。-f参数强制覆盖filter-branch的备份点。 由于filter-branch操作执行的同时git会把状态备份一份在`.git/refs/original/refs/heads`目录里，实际只是备份进行filter-branch之前的那个HEAD的sha-1值而已。必须删除些sha-1备份，执行`rm .git/refs/original/refs/heads/master`。 再执行`git reflog expire --all --expire=now`,要求reflog现在立刻过期。再执行`git fsck --unreachable`可看到很多unreachable的物件了。再启动`git gc --prune=now`立刻让垃圾车把这些物件运走。再检查一遍`git fsck`
