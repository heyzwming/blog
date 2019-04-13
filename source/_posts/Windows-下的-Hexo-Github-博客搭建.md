---
title: Windows 下的 Hexo + Github 博客搭建
date: 2019-03-16 13:07:18
tags: shares
---

# 前置准备

想要在github上搭建一个以hexo为框架的博客预先需要安装git和node.js，安装git是因为我们需要用git来把本地用markdown写的博客push到github上，node.js是一个JavaScript的框架，而Hexo博客框架是基于node.js框架开发的，没有nodejs框架支持无法正常下载使用Hexo博客框架。

## git安装

[git主页](https://git-scm.com/)

[git下载](https://git-scm.com/download/win)

因为git的主页的服务器在国外，没有科学上网的话git的下载可能会比较慢。

这里提供了一个[国内git for windows 镜像](https://github.com/waylau/git-for-win)

![git官网页面](/assets/blogImg/git官网页面.jpg)

点击`Download for windows`下载安装，具体不在本篇博客赘述。

## node.js安装

[安装nodejs](https://nodejs.org/en/)

![nodejs官网页面](/assets/blogImg/nodejs官网页面.jpg)

点击`XX.XX.X LTS` 开始下载（我当前的版本是10.15.3）。

> Tips: LTS的是Long Time Support长期支持版本的缩写，指的是该版本的nodejs会得到长期的支持和维护。

下载后一直点`next`默认设置到安装完毕。

## 检查安装情况

在nodejs安装完成后会有以下两个内容：
nodejs + npm 包管理器

### git

现在来检查以下安装的情况，在windows下按`win+R`键输入`cmd`打开windows的终端命令行界面，输入

> `$ git --version`

就可以看到当前安装的git的版本git version X.XX.X.windows.X，如果出现其他类似于命令不存在则可能是安装出错。

> Tips:一般博客或者官方文档中，当出现需要在命令行中输入命令时，通常会有个‘ $ ’符号，‘ $ ’符号是输入命令的提示符，它不是命令本身的一部分，在windows下是‘ > ’符号。也就是说在博客中出现的‘ $ ’是提示你‘ $ ’符号后面的是一个命令，输入命令的时候不需要加上开头的‘ $ ’符号。

> `$ git --help`

可以看到git的一些命令的帮助信息。

> Tips: 在linux发行版/mac OS下可以先进入进入管理员root用户，在命令行终端输入 `$ sudo su` 并输入用户密码获得管理员权限。


git的具体使用不在本博客赘述，可以参考git的官方的[Pro GitBook](https://git-scm.com/book/zh/v2)。

### nodejs

在命令行中输入

> `$ node -v`

可以看到当前的nodejs的版本v XX.XX.X则为成功安装。

如果出现 `'nodejs' 不是内部或外部命令，也不是可运行的程序
或批处理文件。` 则可以需要重新安装nodejs。

### npm

同样在命令行中输入

> `$ npm -v`

可以看到npm的版本X.X.X即成功安装。


# 安装Hexo

接下来可以安装hexo博客框架了！

> `$ npm install -g hexo-cli`

桥豆麻袋！！因为国内镜像源比较慢，我们先用npm安装一个cnpm （一个淘宝的镜像源）

> `$ npm install -g cnpm --registry=https://registry.npm.taobao.org`

查看cnpm版本  
> `$ cnpm -v`

终于可以安装Hexo框架了！

### 安装Hexo

> `$ cnpm install -g hexo-cli`

> cli：命令行接口                                                
> -g: 全局安装 

现在可以输入以下命令查看安装的Hexo的版本。

> `$ hexo -v`

> hexo-cli: 1.1.0                 
> os: Windows_NT 10.0.17134 win32 x64                     
> http_parser: 2.8.0          
> node: 10.15.3                   
> v8: 6.8.275.32-node.51                    
> uv: 1.23.2                        
> zlib: 1.2.11              
> ares: 1.15.0                      
> modules: 64                 
> nghttp2: 1.34.0                 
> napi: 3                       
> openssl: 1.1.0j     
> icu: 62.1                     
> unicode: 11.0                 
> cldr: 33.1                    
> tz: **2018e**                   

# 开始建站

> 可以参考[Hexo官方文档](https://hexo.io/zh-cn/docs/)

在本地你想要放置Hexo博客相关文件的地方右键 `Git Bash Here` （前提是安装好了Git并勾选了相应选项）打开Git Bash命令行界面。

> 说明：这里把cmd命令行界面换成Git Bash命令行界面是因为前面的安装操作不需要与路径相关，在接下来的步骤中使用cmd + cd 指令找到想要的布置博客文件夹路径对新手有点麻烦，所以直接在windows的图形界面中找到要放置文件夹的目录，再右键使用Git Bash命令行界面就能直接将Git Bash命令行的路径对应到当前文件夹下，而且在Git Bash下可以使用一些windows中不能用的Linux命令 如 `$ ls` 当前目录下的所有文件、`$ pwd`  当前路径等等。

当Bash中出现命令提示符‘ $ ’输入命令

> `$ hexo init <folder>`

> `$ cd <folder>`

> Tips: < > 为可选参数，对应着新建一个文件夹`folder`并在该文件夹下生成并初始化博客文件，如果不填则在当前目录下生成网站并初始化。

> Tips: cd 为Linux命令，意为切换当前工作目录至 < folder > 文件夹下。可以切换到当前文件夹中的某个子文件夹下，也可以切换至任意目录（需要完整路径）。 `$ cd ..` 为回退到父文件夹。

初始化你的博客文件夹并生成一个新的博客网站。

> 在命令行下，使用tab键可以自动补全目录。

![hexo init](/assets/blogImg/hexo-init.jpg)

> INFO  Start blogging with Hexo!

hexo 会在博客文件夹下生成一些内容

```
.
├── _config.yml
├── package.json
├── scaffolds
├── source
|   ├── _drafts
|   └── _posts
└── themes
```

#### _config.yml

Hexo博客框架的配置文件，您可以在此配置大部分的参数。

#### package.json

应用程序的信息。EJS, Stylus 和 Markdown renderer 已默认安装，您可以自由移除。

```json
package.json

{
  "name": "hexo-site",
  "version": "0.0.0",
  "private": true,
  "hexo": {
    "version": ""
  },
  "dependencies": {
    "hexo": "^3.0.0",
    "hexo-generator-archive": "^0.1.0",
    "hexo-generator-category": "^0.1.0",
    "hexo-generator-index": "^0.1.0",
    "hexo-generator-tag": "^0.1.0",
    "hexo-renderer-ejs": "^0.1.0",
    "hexo-renderer-stylus": "^0.2.0",
    "hexo-renderer-marked": "^0.2.4",
    "hexo-server": "^0.1.2"
  }
}
```

#### scaffolds

模版 文件夹。当您新建文章时，Hexo 会根据 scaffold 来建立文件。

Hexo的模板是指在新建的markdown文件中默认填充的内容。例如，如果您修改scaffold/post.md中的Front-matter内容，那么每次新建一篇文章时都会包含这个修改。

#### source

资源文件夹是存放用户资源的地方。除 _posts 文件夹之外，开头命名为 _ (下划线)的文件 / 文件夹和隐藏的文件将会被忽略。

当在命令行中输入`Hexo g`对.md文件和新建的博客文件进行生成的时候，会把这个文件夹下的Markdown和HTML文件解析并放到新生成的 public 文件夹，而图片（头像图片、照片等等）和一些资源文件等其他文件会被拷贝过去。

#### themes

主题 文件夹。Hexo 会根据主题来生成静态页面。

有关在主题文件夹下的_config.yml中部分外链资源的配置：以添加头像图片为例。

将头像图片pic.jpg放置于 `/source/assets/img/` 下并在themes/_config.yml中的相应的位置配置图片的地址`/source/assets/img/pic.jpg`

新建一篇文章

`$ hexo new <layout> title`

如果没有设置`layout`的话，默认使用`_config.yml`中的`default_layout`参数代替，如果标题包含空格的话请用引号括起来。

此时会在`/source/_post/`文件夹下生成一个以`title.md`的Markdown文件。

`$ cd /source/_post/ `

进入该文件夹，可以用vim或者其他Markdown编辑器打开并写博客。

`$ vim title.md`

完成并保存后输入

`$ cd ../..`

回到开始的文件夹中

本地启动预览博客
> `$ hexo server`

> Tips: 本地localhost:4000是用来做预览测试的。

可以使用缩写s来简化命令

> `$ hexo s`

![hexo server](/assets/blogImg/hexo-server.jpg)

> `Ctrl` + `c` 停止预览

接下来就可以在浏览器中的地址访问`localhost:4000`地址来预览hexo生成的模板博客/你自己写的博客了！

### 新建文章

输入以下命令即可以新建一篇博客：

> `$ hexo new "my new blog"`

> INFO  Created: ~\Desktop\blog1\source\_posts\my-new-blog.md

进入生成的博客的文件夹下

> `cd source/_posts/`

生成的博客是markdown格式的，可以看到文件下有个.md文件。

接着在.md文件中用Markdown语法书写博客并保存。

退回到blog文件夹下

> `$ cd ../..`

生成博客的.html和.js文件

> `$ hexo generate`

或者缩写

> `$ hexo g`

![hexo-g](/assets/blogImg/hexo-g.jpg)

# 博客部署

如果没有资金购买服务器和域名可以将博客部署到免费的[github](https://github.com/)上公开使用

注册并进入github主页。

新建仓库

点击`Start project` 或者 `new`一个新的repository仓库

![new-repo2](/assets/blogImg/new-repo2.jpg)

![new-repo1](/assets/blogImg/new-repo1.jpg)

用户部署个人博客的github仓库的命名必须符合特定要求：github账户名.github.io
假设我的github叫 `siriusGo`
那我的仓库名字要设为 `siriusGo.github.io `

![repo3](/assets/blogImg/new-repo3.jpg)

`Description (optional)` 描述随便写。点击`Create`即可。

### git部署插件

安装git部署插件，在命令行下输入：

> `$ cnpm install --save hexo-deployer-git`

有个warning 忽略。

配置blog文件夹下的`_config.yml`文件

在文件最后的Deployment的下面

修改为

    deploy: 
      type: git
      repo: 你的仓库的地址.git
      branch: master

注意冒号后面有一个空格

### 部署到github

部署到github主页命令：

> `$ hexo -deploy`

或

> `$ hexo -d`

输入github账号和密码
如果你的github加了SSH就不用输密码

现在就可以用 “ 你的github名字.github.io ” 来访问你的博客了！

### 更换主题

下载相应的主题

> `$ git clone <主题的github下载连接>`

配置_config.yml文件

找到theme选项，把后面的主题名字改成你要的主题的名字

重新生成博客并部署到github上

> `$ hexo g`


> `$ hexo d`

举个一个受到广泛喜爱的hexo主题：[yilia](https://github.com/litten/hexo-theme-yilia)的栗子。

安装

> `$ git clone https://github.com/litten/hexo-theme-yilia.git themes/yilia`

配置

修改hexo根目录下的 `_config.yml` ： `theme: yilia`

更新

> `$ cd themes/yilia`
> 
> `$ git pull`

该主题相关的配置文件在主题theme/yilia下的_config.yml，可以根据自己需要修改使用。

另外还有一个命令可以稍微了解一下，即

`$ hexo clean`

这是一个清楚缓存文件(`db.json`) 和已生成的静态文件(`public`)的命令。在某些情况（尤其是更换主题后），如果发现您对站点的更改无论如何也不生效您可能需要运行该命令。

更多的命令和内容可以参考[Hexo的主页](hexo.io)