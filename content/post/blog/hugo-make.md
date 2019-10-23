+++
description = ""
tags = [
    "hugo",
    "建站",
    "blog",
    "go",
]
date = "2019-10-23"
categories = [
    "hugo",
    "golang",
]
menu = "main"
+++

# 使用HUGO搭建个人博客

[Hugo](https://gohugo.io/)是一个用Go编写的静态站点生成器，由于具有丰富的主题资源和惊人的生成速度而备受青睐。本博客即是基于Hugo搭建，下面讲述一下我的建站历程以及踩的几个大坑（此处手动Doge…）。

## 安装Hugo以及生成第一篇文章

### 安装Hugo

如果你是`macOS`用户，请使用`Homebrew`快速安装

```shell
brew install hugo
1
```

如果你是`Windows`用户，请使用`Chocolatey`或者`Scoop`快速安装，取决于你使用什么包管理

```shell
choco install hugo -confirm
1
scoop install hugo
1
```

如果你是`Debian`或`Ubuntu`用户，请使用`apt`快速安装

```shell
sudo apt-get install hugo
1
```

基本上使用单行命令都可以成功安装Hugo，具体请移步[这里](https://gohugo.io/getting-started/installing)。

### 生成文章

使用如下命令新建一个名为“mysite”的网站：

```shell
hugo new site mysite
1
```

接下来，在[这里](https://themes.gohugo.io/)找到一个漂亮的网站主题。相信我，Hugo的主题海洋会让你纠结到死……经过一番激烈的挣扎，我最终还是投靠给了[Casper](https://themes.gohugo.io/casper/)，没有什么能比一张大图占据整个屏幕带来的冲击感更强了……（说不定过段时间我又折腾着换别的主题了……）。言归正传，以Casper为例，将主题`clone`到本地的`themes`文件夹内，执行以下两句即可：

```shell
cd themes
git clone https://github.com/vjeantet/hugo-theme-casper.git casper
12
```

`mysite/content`是用来存放文档的地方，我们在其下建立一个新的`Markdown`文件：

```shell
hugo new post/first.md
1
```

在`first.md`中写入一些内容，使用如下命令进行本地预览：

```shell
hugo server -t casper -D
1
```

几乎瞬间编译成功，打开网址 http://localhost:1313/ 即可查看本地生成的静态网站。

## 适配主题

Hugo的每个主题都会有不同的参数配置，而这些配置被存放在根目录下的`config.toml`文件中，对于`Casper`主题，[官方的GitHub](https://github.com/vjeantet/hugo-theme-casper)中已经对此作了说明，以下是我的配置：

```
BaseUrl= "https://shanetian.github.io/"
LanguageCode= "zh-cn"
Title= "I am thinking ..."
theme = "casper"
paginate = 5
DisqusShortname = "suixin-1"  # name in comments
Copyright = "All rights reserved - 2018"
canonifyurls = true

[params]
  description = "路在脚下，心向远方"
  metadescription = "Used as 'description' meta tag for both home and index pages. If not set, 'description' will be used instead"
  cover = "images/boy_bicycle.jpg"
  author = "SuiXin"
  authorlocation = "Chengdu, China"
  authorwebsite = "suixinblog.cn"
  authorbio= "Every man dies, not every man really lives."
  logo = "images/bird.png"
  # googleAnalyticsUserID = "UA-79101-12"
  # Optional RSS-Link, if not provided it defaults to the standard index.xml
  # RSSLink = "http://feeds.feedburner.com/..."
  githubName = "ShaneTian"
  email = "tianxinyqq@163.com"
  twitterName = "ShaneTian6"
  # facebookName = ""
  # codepenName = ""
  # linkedinName = ""
  # stackoverflowId = ""
  # keybaseName = ""
  # flickrName = ""
  # instagramName = ""
  # pinterestName = ""
  # googlePlusName = ""
  # set true if you are not proud of using Hugo (true will hide the footer note "Proudly published with HUGO.....")
  hideHUGOSupport = false

  # Setting a value will load highlight.js and enable syntax highlighting using the style selected.
  # See https://github.com/isagalaev/highlight.js/tree/master/src/styles for available styles
  # A preview of above styles can be viewed at https://highlightjs.org/static/demo/
  hjsStyle = "atom-one-light"

  [params.social]
    twitter = "ShaneTian6"

[[menu.main]]
  name = "Home"
  weight = -1
  identifier = "blog"
  url = "/"

[[menu.main]]
  name = "Machine Learning"
  weight = -2
  identifier = "ml"
  url = "/tags/machine-learning"

[[menu.main]]
  name = "Others"
  weight = -3
  identifier = "other"
  url = "/tags/others"

[[menu.main]]
  name = "About"
  weight = -4
  identifier = "about"
  url = "/about"

  [permalinks]
  post = "/:year/:month/:day/:title/"
12345678910111213141516171819202122232425262728293031323334353637383940414243444546474849505152535455565758596061626364656667686970
```

## 文章内的配置

使用命令`hugo new post/***.md`新建`Markdown`文件之后，文章开头会有默认的被`+++`包住的内容，这些内容即是该文章的配置。在`/mysite/archetypes/default.md`中可修改默认的配置。以下为我的默认配置：

```
+++
title = "{{ replace .Name "-" " " | title }}"  # 文章标题
date = {{ .Date }}  # 自动添加日期信息
draft = true  # 设为false可被编译为HTML，true供本地修改
tags = [""]  # 文章标签，可设置多个，用逗号隔开。Hugo会自动生成标签的子URL
comments = true  # 是否开启Disqus评论功能
share = true  # 是否开启分享
+++
12345678
```

## 代码着色

`Casper`主题在`config.toml`文件中的`hjsStyle`选项提供了`highlight.js`的代码着色。所有的代码主题和支持的语言可在[这里](https://highlightjs.org/static/demo/)看到。
但在实际的配置中更改配置选项发现并没有更改代码的颜色……我……
经历了一番数不清的Google之后发现，`Casper`主题的内置`HTML`文件设置的`highlight.js`版本为9.8.0，而现在的`highlight.js`已经更新到9.12.0，故只需修改`/mysite/themes/casper/layouts/partials/header.html`中几行代码为9.12.0即可：

```html
{{ if .Site.Params.hjsStyle }}
    <link rel="stylesheet" href="//cdnjs.cloudflare.com/ajax/libs/highlight.js/9.12.0/styles/{{ .Site.Params.hjsStyle }}.min.css">
    <script src="//cdnjs.cloudflare.com/ajax/libs/highlight.js/9.12.0/highlight.min.js"></script>
    {{ if or .Site.Params.hjsExtraLanguages .Params.hjsExtraLanguages }}
        {{ range $index, $language := (union .Site.Params.hjsExtraLanguages .Params.hjsExtraLanguages) }}
            <script src="//cdnjs.cloudflare.com/ajax/libs/highlight.js/9.12.0/languages/{{ $language }}.min.js"></script>
        {{ end }}
    {{ end }}
    <script>hljs.initHighlightingOnLoad();</script>
{{end}}
1234567891012345678910
```

## 关于图片

`Casper`主题需要设置一个封面图和一个作者头像，选好图片放置在`/static/images`并在`/config.toml`设置即可。
关于浏览器的小图标`favicon`
![img](hugo%E5%BB%BA%E7%AB%99.assets/006tNbRwly1fvimvpcx7dj30do02g74a.jpg)
可将喜欢的照片在[该网站](https://realfavicongenerator.net/)生成各种分辨率的`.ico`文件，将其命名为`favicon.ico`同样放置在`/static/images`即可。`apple-touch-icon`同理。

## 发布网站到GitHub Pages

在使用`hugo server -D`预览网站无误后可正式发布网站到域名供大家浏览。将要发布的文章内`draft`改为`false`后执行命令

```shell
hugo
1
```

可看到根目录下多出`/public`文件夹出来，该文件夹的内容即Hugo生成的整个静态网站。最终我们需要把这些静态网站的文件部署到一个地方，免费且稳定的GitHub Pages是一个很好的选择。具体操作如下：

1. 在GitHub新建一个Repository命名为`ShaneTian.github.io`，其中`ShaneTian`改成自己的GitHub账户名；
2. 在`/mysite`外建立一个平行的文件夹，此处假设也命名为`/ShaneTian.github.io`；
3. 进入`/public`文件夹将内容复制到`/ShaneTian.github.io`；
4. 将`/ShaneTian.github.io`的内容`push`到远程仓库。

以上命令可在命令行通过以下语句实现：

```shell
mkdir ShaneTian.github.io
cd mysite/public
cp -r . ../../ShaneTian.github.io
cd ../../ShaneTian.github.io
git init
git add .
git commit -m "commit message"
git remote add origin https://github.com/ShaneTian/ShaneTian.github.io.git
git push -u origin master
123456789
```

完成以上命令后，等待一分钟左右即可在 https://shanetian.github.io/ 访问你的网站。
以后每次更新文章后只用将生成的`/public`文件夹的静态网站内容复制到`/ShaneTian.github.io`，然后再`push`到远程仓库即可。也可将步骤写为`Shell`脚本，此处不再赘述。

## 使用自己的域名

当然，GitHub的域名怎么能满足装*的心理，这时可以将网站设置为自己的域名。购买域名的地方很多，如国外知名网站[GoDaddy](https://sg.godaddy.com/zh/)，简体中文页面、支持支付宝付款、不用备案等都带来了很多方便。我的域名[suixinblog.cn](https://suixinblog.cn/)即是在此购买。购买域名后完成下面两步即可访问自己的域名：

1. 在上面存放静态网站的Repository设置里面Custom domain填上自己的域名点击save；
2. 执行以下语句获得自己的GitHub Pages的IP：

```shell
ping shanetian.github.io
1
```

即可得到下面的IP地址：
![img](hugo%E5%BB%BA%E7%AB%99.assets/006tNc79ly1fvmz2x6huhj30zq04ytbn.jpg)
\3. 在域名提供商的网站配置DNS即可，以Godaddy为例配置两条记录如下：
![img](hugo%E5%BB%BA%E7%AB%99.assets/006tNc79ly1fvmz5res74j31ic0nqtbg.jpg)
\4. 可以在GitHub Pages设置强制https协议，这样你的网站就有安全锁啦～
![img](hugo%E5%BB%BA%E7%AB%99.assets/006tNc79ly1fvmzc3x3wtj315m0y2jx9.jpg)
![img](hugo%E5%BB%BA%E7%AB%99.assets/006tNc79ly1fvmzdi2rllj309o01wjrf.jpg)

大功告成！快去访问你的域名吧！
**Tips**：
另外一款好看的Hugo主题：[hugo-nuo](https://github.com/laozhu/hugo-nuo)