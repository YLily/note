# 创建版本库
	mkdir filename  //创建新目录
	cd filename     //跳到新目录
	pwd				//显示当前目录
	git init    	//把当前目录变成git可以管理的仓库(生成.git目录)
	git config --global user.name "Your Name"
  	git config --global user.email "email@example.com"

# 远程仓库

本地库 --> 远程库

```
//新建远程仓库
git remote add origin git@github.com:YLily/仓库名.git  //把本地仓库的内容推送到GitHub仓库


git push -u origin master   //-u参数 git会把本地的master分支和远程的master分支关联起来
git push origin master 	//git会把本地的master分支推送到远程仓库
git push origin branchName //推送其它分支

git remote 					//查看远程仓库的信息
git remote -v  				//显示更详细的信息
git fetch    				//获取服务器端改动

```
创建SSH Key

新建一个目录，在该目录下输入命令

```
ssh-keygen -t rsa -C "email address"  //遇到权限，前面加上sudo

//然后3次回车

id_rsa.pub   //公钥
id_rsa		 //私钥

```

登录GitHub

setting/ssh key 

* 填上任意title
* 复制rsa.pub里面的内容粘贴到key文本框里
* add shh key 


远程库 --> 本地库

```
//新建远程仓库

git clone git@github.com:YLily/repository_name.git  //克隆一个本地库

SSH方式: git@github.com:YLily/repository_name.git
http方式: https://github.com/YLily/ife2017.git

//只能看到本地的master分支

git checkout -b dev origin/dev  //创建远程origin的dev分支到本地

git branch --set-upstream dev origin/dev  //设置本地dev分支与远程origin/dev分支的链接
git pull  //把最新的提交从origin/dev抓下来


```

# git 工作流

工作目录 --> 缓存区 --> HEAD --> 远程仓库

* 已修改 :工作目录
* 已暂存 :暂存区，git add 把文件添加暂存区
* 已提交 :git commit 目录，git commit 把暂存区的所有内容提交到当前分支
* 推送到远程仓库 


```
//添加到缓存区

git add filename		//把文件添加到缓存区
git add ./* 	//添加当前目录中所有文件

//提交改动

git commit -m 提交说明  // 把文件提交到HEAD

//推送改动

git push origin master  //推送到远程仓库
git push origin branchName //推送其它分支

//error

//远程库中已经有一部分代码，不允许直接把代码覆盖上去

git push -f  //强推，本地的代码会替代git仓库的内容

git fetch  //把git的东东fetch到本地
git merge  //然后merge后再push


git status  				//仓库当前的状态
git diff filename					//查看修改的内容
git diff HEAD --filename 	//查看工作区和版本库里面最新版本的区别
git log 					//查看提交历史(显示从最近到最远的提交日志commit id SHA1计算出来的16近进制数)
git log -- pretty=oneline

```
# 撤销修改
```

git checkout -- fileName     //丢弃工作区的修改

git reset HEAD fileName		//把暂存区的修改回退到工作区

git reset --hard HEAD^      //回到上一个版本
git rest --hard 版本      	//回到某某版本
git rest --hard commit id   //commit id SHA1计算出来的一个非常大的数字，用十六进制表示

git reflog					//查看历史命令

HEAD			//当前版本				
HEAD^  			//上一个版本
HEAD^^  		//上上个版本
HEAD~100 		//上100个版本

git rm fileName 			//删除文件， 并git commit
git rm -r foldername        //删除文件夹

git reset --hard origin/master

```

# 分支

master 是默认分支

```
git branch branchName 		//创建分支
git checkout branchName 	//切换分支
git checkout -b branchName  //创建分支并切换

git push origin branchName  //将分支推送到远程

git branch 					//列出所有分支，当前分支前有*号
git branch -d branchName	//删除分支, 不能删除当前分支, master是默认分支不能被删除, 可在setting中修改默认分支

//更新与合并

git pull    //更新本地仓库至最新改动，在本地合并解决冲突后再推送
git merge branchname  //合并其他分支到当前分支
//Fast-forward信息, 合并是“快进模式”，直接把master指向dev的当前提交，所以合并速度非常快
git merge --no-ff-m "merge with no ff" branchName  //禁用 fast forward

git diff source_branch target_branch  //查看

```

### bug分支

```
git stash 					//把当前工作现场"储藏"起来，等恢复现场后继续工作

// 创建分支修复bug,修复完成后，切换到那个分支，并完成合并，最后删除临时分支
git checkout -b bug(branchname)
git add fielname
git commit -m 'fix bug'
git checkout master
git merge --no-ff -m 'merged fix bug' bug(branchname)
git branch -d bug(branchname)

//切换到工作分支
git checkout branchname

//回复工作区
git stash apply  //恢复后，stash的内容未删除， git stash drop 来删除
git stash pop   //恢复的同时把stash的内容删除了 

git stash list 				//查看stash的工作现场

```

### 新功能feature分支

添加新功能时，新建一个feature分支
开发完成后，合并，最后删除feature分支
```
git checkout feature
git add filename
git commit -m 'feature'
git checkout branchname
git merge --no-ff -m 'delete feature' feature
git branch -d feature

```


### tag标签

切换到需要打标签的分支上
```
git tag tabName                     //创建标签
git tag -a tagName -m des			//创建带有说明的标签，用-a指定标签名-m指定说明文字
git tag -s tagName -m des			//-s用私钥签名一个标签
//签名采用PGP签名，必须首先安装gpg（GnuPG），如果没有找到gpg，或者没有gpg密钥对，就会报错

git tag								//查看所有标签
git tag tagName commit id			//默认标签是打在最新提交的commit上
git show tagName 					//查看标签信息
git tag -d tagName					//删除未推送到远程的标签
git push origin tagName				//推送某个标签到远程
git push origin -tags				//一次性推送全部未推送到远程的本地标签
git push origin :refs/tags/tagName	//删除推送到远程的标签，先本地删除，再push

git log   //获取提交ID
```


# fork开源项目

先fork, 从自己的账号下clone,才能推送修改

在GitHub上发起一个pull request


# GitHub pages 直接在网页中浏览

创建gh-pages分支，打开YLily.github.io/目录名/文件


# 自定义git

git config --global user.name "your name "
git config --global user.email "your_email@your_emai.com"
git config --global color.ui true  //彩色的git输出
git config format.pretty oneline  //显示历史记录时只显示一行注释

在git工作目录下穿件一个特殊的.gitignore文件，把要忽略的文件名填进去
把.gitignore提交到Git
所有配置文件可以直接在线浏览https://github.com/github/gitignore

检验.gitignore的标准是git status命令是不是说working directory clean
你想添加一个文件到Git，但发现添加不了，原因是这个文件被.gitignore忽略了，可以用-f强制添加到Git

git check-ignore -v fileName //检查哪个规则写错了

配置别名
--global参数是全局参数，也就是这些命令在这台电脑的所有Git仓库下都有用
不加global只对当前仓库起作用

```
git config --global alias.st status             //git status只用输入git st
git config --global alias.last 'log-1'			//显示最后一次提交信息
git config --global alias.unstage 'reset HEAD'  //git unstage test.py相当于git reset HEAD test.py把暂存区的修改撤销掉，重新放回工作区
git config --global alias.lg "log --color --graph --pretty=format:'%Cred%h%Creset -%C(yellow)%d%Creset %s %Cgreen(%cr) %C(bold blue)<%an>%Creset' --abbrev-commit"

cat .git/config 	//每个仓库的git配置文件都放在。git/config文件中
cat .gitconfig 		//当前用户的git配置文件放在用户主目录下的一个隐藏文件.gitconfig中

gitk  //内建的图形化git
git add -i  //交互地添加文件至缓存区

```
