---
title: Hexo撰文指南
date: 2019-12-11
categories:
- 工具技能学习
- Hexo
tags:
- "Hexo"
- markdown
---
[github*Hexo搭建网站参考网站](https://mp.weixin.qq.com/s?__biz=Mzg5NzIyMzkzNw==&mid=2247483933&idx=1&sn=83025d4b28a2e942b6f6b03afc307e00&chksm=c0745e73f703d765db3a6fc24f438dbf080be564519ed0e9e059f2d05aa11b4ebc41fcdb54e8&mpshare=1&scene=1&srcid=1210QbJ1A3cFhvwTDCBTIVWS&sharer_sharetime=1575974630035&sharer_shareid=a4c679dca6b53ec07fadfa65af7fab43&key=0a80781bf411d282ec9c5a01050c4c3eba63b706a005d47211f2b926a021225b4545d93fc647ceb9d5fee91b844fc26ebdce801bde5251ddea6b944447e21208eb782c7952010ca3124eca4eb7c97abe&ascene=1&uin=MjcyNzI2MjU4Mw%3D%3D&devicetype=Windows+10&version=62070158&lang=zh_CN&exportkey=ASlhsjxzEp1NOdoiRSaztPc%3D&pass_ticket=ohJ2OLzSmGjm11lix78IW3eQC8Pyc5Jkqa%2Bw52NrKcem6YVv7i%2FfQ7Er4Sfm7KAQ)
[安装nodejs](https://zhuanlan.zhihu.com/p/98782798)
然后还需要把.bash_profile添加到~/.bashrc文件中,不然每次启动终端都要重新source.

### 写
[Markdown基本语法链接](https://www.jianshu.com/p/191d1e21f7ed)
1.在`source/_posts`文件夹下面建立*.md新文档。
2.使用的markdown是一种程序化的写文章语言，方便放代码块，但是插图片不方便。
3.插入图片最好的方法，在`/_post`下面建立同名文件夹`hexo n mdName`,将图片放入该文件夹下。文中引用图片时候,`![pngName](*.png)`
![jpg](test.JPG)
4.md语言：
"[超链接] (https://www.jianshu.com/p/191d1e21f7ed)"
>这是引用的内容 前添加符号">"

 **加粗两对*星号**
`单行代码,一对反单引号`

```C++
多行代码
三个反单引号成对
```

### 发布

`hexo clean` 删除public缓存
`hexo g`生成
`hexo s`本地服务器
`hexo d`部署到github，deploy
`sh deploy.sh` 一键部署
