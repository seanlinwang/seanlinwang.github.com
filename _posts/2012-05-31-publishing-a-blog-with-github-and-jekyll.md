---
layout: post
title: 使用github和jekyll来写博客 
---

{{ page.title }}
================

<p class="meta">31 May 2012 - Shanghai</p>
<p class="meta">macos 10.7.4</p>

今天想写点东西,狗狗到几篇比较好的文章:

 * [Publishing a Blog with GitHub Pages and Jekyll](http://blog.envylabs.com/2009/08/publishing-a-blog-with-github-pages-and-jekyll/)
 * [Getting Started With Jekyll](http://asymmetrical-view.com/2009/05/14/starting-wtih-jekyll.html)

----------------------------------

失败过n次后, 总结成功步骤:

* 安装rvm

参考[使用rvm在Mac中安装ruby和rails](http://blog.prosight.me/index.php/2011/09/805)

* rvm安装ruby

<pre>
rvm install 1.9.2
</pre>

* 安装rubygems

 参考[ruby-rails-leopard](http://hivelogic.com/articles/ruby-rails-leopard/)

* gem改成淘宝的镜像

<pre>
sudo gem sources --remove http://rubygems.org/ 
sudo gem sources -a http://ruby.taobao.org/ 
</pre>

* 安装jekyll

<pre>
gem install jekyll rdiscount
</pre>

* 搞模版

我直接用github老大自家的[tom.preston-werner](https://github.com/mojombo/mojombo.github.com)

* 本地启动jekyll检查博客

<pre>
jekyll  --auto --server 
</pre>

* 上传到github

本文就是这样生成的!