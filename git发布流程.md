# 配置篇
在github网页中需要完成：
·创建github账号，需要用户名+邮箱地址
·创建目标代码仓库（repository）

下载安装git客户端（http://git-scm.com/download/）
·首次登录客户端需要绑定自己的账户

	git config --global user.name "xxx"
	git config --global user.email "xxx@yy.com"

为确保传输保密，需要设置ssh key公钥秘钥加密，具体步骤为在git端生成公钥(id_rsa.pub)秘钥(id_rsa)，然后将公钥设置在github账户中(Add SSH key)

	ssh-keygen -t rsa -C "xxx@yy.com"

# 上传篇 git bash
github代码体系分为本地端和网络端，发布代码是指将代码上传至网络端，一般来说是从本地端连接网络端，然后进行代码推送。
本地端有三个部分：本地工作目录、缓存区、HEAD


·本地工作目录存放本地代码，首先需要将本地工作目录下变为git本地仓库（master）从而生成本地文件的三部分

	git init
	
·然后选择本地文件添加至缓存区（可以单个文件、文件夹或者通配符）

	git add ./*/<filename>
	
·然后将缓存区的文件提交至HEAD，HEAD存放最新提交的文件，提交时可以添加提交信息（提示、区分版本等作用）

	git commit -m "Version 1"
	
·HEAD文件就可以推送至远端仓库，但提交之前需要绑定远端仓库

	git remote add origin <仓库SSH地址>
	git remote rm origin		# 可以删除绑定地址
	git remote set-url origin <url>	# 或者直接修改 
	
	git push -u origin master		# 推送至master中
	
此时，代码已经发布至对应的远端仓库位置

# 科学上网 + 下面这些语句可以解决连接问题

	git config --global --unset http.proxy  
	git config --global --unset https.proxy


## 注：
master其实和dome、main的含义差不多，就是指这个项目中主要的部分，主要分支；
除此之外，git还会提供其他分支，相当于待开发部分，支持合并等功能

	git branch -M master
	
改变分支
