##### 配置秘钥
我们知道，使用 git 需要配置 ssh key 来进行授权认证，如果我们工作中有自己的 git 仓库，而你同时你自己平时经常玩 github，这个时候你就需要配置多个 ssh key。具体方法如下：<br>
1. 进入 git 控制台输入命令： ssh-keygen -t rsa -C "1email@company.com” -f ~/.ssh/id-rsa (id-rsa 就是你秘钥文件的名字，这里可以改成你自己的，用来区分不同的 git 仓库)<br>
2. 在 .ssh 目录下新建配置文件 config , 添加如下内容<br>

#gitlab //这个是注释<br>
<div></div>
&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;Host gitlab.com &nbsp;&nbsp;//仓库地址<br>   
&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;HostName gitlab.com&nbsp;&nbsp;//仓库主机名<br>
&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;PreferredAuthentications publickey<br>
&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;IdentityFile ~/.ssh/id_rsa

##### 设置用户名和邮箱
git config --global user.name "用户名" # 设置用户名<br>
git config --global user.email "用户邮箱"   #设置邮箱<br>

--globa 可以换成 --local 表示只是配置你某一个项目的用户名和邮箱

<hr>

当你每次提交项目的时候，可能会提示你输入用户名和密码，输入：<br> 
git config --global credential.helper store <br>
当你下次提交的时候，用户名和密码会被储存下来

<hr>

##### Git 常用命令
git branch -a &nbsp; 查看本地所有分支<br>
git branch -r &nbsp; 查看远程所有分支<br>

git branch name &npsp; 在本地新建一个分支<br>
git push origin namne &nbsp; 将本地新建的分支推送到远程分支<br>

git branch -d name &nbsp; 删除本地分支<br>
git push origin -D name &nbsp; 删除远程分支<br>
