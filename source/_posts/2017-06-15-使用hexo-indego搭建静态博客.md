---
title: 使用hexo+indego搭建静态博客
categories:
  - 笔记随笔
date: 2017-06-15 17:55:33
tags:
---
# 前言
[Github ](https://github.com/)为广大开发者提供了一个非常好的平台，不仅是代码的开源，同时[ Github ](https://github.com/)还提供了开发者可以在[ Github ](https://github.com/)上建立自己的站点（GithubPage）的一个非常有意思的功能。这个功能的局限是只能创建静态的网站，那么我们可以使用一些工具来快速创建这一网站。
本文旨在帮助刚接触[ Github ](https://github.com/)新手，想利用[ Github ](https://github.com/)来创建自己的站点、个人博客等。大神可以忽视__(:з」∠)__。

# 准备
你需要在[ Github ](https://github.com/)上创建一个属于自己的账户，然后新建一个仓库（`new repository`），并命名为 `YourSiteName.github.io/com`，此时[ Github ](https://github.com/)会帮助你初始化一个静态网页，你可以根据自己的喜好选择一些模版（~~这都不是重点~~），接着尝试访问下你所建的站点，成功后就可以开始动工了。

# 关于 Hexo
* **A fast, simple & powerful blog framework**
* **快速，简单而高效的静态博客框架**
 * **超快速度：** Node.js 所带来的超快生成速度，让上百个页面在几秒内瞬间完成渲染。
 * **支持 Markdown：** Hexo 支持 GitHub Flavored Markdown 的所有功能，甚至可以整合 Octopress 的大多数插件。
 * **一键部署：** 只需一条指令即可部署到 GitHub Pages, Heroku 或其他网站。
 * **丰富的插件：** Hexo 拥有强大的插件系统，安装插件可以让 Hexo 支持 Jade, CoffeeScript。

# 关于indego
{% asset_img indego.jpg %}


<!-- more -->

# 正题
上面是对搭建博客的一些技术了解，接下来进入正题。

## Hexo 初始化博客框架
### 1. 安装 Hexo
Hexo 安装和搭建依赖[ Nodejs ](https://nodejs.org/en/)和[ Git ](http://git-scm.com/)，可自行下载。
```
$ npm install -g hexo-cli
```

### 2. 初始化框架
```
$ hexo init <yourFolder>
$ cd <yourFolder>
$ npm install
```
 初始化完成大概的目录：
```
.
├── _config.yml //网站的 配置 信息，您可以在此配置大部分的参数。
├── package.json
├── scaffolds 	//模版 文件夹。当您新建文章时，Hexo 会根据 scaffold 来建立文件。
├── source 	//资源文件夹是存放用户资源的地方。
|   ├── _drafts
|   └── _posts
└── themes 	//主题 文件夹。Hexo 会根据主题来生成静态页面。
```

### 3. 新建文章（创建一个 Hello World）
```
$ hexo new "Hello World"
```
 在 `/source/_post` 里添加 `hello-world.md` 文件，之后新建的文章都将存放在此目录下。

### 4. 生成网站
```
$ hexo generate
```
 此时会将 `/source` 的 `.md` 文件生成到 `/public` 中，形成网站的静态文件。

### 5. 服务器
```
$ hexo server -p 3000
```
 输入 `http://localhost:3000` 即可查看网站。

### 6. 部署网站
```
$ hexo deploy
```
 部署网站之前需要生成静态文件，即可以用 `$ hexo generate -d` 直接生成并部署。此时需要在 `_config.yml` 中配置你所要部署的站点：
 ```
 ## Docs: http://hexo.io/docs/deployment.html
	deploy:
	  type: git
	  repo: git@github.com:YourRepository.git
	  branch: master
  ```
### 7. 更多
* 官网 - [[Hexo]](https://hexo.io/zh-cn/)
* 配置相关 - [[Hexo | 配置]](https://hexo.io/zh-cn/docs/configuration.html)
* 更多的命令 - [[Hexo | 指令]](https://hexo.io/zh-cn/docs/commands.html)

那么到此为止网站的雏形算是完成了，接下来你就要自己去管理和完善个人网站了。

## 使用 indego 主题让站点更酷炫
### 1. 主题安装
```
$ cd your-hexo-site
$ git clone git@github.com:yscoder/hexo-theme-indigo.git themes/indigo
```
 从indego的 `Gihub` 仓库中获取最新版本。

### 2. 切换主题
执行 `git branch` 显示所有本地分支，如果只存在一个分支，可以执行下面的命令获取另一分支的主题。
```
# 获取远程 card 分支，并切换
$ git checkout -b card origin/card

# 获取远程 master 分支，并切换
$ git checkout -b master origin/master
```
此命令只需执行一次，之后使用 `git checkout [branch]` 命令在两个主题之间切换。

### 3. 依赖安装
还是在 Hexo 根目录，如果以下插件已安装过，无需再次安装。

#### Less
主题默认使用 less 作为 css 预处理工具。
```
$ npm install hexo-renderer-less --save
```
#### Json-content
用于生成静态站点数据，用作站内搜索的数据源。
```
$ npm install hexo-generator-json-content --save
```


### 4. 开启标签页
```
hexo new page tags
```
修改 `hexo/source/tags/index.md` 的元数据
```
layout: tags
comments: false
---
```

### 5. 开启分类页
```
hexo new page categories
```
修改 `hexo/source/categories/index.md` 的元数据
```
layout: categories
comments: false
---
```

# 最后
[我的博客](http://hellowangwei.github.io/blog)
