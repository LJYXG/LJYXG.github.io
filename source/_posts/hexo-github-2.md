---
title: Hexo+Github Pages 实现个人博客的小白教程 ——下篇
date: 2022-02-21 18:58:14
tags:
 - Hexo
 - Github
 - Blog
categories: Hexo
toc: true
---

最近有朋友在使用`Hexo`做自己博客，前几日我弄过`Hexo`的安装教程 

恰好我也对`Hexo`感兴趣 便与朋友共同研究下`Hexo`的使用！

由此便引出本篇文章！

<!-- more -->

**读前注意**

>本文章并不包含`Hexo`的安装教程（请见Hexo+Github Pages 实现个人博客的小白教程 ——上篇）
>
> 本人的部署平台为`Github`
>
>本文章使用主题以`Stun`为主（[链接地址](https://github.com/liuyib/hexo-theme-stun)） ，其他主题势必会有异同请自行辨别！
>
>本文要求您有一定的自我学习能力，毕竟这只是我的经验记录，若你怕麻烦 抱歉本文章不适合您
>
>最后温馨提示 本文完整仔细阅读可能需要20min左右，若您想实操 可能需要数小时！！！

## 一、简单的站点设置

### 1、站点的基本信息设置

>tips: 切记此处均为`YAML`语法，请务必注意缩进 以及记得`:`号后面 要有一个空格跟着！！！ 举例:`type: git`

打开位于博客所在位置根目录下的`_config.yml`配置文件进行如下设置

| 参数          | 介绍                                                         |
| :------------ | :----------------------------------------------------------- |
| `title`       | 网站标题                                                     |
| `subtitle`    | 网站副标题                                                   |
| `description` | 网站描述                                                     |
| `keywords`    | 网站的关键词。支持多个关键词。                               |
| `author`      | 作者名字                                                     |
| `language`    | 网站使用的语言。对于简体中文用户来说，使用不同的主题可能需要设置成不同的值，请参考你的主题的文档自行设置，常见的有 `zh-CN`和 `zh-HK`。 |
| `timezone`    | 网站时区。`Hexo` 默认使用您电脑的时区。请参考 [时区列表](https://en.wikipedia.org/wiki/List_of_tz_database_time_zones) 进行设置，如 `America/New_York`, `Japan`, 和 `UTC` 。一般的，对于中国大陆地区可以使用 `Asia/Shanghai`。 |
| `email`       | 你`Github`的邮箱                                             |

下图展示为我的配置情况！

![](/images/psg4/Snipaste_2022-02-18_18-28-15.png)

### 2、站点的部署地址设置


然后我们配置 `Git`自动部署插件 依旧是在根目录下的`_config.yml` 一般在最后面

|参数|介绍|
|---|---|
|`type`|此处为一条指令部署类型 一般为`git`|
|`repo`|远程仓库的地址，鉴于本鸽子添加了我的SSH公钥至`Github`仓库 可以以使用SSH地址以使用免密登录！举个例`git@github.com:UserName/UserName.github.io.git`|
|`branch`|分支，此处注意`Github`的默认分支不再是`master`已经改为了`main`|
|`message`|此为`git`提交到`Github`的附加信息！|

一个例子

```yaml
deploy:
  type: git
  repo: git@github.com/{yourname}/{yourname}.github.io.git
  branch: main
  message: "Update!!!"
```



面也贴一张我的配置图片

![](/images/psg4/Snipaste_2022-02-18_18-38-36.png)

## 二、主题设置与美化。

**注意**

>此处再次说明 鸽子用的是`Stun`主题（[链接地址](https://github.com/liuyib/hexo-theme-stun)），不同注意必有异同，请自行学习！！！
>
>不管是哪一种主题，都要认真仔细阅读，每个主题的文档！！！[此处](https://theme-stun.github.io/docs/zh-CN/guide/quick-start.html)为`Stun`的文档地址

### 1、安装主题

进入 `Hexo` 根目录，执行指令：

```bash
$ git clone https://github.com/liuyib/hexo-theme-stun.git themes/stun
```

该指令会将本仓库中的所有文件克隆下来，其中有很多文件仅用于项目开发，对于普通用户来说完全用不到。因此，如果你想仅克隆主题运行所必需的文件，请用下面的指令代替上面的指令：

```bash
$ git clone -b dist https://github.com/liuyib/hexo-theme-stun.git themes/stun
```
![](/images/psg4/Snipaste_2022-02-18_19-14-17.png)

注意：这样做不方便以后更新，请谨慎使用。

安装依赖 `hexo-renderer-pug`

进入 `Hexo` 根目录，执行指令：

```bash
$ npm install --save hexo-renderer-pug
```

![](/images/psg4/Snipaste_2022-02-18_19-16-45.png)

修改 `Hexo` 根目录下的 `_config.yml` 文件：

`theme: stun`



然后，启动 `Hexo` 服务器：
```bash
$ hexo clean && hexo s
```

结果

![](/images/psg4/Snipaste_2022-02-18_19-24-18.png)

### 2、美化主题

>tips: 主题定义具有高度自定义化！！！
>
>主题配置文件地址为`/thems/stun/_config.yml` 注意主题的配置文件名称同站点文件名一致切记不要弄混
>
#### 2.1改动站点图标以及作者信息更改！

##### 2.1.1站点图标的更改 

首先看主题手册的描述

```yaml
favicon:
  small: /assets/favicon-16x16.png                   # 16x16 像素大小的图片
  medium: /assets/favicon-32x32.png                  # 32x32 像素大小的图片
  # ! ----------------------------------------------------------------------
  # ! 下面的配置项默认没有启用，如果你不想麻烦，可以直接忽略。
  # ! 想要启用，取消注释即可，但启用前必须准备好相应的图片或配置文件。
  # ! ----------------------------------------------------------------------
  apple_touch_icon: /assets/favicon-180x180.png      # 180x180 像素大小的图片
  safari_pinned_tab:
    image: /assets/favicon.svg                       # SVG 格式的图片
    # 表示在 Mac OS 上的 Safari 浏览器中，固定的标签页被选中时，图标的颜色
    # 表示在 MacBook 上的 Touch Bar 中，图标的背景颜色
    color: "#54bcff"
  msapplication:
    image: /assets/favicon-144x144.png               # 144x144 像素大小的图片
    # 表示在 Windows 8/10 的开始菜单中，磁贴的背景颜色
    color: "#54bcff"
    config: /browserconfig.xml 

```

![](/images/psg4/Snipaste_2022-02-18_19-35-37.png)

我就偷懒了 两个都丢了同一个文件

>推荐的图片地址为`/source/images/`

我的设置

```yaml
favicon:
  small: /images/logo.png
  medium: /images/logo.png
  # ! --------------------------------------------------
  # ! If you don't know the following, please ignore it.
  # ! --------------------------------------------------
  # apple_touch_icon: /images/icons/apple-touch-icon.png
  # safari_pinned_tab: /images/icons/stun-logo.svg
  # msapplication: /images/icons/favicon-144x144.png

# Progressive Web Apps
# See: https://github.com/lavas-project/hexo-pwa/
```

![](/images/psg4/Snipaste_2022-02-18_19-41-54.png)

##### 2.1.2作者信息更改

首先看主题手册的描述
```yaml
author:
  enable: true
  # 侧边栏头像
  avatar:
    # 填写图片路径或链接
    url: https://xxxxx.png
    # 是否显示为圆形
    rounded: true
    # 头像透明度（取值：0 ~ 1）
    opacity: 1
    # 鼠标经过时的动画，可选值：turn 或 shake
    animation: turn
  # 格言（可以是任意一句想写的话）
  motto: hello world
```


然后是我的设置

```yaml
author:
  enable: true
  # Avatar in the site sidebar.
  avatar:
    # In theme directory (source/images): /images/avatar.png
    # In site directory (source/uploads): /uploads/avatar.png
    # You can also use a link of image.
    url: /images/logo.png
    # If true, the avatar would be displayed in a circle.
    rounded: true
    # Opacity of avatar (value: 0 ~ 1)
    opacity: 1
    # Mouse hover animation
    # Optional values: turn | shake
    animation: turn
  # Your favorite motto.
  motto: Being Towards Death
```


改动完毕的效果展示（所用指令依旧是`hexo clean && hexo s`）

![](/images/psg4/Snipaste_2022-02-18_19-58-17.png)

#### 2.2改动站点背景以及增添多个独立页面！

首先查看下 需求

![](/images/psg4/Snipaste_2022-02-18_19-59-47.png)

其次 咱们还是看下 官方文档提供的信息 



```yaml
#网站主体设置 Beta v1.7.0
#修改主题配置文件：
  bg_image:
    enable: false
    # 填写图片路径或链接
    url: https://xxxxx.png
    # 是否固定背景图片（相当于设置 CSS 属性 position: fixed）
    fixed: true
    # 是否重复显示背景图片（相当于设置 CSS 属性 background-repeat: repeat/no-repeat）
    repeat: false
  # 网站主体背景图片的遮罩效果
  mask:
    enable: false
    # 透明度（取值：0 ~ 1）
    opacity:
      # 默认情况下，网站主体背景图片的透明度
      default: 0.1
      # 夜晚模式下，网站主体背景图片的透明度
      night_mode: 0.6
```

然后是我的设置， 

> ps 文件依旧是丢在了`/source/images/`

```yaml
header:
  enable: false
  show_on:
    # Whether to show on the article page.
    post: true
  # If you set a percentage, it will be calculated based on the height of screen (Supports all CSS units)
  height: 80%
  # Background image of site header.
  bg_image:
    enable: false
    # In theme directory (source/images): /images/avatar.png
    # In site directory (source/uploads): /uploads/avatar.png
    # You can also use a link of image.
    url:
  # Mask effect of the background image.
  mask:
    enable: false
    # Opacity of the mask (value: 0 ~ 1)
    opacity: 0.5
  nav:
    # Height of the navigation bar (Supports all CSS units)
    height: 50px
    # Background color of the navigation bar (Please use "" to wrap the value).
    bg_color: "#333"
  # The icon that prompts the user to pull down.
  scroll_down_icon:
    enable: false
    # Icon name in FontAwesome, see: https://fontawesome.com/icons
    name: fas fa-angle-down
    animation: true

# The body of site.
body:
  # Background image of site body.
  bg_image:
    enable: true
    # In theme directory (source/images): /images/avatar.png
    # In site directory (source/uploads): /uploads/avatar.png
    # You can also use a link of image.
    url: /images/bg1.png
    # Whether to fixed the background image.
    fixed: true
    # Whether to repeat the image as much as possible to cover the entire area.
    repeat: false
  # Mask effect of the background image.
  mask:
    enable: false
    # Opacity of mask (value: 0 ~ 1)
    opacity:
      # Opacity of the mask by default.
      default: 0.1
      # Opacity of the mask in night mode.
      night_mode: 0.6

```

按照惯例 我们依旧是预览一下我们设置的效果！（所用指令依旧是`hexo clean && hexo s`）

![](/images/psg4/Snipaste_2022-02-18_20-17-59.png)

下面就是增加独立页面了 在此处 我们会增加三个独立页面，两个为存档里推荐的，还有一个是我们等下要介绍的友链

下面我们贴出来 官方文档对此的介绍,大家完全可以照着去做！！

在 `Hexo` 根目录下：

```bash
# 启用分类页，执行这条指令
$ hexo new page categories

# 启用标签页，执行这条指令
$ hexo new page tags
```

修改 Front-Matter
在 `Hexo `根目录下，找到 source/categories 或 source/tags 文件夹中的 Markdown 文件，设置 `Front-Matter`：

```txt
---
# 如果是分类页，设置这个
type: "categories"

# 如果是标签页，设置这个
type: "tags"
---
```

修改主题配置文件

将 categories 或 tags 对应项取消注释：

```yaml
# `||` 分隔符之前是页面路径，`||` 分隔符之后是图标
# 用法（有图标）: `Key: /路径/ || fa(s|r|l|d|b) fa-图标名称`
# 用法（无图标）: `Key: /路径/`
# fas far fal fad fab 是 FontAwesome 5.x 中图标的前缀，你需要根据具体图标自行设置
# 查找图标名称，请访问：https://fontawesome.com/icons
menu:
  home: / || fas fa-home
  archives: /archives/ || fas fa-folder-open
  categories: /categories/ || fas fa-layer-group
  tags: /tags/ || fas fa-tags

```

**此处开始 就是我们要关注的 增加一个独立页面的教程**

如果你想添加自定义页面，需要执行以下步骤：

以添加友链页面为例。

修改主题配置文件

```yaml
#注意此处我修改了 直接将目标改成了 咱们的需求友情链接
menu:
  friendlink: /friendlink/ || fas fa-users
```
生成页面文件

在 `Hexo` 根目录下执行指令：

```bash
# 这里的 friendlink 对应上一步你设置的路径名称
$ hexo new page friendlink
```

国际化设置
>这个是很重要的 直接关系了 你新增的页面是否会正确显示你要表的意思

找到 `languages` 目录下的语言文件，根据你网站使用的语言选择对应的语言文件，例如 `zh-CN.yml`

```yaml
menu:
  friendlink: 友链
```

接下来依旧是展示更改后的效果 指令依旧是`hexo clean && hexo s`

![](/images/psg4/Snipaste_2022-02-18_20-37-20.png)

#### 2.3设置社交链接并引入阿里的图标库！

社交链接 依旧首先引入主题作者的官方文档

```yaml
# `||` 分隔符之前是具体链接，`||` 分隔符之后是图标。
# 用法（有图标）: `Key: /路径/ || fa(s|r|l|d|b) fa-图标名称`
# 用法（无图标）: `Key: /路径/`
# fas far fal fad fab 是 FontAwesome 5.x 中图标的前缀，你需要根据具体图标自行设置。
# 查找图标名称，请访问：https://fontawesome.com/icons
#
# 如果你找不到想要的图标，可以考虑用文字来代替图标显示，用法：
# 通过添加 `origin:` 前缀即可显示文字。例如：`origin:知` 会以 `知` 代替图标显示。
social:
  github: https://github.com/ || fab fa-github
  google: https://plus.google.com/ || fab fa-google
  twitter: https://twitter.com/ || fab fa-twitter
  youtube: https://youtube.com/ || fab fa-youtube
  # segmentfault: https://segmentfault.com/ || origin:sf
  # weibo: https://weibo.com/ || fab fa-weibo
  # zhihu: https://www.zhihu.com/ || origin:知
  # wechat: yournumber || fab fa-weixin
  # telegram: yournumber || fab fa-telegram
  # qq: yournumber || fab fa-qq

# 社交链接的设置
social_setting:
  # 是否启用社交链接
  enable: true
  # 是否只显示图标
  icon_only: true
```

此处我们举例说明一个特殊的例子

例如我们要引入哔哩哔哩的社交链接 很明显列表中并不含还有哔哩哔哩的情况 对于这种情况需要进行国际化设置。

步骤如下：

修改主题配置文件

```yaml
social:
  bilibili: https://bilibili.com/ || fab fa-bilibili
```
>此处留一个伏笔 哔哩哔哩的图标 我按照文档确实添加了 但是 却没起作用 他的解决方案 下面会放出 2333

国际化设置

找到 `languages` 目录下的语言文件，根据你网站使用的语言选择对应的语言文件，例如：

`zh-CN.yml`:

```yaml
social:
  bilibili: 哔哩哔哩
```
这里的国际化设置，对应于鼠标经过图标时，显示的文字。

依旧是成果展示

![](/images/psg4/Snipaste_2022-02-18_20-55-44.png)

下面 我们解决为什么 我们在那个网站设置了 图标 但是却没有显示的问题

首先 这个主题的图标支持的是 `FontAwesome 5.x `但是 哔哩哔哩的图标在这个库里属于`FontAwesome 6.0` 

针对此种情况我们采用`MSDN`的想法 通过`CSS`引入阿里的`iconfont`图标库

> 此处采用`DreamyTZK`的[Hexo博客之优雅使用阿里iconfont图标](https://blog.csdn.net/u012208219/article/details/106883012)
>
>上述文章的内容乍一看并不适用于本主题，但是经过本鸽子实践，本主题是可以用的！
>
>**这再次说明 教程都是单一方面的极致刨析 大家需要会举一反三！！！！**

首先建立项目 即 在 阿里的iconfont选择适合的图标 然后添加到项目 （需要登录）

![](/images/psg4/Snipaste_2022-02-19_18-56-36.png)

点击右上角购物车 然后新建项目 并将选中的图标 添加至项目

<img src="/images/psg4/Snipaste_2022-02-19_18-58-06.png" style="zoom:50%;" />


![](/images/psg4/Snipaste_2022-02-19_18-59-23.png)

如果你是新建项目一般来说 会与我有些许不同 不过 只要创建好后 打开即可

![](/images/psg4/Snipaste_2022-02-19_19-01-39.png)

会得到 如下的CSS文件 

![](/images/psg4/Snipaste_2022-02-19_19-02-51.png)

将此CSS文件复制 等下备用！！

下面我们依旧是查看一下`Stun`这个主题官方文档 是如何介绍引入自定义主题的！！

```markdown
# 自定义样式 Stable v1.0.3
如果你想修改主题的样式，推荐将样式代码添加到 source/css/_custom 目录下的 index.styl 文件中。这样，当主题更新时，不会覆盖你已经修改了的样式代码。

当然，你也可以进行模块化分类：在该目录下新建样式文件，然后通过 @import xxx 语句在同目录下的 index.styl 文件中引入你新建的样式文件。
```

所以我们先打开目录 `themes/stun/source/css/_custom/`

并在其目录下新建`iconfont.styl`

![](/images/psg4/Snipaste_2022-02-19_19-08-25.png)

> 至于如何 新建此文件 简述说明 新建一个文本文件 `.txt` 结尾的 然后变更后缀名
> 
>然后用`VS Code`(可以是其他的编辑器)打开此文件 写入网页上的CSS文件

然后打开上图同目录下的 `index.styl` 并在其结尾 写入如下内容

```txt
@import './iconfont.styl'
```

好了至此已经是成功引入了 阿里的`iconfont`图标 

> tips: 这个引入的CSS只有你在官网添加的图标 如果新增了图标 就要更新代码 并用新的CSS代码 替代 `iconfont.styl` 里旧的CSS代码！！！

回到阿里的`iconfont`图标网站 我们要获取我们新增的图标的代码 

![](/images/psg4/Snipaste_2022-02-19_19-18-08.png)

内容如下  
```css
icon-bilibili
```

下面我们将之前社交链接的`bilibili`图标替换为咱们新设置的 

```yaml
social:
  bilibili: https://bilibili.com/ || iconfont icon-bilibili
```

接下来 我门依旧按照传统渲染 查看效果！指令依旧为之前的指令 

![](/images/psg4/Snipaste_2022-02-19_19-22-03.png)

如图所示 我们已经成功引入阿里的`iconfont`图标 

#### 2.4在设置友链页面！

好的 下面我们正式开始友链的设置

>温馨提示 这又是一个较为困难的部分！！！
>
>本主题作者貌似也未提供 友链的样式 所以我们依旧采用一种 骚操作
>
>此处采用知乎作者`finisky`的[最简单的Hexo友情链接页面定制](https://zhuanlan.zhihu.com/p/384472746)
>
>在`Markdown`引入Html语法 实现友链 详细内容请见原文链接 本处仅作简单使用的介绍！

首先打开`/source/friendlink/index.md`

并在其中添加如下内容（以下内容为原作者内容

```Markdown
---
title: 友情链接
date: 2021-06-16 00:34:27
---

<div class="post-body">
   <div id="links">
      <style>
         .links-content{
         margin-top:1rem;
         }
         .link-navigation::after {
         content: " ";
         display: block;
         clear: both;
         }
         .card {
         width: 45%;
         font-size: 1rem;
         padding: 10px 20px;
         border-radius: 4px;
         transition-duration: 0.15s;
         margin-bottom: 1rem;
         display:flex;
         }
         .card:nth-child(odd) {
         float: left;
         }
         .card:nth-child(even) {
         float: right;
         }
         .card:hover {
         transform: scale(1.1);
         box-shadow: 0 2px 6px 0 rgba(0, 0, 0, 0.12), 0 0 6px 0 rgba(0, 0, 0, 0.04);
         }
         .card a {
         border:none;
         }
         .card .ava {
         width: 3rem!important;
         height: 3rem!important;
         margin:0!important;
         margin-right: 1em!important;
         border-radius:4px;
         }
         .card .card-header {
         font-style: italic;
         overflow: hidden;
         width: 100%;
         }
         .card .card-header a {
         font-style: normal;
         color: #2bbc8a;
         font-weight: bold;
         text-decoration: none;
         }
         .card .card-header a:hover {
         color: #d480aa;
         text-decoration: none;
         }
         .card .card-header .info {
         font-style:normal;
         color:#a3a3a3;
         font-size:14px;
         min-width: 0;
         overflow: hidden;
         white-space: nowrap;
         }
      </style>
      <div class="links-content">
         <div class="link-navigation">
            <div class="card">
               <img class="ava" src="https://cdn.jsdelivr.net/gh/hvnobug/assets/common/avatar.png" />
               <div class="card-header">
                  <div>
                     <a href="https://blog.hvnobug.com/">Emil’s blog</a>
                  </div>
                  <div class="info">这是一个分享IT技术的小站。</div>
               </div>
            </div>
            <div class="card">
               <img class="ava" src="https://yingwiki.top/avatar" />
               <div class="card-header">
                  <div>
                     <a href="https://yingwiki.top">越行勤's Blog</a>
                  </div>
                  <div class="info">努力学习的小菜鸟</div>
               </div>
            </div>
         </div>
      </div>
   </div>
</div>
```

由上面可知以后每增加一个友链 只要增加一个下面的代码段即可！

```html
<div class="card">
   <img class="ava" src="{avatarurl}" />
   <div class="card-header">
      <div>
         <a href="{link}">{name}</a>
      </div>
      <div class="info">{description}</div>
   </div>
</div>
```
这个是上面变量对应的解释
|变量|介绍|
|----|---|
|`{avatarurl}`|朋友站点的头像`url`地址|
|`{link}`|朋友站点的链接|
|`{name}`|朋友站点的名字|
|`{description}`|朋友站点的介绍|


下面贴上我的配置
```markdown
---
title: friendlink
date: 2022-02-18 20:36:28
---

# 熙光鸽子的友情链接


<div class="post-body">
   <div id="links">
      <style>
         .links-content{
         margin-top:1rem;
         }
         .link-navigation::after {
         content: " ";
         display: block;
         clear: both;
         }
         .card {
         width: 45%;
         font-size: 1rem;
         padding: 10px 20px;
         border-radius: 4px;
         transition-duration: 0.15s;
         margin-bottom: 1rem;
         display:flex;
         }
         .card:nth-child(odd) {
         float: left;
         }
         .card:nth-child(even) {
         float: right;
         }
         .card:hover {
         transform: scale(1.1);
         box-shadow: 0 2px 6px 0 rgba(0, 0, 0, 0.12), 0 0 6px 0 rgba(0, 0, 0, 0.04);
         }
         .card a {
         border:none;
         }
         .card .ava {
         width: 3rem!important;
         height: 3rem!important;
         margin:0!important;
         margin-right: 1em!important;
         border-radius:4px;
         }
         .card .card-header {
         font-style: italic;
         overflow: hidden;
         width: 100%;
         }
         .card .card-header a {
         font-style: normal;
         color: #2bbc8a;
         font-weight: bold;
         text-decoration: none;
         }
         .card .card-header a:hover {
         color: #d480aa;
         text-decoration: none;
         }
         .card .card-header .info {
         font-style:normal;
         color:#a3a3a3;
         font-size:14px;
         min-width: 0;
         overflow: hidden;
         white-space: nowrap;
         }
      </style>
      <div class="links-content">
         <div class="link-navigation">
            <div class="card">
               <img class="ava" src="http://www.zjhcofi.com/img/avatar.png" />
               <div class="card-header">
                  <div>
                     <a href="http://www.zjhcofi.com/">ZJHCOFI’s blog</a>
                  </div>
                  <div class="info">这是一个大佬的网站。</div>
               </div>
            </div>
            <div class="card">
               <img class="ava" src="https://gravatar.loli.net/avatar/4cc893d113dd74ceca73f9863f2c5446" />
               <div class="card-header">
                  <div>
                     <a href="https://unce.top">云酱的小站</a>
                  </div>
                  <div class="info">超可爱的正太！</div>
               </div>
            </div>
            <div class="card">
               <img class="ava" src="https://unica-nya.github.io/images/icons/unica-32x32.ico" />
               <div class="card-header">
                  <div>
                     <a href="https://unica-nya.github.io/">云酱的小窝</a>
                  </div>
                  <div class="info">还是辣个正太！</div>
               </div>
            </div>
         </div>
      </div>
   </div>
</div>

```

按照惯例 依旧是 po出一张结果图 （指令与之前一致！

![](/images/psg4/Snipaste_2022-02-19_20-56-01.png)

## 三、撰写发布新文章。

>对于此部分 熙熙不会说太多 推荐详读 官方的文档 
>[官方文档](https://hexo.io/zh-cn/docs/writing)
>对于写作所需的`Markdown`语法熙熙则推荐这篇文章！[Markdown 基本语法](https://www.markdown.xyz/basic-syntax/
)


### 1、新建文章

你可以执行下列命令来创建一篇新文章或者新的页面。(官方文档摘录！)

```bash
$ hexo new [layout] <title>
```
您可以在命令中指定文章的布局（`layout`），默认为` post`，可以通过修改 `_config`.yml 中的 `default_layout` 参数来指定默认布局。

布局（Layout）
`Hexo` 有三种默认布局：`post`、`page` 和 `draft`。在创建这三种不同类型的文件时，它们将会被保存到不同的路径；而您自定义的其他布局和 `post` 相同，都将储存到 `source/_posts` 文件夹。

|布局|路径|
|---|---|
|post|source/_posts
|page|source
|draft|source/_drafts

同时也可以设置额外的参数

|参数|描述|
|---|---|
|`-p, --path`|自定义新文章的路径|
|`-r, --replace`|如果存在同名文章，将其替换|
|`-s, --slug`|文章的 Slug，作为新文章的文件名和发布后的 URL|

下面我们举一个实际的例子

假设熙熙要写一篇文章叫做 在Ubuntu上安装Nginx 想要指定文档文件名发布后的路径为 `ubuntu-nginx` 布局为 `post`

则指令应该为

```bash
$ hexo new post "在Ubuntu上安装Nginx" -s ubuntu-nginx
#注意 因为layout的默认值为 post 所以可以精简为
$ hexo new "在Ubuntu上安装Nginx" -s ubuntu-nginx
```

### 2、撰写文章

撰写文章主要是打开原`Markdown`文件,并在文件中用Markdown语法撰写文章内容

我们依旧延续上面的例子 打开`/source/_posts/ubuntu-nginx.md`

#### 2.1 设置`Front-Matter`

打开文章的第一步便是设置该文章的 `Front-Matter`

打开后可以清晰的看见 `Hexo` 程序已经生成好了 一些必需的 `Front-Matter` 

>强烈建议你 观看官方的文档 [文章链接](https://hexo.io/zh-cn/docs/front-matter)
>同时也推荐您详细阅读`Stun`的文档，以获得该主题独有的
>本文仅简单介绍常用的`Front-Matter` 

|参数|功能|介绍|
|---|----|---|
|`title`|标题|该文章的标题 一般已经由`Hexo`自动填写好|
|`date`|建立时间|该文章的创建时间 一般已经y由`Hexo`自动填写好|
|`tags`|标签|该文档包含那些标签|
|`categories`|分类|该文章的分类（ps 若为多级分类 请务必参看官方文档！）|
|`toc`|是否开始目录|是否开启文章目录 应填布尔值|
|`comments`|是否开启评论|是否开始目录 同为布尔值|

下面我们展示一下一个新建的文章的Markdown文件中的默认 `Front-Matter` 

```txt
---
title: 在Ubuntu上安装Nginx
date: 2022-02-20 13:32:25
---
```
下面我们按照我的需求加上其他的
例如 分类为Web 标签为 Ubuntu Nginx 开启文章目录 不开启评论

```txt
---
title: 在Ubuntu上安装Nginx
date: 2022-02-20 13:32:25
categories: 
 - Web
tags: 
 - Ubuntu
 - Nginx
toc: true
comments: false
---
```

#### 2.2 文章正文的撰写

正文的撰写 在 `Front-Matter`的下方 

按照`Markdown`语法撰写即可 很简单 并不困难

对于撰写的编辑器 我推荐一下两个 当然其他的可是可以的 

第一个 当属地表最强编辑器 `VS Code` 配合一款叫做 `Markdown All in One`的插件即可

第二个 则是 `Typora` ,一款国人开发的所见即所得的`Markdown`编辑器,不过有一个小小的缺点，此软件自从正式版开始是一款付费软件 价格为$14,99 三台电脑授权 ，对于手头宽松的可以考虑 ，不太富裕的还是推荐`VS Code`或者—>（当然若您想体验一下的化 可以使用测试版 作者并未关闭测试版的下载及使用权 不过需要您自己在官网探索 本文并不会贴出地址~


文章写完 保存完毕后在博客根目录下执行下面的指令便可生成本地静态文件 并运行测试服务器 

```bash
$ hexo g && hexo s 
```

确认可以后 便可在博客根目录下使用如下命令部署到您的`Github Pages`（此处需要您在安装时 添加了您的SSH公钥至 `Github` 并且 正确按照本文配置！

```bash
$ hexo d
```

完成后 您大概需要等待3min左右 便可访问网址观看您的成果

### 3、删除文章

删除文章也很简单 您只需进入到文章的根目录 删除所需要删除的以.md结尾的源文件即可 

删除后 执行下面的指令 重新生成本地文件并部署即可

```bash
$ hexo g && hexo d
```

若上面的不起作用 可以尝试下面的指令

```bash
$ hexo clean && hexo g && hexo s 
```

## 四、后记

至此 本教程已经完全完毕！！

若您想丰富您的技巧 以及使用能力 或者扩展您的博客 进一步美化您的博客 

那么请仔细阅读 `Hexo`文档 以及主题使用文档 这些都是不可或缺的一环 

祝您 在日后的使用`Hexo`的生活中，能愉快的记录您的每一次开心的瞬间，以及对新知识掌握喜悦

> **附文档链接**
>
> `Hexo`文档: [点我！！！](https://hexo.io/zh-cn/docs/)
>
> `Stun`主题使用文档：[点我~ ~ ~](https://theme-stun.github.io/docs/zh-CN/)
