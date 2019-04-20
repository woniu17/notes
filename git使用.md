# 在服务器上创建远程仓库
```
mkdir -p repo/hello.git
cd repo/hello.git
git --bare init
```

# 从本地拉取远程仓库
```
git clone ssh://linqx@192.168.2.125:22/Users/linqx/repo/hello.git
```

# 设置提交者用户和邮箱
1. 全局设置，修改~/.gitconfig文件
	```
	git config -e --global
	```
1.  单仓库配置，修改.git/config文件
	```
	git config -e
	```
1. 配置内容
	```
	[user]
		name = linqx
		email = linq.x@qq.com
	```

# 提交到master分支
1. 新增hello.c文件，添加如下数据
	```
	aaaaaa
	```
1. 将修改添加到暂存区 
	```
	git add hello.c
	```
1. 提交commit到本地master分支
	```
	git commit -m "commit a"
	```
1. 将提交推送到远程仓库的master分支
	```
	git push origin master:master
	```

# 创建个人分支（linqx分支）
1. 查看所有本地分支
	```
	git branch
	```
1. 查看所有分支，包括本地、远程分支
	```
	git branch -a
	```
1. 从master分支分出linqx分支
	```
	git branch linqx
	```
1. 切换到linqx分支
	```
	git checkout linqx
	```
1. 确认当前所处的分支
	```
	git status
	```

# 在个人分支进行开发
1. 新增world.c文件
	```
	bbbbbb
	```
1. 将修改添加到暂存区
	```
	git add world.c
	```
1. 将修改提交到本地linqx分支
	```
	git commit -m "commit b"
	```
1. 本地将linqx分支推送到远程仓库
	```
	git push origin linqx:linqx
	```

# 将个人分支上的提交合并到master分支（无冲突）
1. 切换本地master分支
	```
	git checkout master
	```
1. 将远程的master分支拉取到本地master分支，保证本地master分支为最新
	```
	git fetch origin master:master
	```
1. 假设在合并linqx分支前，master分支对hello.c文件进行修改
	```
	aaaaaa
	cccccc
	```
1. 将本地linqx分支上的提交合并到master分支
	```
	git merge linqx
	```
1. 将合并后的本地master分支同步到远程master分支
	```
	git push origin master:master
	```

# 合并master分支到个人分支，继续开发
1. 切换到linqx分支
	```
	git checkout linqx
	```
1. 将master分支合并到linqx分支
	```
	git merge master
	```
1. 将合并后的本地linqx分支推送到远程linqx分支
	```
	git push origin linqx:linqx
	```
1. 修改hello.c
	```
	aaaaaa
	cccccc
	dddddd
	```
1. 提交修改，并推送到远程linqx
	```
	git add hello.c
	git commit -m "commit c"
	git push origin linqx:linqx
	```
# 个人分支代码合并到master分支（产生冲突）
1. 假设在合并linqx分支前，master分支修改了hello.c
	```
	aaaaaa
	cccccc
	eeeeee
	```
1. 切换到master分支，并且合并linqx分支
	```
	git checkout master
	git merge linqx
	```
1. hello.c文件产生冲突，打开该文件，修改文件为：
	```
	aaaaaa
	cccccc
	dddeee
	```
1. 将修改的好的hello.c文件提交到暂存区
	```
	git add hello.c
	```
