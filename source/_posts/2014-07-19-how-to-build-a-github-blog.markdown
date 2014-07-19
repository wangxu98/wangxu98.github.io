---
layout: post
title: 如何拥有一个Github博客？
date: 2014-7-19 17:22:59 +0800
comments: true
---

**Q**: 为什么在Github上搭建博客？

**A**: Github page可以很好的展现静态网页页面，而且通过Git来管理博客文件似乎是一件比较酷的事情。[octopress](http://octopress.org/)是一个利用[Jelly](https://github.com/jekyll/jekyll)开发的一个博客系统，将octopress部署到Github上可以很好的搭建一个个人博客。

**Q**: 在Github上搭建博客有哪些具体步骤？

**A**: 依据我在Ubuntu12.04版本上的经验，详细步骤如下:

- **安装Git**

现在ubuntu发行版都自带Git,如果还需了解请点击[这里](http://git-scm.com/)

- **安装Ruby**

RVM(Ruby Version Manager)是管理Ruby的Tool, 所以首先安装RVM:

    $ curl -L https://get.rvm.io | bash -s stable --ruby

Octopress需要的Ruby最低版本是1.9.3，所以运行以下命令:

    $ rvm install 1.9.3
    $ rvm use 1.9.3
    $ rvm rubygems latest

*Tips:如果期间遇到“apt-get 404”的错误该怎么办?*
*A: 首先运行    rvm autolibs read-fail*
*根据提示安装缺失的库文件，然后运行以下命令就可以了  rvm autolibs enable*

- **安装octopress**

利用Git将octopress仓库clone到本机上：

    $ git clone git://github.com/imathis/octopress.git octopress
    $ cd octopress
    $ ruby --version # the version at least is 1.9.3

接着安装相关依赖项：

    $ gem install bundler
    $ rbenv rehash    # If you use rbenv, rehash to be able to run the bundle command
    $ bundle install

*Tips:如果gem install bundler执行很慢，请将根目录下面的Gemfile的源换成[淘宝镜像](http://ruby.taobao.org)*

安装octopress默认主题

    $ rake install

- **配置octopress**

通常情况下只需要配置_config.yml和Rakefile。

Rakefile是跟博客的部署有关，一般情况下不需要改动。

_config.yml主要包括三个部分：Main Configs, Jekyll & Plugins, 3rd Party Settings.

Main Configs主要配置url, title, subtitle等等， Jekyll & Plugins主要包括自带的插件等，3rd Party Settings主要包括第三方插件，以后的扩展主要在这个部分。

更多内容，可以参考[octopress介绍](http://octopress.org/docs/configuring/)

- **博客部署**

首先需要去[Github官网](https://github.com/)申请一个账号，然后建立一个username.github.com的仓库。通常博客的源码会放到source分支下面，生成的内容会放到master分支下。

首先配置github page, 期间会要求提示输入url, 这个url是仓库的地址。配置完了以后会生成_deploy目录，保存生成到master分支的内容。

    $ rake setup_github_pages

完成配置以后，就可以真正部署博客到Github上了。

    $ rake generate
    $ rake deploy

上面的命令是git相关命令的wrapper, 主要生成博客文件并拷贝到_deploy目录里，然后commit和push到远端仓库的master分支。

现在可以通过"http://username.github.com"访问你的博客了！

不过还有一件事情没做，就是把博客源代码管理起来，push到远端仓库的source分支上。

    $ git add .
    $ git commit -m "Initial source commit"
    $ git push origin source

更多内容，可以参考[官方介绍](http://octopress.org/docs/deploying/)

- **markdown格式的博文**

生成一个新的博文：

    $ rake new_post["title"]

title最好是英文，因为该命令会在source/_post/目录下生一个名为YYYY-MM-DD-title.markdown的文件，这个文件名中的title不支持中文。该文件会自动生成以下内容:

    ---
    layout: post
    title: "title"
    date: 2014-7-19 17:22:59 +0800
    comments: true
    ---

可以将title的英文改成中文。

你可以在生成内容以后添加你的博客内容，必须遵守极简的markdown语法，有个入门的教程来自[图灵社区](http://www.ituring.com.cn/article/23)，我觉得非常适合初学者。阳志平老师的介绍[markdown写作](http://www.yangzhiping.com/tech/r-markdown-knitr.html)的文章也非常好，当然还有[官方文档](http://wowubuntu.com/markdown/)。

*Tips:利用sublime来编辑markdown文件，用markdown preview插件来预览*

接下来可以进行部署了：

    $ rake generate
    $ git add .
    $ git commit -m "Publish a new article"
    $ git push origin source
    $ rake deploy

更多内容，参考[官方介绍](http://octopress.org/docs/blogging/)

- **参考资料**

http://beyondvincent.com/blog/2013/08/03/108-creating-a-github-blog-using-octopress/

http://blog.devtang.com/blog/2012/02/10/setup-blog-based-on-github/

http://www.ituring.com.cn/article/23

**Q**: 如何更换默认主题？

**A**: 首先从3rd Party Themes里选择一款你喜欢的主题或者去Google一款，然后执行以下命令：

    $ cd octopress
    $ git clone GIT_URL .themes/THEME_NAME
    $ rake install['THEME_NAME']
    $ rake generate
    $ rake deploy

然后去"http://username.github.com"去看一下你的新主题吧。

**Q**: 如何为我的octopress配置更多的功能？

**A**: 这个问题留着下一篇文章再写吧，因为我也和你们一样在探索成长。





