---
title: Debian安装Apache简单记录
date: 2022-05-02 21:50:00
tags: 
 - Arch
 - Linux
---

# ArchLinux安装教程(安装简记)


<!-- more -->

![](https://raw.githubusercontents.com/LJYXG/image-host/main/img/nord-wave.png)

> 图片来源[Arch Linux minimal wallpapers](https://github.com/pablocorbalann/arch-minimal-wallpapers)

**写在前面**

> 1、本文章是本鸽子安装Arch过程的简单记录，略微加工以作为简单的教程
>
> 2、**注意：这里安装说的是手动安装，Arch官方是有官方安装脚本的很好用！！！不过如果是双系统最好手动装，官方的脚本目前还是不能装在分区里面！！！**
> 
> 3、本文章可能不会很小白，如果您有安装并使用其他Linux发行版的经历则会更好
>
> 4、作为Arch用户强烈建议你看完Arch Wiki官方的安装介绍（[Installation Guide (简体中文)](https://wiki.archlinux.org/title/Installation_guide_(%E7%AE%80%E4%BD%93%E4%B8%AD%E6%96%87))）
>
> 5、注意由于Arch采用滚动更新模式，所以最新版某些部分可能会与本教程写的时间不同，写作时间 2022年4月8日
> 
> 6、本教程大部分是截图自虚拟机（穷没法外录采集），实机大同小异
>
> 7、本教程认为您有一定基础，很多细节内容鸽子不会细说，默认你是会的
>
> 8、本文图床依旧是Github，不过用了镜像加速的国内Raw源（[链接](https://raw.githubusercontents.com/)）

**本文参考链接**

- Arch 官方Wiki的安装指导 [Installation Guide (简体中文)](https://wiki.archlinux.org/title/Installation_guide_(%E7%AE%80%E4%BD%93%E4%B8%AD%E6%96%87))

- [ArchLinux安装](https://www.chenrenhao.top/linux/archlinux%e5%ae%89%e8%a3%85.html)

- [2021 Archlinux双系统安装教程（超详细）](https://zhuanlan.zhihu.com/p/138951848)


**本人电脑配置**

- Type: Dell Inspiron 5502

- CPU: Intel Core i7-1165G7

- 内存：8Gx2

- 显卡：核显（Intel iRIS Xe）

## 安装前准备

### 一、准备网络环境

如果你的电脑有线则可以忽略这一条 无线网络的话请确保你的网络名称为英文 否则后面联网时 因为无法显示和键入中文你会连不上网!!!!!

### 二、下载ISO镜像文件

推荐在国内的镜像源下载

清华大学开源镜像站[链接](https://mirrors.tuna.tsinghua.edu.cn/archlinux/iso/)

中国科技大学开源镜像[链接](http://mirrors.ustc.edu.cn/archlinux/iso/)

选择最新的镜像进入 并选择和你电脑对应的架构版本下载

以我这次安装为例 我这时候的最新版本为`2022.04.05` 

进入后选择对应我的架构的 `archlinux-2022.04.05-x86_64.iso` 进行下载即可 

### 三、准备引导盘

本人时忠实的[Ventoy](https://www.ventoy.net/cn/index.html)使用者 如果你也是 就很简单了 把Ios丢尽U盘就行

其他用户 如果可以还是推荐尝试一下 `Ventoy` 不然的话就如下所说

制作工具建议使用 [Rufus](https://rufus.ie/zh/)，写入方式为DD而非ISO，选项那选择GPT而非默认的MBR

### 四、硬盘分区

因为我是要弄双系统所以要提前分区 如果你是整块安装 可以忽略这一步

分区用win自带的就可以

快捷键 `Win + X` 然后选择磁盘管理器

![](https://raw.githubusercontents.com/LJYXG/image-host/main/img/Snipaste_2022-04-08_20-20-53.png)

然后选择对应的分卷右键选择压缩卷即可 推荐大小至少30G 我就直接压缩30G了

> tips 这里所用的是MB 换算GB的需要乘1024 e.g.  30G = 1024 X 30 = 30720MB

这个分区不需要分配 压缩出来就好

![](https://raw.githubusercontents.com/LJYXG/image-host/main/img/Snipaste_2022-04-08_20-27-35.png)

### 五、BIOS的设置

> tips 这下面所说的 进去 BIOS 设置 进到引导选择的嗯妞 需根据品牌而定！！！

摁下开机键后狂按对应键进入BIOS设置 比如我的 Dell Inspiron 就是`F 2`

进去之后

1.禁用`Secure Boot`

2.如果你的硬盘是NVMe的，把 从硬盘的启动方式 改成 `AHCI`

保存退出BIOS 然后重复上面

摁下开机键后狂按对应键进入选择引导的页面 比如我的Dell Inspiron就是`F 12`

选择你的U盘的引导

## 开始安装

### 一、设置网络

**tips:**如果你的电脑是有线连接网络 就可以跳过这一步了

对于无线链接的网络需要使用 `iwctl` 进行联网

输入一下指令

```
iwctl
```

进入iwd模式，输入

```
device list
```

查看你的网卡名字，一般你一块网卡的话就是`wlan0`，输入

```
station wlan0 scan
```

用这块`wlan0`网卡检查扫描网络，输入

```
station wlan0 get-networks
```

查看扫描出来的网络名字，例如我这里的WIFI是 `Xiaomi_2735_5G`

```
station wlan0 connect Xiaomi_2735_5G
```

接着输入密码，然后可以输入

```
exit
```

退出`iwd`模式

退出后可以`ping`一下百度测试网络联通

```bash
ping baidu.com
```

### 二、筛选镜像源，使用中国的镜像源

```bash
reflector -c China -a 6 --sort rate --save /etc/pacman.d/mirrorlist
```

> 我在试的时候会有一个镜像源提示超时，忽略就行

### 三、硬盘分区

**注意此处开始所有图片均来自虚拟机 不过实机大同小异！！**

首先列出所有可用磁盘及分区

```bash
lsblk
```

>**tips** 像是 `nvme0n1` 这样的事NVME的固态 而 `sda`  这样的是普通硬盘 
>
>借用一张知乎作者的图
>
>![](https://raw.githubusercontents.com/LJYXG/image-host/main/img/Snipaste_2022-04-08_20-34-00.png)

下面的图是我虚拟机中执行完所显示的结果

![](https://raw.githubusercontents.com/LJYXG/image-host/main/img/Snipaste_2022-04-09_19-52-23.png)

首先我们要知道我们要准备那些分区 

对于小白来说 我们要准备的分区主要有以下几个 

|分区|用途|大小|
|---|---|---|
|EFI分区|用来引导系统（如果是双系统不用创建这个分区）|至少300M|
|SWAP| Linux swap (交换空间)|大于 512 MiB|
|主分区|用来放Linux的根目录|剩余的所有空间|

当你对Linux更加熟悉后 可以尝试增加更多的分区 例如 `/home` 不过对于最基本的教程来说都是可以忽略的

下面开始正式分区 首先说我要什么分区以及他们的大小 

`EFI `300M（如果是双系统不用创建 用Win的就行）` SWAP` 1024M  

剩余全部给根目录 

分区所用指令如下 其中 `/dev/sda` 为上面 查看我们可用磁盘及分区中所显示的

```bash
cfdisk /dev/sda
```

这个软件是一个半图形化的 很简单 我会贴创建EFI分区的 图  剩下的两个 不在赘述 大同小异

进入软件后 首先选`gpt`分区

我是虚拟机没用多余的分区 如果是实体机 记得选之前创建的 30G空白分区 

然后按左右方向键 选择下方的 `New`

![](https://raw.githubusercontents.com/LJYXG/image-host/main/img/Snipaste_2022-04-09_20-04-30.png)

![](https://raw.githubusercontents.com/LJYXG/image-host/main/img/Snipaste_2022-04-09_20-05-31.png)

![](https://raw.githubusercontents.com/LJYXG/image-host/main/img/Snipaste_2022-04-09_20-06-22.png)

![](https://raw.githubusercontents.com/LJYXG/image-host/main/img/Snipaste_2022-04-09_20-07-01.png)

然后上下移动光标 选择 `Free Space` 继续创建剩下的分区  都创建并保存后 退出

完全分好区的图片

![](https://raw.githubusercontents.com/LJYXG/image-host/main/img/Snipaste_2022-04-09_20-11-20.png)

### 四、格式化分区

我们要对我们新建的分区仅限格式化 

上一步中我们供创建了三个分区  `sda1` EFI分区 `sda2` SWPA `sda3` 根分区

#### 1、EFI分区格式化

首先是我们的 EFI 分区 如果你是双系统 忽略EFI分区的格式化 因为你不用 !!!

```bash
#指令中 efi_system_partition 为EFI分区名字 本教程中应为sda1
mkfs.fat -F 32 /dev/efi_system_partitionsda1
#所以我应该用的指令是
mkfs.fat -F 32 /dev/sda1
```

![](https://raw.githubusercontents.com/LJYXG/image-host/main/img/Snipaste_2022-04-09_20-21-05.png)

#### 2、然后是初始化SWAP分区

指令如下所示

```bash
#指令中 swap_partition 为交换空间分区名字 本教程中应为sda2
mkswap /dev/swap_partition（交换空间分区）
#所以我应该用的指令是
mkswap /dev/sda2
```

![](https://raw.githubusercontents.com/LJYXG/image-host/main/img/Snipaste_2022-04-09_20-25-26.png)

#### 3、根分区格式化

最后是根分区 我用的文件格式为`ext4`

```bash
#指令中 dev/root_partition 为分根分区名字 本教程中应为sda3
mkfs.ext4 /dev/root_partition（根分区）
#所以我应该用的指令是
mkfs.ext4 /dev/sda3
```

![](https://raw.githubusercontents.com/LJYXG/image-host/main/img/Snipaste_2022-04-09_20-21-41.png)

### 五、挂载分区

下面我贴出 Arch Wiki 安装指导的建议

|挂载点|分区|分区类型|建议大小|
|---|---|---|---|
|/mnt/boot |	/dev/*efi_system_partition* | EFI 系统分区 | 至少 300 MiB
|[SWAP] |	/dev/*swap_partition* | Linux swap (交换空间) | 大于 512 MiB
|/mnt |/dev/*root_partition* | Linux x86-64 根目录 (/)|	剩余空间

#### 1、挂载根分区

首先我们要挂载的就是根分区 根分区是sda3(我的是这个 你的需要按需更改！！！)

所以执行如下指令

```bash
mount /dev/sda3 /mnt
```

#### 2、挂载EFI分区

首先是创建对应的文件夹 

```bash
mkdir /mnt/boot
```

然后挂载EFI的引导分区 

这里需要注意 如果你是双系统 请直接挂载你的win的EFI分区 

我是虚拟机 所以直接挂载 我创建的sda1

```bash
mount /dev/sda1 /mnt/boot
```

#### 3、启用SWAP分区

用指令启用我们前面分好的1024M的SWAP分区sda2

```bash
swapon /dev/sda2
```

### 六、基本系统框架安装 

在这一步我们需要安装基本的Arch Linux 框架 

有 `base` 最基础的软件包 `Linux` Linux的内核 `linux-firmware` 常规硬件的固件 `vim` 文本编辑器 方便后面写入配置！

```bash
pacstrap /mnt base linux linux-firmware base-devel vim
```

> linux的内核 可以选择其他的内核 可以看Arch的wiki [内核](https://wiki.archlinux.org/title/Kernel_(%E7%AE%80%E4%BD%93%E4%B8%AD%E6%96%87))

然后等待下载并安装完成 

完成后我们要生成fstab文件 目的是使 Linux 开机后自动挂载磁盘！

```bash
genfstab -U /mnt >> /mnt/etc/fstab
```

执行完后最好看一下是否有内容 保险！！！

```bash
cat /mnt/etc/fstab
```

![](https://raw.githubusercontents.com/LJYXG/image-host/main/img/Snipaste_2022-04-09_21-03-36.png)

### 七、正式配置新系统

首先切换到新系统环境中去 执行下面的指令进入到新系统 

```bash
arch-chroot /mnt
```

#### 1.设置时区

首先设置我们的时区为中国上海

```bash
timedatectl set-timezone Asia/Shanghai
#同步硬件时钟
hwclock --systohc
```

#### 2.设置locale

首先使用vim编辑器打开 `/etc/locale.gen` 这个文件

```vim
vim /etc/locale.gen
```

然后 按`/`,搜索en_US  解除UTF-8那一行前面的注释符`#`

再搜索zh_CN ,解除UTF-8那一行的注释一行前面的注释符`#`

然后输入`:wq`保存退出。

> **注意** 此处不详细展开`Vim`的使用教学 您已经想要安装Arch了 那么基本的Vim使用能力 相信您一定有！

之后我们需要生成 locale 执行下面的指令生成 locale

```bash
locale-gen
```

![](https://raw.githubusercontents.com/LJYXG/image-host/main/img/Snipaste_2022-04-09_21-06-22.png)

然后执行下面的指令设置默认locale

```bash
echo 'LANG=en_US.UTF-8' >> /etc/locale.conf
```
> **注意** 在安装汉语字体并进入桌面之前这里如果换成汉语,会因为没字体而都是方块

#### 3.创建hostname

执行下面的指令设置电脑的 `hostname` 我的叫 `XG-Studio` 你可以把这个改成任何你想的

```bash
echo 'XG-Studio' >> /etc/hostname
```


#### 4.修改本地hosts

打开文件 `etc/hosts`

```bash
vim /etc/hosts
```

并输入一下内容

```bash
127.0.0.1   localhost
::1           localhost
127.0.1.1   XG-Studio.localdomain
#上面这个中的 XG-Studio 记得改成你的 hostname 
```

![](https://raw.githubusercontents.com/LJYXG/image-host/main/img/Snipaste_2022-04-09_21-14-46.png)

#### 5.用户密码设置

首先为root用户设置密码 输入下面的指令

```bash
passwd
```

然后输入你的密码，他还会让你再输入一次以确认密码

> tips 密码不会回显 ！！！

然后添加用户，如果不添加用户是无法在桌面启动器里面登录的！！！！

```bash
useradd -m -g users -s /bin/bash xg
# xg 是我的用户名 可以换成你的 
```

为新创建的用户 `xg` 创建密码

```bash
passwd xg
```

然后输入你的密码，他还会让你再输入一次以确认密码

还有很重要的一点 授予 `xg` 这个用户 `sudo` 权限

使用下面的指令打开 `/etc/sudoers`

```bash
vim /etc/sudoers
```

在最后加上 

```conf
xg ALL=(ALL) ALL
# xg换成你的用户的名字！！
```

之后使用 `wq!` 保存退出

#### 6.安装一些基础的包

```bash
pacman -S grub efibootmgr networkmanager network-manager-applet dialog wireless_tools os-prober mtools dosfstools ntfs-3g linux-headers reflector git sudo
```

**注意 : 上面的包务必安装（如果你不知道他们是干什么的话）并请再三检查是否有拼写错误**

当然你也可顺便安装一些其他的包 例如`htop`

等待安装完成后  还要安装的对应处理器（CPU）的微码包

如果你是Intel的cpu

```bash
pacman -S intel-ucode
```

如果是amd

```bash
pacman -S amd-ucode
```

#### 7.配置安装引导程序

`Grub 2.06` 更新后 `os-prober` 用户需要手动干预 一下下

我们需要打开`/etc/default/grub`

```vim
vim /etc/default/grub
```
取消最后一行 `GRUB_DISABLE_OS_PROBER=false` 前面的注释符号 `#`

如果没有的化 请添加至文件的最后一行

最后记得保存退出

下面正式开始配置安装引导程序

```bash
grub-install --efi-directory=/boot --bootloader-id=Arch
#上面最后面 Arch 可以自己起
```

然后是生成`grub.cfg`

```bash
grub-mkconfig -o /boot/grub/grub.cfg
```

![](https://raw.githubusercontents.com/LJYXG/image-host/main/img/Snipaste_2022-04-09_21-36-00.png)

### 八、安装桌面环境（KDE）

我是很喜欢的 KDE 的 所以这里以kde为例 其他的就需要你去翻找别的教程了！

> 这之前需要安装显卡驱动的 不过我是 Intel 的核显 属于即装即用的类型所以我就不需要装其他的驱动
>
> 如果是AMD集显/独显 NVDIA的独显 是需要的 具体的可以百度也可查看Arch Wiki

首先安安装 `Display Server` 采用的是开源世界最为流行的 `xorg`

安装指令如下

```bash
pacman -S xorg
```

再然后安装 `Display Manager` 我用 `KDE` 所以装 `sddm`

```bash
pacman -S sddm
#并设置开机自动启动
systemctl enable sddm
```

最后安装安装`Desktop Environment`  我用KDE 所以执行如下的指令

```bash
pacman -S plasma kde-applications packagekit-qt5
```

> tpis `kde-applications` 是一些kde桌面的基础软件包 你也可以只装一个终端 `konsole` 后面装好 `yay `后舒服的安装各种AUR源中的软件！

### 九、其他配置

添加 `archlinuxcn `  源 这里面有一些官方有但是没编译的软件 例如 yay 这个源里就有编译好的 也有一些国内没开源但是有Linux版本的软件 例如 WPS

使用vim打开`/etc/pacman.conf`

```bash
vim /etc/pacman.conf
```

在最后加上下面两行（我这里使用了北外的镜像站）

```conf
[archlinuxcn]
Server = https://mirrors.bfsu.edu.cn/archlinuxcn/$arch
#上面的源是北京外国语大学的镜像 也可以用其他镜像！
```

ps 也要同时取消对multilib源的注释

![](https://raw.githubusercontents.com/LJYXG/image-host/main/img/Snipaste_2022-04-09_22-00-12.png)

保存退出之后 需要同步并安装 `archlinuxcn-keyring`

```bash
pacman -Syu && pacman -S archlinuxcn-keyring
```

然后安装中文的字体，如果这一步不装进去图形界面之后还是要装的

```bash
pacman -S ttf-sarasa-gothic noto-fonts-cjk
#这里安装的是更纱黑体和noto cjk
```

上面的安装好后 配置一个开机启动项 `NetworkManager` 开机后方便网络链接！

```bash
systemctl enable NetworkManager
```

最后退出新系统 可以使用指令 `exit` 可以使用 `Ctrl + D`

然后键入下面的指令重启

```bash
reboot
```

## 进入新系统

在上面的重启完成就就可以看到Grub引导的选择了 

选择我们设置的第一个Arch 就可以进入了系统了

如下图

![](https://raw.githubusercontents.com/LJYXG/image-host/main/img/Snipaste_2022-04-09_22-32-05.png)

![](https://raw.githubusercontents.com/LJYXG/image-host/main/img/Snipaste_2022-04-09_22-06-17.png)

我们输入用户的密码（`xg`就是我的用户名）就可以进入系统了 

> 提示 这个时候小键盘是未启用的 需要开一下哦 一般来说 Linux开机默认都不是开的 你可以去学习如何开启

![](https://raw.githubusercontents.com/LJYXG/image-host/main/img/Snipaste_2022-04-09_22-10-20.png)

至此Arch Linux的安装就完整结束了  

后面你可以在设置中把语言改为 汉语

也可以安装 `yay` 体验 Arch Aur 爽感

至于汉语输入法 墙裂推荐知乎这篇文章！

[Arch Linux - 中文输入法](https://zhuanlan.zhihu.com/p/393746270)

最终至此 本文已经完全结束了 

不过还是有一点需要提示 **记得一周至少更新一下系统（pacman -Syyu）不然长时间不更新 然后突然一更新容易滚挂！！！！**

最后祝你能愉快的使用 Arch !!!!!