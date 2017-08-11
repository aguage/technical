一、删除旧版本
如果已经安装过php就先删除之前的版本。检查方法如下：
yum list installed | grep php
然后将安装的包进行删除
比如 yum remove php.x86_64 php-cli.x86_64 php-common.x86_64 php-gd.x86_64 php-ldap.x86_64 php-mbstring.x86_64 php-mcrypt.x86_64 php-mysql.x86_64 php-pdo.x86_64
具体根据显示的安装列表的名称进行相应的删除
二、安装新版版
1. 更新yum安装包
CentOS 7.x
rpm -Uvh https://mirror.webtatic.com/yum/el7/epel-release.rpm
rpm -Uvh https://mirror.webtatic.com/yum/el7/webtatic-release.rpm
CentOS 6.x
rpm -Uvh http://mirror.webtatic.com/yum/el6/latest.rpm
CentOS 5.x
rpm -Uvh http://mirror.webtatic.com/yum/el5/latest.rpm
2. 通过云进行php和其他组件的安装
yum install php70w.x86_64 php70w-cli.x86_64 php70w-common.x86_64 php70w-gd.x86_64 php70w-ldap.x86_64 php70w-mbstring.x86_64 php70w-mcrypt.x86_64 php70w-mysql.x86_64 php70w-pdo.x86_64
yum install php70w-fpm
其他需要的组件在自行安装。
三、其他版本php安装
如果想安装php5.5或者5.6版本，将上面的php70w替换为php55w或者php56w就可以了。