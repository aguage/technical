生产服务器管理  
1.用户管理 root 与 平时登陆用户  ， 软件运行用户 权限
为运维，前端，后端创建用户，隔离权限
2.软件包，yum，更改yum软件源
3.web运行环境，nginx  php mysql 
4.管理工具  nload  htop git composer jre

==== 
具体执行步骤：
租用服务器
ssh以root，22端口链接到服务器
从第一条命令开始
---服务器初始化 --  
查看yum源
yum repolist
1.centOS 备份软件源 下载新的yum源，具体步骤 在阿里云网站有http://mirrors.aliyun.com/help/centos
命令：
mv /etc/yum.repos.d/CentOS-Base.repo /etc/yum.repos.d/CentOS-Base.repo.backup   备份
wget -O /etc/yum.repos.d/CentOS-Base.repo http://mirrors.aliyun.com/repo/Centos-7.repo 下载新的CentOS-Base.repo 到/etc/yum.repos.d/

添加阿里epel源 具体步骤http://mirrors.aliyun.com/help/epel
mv /etc/yum.repos.d/epel.repo /etc/yum.repos.d/epel.repo.backup
mv /etc/yum.repos.d/epel-testing.repo /etc/yum.repos.d/epel-testing.repo.backup
wget -O /etc/yum.repos.d/epel.repo http://mirrors.aliyun.com/repo/epel-7.repo

yum clean all
yum makecache    生成缓存
yum update 升级原系统的软件

针对yum软件源软件版本低的问题解决方案有-1.到官方网站下载rpm包安装，或到官方网站下载源码，自己编译安装。或更换添加第三方软件源
2.更换root密码
命令：
passwd 新密码

3.在日常的服务器管理过程中，您并不需要使用root来登录。最佳实践是创建一个拥有一定权限的普通用户进行日常的操作与维护。
添加非ROOT用户 useradd -d /usr/用户名 -m 用户名
修改密码 passwd 用户名
赋予用户权限  vim /etc/sudoers  找到root 行 增加 用户root权限
重新用用户名链接服务器

命令：
# 创建一个名为chris的用户
adduser chris
# 给chris创建登录密码
passwd chris
# 通过编辑sudoers文件给chris赋权
visudo

这个命令使用vi打开了sudoers文件，我们可以通过vi来对这个文件进行编辑。在sudoers的最后一行加入：
chris    ALL=(ALL)       ALL

4.禁用root直接登录

vi /etc/ssh/sshd_config #编辑

找到PermitRootLogin，将后面的yes改为no

:wq! #保存退出

5.更改ssh默认端口、禁止root登录
我们可以对ssh服务进行配置，通过修改ssh服务的默认监端口址、禁止root登录等手段，加强云主机的安全性。我们使用刚刚创建的chris账号登录云主机，登录后执行：

vi /etc/ssh/sshd_config
在打开的配置文件末尾输入以下内容，其中Port您可以自行设置：

Port 10022
Protocol 2
PermitRootLogin no
保存后退出，然后reload ssh服务，使配置生效：

service sshd reload
当再次登录云主机时，您需要在配置文件中指定的Port参数：

ssh -p 10022 chris@211.155.92.230


6.安装web运行环境，配置环境变量
查看默认安装  rpm -qa| grep 软件名
采用到官方网站最新源码包，自己编译
不过这个有个缺点就是要解决软件依赖问题。
解决方案：事先将要依赖的文件整理出来，然后编译安装完，再安装最后的软件。
比如说安装nginx，需要的依赖包有pcre openssl等，或者说要开启这些模块，要安装这些依赖。然后将这些先安装好，最后再安装nginx。