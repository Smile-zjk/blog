---
title: git笔记
date: 2018-12-26 09:47:07
tags:
---
# Git和GitHub笔记



### 设置签名
* 形式
 * 用户名   `tom`
 * Email地址： `goodMorning@atguigu.com`
	* 作用：区分不同开发人员的身份
	* 辨析：这里设置的签名和登录远程库（代码托管中心）的账号、密码没有任何关系
* 命令
 * 项目级别/仓库级别：仅在当前本地库范围内有效
   	```
    git config user.name tom_p
    git  config user.email goodMorning@atguigu.com
	```
   * 信息保存目录 .git/config
 * 系统用户级别登录当前操作系统的用户范围
	```
	git config --global user.name tom_g
	git config --global user.email goodMorning@atguigu.com
	```
	* 信息保存目录 `~/.gitconfig`
	* 级别优先级
	* 就近原则：项目级别优先于系统用户级别
 * 状态查看操作   
 	`git status`  查看工作区、暂存区状态  
 * 添加操作  
	 `git add [file name]` 将工作区的“新建/修改”添加到暂存区  
 * 提交操作  
	 `git commit -m "commit message" [file name]` 将暂存区的内容提交到本地库  
 * 查看历史记录  
	 `git log` 最完整形式    

	 ![](http://wx2.sinaimg.cn/mw690/005P5V6Qly1fyjuucqe7mj30d802ngli.jpg)  

	 `git log --pretty=oneline`  每个只显示一行  

	 ![](http://wx4.sinaimg.cn/mw690/005P5V6Qly1fyju6qd96sj30hq01k3yg.jpg)   

	 `git log --oneline`  

	 ![](http://wx2.sinaimg.cn/mw690/005P5V6Qly1fyju6qdc2rj308n01iq2r.jpg)   

	 `git reflog`

	 ![](http://wx3.sinaimg.cn/mw690/005P5V6Qly1fyju6qdabyj30dt01ojr9.jpg) 

	 `HEAD@{移动到当前版本需要多少步}`  
 * 基于索引值操作  
 	`git reset --hard [局部索引值]`  
    使用^符号：只能后退   `git reset --hard HEAD^`    一个^表示后退一步，n个表示后退n步  
	使用~符号：只能后退   `git reset --hard HEAD~3` 后退三步  
   * reset命令的三个参数对比  
   `--soft` 仅仅在本地库移动HEAD指针  
   `--mixed` 在本地移动HEAD指针，重置缓存区  
   `--hard` 在本地库移动HEAD指针，重置缓存区，重置工作区
 * 分支操作
	* 创建分支
	`git branch [分支名]`
	* 查看分支
	`git branch -v`
	* 切换分支
	`git checkout [分支名]`
	* 合并分支
		* 第一步：切换到接受修改的分支上
		`git checkout [分支名]`
		* 第二步：执行merge命令
		`git merge [分支名]·
	* 解决冲突
	    * 冲突的表现  

		![](http://wx3.sinaimg.cn/mw690/005P5V6Qly1fyju6qh03ej30j506aadb.jpg)

		* 冲突的解决
			* 第一步：编辑文件，删除特殊符号
			* 第二步：把文件修改到满意的程度，保存退出
			* 第三步：git add [文件名]
			* 第四步：git commit -m "备注"  

		```
			git remote -v 查看当前所有远程地址别名
			git remote add [别名] [远程地址]
		```
		![](http://wx4.sinaimg.cn/mw690/005P5V6Qly1fyjuva8t0ej30c9044jtq.jpg)
	
* 推送  
	* `git push [别名] [分支名]`  

		![](http://wx1.sinaimg.cn/mw690/005P5V6Qly1fyjuvcyotzj30bw044jty.jpg)
* 克隆  
	* 命令  
	`git clone [远程地址]`  

		![](http://wx4.sinaimg.cn/mw690/005P5V6Qly1fyjuva8t0ej30c9044jtq.jpg)
	* 效果  
		* 完整的把远程库下载到本地
		* 创建origin远程地址别名
		* 初始化本地库
* 拉取  
	```
	pull = fetch+merge
	git fetch [远程库地址别名] [分支名]
	git merge [远程库地址别名] [分支名]
	git pull [远程库地址别名] [分支名]
	```	
* 解决冲突
	* 要点
		* 如果不是基于GitHub远程库的最新版所做的修改，不能推送，必须先拉取
		* 拉取下来后如果进入冲突状态，则按照“分支冲突解决”操作解决即可
* SSH登录
	* 进入当前用户的家目录  
	`$ cd ~`
	* 删除.ssh目录  
	`rm -rvf .ssh`
	* 运行命令生成.ssh密钥目录  
	`$ ssh-keygen -t rsa -C Smile_zjk@qq.com`
	* 进入.ssh 目录查看文件列表  
	`$ cd .ssh`  
	`$ ls -lF`
	* 查看*id_ras.pub* 文件内容
	* 复制*id_ras.pub*文件内容，登录*GitHub*，点击**用户头像--->Settings--->SSH and GPG keys**  
	`New SSH Key`
	* 输入复制的密钥信息
	* 回到Git bash 创建远程地址别名
	* git remote add origin_ssh git@github.com:Smile_zjk/test.git
	* 推送文件进行测试

