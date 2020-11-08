---
title: Hexo&github blog搭建指南
date: 2019-12-11 12:00:00
categories:
- 工具技能
- Hexo
tags:
- Hexo
---
# Hexo.Github搭建

- [github*Hexo搭建网站参考网站](https://mp.weixin.qq.com/s?__biz=Mzg5NzIyMzkzNw==&mid=2247483933&idx=1&sn=83025d4b28a2e942b6f6b03afc307e00&chksm=c0745e73f703d765db3a6fc24f438dbf080be564519ed0e9e059f2d05aa11b4ebc41fcdb54e8&mpshare=1&scene=1&srcid=1210QbJ1A3cFhvwTDCBTIVWS&sharer_sharetime=1575974630035&sharer_shareid=a4c679dca6b53ec07fadfa65af7fab43&key=0a80781bf411d282ec9c5a01050c4c3eba63b706a005d47211f2b926a021225b4545d93fc647ceb9d5fee91b844fc26ebdce801bde5251ddea6b944447e21208eb782c7952010ca3124eca4eb7c97abe&ascene=1&uin=MjcyNzI2MjU4Mw%3D%3D&devicetype=Windows+10&version=62070158&lang=zh_CN&exportkey=ASlhsjxzEp1NOdoiRSaztPc%3D&pass_ticket=ohJ2OLzSmGjm11lix78IW3eQC8Pyc5Jkqa%2Bw52NrKcem6YVv7i%2FfQ7Er4Sfm7KAQ)
- 安装nodejs: https://zhuanlan.zhihu.com/p/98782798
- 然后还需要把.bash_profile添加到~/.bashrc文件中,不然每次启动终端都要重新source.
- [参考博文](https://io-oi.me/tech/hexo-next-optimization/)
<!--more-->

# Github添加SSH-key

要将本地的库推送到github远程仓库中必须首先生成本地的密钥并添加到github账号中。

https://www.cnblogs.com/syw20170419/p/10732826.html

# Hexo Blog文章撰写

1. 在`source/_posts`文件夹下面建立 .md新文档。
2. 使用的markdown是一种程序化的写文章语言，方便放代码块
3. md语法参考文章[markdown书写指南](./markDown书写指南.md)
4. 插入图片最好的方法，在`/_post`下面建立同名文件夹`hexo n mdName`,将图片放入该文件夹下。

# Hexo发布到网页

Welcome to [Hexo](https://hexo.io/)! This is your very first post. Check [documentation](https://hexo.io/docs/) for more info. If you get any problems when using Hexo, you can find the answer in [troubleshooting](https://hexo.io/docs/troubleshooting.html) or you can ask me on [GitHub](https://github.com/hexojs/hexo/issues).

`hexo clean` 删除public缓存
`hexo g`生成
`hexo s`本地服务器
`hexo d`部署到github，deploy
`sh deploy.sh` 一键部署

### Create a new post

``` bash
$ hexo new "My New Post"
```

More info: [Writing](https://hexo.io/docs/writing.html)

### Run server

``` bash
$ hexo server
```

More info: [Server](https://hexo.io/docs/server.html)

### Generate static files

``` bash
$ hexo generate
```

More info: [Generating](https://hexo.io/docs/generating.html)

### Deploy to remote sites

``` bash
$ hexo deploy
```

More info: [Deployment](https://hexo.io/docs/one-command-deployment.html)

# Github站点配置

hexo提供了很多主题: https://hexo.io/themes/

## 主题配置
1. Next主题,黑白那种:https://theme-next.js.org/docs/advanced-settings/custom-files.html
2. Sakura二次元主题:https://docs.hojun.cn/sakura/docs/#/home
3.diaspora二次元主题:https://github.com/Fechin/hexo-theme-diaspora

## 按更新时间排序文章

> 我用的 VSCode 写博客，所以选择了这个工具 [Insert Date String](https://marketplace.visualstudio.com/items?itemName=jsynowiec.vscode-insertdatestring)。安装好后，OS X 按 `⇧ + ⌘ + I`，Windows 和 Linux 按 `Ctrl + Shift + I`，即可插入当前时间了，体验很爽的！
>
> 有了修改时间，我们再修改默认配置文件就能实现按 updated 排序了，如下。
>
> 找到主配置文件 `_config.yml`，然后修改 `index_generator` 的 `order_by` 为 `-updated` 即可:

```shell
# Home page setting
# path: Root path for your blogs index page. (default = '')
# per_page: Posts displayed per page. (0 = disable pagination)
# order_by: Posts order. (Order by date descending by default)
index_generator:
  path: ''
  per_page: 10
  order_by: -updated
```
最方便的方法:下载`hexo-generator-index-plus`,去官网搜索.

## 修改主页摘要
1. 方法一
在文中添加分隔符:`<!--more-->`
2. 方法二
在文章头部添加description:这是描述
3. 方法三
自动生成摘要.在主题配置文件中加
```shell
auto_excerpt:
  enable: true
  length: 150
```
