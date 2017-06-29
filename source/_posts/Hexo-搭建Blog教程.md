---
title: Hexo 搭建Blog教程
date: 2017-06-28 19:48:25
tags:
copyright: true
---
平时在浏览博客的时候发现好多人喜欢利用Hexo + Github搭建个人博客，正好一直也想搭建一个博客去记录和分享。在此过程中，折腾各种配置花了一些时间，所以详细记录一下遇到的相关问题。
###  1.安装Git
下载[git](https://git-scm.com/download/win),安装很简单，一路next下去
###  2.安装Node.js
下载[node.js](https://nodejs.org/en/)，安装也是一路next
### 3.本地搭建hexo博客
安装hexo
``` bash
$ npm install -g hexo-cli
```
测试hexo是否安装成功
``` bash
$ hexo version
```
新建一个文件夹，如MyBlog
进入该文件夹内，右击运行git，依次输入
``` bash
$ hexo init
$ npm install
$ hexo g
$ hexo s
```
这时候在浏览器输入http://localhost:4000/ 就可以看到hexo已经成功生成了博客，当然这只是我们本地可以看到的
### 3.Github配置
登陆github后点击new repository，Repository name填写自己的名称 + .github.io，
![](/upload_image/1.jpg)
直接点Create repository就可以了。
### 4.配置Github SSH密钥
在桌面空白处鼠标右键选择Git Bash Here
``` bash
$ ssh-keygen -t rsa -C "your's emaill address"
```
引号里面的内容输入你的邮箱地址，然后回车，会提示你文件保存的路径，按回车键确认
然后会提示你输入密码，然后会确认输入一次，就可以在刚刚的路径看到生成了两个文件，一个是id_rsa，另一个是id_rsa.pub，用notepadd++打开id_rsa.pub然后选中里面的全部内容，复制下来。
登陆github，点击头像进入setting选项
![](/upload_image/2.jpg)
### 5.将hexo博客与Github关联
修改_config.yml
``` bash
deploy:
  type: git
  repository: http://github.com/yourname/yourname.github.io.git
  branch: master
```
打开Git bash here
``` bash
$ hexo g
$ hexo d
```
如果出现如下异常
``` bash
ERROR Deployer not found: git
```
输入以下命令，然后重新执行上面两条命令
``` bash
$ npm install hexo-deployer-git --save
```
这时候如果弹出一个对话框，输入在guthub上面的用户名和密码即可
这时候我们就可以在浏览器输入http://yourname.github.io（yourname替换成github上的名称）就可以看到博客已经搭建成功了。
### 更新博客内容
在MyBlog目录下执行：hexo new “我的第一篇文章”，会在source->_posts文件夹内生成一个.md文件。
编辑该文件（遵循Markdown规则）
修改起始字段
title 文章的标题
date 创建日期 （文件的创建日期 ）
updated 修改日期 （ 文件的修改日期）
comments 是否开启评论 true
tags 标签
categories 分类
permalink url中的名字（文件名）
编写正文内容（MakeDown）
hexo clean 删除本地静态文件（Public目录），可不执行。
hexo g 生成本地静态文件（Public目录）
hexo deploy 将本地静态文件推送至github（hexo d）
#### MarkDown语法
``` bash
[hexo](http://www.baidu.com)  表示超链接
##大标题
###小标题
<!-- more -->
<!-- 标签别名 -->
{% cq %}blah blah blah{% endcq %}
空格  中文全角空格表示
---
文章标题
---
>内容     区块引用
*1
*2
*3
列表
*内容*     表示强调内容
![Alt text](/path/to/img.jpg)  图片
![](/upload_image/20161012/1.png)

```
<blockquote class="blockquote-center"></blockquote>
