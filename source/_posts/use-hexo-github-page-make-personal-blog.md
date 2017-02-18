---
title: Hexo+Github Page搭建个人博客
date: 2017-02-18 23:36:09
tags: [daily]
---


也就是介绍这个在我一下午无聊时光中折腾出来的网站的由来。

# Github Page

1. 拥有一个自己的github账号。
2. 创建一个Repository，也就是代码仓库，名字为[你的github ID].github.io
3. 暂时先到这里，等会会用到。

# Hexo

这里我用的是Hexo，一个静态博客生成系统，其实Github Page更搭应该是Jkeyll才对，因为之前两次Markdown配置的都不理想，所以这次也就不在那边折腾了。

## 环境配置

这里我以自己的Linux环境为例简单介绍，其它环境参考[官方文档](https://hexo.io/docs/index.html)


### 安装Node.js

``` bash
$ curl https://raw.githubusercontent.com/creationix/nvm/master/install.sh | sh
$ nvm install stable
```

### 安装Hexo

``` bash
$ npm install -g hexo-cli
```

### 初始化目录

``` bash
// 创建自己的工作目录
$ mkdir <folder>
$ hexo init <folder>
$ cd <folder>
$ npm install
```
现在的目录应该是这样的：
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
这里目前我们仅需要关注站点全局配置文件`_config.yml`和存放主题的目录themes即可。

### 验证

因为初始环境中便有一篇Hello-world文章供我们预览，所以我们可以运行下面命令在本地生成预览站点：
```
$ hexo generate
$ hexo server
```
现在在浏览器中打开http://localhost:4000 应该就能看到默认landscape主题的站点了。

### 其它

由于稍后hexo只会把生成好的站点push到我们的github仓库里，如果你有多个工作机都可能参与创建博客，我建议你把当前的工作目录用git管理起来，因为你的站点配置文件、主题配置、主题定制等并不会push到之前创建的那个仓库里，我们可以将环境push自己另一个代码仓库里，比如叫blog-workshop。

## 配置主题

默认的主题是landscape，themes目录里面每个目录都用于存放对应主题。不满意默认主题的可以去github上面搜一下关键字"hexo theme"，我这里用的是上面star最多的next主题，这里仅简单介绍，详细配置参考主题的[官方文档](http://theme-next.iissnan.com/getting-started.html)。


### 下载主题

```
// 切换到之前环境配置的工作根目录下
$ cd <your-hexo-site>
$ git clone https://github.com/iissnan/hexo-theme-next themes/next
```

### 启用主题

编辑`<your-hexo-site>`下的`_config.yml`,找到theme字段，值更改为next。

这里我们可以顺便编辑一下其它常用字段，比如title、author、language等。更多字段含义参考[文档](https://hexo.io/docs/configuration.html)。

### 测试主题

```
$ cd <your-hexo-site>
$ hexo server --debug
```
在浏览器中打开之前的连接，看一下新主题是否还满意。

### 个性化配置

主题本身的配置更改是在主题目录theme/next下的`_config.yml`中。里面可定制的东西还是比较多的，这里我挑几个自己做过的说吧。

1. next主题里面自带有三种模式，在Schemes Settings那里配置，具体效果就需要自己预览了。

2. SiderBar Setting里面可以配置需要展示的社交站点，如github、微博等。

3. 配置评论框，next支持的评论种类比较多，我这里选择的是disqus，精简风格比较贴合主题样式。这里仅需要填写disqus_shortname即可，但是这个shortname不是用户ID或者昵称，需要到disqus官网的Install on Site里面配置需要安装的站点，并在Advanced选项中的Trusted Domains里面填写你的站点域名。[Ref](http://www.jianshu.com/p/a87c070dfcf8)
![disqus](/uploads/blog-disqus.jpg)

4. 统计文章阅读量，里面用的是leancloud提供的服务，具体配置参考[文档](https://notes.wanghao.work/2015-10-21-%E4%B8%BANexT%E4%B8%BB%E9%A2%98%E6%B7%BB%E5%8A%A0%E6%96%87%E7%AB%A0%E9%98%85%E8%AF%BB%E9%87%8F%E7%BB%9F%E8%AE%A1%E5%8A%9F%E8%83%BD.html#%E9%85%8D%E7%BD%AELeanCloud)

5. 站内搜索，参考[Ref](http://theme-next.iissnan.com/third-party-services.html#swfitype)

## 发布

调整到自己满意的配置并在本地进行测试
```
$ hexo server --debug
```
如果站点没有问题，终端日志里也没有错误，我们便可以发布站点到之前创建的Github Page中了，在发布之前，需要在`_config.yml`中配置部署信息。编辑配置文件找到deploy项,填入下列信息即可。
```
deploy:
  type: git
  repo: https://github.com/<github id>/<github id>.github.io
  branch: master
  message: [message] //可不填
```
如此保存后，进行清除、生成、部署
```
$ hexo clean
$ hexo generate
$ hexo deploy
```
交互过程中，需要输入github的账号和密码，便将生成好的网站push到github了。可以进入github查看代码，并访问`https://<github id>.github.io`查看自己最终的成果吧。

# 域名

之前看到新闻说[Godaddy](https://sg.godaddy.com/zh?ci=)已经支持支付宝，果断去买一个域名。挑一个自己喜欢的名字去搜一下相关的域名后缀是否还有，有就果断买下吧。我选的是xyz结尾的，仅因为便宜+好玩。购买之后需要进行一些配置：

1. 配置DNS
在你的DNS记录里面填上下面三条记录：
```
类型   名称           值
A       @      192.30.252.153
A       @      192.30.252.154
CNAME  www   <github id>.github.io
```
TTL都选600s吧，见效快一点。

2. 在站点中创建CNAME文件
在souces目录里面创建名为CNAME的文件，里面填入的你域名，比如我的crazykev.xyz。
再推入github
```
$ hexo g
$ hexo d
```
十分钟后，应该就可以通过该域名访问你的站点了。
