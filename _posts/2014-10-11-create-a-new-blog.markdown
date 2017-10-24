---
layout: post
title:  Create A New Blog
date:   2014-10-11 21:27:00
category: "Tools"
---

使用Github Pages 和Jekyll Theme 来新建一个博客
=======

怎么使用Github Pages 创建一个博客呢？
 1. 好吧，最简单的办法，就是clone 一个现有的 [模板](https://github.com/Tikitoo/Android-Getting-Started/blob/master/Tools/Jekyll-theme.md)，就OK 啦。

 2. 然后呢，把工程名改为``username.github.io``，把一些必有的参数改一下，``_config.yml`` 中的URL，等等，其他参数也要改啦。
 3. 然后就是在Github 中，先交New Repo，起名username.github.io；
 4. 最后就是在本地敲Git 啦，
bash
$ git add .
$ git commit -m "first post"
$ git remote add origin https://github.com/username/jekyll_demo.git
$ git push origin gh-pages
```

错误解决 <br />
"fatal: remote origin already exists" <br />
[http://stackoverflow.com/questions/10904339/github-fatal-remote-origin-already-exists](http://stackoverflow.com/questions/10904339/github-fatal-remote-origin-already-exists) <br />
```bash
git remote rm origin
git remote add origin git@github.com:ppreyer/first_app.git
```

#### 注意事项：
要修改的文件：
> - _config.yml
>	- title: Tikitoo Blog  自己的网名
>	- baseurl: "/blog"	   自己创建Repo(仓库)的名
>	- url: 				   设置为空
> - package.json
>	- title: Tikitoo Blog 自己的网名
>	- "repository": {		修改为自己的仓库
>			"url": "https://github.com/Tikitoo/blog.git"
>		}

#### 参考来自：
- [搭建一个免费的，无限流量的Blog----github Pages和Jekyll入门](http://www.ruanyifeng.com/blog/2012/08/blogging_with_jekyll.html)

- [https://github.com/mojombo/tpw](https://github.com/mojombo/tpw)

- [A Jekyll version of the Clean Blog theme by Start Bootstrap](https://github.com/IronSummitMedia/startbootstrap-clean-blog-jekyll)
