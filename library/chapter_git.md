Git 有三区：工作区、提交暂存区(提交任务stage)、版本库

![](https://ws4.sinaimg.cn/large/006tNc79gy1fswhxmljubj30ok0bcds6.jpg)

index表示暂存区

origin 远程主机名

### git全局配置
```
git config --global user.name "jp"
git config --global user.email evilester@sina.com
git config --global core.editor emacs
git config --global merge.tool vimdiff####git配置
git config -e  版本库级别
git config -e --global 全局配置（主用户目录下.gitconfig）
git config -e --system 系统配置 默认打开/etc/gitconfig
git config --system (or --global or --local) --unset hooks.announcelist 
git config --system (or --global or --local) --unset-all hooks.announcelist
```

### 重置提交用户名或邮件等信息
```
git commit --amend --allow-empty --reset-author
```
### git日志
```
git log --stat 查找变更文件
git log --pretty=fuller 查找提交信息和提交者
git log ---pretty=online 精简信息
git log -l --pretty=raw 查看日志详尽输出
git log --pretty=raw --graph <commit_id>查看提交跟踪链，可看parentid,treeid
git reflog show master | head -5 查看日志
git reflog show stash 查看stash的日志
git log --graph --pretty=raw refs/stash -2 查看stash的日志，保存像别起一分支，pop表示合并然后add回暂存区
git log --oneline --decorate -4 在提交id旁显示提交关联的引用 （tag或branch）
```

### git 状态
```
git status -s 精简状态变化,第一个表示
```
![](https://ws4.sinaimg.cn/large/006tNc79gy1fswhyxnoytj3056022dfu.jpg)

第一个M：版本库中的文件与中间态的暂存区文件相比有改动 
第二个M：工作区的文件与中间态的提交暂存区的文件相比有改动

### git比较
```
git diff 工作区与暂存区
git diff HEAD 工作区与HEAD(当前工作分支)
git diff --cached或--staged 提交暂存区与版本库上比
git diff B A  tag B和tag A比较
git diff A    工作区和tagA比较
git diff --cached  A 暂存区和tagA比较
git diff --word-diff 逐词比较
```
### git
```
git reset HEAD 暂存区的目录树会被master分支指向的目录树替换，但工作区不受影响
git rm --cached <file>,直接从暂存区删除文件，工作区则不做出改变
```

### git 查看
```
git clean -fd 清除工作区改动
git ls-tree -l HEAD 查看HEAD指向的目录树
*git ls-files -l -s 查看暂存区的目录树
git write-tree | xargs git ls-tree -l -r -t 将暂存区保存为目录树并递归查看
git rev-parse refs/heads/master 直接查看maste中的commit id
git rev-parse HEAD master 查看HEAD和master的id
git rev-list HEAD ｜ wc -l 查看有几次提交
git ls-remote origin 查看远程分支id
```

### git cat-file 查看提交类型
```
commitid:本地提交的唯一标识
tree:本次提交的目录树
parent:本地提交的父提交(上一次提交)
git cat-file -t <id> 查看id类型
git cat-file -p <id> 查看该对象的原生内容 pretty-print object's content
git log --graph --online online缩小commit_id长度
```
### git 对象
```
master代表分支最新提交，或refs/heads/master或heads/master
HEAD最近一次提交
HEAD^父提交
HEAD^^  HEAD^的父提交
HEAD^3  HEAD的父提交的第3个父提交，一般是合并过的，有多个parent,指第3个parent
HEAD~5 HEAD^^^^^
HEAD^{tree} HEAD对应的tree
HEAD:path/to/file
```
### git 重置游标
```
1.替换引用的指向，引有指向新的提交ID
2.替换暂存区。替换后，暂存区的内容和引用指向的内容的目录树一致
3.替换工作区。替换后，工作区的内容变得和暂存区一致，也和HEAD指向的一致
git reset --hard HEAD^ 重置至HEAD的父提交， 会丢失提交历史
--hard 1、2、3步
--soft 第1步，不改变工作区和暂存区
--mixed 第1、2步，除工作区其它都替换，默认为此项。
git reset HEAD == git reset 将所有操作从暂存区撤出，即去掉所有add
git reset -- filename 将filename撤出暂存区，相当于add的反向操作
git reset --hard master@{2} 重置至reflog显示的commit
reset默认值是暂存区，覆盖暂存区，checkout默认值是HEAD，覆盖工作区。
reset –soft命令，只是撤销了commit的提交记录，commit改动的代码仍然存在，很受用。 
reset --soft commit-id,其中的commit-id指的是撤销之前的那个commit id.
```

### git checkout
```
git checkout <commitid> 分离头指针，HEAD指向<commitid>
git checkout 汇总工作区、暂存区和HEAD的差异
git checkout HEAD 同上 ,并且会切换至HEAD指针
git checkout master 将head的指针指向master指针
git checkout -- filename 检出filename覆盖相应文件 
git checkout branch -- filename 用branch所指向的filename覆盖暂存区和工作区的文件 
git checkout -- . 或git checkout . 用户暂存区的文件覆盖工作区的文件 
git checkout . 或git checkout -- <file>用暂存区的有的文件替换工作区的文件，清除未添加的
git checkout HEAD .或git checkout HEAD  <file> 有HEAD指向的master分支全部内容替换工作区的文件，也会清除暂存区的文件
git checkout source_branch -- <paths>... 从指定分支获取一个文件
```
### git merge
```
git merge <commit_id> 挽救分离头指针，将commit_id合并至当前分支
```

### git 分支
```
  git branch -v 查看当前分支
```  
### git 暂存区命令，工作区和暂存区会分别保存，没add的不会保存
```
git stash 保存当前进度
git stash save "message" 保存进度
git stash list 查看保存的进度
git stash pop 恢复相应的进度，恢复完毕会删除相应存储的
git stash apply 恢复相应的进度，不删除相应存储的
git stash drop <stash_id>删除相应的进度
```

### git clean
```
git clean -nd 查看工作区要清除的文件
git clean -fd 清除工作区所有改动的文件
```

### git push
```
git push origin new_branch:master 将new_branch提交至远程master
```
### git tag
```
git tag -m "<message>" tag_name 打tag
git describe describe可以做为软件的版本号 
git tag v1.0  # 默认标签是打在最新提交的commit上的
git tag   # 查看所有标签
git log --pretty=oneline --abbrev-commit#找到历史提交的commit id
git tag v0.9 6224937  # 在某个commit id上打标签git show v0.9 # 查看标签信息#创建带有说明的标签，用-a指定标签名，-m指定说明文字
git tag -a v0.1 -m "version 0.1 released" 25dfc7#通过-s用私钥签名一个标签
git tag -s v0.2 -m "signed version 0.2 released" fec145a  
推送标签至远程
git origin tagname#推送指定标签
git origin --tags  #推送全部标签
本地删除
git tag -d v0.9
远程删除
git tag -d v0.9 #先本地删除
git origin :refs/tags/<tagname>
Git v1.7之后可以git origin -- tag <tagname> 来删除
```
### git rm
```
git rm filename
rm *.*   git add -u 功能同上
git cat-file -p HEAD~1:filename>filename 回复上一版本的filename去当前的filename
git show HEAD~1 == git cat-file -p
git checkout HEAD~1  -- filename 同上
git add -A  将工作区中的所有改动以及增加的文件添加到暂存区
git rm --cached filename/folder  不跟踪文件
```
### 文件忽略.gitignore
```
cat > .gitignore << EOF
ignorename
EOF 
忽略只对未跟踪的文件有效，对于已加入版本库的文件无效
git commit -a 偷懒模式，-a会自动执行git add
忽略文件语法
*.a忽略所有以.a为扩展名的文件
!lib.a 即使设置了.a的忽略，但该文件不忽略
/TODO 只忽略些目录下的TODO文件，子目录的TODO不忽略
build/忽略所有build目录下的文件
doc/*.txt 忽略如doc/asdfa.txt但是文件如doc/server/text.txt不被忽略
```
### 文件归档
```
git archive  -o xxx.zip HEAD  将最新提交归档至xxx.zip
git archive -o xxx.tar HEAD src doc 只将目录src和doc 归档至xxx.tar
git archive --format=tar --prefix=1.0/   v1.0 | gzip > xxx.tar.gz 基于tagv1.0建立归档，并为归档中的文件添加目录前缀1.0
```
### 文件追溯
```
git blame finename 显示每一行在哪个版本引入，由谁引入
git blame -L 6,+5 filename 第6行后的5行
```
### git 二分查找
```
git bisect start 开始二分查找
git bisect bad 当前版本标记为坏版本
git bisect good G 将G(此处可以为<commit id>，也可以为tag)版本票房为好版本
git describe 自动定位
git bisect good 
git bisect bad 
 .....
git bisect reset
至此二分查找结束

git bisect log  > logfile二分查找日志
git bisect reset 先结束
git bisect replay logfile 可以删除一些步骤，然后通过该日志恢复
git describe 切换
```
#### 二分查找支持脚本自动化测试
```
xx.sh 退出码为0表示是好版本，125跳过，1-127坏版本
git bisect start master G 从master至G开始二分查找
git bisect run sh good-or-bad.sh 运行日志
git describe refs/bisect/bad 定位坏版本
```
### git禁止非快进式提交
```
git --git-dir=/path/to/repos  config receive.denyNoFastForwards true 禁止非快进式提交，即禁止不更新，使用-f强制覆盖提交
```
### git 冲突
```
 git merge [选项] <commid_id> 合并分支，合并会自动提交 ，加上--no-commit 则合并结果存上暂存区
.git/MERGE_HEAD 记录所合并的提交ID
.git/MERGE_MSG 记录合并失败的信息
.git/MERGE_MODE标识合并状态
git ls-files -s 查看暂存区信息，第三个字段表示暂存区编号，当冲突发生时，会用到0以上的暂存区编号
git show :1:filename 查看暂存区中编号为1的文件
<<<<<< =====之间的当前分支所更改的内容 ，>>>>>=====是合并后添加的
合并之后，执行git add命令将文件添加到暂存区(标号为0),然后再提交,使用reset可以撤销合并至冲突前
如果遇到不能自动合并的冲突，会切换到一个以<commitid>命令的分支，解决冲突后
git add -u 标记为已解决
git rebase --continue 继续rebase 此时会回到master分支
git push origin master 提交

git merge FETCH_HEAD 和fetch head 合并，在.git/FETCH_HEAD里有fetch的最新的commitid
```

### git 分支
```
git branch 查看当前分支
git branch <branch name> 创建新分支
git branch <branch name> <start-point> 基于<start-point> 创建新分支
git branch -d <branch name> 删除分支,删除前会检查是否合并于其它分支  
git branch -D <branch name> 强制删除分支
git branch -m <oldbranch> <newbranch> 重命名分支
git branch  -M <oldbranch> <newbranch> 强制重命名分支
git branch -b <new_branch> [<start-point>] 创建并切换分支
git branch -r 查看远程分支
git checkout -b newBrach origin/master 取回远程主机的更新以后，可以在它的基础上，使用git checkout命令创建一个新的分支。
```
### git push
```
git push origin <远程分支名> 推至远程分支，如果该分支不存在，会在远程创建该分支
PS:git push origin release1.4:releases/release1.4.0      <本地版本>：远程版本
```
### git cherry-pick
```
git cherry-pick <commit_id> 或<branchname> 等git对象（见上文git对象部分） ,从其它分支偷相应的分支并合并至当前分支上
git log master...otherbranch  查看冲突， ...可做为分隔符
```
### rebase
```
git rebase --abort 暂停rebase
```
### 修正当前分支父类
```
git branch --set-upstream-to=origin/releases/release1.4.0
git branch --set-upstream rohan_branch1 nickname/anotherBranch
git branch -u  分支 设置更新的远程分支
```
### submodule
```
git submodule add <git@github.comgit l:xx.git> /path/to/clone 添加子模块至克隆至/path/to/clone
git submodule 查看当前子模块状态
git submodule init 就是在.git/config中注册子模块的信息子模块的信息
git submodule update 检查子模块，并克隆出还没克隆的子模块

git clone --recursive /path/to/repos/foo.git     –recursive参数的含义：可以在clone项目时同时clone关联的submodules。
git submodule foreach git pull  循环进入（enter）每个子模块的目录，然后执行foreach后面的命令。
该后面的命令可以任意的，例如 git submodule foreach ls -l 可以列出每个子模块的文件列表
```
### 删除submodule
```
git submodule deinit asubmodule     asubmodule为submodule在本地的路径 module/submodule
git rm asubmodule 
# Note: asubmodule (no trailing slash) # or, if you want to leave it in your working tree 
git rm --cached asubmodule
But you seem to still need a:
rm -rf .git/modules/asubmodule
```
默认git submodule update并不会将submodule切到任何branch，所以，默认下submodule的HEAD是处于游离状态的(‘detached HEAD’ state)。所以在修改前，记得一定要用git checkout master将当前的submodule分支切换到master，然后才能做修改和提交。 

如果你不慎忘记切换到master分支，又做了提交，可以用cherry-pick命令挽救。具体做法如下： 

用 git checkout master 将HEAD从游离状态切换到 master 分支, 
这时候，git会报Warning说有一个提交没有在branch上，记住这个提交的change-id（假如change-id为 aaaa)用 

git cherry-pick aaaa 来将刚刚的提交作用在master分支上用 
git push 将更新提交到远程版本库中 
对于submodule在主项目中的修改会麻烦一些，所以应该切至master分支，然后对该moduel提交一次，然后至主项目中 

需要更新submodule的id，所以还要在主项目中再提一次，将submodule的关联id更新 
submodule需要先注册才能更新，所以要先init,然后再update,注册信息可以在主项目根目录下通过 

cat .git/config 查看注册信息

### 更新fork代码(github）
```
git remote add <mrdoob> git://github.com/mrdoob/three.js.git  增加源分支地址到你项目远程分支列表中 ,添加了新的主机名，mrdoob，通过git remote -v查看，然后通过该主机来更新  
git fetch <mrdoob> 更新mrdoob主机上的内容
git merge <mrdoob>/master    merge mrdoob主机下的master分支
git push origin master push至远程

git br -avv 查看分支所有的信息，包括版本和远程本地分支的对应关系
```
### git remote
```
git remote -v 查看远程主机信息
git remote add <主机名> git://path/to/git.git
git remote rename 主机名 新主机名
git remote rm 主机名 删除远程主机
```
### git 册除Fetch下的内容
```
以下两个指的是有remote references
git remote prune origin 
git fetch --prune
git prune 一般清除unreachable objects，commits aren't reachable in any branch or tag,所以一般没什么用，删除本地分支用git branch -D

git commit -c <commit id > 将该次提交amend至<commit id>中
本地未回退，直接删除远程的记录：
git push -f origin HEAD~i:master
git reset --hard HEAD~2                   # 取消当前版本之前的两次提交
git push origin HEAD --force              # 强制提交到远程版本库，从而删除之前的两次提交数据

git mergetool Git图形化合并工具
git强制更新
git fetch --all 
git reset --hard origin/master

git 更新某个文件
git fetch
git checkout origin/master -- /path/to/file

git 回退某个版本文件
git reset <commitId> /path/to/file

git merge 按时间顺序合并， git rebase 将自已的commit 插入至合并分支的最头上
```

## 进阶操作
1. revert 造成 merge 代码丢失
[revert-a-faulty-merge](https://github.com/git/git/blame/master/Documentation/howto/revert-a-faulty-merge.txt)
```
git revert <revert_id> <revert revert commit>
git rebase --no-ff P (批量更换Commit Id)
```

### 相关链接
[git学习网站1](http://www.ruanyifeng.com/blog/2014/06/git_remote.html)

[Pro Git](http://git-scm.com/book/zh/v1/???-Git-Git??)

[图解git](https://github.com/yanrk/HelloWorld/blob/master/%3C%3CGit%E5%9B%BE%E8%A7%A3%3E%3E%E7%AC%94%E8%AE%B0)

[git 入门](http://www.bootcss.com/p/git-guide/)