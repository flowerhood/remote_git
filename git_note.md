# [为你自己学git 高见龙](https://gitbook.tw/chapters/introduction/what-is-git)
# [(99+ 封私信) GIT从入门到精通 - 知乎](https://zhuanlan.zhihu.com/p/653156299) 
# [史上最全 Git 图文教程（非常详细）零基础入门到精通，收藏这一篇就够了-CSDN博客](https://blog.csdn.net/Javachichi/article/details/140660754) 
# [Git从入门到精通（全）-阿里云开发者社区](https://developer.aliyun.com/article/1104558) 
# [【Git保姆级使用教程】Git从入门到精通超级详细的使用教程，一套教程带你搞定Git（高见龙版本）。_git从入门到精通pdf-CSDN博客](https://blog.csdn.net/syu_acm/article/details/141530578) 


## 第1节

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
1. 查看`git branch`
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
	- 复制`test.pub`公钥文件全部内容，粘贴到`github→settings→SSH and GPG keys→new SSH key`
	- 修改当前目录`~/.ssh`下的`conifg`文件，添加5行至该文件尾部
	```
# github
Host github.com
HostName github.com
PreferredAuthentications publickey
IdentityFile ~/.ssh/test
	```

	-  完成上述后，此时可执行`git clone git@github.com:flowerhood/remote_git.git`

3. 几种同步方式
	1. （先有远程仓库）可以先在github建立一个空仓库，然后clone至本地，在本地添加和编辑文件后，再使用`git push`不用任何参数直接推送至github端。再在github上增加一个readme文件，在本地上执行`git pull`拉取至本地。
	2. （先有本地仓库），还是要预先在github建立一个新的空仓库如`remote_git`，切换到本地仓库main分支，执行`git remote add origin git@github.com:flowerhood/remote_git.git`,再通过`git remote -v`查看当前本地仓库对应的远程仓库的别名和地址，再执行`git branch -M main`将本地master分支更名为`main`分支，以便和github默认的main分支名称保持一致。再执行`git push -u origin master:main`，其中u是	`upstream`的缩写，意思将本地仓库和别名为origin的远程仓库关联起来。`msaster:main`表示本地仓库的master内容推送到远程仓库的main分支，如本地master已更名为main分支，则改为`git push -u origin main:main`或进一步简写为`git push -u origin main`
	3. 上一步容易出现[解决Git上传文件出错：[rejected] master -> master (fetch first)错误](https://cloud.baidu.com/article/3318156) 








