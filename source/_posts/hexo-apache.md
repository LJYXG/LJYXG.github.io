---
title: Hexo+Apapche建站小白教程
date: 2022-02-16 18:03:41
tags:
 - Hexo
 - Apache
 - Linux
 - Centos8
categories: Blog
toc: true
---
 
 这是一个借助Hexo框架生成静态博客 并且采用Git工具将网站同步至自己的Apapche服务器，部署网页的一个安装教程！

<!-- more -->

#  Hexo+Apapche建站小白教程

##  第一节、**首要说明**
1、本文主要讲述的是**静态个人博客的搭建**，除此外仅含有少量与Hexo有关内容提及，没有Html语句的教学，也没有服务器申请、域名申请的教学
2、拟实现的目标为:Windows部分Hexo编写博客，Server部分部署Apache公开网页 ，借助Git进行同步，适用于静态个人博客，系统：**CentOS 8**
3、如无特殊说明，本教程均使用**root**权限操作
4、本教程定位为小白教程，但需要您或有一定的Linux基础，不过**必需良好掌握在网络上获取知识**
5、本教程写于**2021年11月27日**，教程因为软件更新，会有出入请注意判别！
6、本人对服务器进行的配置是很基础很基础的那种，能让网站正常运作，但没有防攻击、防查水表的功能
7、本人是自学的，欢迎各位大佬提建议，如果有不好的地方请轻喷
8、本文主要为四大部分**首要说明、Windows 端记录、Server 端记录、简单的Hexo使用教程**

> 【特别提示】如果没有强烈的兴趣爱好或者其他必要情况需要自行建立博客，请不要轻易购买域名或服务器，装个虚拟机玩玩就行，否则，您的项目和花掉的钱会随着您的热度衰减而变得没有任何意义。

## 第二节、Windows 端记录

**参考链接列表**

- [GitHub+Hexo 搭建个人网站详细教程](https://zhuanlan.zhihu.com/p/26625249)

- [基于CentOS搭建Hexo博客](https://segmentfault.com/a/1190000012907499)

### 一、安装Node.js

#### 1、官网下载Windows版本

https://nodejs.org/zh-cn/

#### 2、检查安装

安装好后shift+右键打开Powershell并检查是否安装

![](/images/psg2/part1/check.png)

### 二、安装Git并配置

#### 1、官网下载并安装

https://git-scm.com/

#### 2、配置

##### ①打开Git Bash右键

![](/images/psg2/part1/Snipaste_2021-11-27_10-59-23.png)

##### ②配置

**注意：**此处需求你已经注册过Github （本文并不会用到Gayhub

```bash
# 将此处的"yourname"替换成自己Github的用户名
git config --global user.name "yourname"

# 将此处的"youremail"替换成自己GitHub的邮箱
git config --global user.email "youremail"
```

![](/images/psg2/part1/Snipaste_2021-11-26_22-31-28.png)


##### ③检查是否有SSH Key

输入以下代码检查是否有SSH Key

```bash
cd ~/.ssh
```

![](/images/psg2/part1/Snipaste_2021-11-26_22-33-46.png)

此图说明当前客户端不存在SSH Key，则需按照如下步骤创建SSH Key

```bash
# 将此处的"youremail"替换成自己Github的邮箱
ssh-keygen -t rsa -C "youremail"
```

![](/images/psg2/part1/Snipaste_2021-11-26_22-38-18.png)

我为了方便直接不设置密码了 （鉴于鸽子也没咋用过Git 不确定这里输入密码是否会回显  如果键入密码后不显示 可能是Unix特性 并不是没输入进去）

##### ④查看并记录SSH Key

接下来查看刚刚创建的SSH Key 并将其记录下来 等下后面的步骤会用到

```bash
cat ~/.ssh/id_rsa.pub
```

![](/images/psg2/part1/Snipaste_2021-11-26_22-42-47.png)

**Tips**在Linux Terminal以及Git bash中复制文本为Ctrl+Shift+C 黏贴为Ctrl+Shift+V

### 三、安装Hexo

#### 1、全局安装

```bash
npm install -g hexo-cli
```

##### ![](https://raw.githubusercontent.com/LJYXG/image-host/main/Snipaste_2021-11-27_10-59-23.png)

**此处开始所有的Git Bash 均需要管理员身份运行**

![](/images/psg2/part1/Snipaste_2021-11-27_10-59-23.png)

#### 2、创建目录 并在此目录初始化Hexo

此处我们的目录为演示方便创建在了桌面Blog文件夹

Git Bash切换到目录

![](/images/psg2/part1/Snipaste_2021-11-27_11-05-59.png)

在当前目录初始化Hexo

```bash
hexo init blog
```

![](/images/psg2/part1/Snipaste_2021-11-27_11-08-25.png)

![](/images/psg2/part1/Snipaste_2021-11-27_11-09-08.png)

#### 3、安装Git插件及其其他插件

首先在blog文件下打开Git Bash，在输入`npm install`进行包的安装。

![](/images/psg2/part1/Snipaste_2021-11-27_11-17-44.png)


而后在Bash中键入如下指令安装Git插件

```npm
npm install hexo-deployer-git --save
```

![](/images/psg2/part1/Snipaste_2021-11-27_22-52-39.png)

而后在Git bash里输入`hexo s` 然后在浏览器中打开`localhost:4000`即可以看到搭建完成的Hexo

![](/images/psg2/part1/Snipaste_2021-11-27_11-20-49.png)

![](/images/psg2/part1/Snipaste_2021-11-27_11-22-05.png)

#### 4、额外设置

**注意：此处在服务器端设置完成后才会有用此处为方便写在一起了下面的第三大节还会介绍这部分内容**

设置博客根目录下的_config.yml文件

```yaml
deploy:
    type: git
    repo: git@SERVER:/home/git/blog.git       #此处的SERVER需改为你自己服务器的ip
    branch: master                            #这里填写分支
    message:                                  #提交的信息
```

![](/images/psg2/part1/Snipaste_2021-11-27_11-33-25.png)

**截至到此处Windows端的安装已经完成下面开始Server端安装**

## Server 端

**参考链接列表**

- [基于CentOS搭建Hexo博客](https://segmentfault.com/a/1190000012907499)
- [Centos 8 安装apache、mysql、php7.3、PHPmyadmin及多域名多ssl配置方法](https://www.niuqi360.com/lamp-config/centos-8-install-apache-mysql-php73-phpmyadmin-ssl/)

**此处说明鸽子使用的服务器系统不同的系统需要自行更改部分指令**:Crntos-8-x64

### 一、安装Apache-http并配置

#### 1、安装Apache 

Tips:

Ⅰ、此处开始默认您使用root账户如果不是请加上`sudo` e.g.`sudo yum install -y xxxx`

Ⅱ、Linux下可以使用Tab键对指令 路径进行补全

Ⅲ、Linux Terminal复制和粘贴为

##### ①升级更新CetOS8

这部分会很漫长 你可以刷刷啊b

```shell
yum update
```

##### ②安装apache并检查安装

```shell
yum install -y httpd 			#-y是个参数 表示接下来的默认选择YES
rpm -qi httpd
```
![](/images/psg2/part2/Snipaste_2021-11-27_12-03-09.png)

##### ③启动apache并查看状态

```shell
systemctl start httpd
systemctl status httpd
```

![](/images/psg2/part2/Snipaste_2021-11-27_12-27-05.png)


启动好在浏览器中打开http://youserverip即可看到apache的默认页面

![](/images/psg2/part2/Snipaste_2021-11-27_12-33-16.png)




**额外内容(指令说明)**

```shell
systemctl start httpd			#启动Apache
systemctl stop httpd			#停止Apache
systemctl reload httpd			#重载Apace
systemctl restart httpd			#重启Apache（这个后面会用到）
systemctl enable httpd			#开机自启Apache（这个建议启动后就打开）
systemctl disable httpd			#取消开机自启Apache
```

### 2、配置Apache

默认情况下，Apache服务器仅配置一个网站的运行，如果你想在服务器上运行多个网站，那么需要进行一些必要的配置工作。Apache虚拟主机网站安装目录为`/var/www/html`，这只能运行一个网站，想要同时运行多个网站，那么我们需要创建新的网站目录结构。

**注意**首先咱们先根据本次教程定义一些参数 这些参数可以根据你的需求自行更改：
域名 ：domain.com(虚构的)
**建议**从此处开始 最好已经加域名绑定ip不然可能我无法进行下面的虚拟机设置

##### ①创建新的网站安装目录：

**注意**在此之前我po出我喜欢的网站目录结构

```txt
/var/www/
|---domain.com/			推荐用分配的域名做文件夹名称方便管理
|	|---public/			存放网页文件
|	|	|···
|	|---logs/			存放网页的日志
|	|	|···
```

可以输入以下命令

```shell
mkdir -p /var/www/domain.com/public
mkdir -p /var/www/domain.com/logs
```

给予服务用户相关权限（不然的网页会打不开的

```shell
chown -R $USER:$USER /var/www/domain.com/public 
chmod -R 755 /var/www
```

##### ②创建对用虚拟主机网页的对应配置文件

为了对虚拟主机以及网站进行多域名的配置以及方便管理，需要创建`sites-available` 和 `sites-enabled`两个文件夹。

虚拟主机的配置文件被保存在 `sites-available` 目录中，表示为可用的网页配置文件

而 `sites-enabled` 通过创建软连接`sites-available` 目录中的文件的方式选择启用对应的站点配置文件

```shell
mkdir /etc/httpd/sites-available
mkdir /etc/httpd/sites-enabled
```

Tips:`/etc/httpd/`这个目录下储存着http所有的配置

接下来对Apache的主要配置文件进行简单配置，目的是让Apache读取上一步创建的`sites-enabled` 中的配置

```shell
vim /etc/httpd/conf/httpd.conf
```

在配置文件的末尾添加如下打码,然后保存退出

```conf
IncludeOptional sites-enabled/*.conf
```

Tips：Vim/Vi 简单使用教程

```
移动光标 上下左右键
进入编辑模式 i 键
退出编辑模式 esc 间
搜索 /
强制退出 先按:进入指令模式 而后键入q! 最后回车
保存退出 先按:进入指令模式 而后键入wq 最后回车
```

下面我们创建虚拟机配置文件

```shell
touch /etc/httpd/sites-available/domain.com.conf
```

用Vim打开然后编辑内容(Tips 此处可以直接用下面的指令而不用上面的先创建，因为vim打开的文件不存在时会直接创建空白文件)

```shell
vim /etc/httpd/sites-available/domain.com.conf
```

然后在其中填写如下内容并保存退出

```conf
<VirtualHost *:80>
    ServerName domain.com
    #此处填虚拟机所用域名（无论一级还是二级）
    ServerAlias domain.com
    #此处填写服务器顶级(一级)域名
    DocumentRoot /var/www/domain.com/public
    #填写上文创建的网页地址
    ErrorLog /var/www/domain.com/logs/error.log
    #此处地址填写上文创建的日志地址，后面的error.log不要动
    CustomLog /var/www/domain.com/logs/requests.log combined
    #此处地址填写上文创建的日志地址，后面的requests.log combined不要动
</VirtualHost>
```
![](/images/psg2/part2//Snipaste_2021-11-27_18-19-34.png)

**注意**复制时最后记得把以#号开头的的注释行删掉 否则可能会产生意想不到的bug！！！

现在创建软链接把`sites-available` 的文件链接到`sites-enabled`启用我们刚刚设置的配置文件

```shell
ln -s /etc/httpd/sites-available/domain.com.conf /etc/httpd/sites-enabled/domain.com.conf
```

最后重启Apache启用上述配置

```shell
systemctl restart httpd	
```



至此我们的Apache配置到此结束，（此时我们打开域名时啥都没有的 不要急 ）

### 二、安装Nodejs

输入以下代码进行Nodejs的安装。

```shell
yum install -y nodejs
```

### 三、安装Git并进行相关配置

#### 1、git安装

输入以下代码，进行Git的安装

```
yum install -y git
```
![](/images/psg2/part2/Snipaste_2021-11-27_18-34-43.png)

2、创建git用户以及设置密码

指令如下

```shell
#创建用户,用户名为git
adduser git
#设置密码
passwd git
```
![](/images/psg2/part2/Snipaste_2021-11-27_18-38-05.png)

3、把git用户添加到sudo用户组中
输入以下代码`sudo vim /etc/sudoers`，打开sudoers文件，输入`:/root`进行搜索，搜索到代码行`root ALL=(ALL) ALL`,然后在这一行下添加以下代码`git ALL=(ALL) ALL`。输入完毕之后，按`wq!`强制保存退出vim。

![](./images/psg2/part2/Snipaste_2021-11-27_20-09-29.png)

#### 4、设置SSH Key文件并赋权。

切换到git用户，添加SSH Key文件并且设置相应的读写与执行权限

输入以下代码：

```shell
# 切换用户
su git
# 创建目录
mkdir ~/.ssh
# 新建文件
vim ~/.ssh/authorized_keys
```

然后把之前在客户端设置的SSH Key,复制到authorized_keys文件中，保存后退出。如下图：

![](/images/psg2/part2/Snipaste_2021-11-27_20-16-39.png)

接下来设置文件权限，把authorized_keys文件设置成只有属主有读写权限，把ssh目录设置为只有属主有读、写、执行权限。代码如下：

```shell
chmod 600 ~/.ssh/authorized_keys
chmod 700 ~/.ssh
```

设置完后，返回客户端，打开Git Bash，输入以下代码，测试是否能连接上服务器：

```bash
# ServerIP为你自己服务器的ip
ssh -v git@ServerIP
```

结果如下图：
![](/images/psg2/part2/Snipaste_2021-11-27_20-30-00.png)

#### 5、网页地址授权

把之前创建好的网页文件夹 授权给git用户，以便于上传文件，代码如下：

```shell
# 使用sudo指令，需要输入git用户的密码
sudo chown -R git:git /var/www/domain.com/public
```

#### 6、初始化一个git裸库

切换到git用户，然后切换到git用户目录，接着初始化裸库，代码如下：

```shell
su git
cd ~
git init --bare blog.git
```

接着新建一个post-receive文件

```shell
vim ~/blog.git/hooks/post-receive
```

然后在该文件中输入以下内容：

```shell
git --work-tree=/var/www/domain.com/public --git-dir=/home/git/blog.git checkout -f
```

如下图：

![](/images/psg2/part2/Snipaste_2021-11-27_23-00-11.png)

保存退出之后，再输入以下代码，赋予该文件可执行权限。

```arcade
chmod +x ~/blog.git/hooks/post-receive
```

#### 7、设置博客根目录下的_config.yml文件。

**注意：这里我们在windows端已经设置过一次 为了保险起见我们再次重复内容**

设置博客根目录下的_config.yml文件

```yaml
deploy:
    type: git
    repo: git@SERVER:/home/git/blog.git       #此处的SERVER需改为你自己服务器的ip
    branch: master                            #这里填写分支
    message:                                  #提交的信息
```

![](/images/psg2/part2/Snipaste_2021-11-27_11-33-25.png)

保存后，在博客根目录打开Git Bash，输入以下命令：

```ebnf
hexo g
hexo d
```

部署完毕之后，即可在浏览器输入你的服务器/域名进行访问你的博客了。
