---
title: Hexo+Github Pages 实现个人博客的小白教程 ——上篇
date: 2022-02-21 18:58:03
tags:
 - Hexo
 - Github
 - Blog
categories: Hexo
toc: true
---

最近有朋友在使用`Hexo`做自己博客，前几日我弄过`Hexo`的安装教程 

不过上次的是 部署到了一台自己的`VPS`上

这篇将上次的文章 修改增加了一点 以部署带 `Github Pages`

<!-- more -->

**读前注意**

>本文章并不包含`Hexo`的使用教程（请见Hexo+Github Pages 实现个人博客的小白教程 ——下篇）
>
>本人的部署平台为`Github` 
>
>本文要求您有一定的自我学习能力，毕竟这只是我的经验记录，若你怕麻烦 抱歉本文章不适合您
>
>最后温馨提示 本文完整仔细阅读可能需要5min左右，若您想对照实操 可能需要半个小时！！！ 动手前请确保 您有此需求！！！


## 一、安装`Node.js`
### 1、官网下载Windows版本

https://nodejs.org/zh-cn/

>推荐选用 LTS长期支持版本

### 2、安装好后shift+右键打开Powershell并检查是否安装

![](/images/psg3/openterminal.png)

![](/images/psg3/check.png)

## 二、安装`Git`并配置

### 1、官网下载并安装

https://git-scm.com/

>国内下载速度慢的化 可以搜索镜像源~
>
>推荐[清华大学镜像源](https://mirrors.tuna.tsinghua.edu.cn/#)


### 2、配置

#### ①打开`Git Bash`右键

![](/images/psg3/Snipaste_2021-11-26_22-23-23.png)

#### ②配置

>GitHub的注册并不会提供教程，请自行百度~

```bash
# 将此处的"yourname"替换成自己Github的用户名
git config --global user.name "yourname"

# 将此处的"youremail"替换成自己GitHub的邮箱
git config --global user.email "youremail"
```

![](/images/psg3/Snipaste_2021-11-26_22-31-28.png)

#### ③输入以下代码检查是否有`SSH Key`

```bash
cd ~/.ssh
```

![](/images/psg3/Snipaste_2021-11-26_22-33-46.png)

此图说明当前客户端不存在SSH Key，则需按照如下步骤创建SSH Key

```bash
# 将此处的"youremail"替换成自己Github的邮箱
ssh-keygen -t rsa -C "youremail"
```

![](/images/psg3/Snipaste_2021-11-26_22-38-18.png)

我为了方便直接不设置密码了 （这里输入密码是不会回显 如果键入密码后不显示 是特性哦 并不是没输入进去）

#### ④接下来查看刚刚创建的SSH Key 并将其记录下来 等下后面的步骤会用到

```bash
cat ~/.ssh/id_rsa.pub
```

![](/images/psg3/Snipaste_2021-11-26_22-42-47.png)

全选后 右键选择Copy复制知道一个位置临时保存 等下还会用

## 三、安装`Hexo`

### 1、全局安装

```bash
npm install -g hexo-cli
```

![](/images/psg3/Snipaste_2021-11-27_10-59-23.png)

(上图的意思是 以管理员方式打开 `Git Bash`  并不是 "从列表中删除哦~" 

>后面鸽子测试 不用管理员权限也是可以的 如果遇到权限问题 可以试试用管理员模式


![](/images/psg3/Snipaste_2021-11-27_11-01-14.png)

### 2、创建目录 并在此目录初始化`Hexo`

此处我们的目录为演示方便创建在了桌面`Blog`文件夹

`Git Bash`切换到目录

![](/images/psg3/Snipaste_2021-11-27_11-05-59.png)

在当前目录初始化`Hexo`

```bash
hexo init blog
```

>**注:**这里鸽子打错了 一般在一个文件夹下直接输入` hexo init `即可，这里鸽子加了一个` blog `意味着 在当前的目录下 新建一个 `blog` 文件夹 ，并在这个文件夹下初始化

![](/images/psg3/Snipaste_2021-11-27_11-08-25.png)

![](/images/psg3/Snipaste_2021-11-27_11-09-08.png)

### 3、安装`Git`插件及其其他插件

首先在`blog`文件下打开 `Git Bash`，在输入`npm install`进行包的安装。

![](/images/psg3/Snipaste_2021-11-27_11-17-44.png)

而后在`Git Bash`中键入如下指令安装`Git`插件

```bash
npm install hexo-deployer-git --save
```

![](/images/psg3/Snipaste_2021-11-27_22-52-39.png)

而后在Git bash里输入`hexo s` 然后在浏览器中打开`localhost:4000`即可以看到搭建完成的`Hexo`

![](/images/psg3/Snipaste_2021-11-27_11-20-49.png)

![](/images/psg3/Snipaste_2021-11-27_11-22-05.png)


## 四、GitHub设置

> 此处不提供`Github`的注册教程 请自行百度~

###  1、在`Github`上创建对应的仓库，

![](/images/psg3/Snipaste_2022-02-20_19-36-07.png)

![](/images/psg3/Snipaste_2022-02-20_19-37-00.png)

注意此处 `{Username}.github.io`

{Username}应与你的GitHub名称一致 如上所示 本人为 `XGLJY` 所以我的仓库名为 `XGLJY.github.io`

然后注意点选 下面的 `Add a README file `

![](/images/psg3/Snipaste_2022-02-21_13-09-15.png)

最后点击下面的绿色 摁键创建仓库 

稍等片刻 进入 这个仓库 便可以看到 `README.md` 已经显示在下面 (别问为啥换了一个账号 问就是小号被封了 呜呜呜，创建到被封 一共20分钟呜呜呜 ，在此处接了朋友的`GayHub` ,朋友的主页➡️[点我点我](http://RFTyitoti.com))

![](/images/psg3/Snipaste_2022-02-21_13-11-31.png)

然后我们打开`{YourName}.github.io  ({YourName}替换成你自己的)`就会发现`GitHub Pages` 已经自动创建好 （若没有 稍稍等一下 同步需要时间）

下面是打开的图

![](/images/psg3/Snipaste_2022-02-21_13-22-34.png)

### 2、并将你的SSH Key添加至你的GitHub账户 

在Git的设置中我们生成了SSH Key 现在我们将其添加进你的Github账号 首先点击账号设置（如图

![](/images/psg3/Snipaste_2022-02-21_13-34-40.png)

![](/images/psg3/Snipaste_2022-02-21_13-35-22.png)

![](/images/psg3/Snipaste_2022-02-21_13-42-00.png)

![](/images/psg3/Snipaste_2022-02-21_13-44-27.png)

添加完`SSH Key`的截图

![](/images/psg3/Snipaste_2022-02-21_13-45-17.png)

然后我们回到`{YourName}.github.io`这个仓库的首页

点击绿色的Code并 在弹出的框中 选择SSH的链接复制 备用

![](/images/psg3/Snipaste_2022-02-21_14-01-21.png)

## 五、Hexo简单使用 

首先打开位于站点根目录下的`_config.yml`文件 

此文件为站点的配置文件 我们需要配置下 一键部署（Deployment）的配置 

此配置项一般在配置文件的最下面 

![](/images/psg3/Snipaste_2022-02-21_14-09-47.png)

接下来就要对此项进行配置，首先介绍一下需要配置的参数 以及介绍

>**注意** 切记注意`Yaml`语法的缩进 以及冒号后面的空格

|参数|介绍|
|---|---|
|`type`|此处为一条指令部署类型 一般为`git`|
|`repo`|远程仓库的地址，鉴于本鸽子添加了我的SSH公钥至`Github`仓库 可以以使用SSH地址以使用免密登录！举个例`git@github.com:UserName/UserName.github.io.git`|
|`branch`|分支，此处注意`Github`的默认分支不再是`master`已经改为了`main`|
|`message`|此为`git`提交到`Github`的附加信息！|

下面这个例子，便是我需要填写的 需要按照你的站点更改对应的

```yaml
deploy:
  type: git
  repo: git@github.com:RFTyitoti/RFTyitoti.github.io.git
  branch: main
  message: "Update!!!"
```

编辑完毕后 保存退出

在根目录右键打开Git Bash 输入如下指令 创建本地静态文件 并运行本地服务器

```bash
#创建本地静态文件
hexo g
#运行本地服务器
hexo s
```

（ 不贴图了 hexo 安装的时候有图

检查不存在问题后  使用`hexo d`部署至`GitHub`

首次链接可能会出现这个 输入yes就行

![](/images/psg3/Snipaste_2022-02-21_14-26-10.png)

之后等待提交完毕就可以在 `{YourName}.github.io` 看到你Hexo的初始页面了

![](/images/psg3/Snipaste_2022-02-21_14-30-31.png)

至此 本文章结束 较为详细的使用请看下篇文章！